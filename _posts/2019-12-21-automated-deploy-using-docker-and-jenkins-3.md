---
layout: post
title: 도커, 젠킨스, AWS ECR, ECS 로 CI/CD 구축 - 3 AWS ECS  
tags: [AWS ECS, DevOps]
categories: [DevOps]
---

이제 `AWS ECS`를 설정해서 파게이트를 띄워야 한다. `AWS ECS`를 쓰려면 알아야 할 것들을 정리했다.



#### 클러스터(cluster)

ECS의 기본단위로, 컨테이너를 실행하는 가상의 공간이다. 



#### 컨테이너 인스턴스(container instance)

클러스터에 속한 인스턴스다. EC2 또는 파게이트를 선택할 수 있다. 



#### 태스크 디피니션(task definition)

태스크를 실행하기 전에 만드는 스펙이다. 여기에는 컨테이너 네트워크 모드, 역할, 이미지, 실행 명령어 등등 설정을 할 수 있다. 태스크 디피니션을 아마존 콘솔에서 만들어 놓고, `task definition.json` 을 프로젝트에 포함시켜 배포할 때 마다 다르게 설정 할 수 도 있다.



#### 태스크(task)

컨테이너를 실행하는 단위. 


#### 서비스(service)

태스크를 관리하는 단위. 



요약하자면 클러스터 안에는 컨테이너 인스턴스가 있고, 컨테이너 인스턴스를 실행하려면 태스크가 필요하다. 태스크 디피니션을 만들어서 태스크를 어떻게 실행할 것인지 결정한다. 한번 만들어진 태스크 디피니션의 설정값은 디폴트 값처럼 적용된다. 태스크 디피니션은 컨테이너를 배포할 때 마다 변경할 수 있다. 

1. 클러스터 생성  
먼저 클러스터를 만든다. 파게이트를 선택하고, 이름을 정한다. VPC는 원한다면 새로 만들고, 기존 것을 사용할거면 그대로 둔다. 컨테이너 인사이트는 클러스터, 컨테이너, 서비스 레벨에서 지표를 수집하는 기능이다. 나중에 aws-cli로도 활성화 시킬 수 있다.
![ECS 1](/images/posts/ecs-01.png)
![ECS 2](/images/posts/ecs-02.png)
create 버튼만 누르면 ECS 클러스터가 바로 생성된다. 

2. 태스크 디피니션 생성  
이제 태스크 디피니션을 생성한다. 
![task definition](/images/posts/taskdefinition-01.png)
create 을 선택하면 첫 화면에서 파게이트/EC2 둘 중에 하나를 선택할 수 있다. 이번에는 파게이트를 선택한다.  
![task definition](/images/posts/taskdefinition-02.png)
태스크 디피니션의 이름을 정한다. 그리고 IAM Role 도 선택해야 한. ECS와 클라우드 워치 관련 권한을 가 role 을 만들어서 추가한다.  
[task definition](/images/posts/taskdefinition-03.png)
다음은 태스크 사이즈를 정해햐 한다. 일단은 가장 작은 사이즈로 정한다. 

