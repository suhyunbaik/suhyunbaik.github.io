---
layout: page
title: 알파인 리눅스
tags: [DevOps, AlpineLinux]
categories: [DevOps]
---



1.앱/프로그램 설치

2.서비스 



알파인 리눅스는 용량이 80M인 경량화된 배포판으로, 도커에서 많이 사용한다. 알파인 기본 쉘은 `busybox`다.



1.앱/프로그램 설치

```shell
apk update
apk upgrade
apk add [package name]
apk del [package name]

```



로컬에 패키지 인덱스를 저장하지 않으려면

```shell
apk --no-cache 
```





2.서비스

알파인에서 서비스를 확인할때는 rc-servicec 를 사용한다.

```shell
rc-service --list
rc-service --list | grep -i nginx
rc-service [service name] start
rc-service [service name] stop
rc-service [service name] restart
```



 



Reference

* http://alpinelinux.org





