---
title: 레거시 프로젝트에 강결합된 배포&운영 서비스 교체 -1 
layout: page
tags: [DevOps, Django, EC2 Fargate]
categories: [DevOps]
---


문제의 레거시 프로젝트는 대략 2012년부터 시작해, 문서 한장 없이 구전을 통해 스펙이 전해졌다. 이 프로젝트는 서버 모니터링을 할때 AWS OpsWorks 라는 서비스를 사용하고, Django Fabric 패키지를 사용해 짠 코드를 사용해 배포한다. 

OpsWorks 와 Fabric 을 사용하는 배포 방식은 옛날에는 아무 문제가 없었을테지만, 내가 맡게 된 2019년에는 이미 여러가지 문제점이 있었다. 

문제점들

* Fabric 코드와 OpsWorls 서비스는 강하게 결합 되어있어, 다른 서비스(예를 들지만 code deploy)를 사용할 수 없다. 만약 다른 서비스를 사용하고 싶다면 Fabric을 사용하는 코드 부분을 전부 바꾸거나, 덜어내야한다. 

* OpsWorks 내에 인스턴스를 등록할때 인스턴스에 이름을 줄 수 있다. 만약 레디스 용 인스턴스를 런칭한 후 OpsWorks에 등록해 Redis1이라는 이름을 줬다면, 다른 인스턴스 설정에는 레디스 인스턴스의 IP 대신 Redis1이라고 적어도 바로 연결된다. 편리해 보이지만 지금처럼 OpsWorks를 제거하려고 하면 모든 설정파일에서 Ip 주소 대신 이름이 적힌 부분을 찾아서 다 바꿔줘야 한다.

* OpsWorks로 런칭한 인스턴스에는 OpsWorks agent 가 자동으로 설치된다. AMI를 뜬 후 해당 AMI 를 사용해서 인스턴스를 런칭하면 agent 프로세스가 계속 실행되면 인스턴스의 상태를 AWS 쪽에 쏘게되고. 해당 agent 를 제거하는 방법이 안쉽다.

* OpsWorks에서 배포할때는 chef 라는 걸 사용하는데, 해당 프로그램은 루비로 프로그래밍한다. 만약 cronjob이나 virtualenv, python, package 등등 의 버전이 변경될 경우 chef 를 수정해줘야 한다. (그리고 이팀에는 루비 개발자가 없다)

* 로드밸런서가 elb 가 아니고, OpsWorks classic 로드밸런서를 사용한다. OpsWorks classic 로드밸런서는 elb에 비해서 기능이 제한적이다.

* 트래픽이 몰려서 로드밸런싱할때, AMI에서 인스턴스를 런칭하는 방식이 아니다. OpsWorks 에 미리 등록된 인스턴스를 부팅 한 후, 쉐프가 처음부터(virtualenv 패키지 설치 부터 완전히 새로) 세팅을 한다. 이 작업이 끝난 후에야 로드밸런서에 연결되기 때문에 느리다.

* OpsWorks를 이제 많이 사용하지 않는 추세이기 때문에 문제점이 발생했을 때 찾아보기가 힘들고, 공식문서도 더이상 유지보수 되지 않는다.

  

이 외에도 다양한 문제점이 있어서 OpsWorks 서비스를 더이상 사용하지 않기로 결정헀다. 가장 많이 사용하는 기능이 배포 기능이라, 배포를 다른 서비스로 바꿔야 했다. 그런데 Ec2 Instance를 버리고 컨테이너 기반 Fargate 로 가기로 결정이 됐다. OpsWorks를 다른서비스로 교체하는 결정도 기술적인 큰 변화인데, 교체와 동시에 Fargate를 사용하는 것도 큰 기술 변화라 걱정이 앞섰다.
