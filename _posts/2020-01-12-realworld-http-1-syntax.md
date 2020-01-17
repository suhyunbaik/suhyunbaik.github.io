---
layout: default
title: 리얼월드 HTTP - 1 HTTP/1.0 신택스
tags: [Network, HTTP]
categories: [Network]
---


1. HTTP의 네가지 기본요소
2. HTTP의 역사
3. HTTP 프로토콜과 관련된 고유명사
4. 헤더와 MIME 타입
   1. 클라이언트가 서버에 보내는 헤더
   2. 서버가 클라이언트에게 보내는헤더
   3. MIME 타입
   4. Content-Type 과 보안

5. 메서드와 스테이터스 코드
6. URL
7. 바디



###### 1. HTTP의 네가지 기본요소

* 메서드와 경로
* 헤더
* 바디
* 스테이터스 코드



###### 2. HTTP의 역사

* 1990: HTTP/0.9
* 1996: HTTP/1.0
* 1997: HTTP/1.1
* 2005: HTTP/2



###### 3. HTTP 프로토콜과 관련된 고유명사

* IETF(The Internet Engineering Task Force) : 인터넷의 상호 접속성을 향상시키는 목적으로 만들어진 임의 단체
* RFC(Request For Comments) : IETF가 만든 규약 문서
* IANA(Internet Assigned Numbers Authority) : 포트 번호와 파일 타입(Content-Type) 등 웹에 관한 데이터베이스 관리 단체
* W3C(World Wide Web Consortium) : 웹관련 표준화를 하는 비영리 단체
* WHATWG(Web Hypertext Application Technology Working Group) : 웹 관련 구격을 논의하는 단체. 



HTTP의 디폴트 포트는 80번을 이용한다.



##### 4. 헤더와 MIME 타입

HTTP의 헤더는 요청과 응답 양쪽에서 사용한다. 이 기능은 메일시스템에서 따왔다. 전자 메일은(RFC822) 인터넷의 전신인 아파넷에서부터 사용하고 있었다. 헤더의 원형은 1977년 RFC733에서 명문화됐다. 

헤더에는 본문 이외의 거의 모든 정보가 포함돼있다.

###### 4-1. 클라이언트가 서버에 보내는 헤더

* User-agent: 클라이언트가 애플리케이션 이름을 넣음
* Referer: 서버에서 참고하는 추가 정보. 클아이언트가 요청을 보낼 때 보고있던 페이지의 URL
* Authorization: 인증 정보. RFC 표준형식(Basic/Digest/Bearer) 또는 아마존 웹 서비스나 깃허브처럼 자체 표기 가능

###### 4-2. 서버에서 클아이언트로 보낼 때 부여하는 헤더

* Content-Type: 파일 종류 지정.
* Content-Length: 바디 크기, 압축을 할 경우 압축 후 크기
* Content-Encoding: 압축한 경우 압축 형식
* Date: 문서 날짜

X- 로 시작하는 헤더는 각 애플리케이션이 자유롭게 사용할 수 있다.



###### 4-3. MIME 타입

MIME 타입은 파일 종류를 구별하는 문자열이다. 



###### 4-4. Content-Type 과 보안

파일 종류를 특정할 때 Content-Type 헤더에 지정된 MIME 타입을 사용한다. 인터넷 익스플로러는 옵션에 따라서 내용을 보고 파일 형식을 추측하려 한다. 이런 동작을 콘텐트 스니핑(content sniffing) 이라고 한다. 서버 설정이 잘못 되어도 실행되기 때문에 테스트로만 표시돼야 하는 파일이 HTML 자바스크립트가 포함되어있으면 실행되버리는 경우도 있다. 이를 방지하기 위해서 다음과 같은 헤더를 전송한다.

`X-Content-Type-Options: nosniff` 



##### 5. 메서드와 스테이터스 코드

뉴스그룹은 분산 아키텍쳐로 되어있고, 복수의 서버가 마스터/슬레이브 구조로 연결되 슬레이브 서버도 마스터 서버에 접속해 정보를 갖고와 로컬에 저장한다. HTTP 는 뉴스그룹에서 메서드와 스테이터스 코드 두가지 기능을 도입했다.

###### 5-1. 메서드

* GET : 서버에 헤더와 콘텐츠 요청
* HEAD : 서버에 헤더만 요청
* POST : 새로운 문서 투고
* PUT : 이미 존재하는 URL의 문서 갱신
* DELETE: 지정된 URL 문서 삭제



###### 5-2. 스테이터스 코드

* 100번대 : 처리가 계속됨.
* 200번대 : 성공
* 300번대: 서버에서 클라이언트로의 명령. 오류는 아니다. 리다이렉트나 캐시 이용 지시.
* 400번대: 클라이언트 요청에 오류
* 500번대: 서버 내부 오류



##### 6. URL

URL(Uniform Resource Locator)는 URI(Uniform Resource Indicator) 와 자주 혼동된다. URI에는 URN(Uniform Resource Name) 이라는 이름 부여 규칙도 포함된다.

URL은 다음과 같이 구성된다.

`스키마://호스트명/경로`

예시

```shell
스키마: https
호스트명: www.naver.com
경로: index.html
```

```shell
스키마://사용자:패스워드@호스트명:포트/경로#프래그먼트?쿼리
```

브라우저는 스키마를 해석해 접속방법을 정한다. HTTP는 80, HTTPS는 443번이다.



##### 7. 바디

HTTP 메서드 중 GET, HEAD 메서드는 바디를 포함할 것을 기대하지 않는다. 강제로 바디를 송신할 수 있지만, RFC에서 추천하지 않는다.

OPTIONS, CONNECT 각 메서드는 페이로드 바디를 가질 수 있지만 구현에 따라 서버가 거부할 수 있다. TRACE는 페이로드에 절대 바디를 포함해서는 안된다.



##### Reference

* [https://velog.io/@pa324/%EA%B0%9C%EB%B0%9C%EC%83%81%EC%8B%9D-URI-URL-%EC%B0%A8%EC%9D%B4-%EC%A0%95%EB%A6%AC](https://velog.io/@pa324/개발상식-URI-URL-차이-정리)


