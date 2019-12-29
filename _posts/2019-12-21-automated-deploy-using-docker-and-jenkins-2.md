---
layout: post
title: 도커, 젠킨스, AWS ECR, ECS 로 CI/CD 구축 - 2 젠킨스 파일 작성
tags: [Docker, Jenkins, DevOps]
---
updated: 2019-12-27  
이번에는 젠킨스 파일을 작성한다.

젠킨스는 2가지 문법을 지원한다. `Scripted`, `Declarative` 2가지가 있다. `Scripted` 문법은 쉘 스크립트처럼 작성해서 파이프라인을 구성할 수 있다. `Groovy` 를 써봤으면 쉽게 쓸 수 있다. 그래서 이번에는 `Scripted` 문법을 사용했다.

다음은 `Scripted` 문법 전용 `Directive` 다.

`stage`  : 파이프라인의 각 단계

`git` : git 에서 프로젝트 clone

`sh` : unix 환경에서 실행할 명령어 실행

`def`: 변수 또는 함수 선언

`dir`: 명령을 수행할 디렉토리/ 폴더 정의

나는 스테이지를 3개로 나눴다. 

첫번째 스테이지는 도커 이미지를 빌드한다. 내가 작성한 도커 파일은 `AWS ECR`에 있는 베이스 이미지를 갖고 오기 위해 AWS에 로그인 한다. `region`은 명시하지 않아도 된다. 젠킨스에서는 `aws`에서 로그인 할 때 `aws cli` 를 사용한다. `aws cli`를 사용해봤던 개발자들은 알고 있겠지만, `AWS configuration`을 해야 `aws cli`를 사용할 수 있다. 내가 사용하는 사내 젠킨스는 서버내 `aws configuration`이 다른 팀에서 사용하는 다른 리전의 계정이 세팅되어 있어서, 리전을 명시해줘야 내가 만든 `AWS ECR`에 접근 할 수 있었다. 로그인을 하면 캐싱없이 베이스 이미지를 기반으로 빌드한다. 

두번째 스테이지에서는 빌드한 도커 이미지를 `AWS ECR`에 푸시한다. 

세번째 스테이지에서는 `AWS ECS` 에 배포한다. 첫번째 스테이지에서 로그인을 해도, 다른 서비스를 사용하려면 `region`을 명시해줘야 `No basic auth` 라는 `authorization`  관련 에러를 피할 수 있다. ECS에 배포하기 전에 ECS container의 spec이 적힌 task definition의 파일의 내용을 적절하게 바꾼다. 파일을 열어서 문자를 치환할때는 `sed` 명령어가 유용하므로 그것을 써서 치환한다.



```shell
pipeline {
    agent any
    environment {
        imageUrl = "$ecsRegistry:$BUILD_NUMBER"
    }
    stages {
        stage("Docker Build") {
            steps {
                withAWS(credentials: "$awsCredentials", region: "us-east-1") {
                    script {
                        def login = ecrLogin()
                        sh "${login}"
                    }
                }
                sh "docker pull $baseImage"
                sh "docker build --no-cache -t $imageUrl --build-arg SSH_PRIVATE_KEY='$privateKey' --build-arg BASE_IMAGE='$baseImage' ."
            }
        }
        stage("Docker Push") {
            steps {
                sh "docker push $imageUrl"
            }
        }
        stage("Deploy ECS") {
            steps {
                withAWS(credentials: "$awsCredentials", region: "us-east-1") {
                    script {
                        def safeImageUrl = imageUrl.replace("/", "\\/")
                        sh "sed -e \"s/%memoryUnit%/$memoryUnit/g; s/%cpuUnit%/$cpuUnit/g; s/%imageUrl%/$safeImageUrl/g; s/%taskFamily%/$taskFamily/g; s/%awsLogGroup%/$awsLogGroup/g; s/%awsLogRegion%/$awsLogRegion/g; s/%awsLogPrefix%/$awsLogPrefix/g;\" taskDefinition > taskDef.json"
                        def taskVersion = sh(script: "aws ecs register-task-definition --cli-input-json file://taskDef.json | jq --raw-output .taskDefinition.revision", returnStdout: true).trim() as Integer
                        sh "echo $taskVersion"
                        sh "aws ecs update-service --cluster $clusterName --service $serviceName --task-definition $taskFamily:$taskVersion"
                    }
                }
            }
        }
    }
}
```

이제 `AWS ECS`를 설정하고, 배포할때 사용할 `task definition.json`파일을 작성해야 한다.
