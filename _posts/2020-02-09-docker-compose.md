---
layout: page
title: 도커 컴포즈 사용하기
tags: [DevOps, Docker compose]
categories: [DevOps]
---



1.Django 서비스

​	1-1.`build` 

​	1-2. `environment`

​	1-3 `ports`

​	1-4 `volumes`

​	1-5 `depends_on`, `links`

​	1-6 `stdin_open`, `tty`

​	1-7 `network`

2.mysql 서비스

​	2-1 `command`

3.docker compose 실행, 중지



로컬에서 개발하며 테스트를 하려면 `rabbitmq	`, `elastic search`, `redis	` 등 필요한 서비스가 많아서, 개발용 도커 컴포즈 파일을 만들기로 했다. 



도커 컴포즈 버전을 먼저 명시한다.

```docker
version: '3'
```



도커 컴포즈에서는 `service`라는 개념을 사용한다. `service`밑에 사용할 컨테이너를 정의한다.

```shell
services:
```



1.Django 서비스



장고 웹서버 서비스 이름을 `django` 로 정했다.

```docker
django:
    build:
        context: .
        dockerfile: Dockerfile-dev
        args:
          - SSH_PRIVATE_KEY=id_rsa
    ports:
      - "80:80"
    restart: always
    environment:
      - DJANGO_DEBUG=True
      - DJANGO_DB_HOST=mysql
      - DJANGO_DB_PORT=3306
      - DJANGO_DB_NAME=rabbit
      - DJANGOD_DB_USERNAME=user
      - DJANGO_DB_PASSWORD=psswd
      - MQ_HOST=mq
      - ES_HOST=es
      - REDIS_HOST=redis
    volumes:
      - ".:/django"
    depends_on:
      - mysql
      - mq
      - es
      - redis
    links:
      - mysql
      - mq
      - es
      - redis
    stdin_open: true
    tty: true
    networks:
      - default
```



1-1.`build` 

 `rabbitmq`, `mysql` 같은 서비스는 도커 허브에 있는 이미지를 이용하면 되기 때문에 build 옵션이 필요없다. 앱 서비스는 보통 직접 만든 도커 파일을 사용해 빌드한 이미지로 컨테이너를 띄우기 때문에, 도커 컴포즈에서도 도커 파일을 가지고 빌드 하도록 해줘야 한다. 이럴떄 쓰는게 `build` 옵션이다.

```dockerfile
build:
	context: .
	dockerfile: Dockerfile-dev
  args:
  	- SSH_PRIVATE_KEY=id_rsa
```

`context` 는  `docker build` 를 실행할 디렉토리 경로다.

`dockerfile` 에는 개발용으로 만든 도커 파일을 지정한다.

`args` 는 도커 이미지를 빌드할때 넘겨줄 `arguments` 를 적는다. 



1-2. `environment`

`environment` 에는 `docker run	` 할때 주는 옵션을 적는다.

```docker
environment:
      - DJANGO_DEBUG=True
      - DJANGO_DB_HOST=mysql
      - DJANGO_DB_PORT=3306
      - DJANGO_DB_NAME=rabbit
      - DJANGOD_DB_USERNAME=user
      - DJANGO_DB_PASSWORD=psswd
      - MQ_HOST=mq
      - ES_HOST=es
      - REDIS_HOST=redis
```

앱 서비스가 mysql 서비스와 연결하려면 `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USERNAME`, `DB_PASSWORD`  등등의 정보가 필요하다 .

 `DB_HOST` 에는 mysql 서비스 이름을 적는다. 그래야 django 서비스가 mysql 서비스에 접속할 수 있다.



1-3 `ports`

    ports:
      - "80:80"

서비스의 포트와, 여기에 연결할 호스트의 포트를 적는다.



1-4 `volumes`

볼륨을 생성한다.



1-5 `depends_on`, `links`

`depends_on`	은 의존성을 정하고 컨테이너 생성 순서를 정한다. `links`는 서비스 이름이나 알리아스로 서비스에 접근할 수 도록 해준다.



1-6 `stdin_open`, `tty`

`stdin_open=true`, `tty=true` 옵션을 주면 서비스가 입력을 받을 수 있는 상태로 실행된다.



1-7 `network`

 서비스들을 같은 네트워크에 할당하면 서비스끼리 서로 연결할 수 있다. 



2.Mysql서비스 

```docker
  mysql:
    image: mysql:5.7
    command: mysqld --character-set-server=utf8mb4 --collation-server=utf8mb4_general_ci
    restart: always
    ports:
      - "3306:3306"
    environment:
      - MYSQL_DATABASE=bghd
      - MYSQL_USER=user
      - MYSQL_PASSWORD=psswd
      - MYSQL_ALLOW_EMPTY_PASSWORD=1
    volumes:
      - ".:/mysql"
    networks:
      - default
```



mysql 은 도커 허브에서 공식 이미지를 다운받아 빌드한다.

2-1 `command`

`docker run` 로 컨테이너를 실행할 때 주는 명령어 부분이다. 여기서는 `character-set` 과 `collation-server` 를 `utf8mb4`로 지정해줬다.

나머지 부분은 앱 서비스를 작성했을때와 비슷하다.



2.도커컴포즈 관련 명령어

```shell
docker-compose up #실행
docker-compose up --build #이미지 빌드 후 실행
docker-compose stop #중지
docker-compose down #삭제
docker-compose down --volume #볼륨에 저장된 데이터도 삭제
```





