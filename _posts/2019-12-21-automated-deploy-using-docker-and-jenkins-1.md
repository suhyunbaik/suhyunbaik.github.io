---
layout: post
title: 도커, 젠킨스, AWS ECR, ECS 로 CI/CD 구축 - 1 도커파일 작성 
tags: [Docker, Jenkins, DevOps]
categories: [DevOps]
---

updated: 2019-12-25  
최근 ECS를 사용해서 컨테이너 기반으로 스테이징 서버를 구축했다. Circle CI를 써보고 싶었는데, 이미 사내에 젠킨스가 구축되어 있어서 젠킨스를 쓰기로 했다.  

![deploy process](/images/posts/deploy-process.png)  
먼저 빗버킷 `dev`, `staging` 브랜치에 푸시가 되면 젠킨스가 변화를 감지하고 스크립트 실행한다. 젠킨스 스크립트는 브랜치별로 다른 configuration 파일을 s3 버킷에서 다운받는다. 해당 파일을 소스코드에 추가하고 도커 이미지를 생성하, 생성된 도커 이미지는 `AWS ECR`에 푸시한다. 새 이미지를 가지고 `AWS ECS` 파게이트에 배포한다.

먼저 도커파일을 작성해서 로컬에서 띄어보았다. 해당 프로젝트는 빗버킷 프라이빗 리포지토리를 `pip install` 로 설치해 라이브러리로 사용하는데, 이때 ssh key 가 필요하다. 그래서 로컬의 ssh key를 카피해서 도커 실행시 argument로 전달하는 방법으로 해결했다. 


`python 2.7 alpine` 버전을 사용하는 도커 파일을 작성한다.

```dockerfile
# arguments
ARG SSH_PRIVATE_KEY

FROM python:2.7-alpine

# set envs
ENV DJANGO_SETTINGS_MODULE=staging
ENV PYTHONIOENCODING=utf-8

RUN apk add --no-cache\
	python2-dev\
	build-base\  # for gcc
	mariadb-dev\  # for mysqlclient
	openssh-client\ # for ssh-keyscan
	zlib-dev\  # for pillow
	jpeg-dev\  # for pillow
	libressl-dev\  # for scipy, numpy
	libffi-dev\  # for scipy, numpy
	lapack-dev\  # for scipy, numpy
	gfortran\  # for scipy, numpy
	musl-dev\  # for scipy, numpy
	git\
	nginx\
	vim\
    && rm -rf /var/cache/apk/* \ #remove cache
    && pip install cpython  # for scipy, numpy

RUN mkdir -p /root/.ssh && \
    chmod 0700 /root/.ssh && \
    ssh-keyscan bitbucket.org > /root/.ssh/known_hosts

RUN echo "$SSH_PRIVATE_KEY" > /root/.ssh/id_rsa && \
    chmod 400 /root/.ssh/id_rsa

# create working directory
RUN mkdir -p /app

ADD app /app

# set working directory
WORKDIR /app

# install dependencies
RUN pip install -r requirements.txt

# run django project
CMD ["python", "manage.py", "runserver", "0.0.0.0:80"]

# open port 80, 5672 for rabbitmq
EXPOSE 80 5672

```

도커 이미지를 `test` 라는 이름으로 빌드한다.
```shell
docker build -t test --build-arg SSH_PRIVATE_KEY="$(cat ~/.ssh/id_rsa)" .
```


도커 이미지로 컨테이너를 띄운다.
```shell
docker run -it --rm test:latest
```



도커 컨테이너가 떴는지 확인한다.
```shell
docker ps
```



만약 컨테이너가 떴다면 터미널 화면 또는 인터넷 브라우저에서 `0.0.0.0:8000` 주소로 확인할 수 있다. 만약 문제가 있어서 컨테이너가 안떴다면 다음 명령어로 정지한 컨테이너를 확인할 수 있다.
```shell
docker ps -a
```



도커 컨테이너를 확인하고 컨테이너를 삭제한다. 이 방법은 젠킨스에서 자동배포할때 사용할 수 없다. 그리고 해당 프로젝트가 설치하는 패키지가 많아서 이미지를 빌드하는데 기본적으로 1분이 넘었다. 여러가지 방법이 있을 수 있겠지만 나는 이전에 빌드된 이미지를 베이스 이미지로 사용하는 방법을 선택했다. 해당 프로젝트는 사용하는 라이브러리가 크게 바뀌지 않을 예정이여서 가능했다. 먼저 빌드한 이미지를 `base image` 라는 이름으로 `AWS ECR`에 푸시하고, 이미지를 빌드 할 때 마다 `base image` 를 갖고와서 그 이미지를 기반으로 빌드하는 방법이다.    

이 방법은 다음과 같은 스텝을 거쳤다.  
1. 베이스 이미지로 사용할 이미지를 로컬에서 빌드한다. 
2. `AWS ECR`에 푸시한다.
3. 젠킨스에 `Build with parameters` 설정에서 Base image의 태그를 추가한다.
4. 테스트 한다.

`AWS ECR` 콘솔로 들어가서 `create repository` 를 선택한다.
![aws ecr console](/images/posts/ecr-console.png)  

repository 이름만 정하면 바로 생성할 수 있다. 생성된 repository 를 목록에서 클릭하면 `view push commands` 에서 이미지를 푸시, 풀하는 데 사용하는 커맨드를 볼 수 있다.
이미지를 푸시하는 커맨드를 사용해 로컬에 있는 베이스 이미지를 푸시한 뒤, 도커 파일의 윗부분을 수정한다.

```dockerfile
# arguments
ARG SSH_PRIVATE_KEY
ARG BASE_IMAGE

FROM $BASE_IMAGE
```

이제부터 도커 이미지를 빌드할때는 argument로 준 이미지를 기반으로 빌드하게 된다. `AWS ECR`에 있는 이미지를 받아오는 부분은 젠킨스가 할 일이다.  
2편에서는 젠킨스를 설정한 방법을 정리했다.

