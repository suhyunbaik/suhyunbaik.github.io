---
layout: post
title: AWS Lambda를 사용해 스테이징 서버 start, stop을 자동화하기
tags: [AWS Lambda, DevOps]
---



서버 비용 절감을 위해서 스테이징 서버를 근무시간에만 띄우기로 결정했다. db도 최근 스냅샷을 갖고와서 런칭해 사용하기로 했다. 스테이징 서버는 웹서버용 `AWS ECS fargate`와, `Redis`용 `EC2 instance`, `Rabbitmq` 용 `EC2 instance`, `RDS  instance` 1개로 구성된다. `AWS Console`에 들어가서 마우스 클릭으로도 충분히 서버를 실행하고 끌 수 있지만, 다른 instance 를 실수로 중지할 수 있는 위험이 있다. 그리고 현재 사용하고 있는 스테이징 용 db와 새 스테이징의 vpc가 다르다. 스냅샷으로 인스턴스를 런칭할 때 다른 vpc에서 instance 를 런칭해야 하는데, 이렇게 하려면  `aws console` 의 `vpc` 섹션에서 수동으로 매번 지정해줘야 한다. 그리고 서버 사용이 끝나면 `rds instance`를 삭제해야 하는데 이 작업 역시 수동으로 하고 싶지 않는 작업이다.

이런 반복 작업에 버리는 시간이 아까워서 `AWS Lambda` 에 `boto3` 라이브러리를 사용해 자동으로 서버를 띄우고 내리는 기능을 만들었다.



스테이징 서버를 중지하는 스크립트.

```python
# stop staging server
import boto3
import os

REDIS = os.environ['REDIS']
RABBITMQ = os.environ['RABBITMQ']
DB = os.environ['DB']
TASK = os.environ['TASK']


def lambda_handler(event, context):
    # stop redis, rabbitmq
    boto3.client('ec2', region_name='us-east-1').stop_instances(InstanceIds=[REDIS, RABBITMQ])

    # delete rds
    boto3.client('rds', region_name='us-east-1').delete_db_instance(DBInstanceIdentifier=DB,
                                                                    SkipFinalSnapshot=True,
                                                                    DeleteAutomatedBackups=True)
    # stop ecs task
    boto3.client('ecs', region_name='us-east-1').stop_task(cluster='cluster',
                                  task='',
                                  reason='')

```



스테이징 서버를 시작하는 스크립트.

```python
# start staging server
import boto3
import os

REGION = 'us-east-1'
REDIS = os.environ['REDIS']
RABBITMQ = os.environ['RABBITMQ']
STAGING_IDENTIFIER = os.environ['STAGING_IDENTIFIER']
MASTER_IDENTIFIER = os.environ['MASTER_IDENTIFIER']
DB_CLASS = os.environ['DB_CLASS']
VPC_ID = ''


def lambda_handler(event, context):
    # rds 시작
    start_db()

    # redis, rabbitmq 시작
    boto3.client('ec2', region_name=REGION).start_instances(InstanceIds=[REDIS, RABBITMQ])


def start_db():
    rds = boto3.client('rds', region_name=REGION)
    snapshot = get_latest_snapshot(rds)
    restore_db_instance(rds, snapshot)


def get_latest_snapshot(client):
  	# 가장 최근 스냅샷을 갖고온다.
    snapshots = client.describe_db_snapshots(
        DBInstanceIdentifier=MASTER_IDENTIFIER,
        SnapshotType='automated',
        MaxRecords=20)['DBSnapshots']

    ordered_snapshots = sorted(
        snapshots, key=lambda x: x["SnapshotCreateTime"], reverse=True
    )

    source = ordered_snapshots[0]
    return source


def get_current_db_instance_settings(client, snapshot_db_identifier):
  	# db 인스턴스 세팅을 갖고온다.
    db_instance = client.describe_db_instances(
        DBInstanceIdentifier=snapshot_db_identifier
    )['DBInstances'][0]

    return db_instance


def restore_db_instance(client, snapshot):
  	# 스냅샷과 db 인스턴스 세팅으로 새 instance를 다른 vpc에 런칭한다.
    snapshot_db_identifier = snapshot['DBInstanceIdentifier']
    snapshot_identifier = snapshot['DBSnapshotIdentifier']

    current_db_instance_settings = get_current_db_instance_settings(
        client, snapshot_db_identifier
    )

    availability_zone = current_db_instance_settings['AvailabilityZone']

    db_subnet_group_name = ''
    engine = current_db_instance_settings["Engine"]
    storage_type = current_db_instance_settings['StorageType']
    vpc_security_group_ids = ['vpc_security_group_id']

    db_parameter_group_name = current_db_instance_settings[
        'DBParameterGroups'][0]['DBParameterGroupName']

    client.restore_db_instance_from_db_snapshot(
        DBInstanceIdentifier=STAGING_IDENTIFIER,
        DBSnapshotIdentifier=snapshot_identifier,
        DBInstanceClass=DB_CLASS,
        AvailabilityZone=availability_zone,
        DBSubnetGroupName=db_subnet_group_name,
        MultiAZ=False,
        PubliclyAccessible=False,
        AutoMinorVersionUpgrade=True,
        Engine=engine,
        StorageType=storage_type,
        VpcSecurityGroupIds=vpc_security_group_ids,
        DBParameterGroupName=db_parameter_group_name
    )
```

인스턴스를 런칭 또는 중지 하는 액션을 할때는 람다에 서브넷을 설정해 줄 필요가 없다. 해당 스크립트를 람다 콘솔에 작성한 후 `AWS cloud watch` 트리거를 설정해준다. 
![deploy process](/images/posts/lambda-trigger.png)  

크론탭을 사용해봤던 사람이라면 쉽게 트리거를 작성할 수 있다. 나는 근무시간에만 스테이징 서버를 띄울 계획이기 때문에, 월요일 부터 금욜일 까지 9-6를 cron expression으로 작성해 추가해준다.
