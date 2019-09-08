---
layout: post
title: 수습기간 회고
tags: [flask, python, boilerplate]
---

Probation period

1. 파이썬 플라스크 보일러 플레이트 제작
2. SQLAlchemy 도입
3. Status code 적용
4. Swagger 추가
5. Response body format 정함
6. REST 아키텍쳐 적용


수습기간 동안 한 일을 정리해봤다.


1. 파이썬 플라스크 보일러 플레이트 제작  
기존 서비스는 모놀리스 아키텍쳐 였다. 마이크로 아키텍쳐를 도입하기로 결정된 뒤, python3 + Flask + pytest 보일러플레이트를 제작했다. Flask의 기능을 좀 더 활용할 수 있도록 리팩토링할 수 있는 부분이 있어서 구조도 같이 개선했다. 특히 데이터베이스 연결 쪽을 많이 뜯어고쳤다. 우선 flask 에서는 werkzeug 에서 제공하는 localproxy를 사용해서 데이터 베이스를 연결하고, request teardown을 사용해서 session 또는 transaction을 관리하도록 권장한다. 이러한 방법의 장점은 우선 1. 플라스크에서 권장하는 방식으로 연결하기 때문에 좀 더 플라스크의 구조에 맞는 방식으로 연결하게 된다는 점이고 2. session 의 생명주기 단위가 api 가 된다는 점이다.
api 에서 세션을 사용한뒤 api request가 완료되면 에러가 나더라도 무조건 request teardown을 호출하게 되어있기 때문에 세션을 관리하기가 쉽다. 

2. SQLAlchemy 도입  
기존 프로젝트에는 SQLAlchemy가 설치돼있지만 데이터베이스 연결, 세션관련 작업에만 사용하고 쿼리를 보낼때는 raw query를 사용하고 있었다. 기존 방식에서는 문제가 없었지만 마이크로 아키텍쳐를 적용하기 위해 데이터베이스와 서버를 나누려고 하니 문제가 됐다. SQLAlchemy를 도입하길 바라는 개발자들과 얘기를 나눠보니, 팀에 SQLAlchemy를 많이 사용해본 개발자가 없어서 더더욱 도입을 망설인 것 같았다. 그래서 파이선 3로 포팅하면서 기존의 raw query도 SQLAlchemy로 전부 옮기기 시작했다. sqlalchemy를 사용하면 우선 파이소닉한 표현을 쓸 수 있고 데이터베이스에 대한 의존성이 줄어들어서 msa 이전 작업시 삽질을 덜어주기 때문에 좋다.

3. Status code 도입  
기존 코드를 읽어보니 status code를 사용하지 않고 있었다. 그래서 client에서는 response body에 담긴 데이터를 꼭 읽어야만 했다. 예를 들어 권한이 없는 유저가 로그인을 시도할 경우에도 response status code는 200이고, unauthorized 라는 메세지를 response body에 담아서 반환한다. 
이런 방법은 개발, 서비스 시간이 더 들기 때문에 비효율적이다. Status code 만으로 바로 분기를 할 수 있다면 개발 시간이 줄고 서비스 속도도 더 빨라질것이다. 그래서 새로 제작한 보일러플레이트에 status code를 도입했다. 그리고 status code가 익숙하지 않은 다른 개발자들을 위해서 관련 문서도 공유했다.

4. Swagger 추가 
기존에는 API를 만들고 나 뒤 컨플루언스에 문서를 작성한 뒤, 공유하는 방식으로 문서화 작업이 이뤄졌다. 그리고 postman으로도 작업해서 프론트엔드 개발자와 공유해야 했다. 문서화에 투입하는 시간이 많은 것 같아 swagger 샘플을 작업해서 보여주니 다들 좋아했다. 때마침 postman free tier를 넘는 바람에 열심히 등록한 collections를 못쓰게 됐다. 이렇게 swagger도 도입하고 문서화 시간도 줄였다.

5. response format 정리  
api마다 response body format이 다 달랐다. 이력을 찾아보니 포맷을 명시적으로 정한 적이 없어서 벌어진 일이었다. 이 부분도 의견을 취합해 보일러플레이트에 반영했다.

6. REST 아키텍쳐 적용  
기존 api는 endpoint나 메소드 사용에 일정한 규칙이 없었다. 그래서 옮길때 RESTful 하게 리팩토링을 했다. Flask-restful 패키지를 추가해서 기존 api들을 전부 classview로 리팩토링했다.

이 작업을 하면서 한가지 배운 점이 있다면, 어떤 조직에 이제 막 참여한 사람이 새로운 것을 도입하는건 큰 일이라는 것이다. Swagger처럼 결과가 즉각적으로 눈에 보이는 라이브러리는 도입이 수월하다. 반면 효과가 가시적이지 않은 새로운 아키텍처(REST), 라이브러리, 코드 스타일은 도입하기 전에 우선 사람들을 설득해야 한다. 내가 합류한지 며칠 안됐을때 위의 요소들을 제안했을때는 부정적인 피드백을 받았다. 나중에 새로운 멤버들이 대거 합류한뒤 개선점을 찾고 수정하자는 목소리가 커지는 분위기로 변했다. 분위기를 타서 똑같은 제안을 다시 했더니 받아들여졌다. (그래도 여전히 작업과정은 순탄하지 않았다.) 이러한 과정이 약 2개월 정도 걸렸다. 기회도 힘들게 얻었고 작업과정도 힘들었지만 결과물은 마음에 들었다. 

[flask boilerplate](https://github.com/suhyunbaik/flask-restful-boilerplate)











