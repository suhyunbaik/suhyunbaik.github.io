---
layout: post
title: 도커, 젠킨스, AWS ECR, ECS 로 CI/CD 구축 - 2 젠킨스 파일 작성
tags: [Docker, Jenkins, DevOps]
---

이번에는 젠킨스 파일을 작성한다.

젠킨스는 2가지 문법을 지원한다. `Scripted`, `Declarative` 2가지가 있다. `Scripted` 문법은 쉘 스크립트처럼 작성해서 파이프라인을 구성할 수 있다. `Groovy` 를 써봤으면 쉽게 쓸 수 있다. 그래서 이번에는 `Scripted` 문법을 사용했다.

다음은 `Scripted` 문법 전용 `Directive` 다.

`stage`  : 파이프라인의 각 단계

`git` : git 에서 프로젝트 clone

`sh` : unix 환경에서 실행할 명령어 실행

`def`: 변수 또는 함수 선언

`dir`: 명령을 수행할 디렉토리/ 폴더 정의



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

