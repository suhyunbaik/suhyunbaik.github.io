---
layout: post
title: Django, Flask를 사용하는 개발자들이 REST를 생각할때 갖기 쉬운 오해들
tags: [REST, Flask, Django]
---

* 제목을 지을때 고민을 많이 했다. Django, Flask를 사용한다고 해서 모두가 이런 오해를 하는게 아니다.  

최근에 팀에서 플라스크 보일러플레이트와 관련된 이슈가 있었다. 몇가지 이슈중에 가장 인상깊었던 것은 API를 `class` 로 할것인지, 아니면 `def`로 할것인지에 대한 것이였다. 
나는 결정이 빨리 나길 바랬다. 하지만 나의 바람과는 다른 방향으로 대화가 흘러갔다. `class`를 지지하는 개발자들은 `REST`를 적극 사용하고 싶어했다. `def`를 지지하는 개발자들은 `REST`가 현실세계를 반영하지 못하기 때문에 사용하기 싫어했다. 
내 입장은 기본적으로 `REST`아키텍쳐를 적극 활용하되, 대신 현실과 적당히 타협하는 것이다. 그러나 나는 그자리에서 내 의견을 밝히지 않았다. 왜냐면 `class`와 `def`에 대한 논의가 왜 `REST`아키텍쳐 도입 여부로 흘러가는지 이해할 수 없었기 때문이다. 그리고 이 미스터리는 신입 개발자와의 대화로 풀렸다.


"우리 회사의 프로젝트들 대부분이 `flask`를 기반으로 만들어져서 `def`로 되어있고 단 몇개의 프로젝트만이 `django`로 만들어졌습니다. 그래서 REST로 바꾸는 건 리소스가 많이 드는 일입니다. 그리고 플라스크에서 `class`를 쓰려면 패키지를 따로 설치해야 합니다." 
내가 반문했다. "REST 아키텍쳐를 구현하기 위해서 꼭 class를 사용해야 하는 건 아닙니다. def를 사용해도 충분히 구현할 수 있습니다. 그리고 blueprint에 methodview가 있어요. 패키지 설치가 싫으면 그걸 쓰면 됩니다."
"def인데 어떻게 REST를 구현할 수 있죠?"
"REST의 제약조건에는 무조건 class로 구현하라는 조건이 없습니다."
신입 개발자는 django drf나 flask-restplus, flask-restful 패키지에서 제공하는 classview를 사용해야 REST가 된다고 생각하고 있었다. 


나는 그 자리에서 상대방에게 REST에 대해 설명을 하고 싶었지만, 여기서는 밝힐 수 없는 사정으로 그럴 수 없었다. 대신 몇가지 오해들을 글로 정리해야 할 필요성을 느꼈다.
이 밑에 정리한 몇가지 오해들은 개발자 중에서도 REST에 익숙하지 않은 python 개발자들이 특히 자주 빠지기 쉬운 오해다.


### 1. django drf, flask rest-plus를 사용해야 RESTful api 이다.
위의 패키지들은 RESTful한 API구현을 쉽게 도와주는 패키지다. 해당 패키지를 사용하더라도 REST의 제약조건들을 충족하지 못하면 RESTful API가 아니다.
예제를 통해 알아보자. REST는 uri로 자원을 표현하고, 자원에 대한 행위를 method를 통해 구현해야 한다는 제약 조건이 있다. 
`drf`, `flask-restful`, `flask-restplus` 패키지 중 하나를 사용해 회원 계정 삭제 API를 다음과 같이 구현했다고 하자.

<pre><code> DELETE /new/members/delete/account </code></pre>
`uri`에 자원에 행할 행위 `delete`가 포함됐다. 제약조건에 위배되기 때문에 RESTful 하지 않다.
참고로 `uri`에서 행위를 표현할 수 없기 때문에 `uri`에 동사를 사용하면 안된다.


<pre><code> GET /members/account/ </code></pre>
`uri`에 동사가 없지만 method가 get이다. 계정을 삭제(delete) 할 API를 get으로 부르도록 개발한다면 client가 착각할 위험이 있다. 계정 정보를 갖고오려다가 계정을 삭제해버리는 불상사가 일어날 것이다.


<pre><code> DELETE /members/1 </code></pre>
uri와 method가 정확하게 API를 설명하고 있다.

보통 이런 오해는 REST를 개념이 아닌 패키지, 모듈, 또는 코드로 이해했을 때 발생한다. 
이럴때는 개발자와의 충분한 대화를 통해서 REST architecture는 개념이고, 구현하는 방법은 개발자에게 달려있다고 알려줘야 한다. 


### 2. RESTful 하게 API를 설계하면 status code를 상황에 따라 다양하게 내려줘야 하기 때문에 개발공수가 더 든다.
status code는 REST 아키텍쳐가 아니라 http와 관련있다. http 통신을 한다면 status code를 반환해줘야 한다. 상황에 따라 다른 상태코드를 내려주는 행위가 불편하고 무의미 하다고 생각한다면 팀의 협의에 따라 사용하지 않을 수 있다.
하지만 이럴 경우에 API는 어떤 상황에서든 200을 반환하게 될 것이고, 서버가 죽었을때는 500을 반환하니 결과적으로 200 아니면 500 코드만 존재하게 된다.
django drf [공식문서](https://www.django-rest-framework.org/api-guide/status-codes/)를 읽어보면 status code에 대한 정리가 잘 되어있다. 
예제에는 response를 반환할때 status code를 적극 활용한다. flask문서에는 status code 반환 부분이 빠져있다. 그래서 이런 오해가 발생하는 것 같다. 



### 3. `REST`는 단 4가지 methods, `get`, `post`, `put`, `delete`만을 허용한다.
REST의 제약조건 중에 method의 종류를 제한하는 조건은 없다. 


### 4. REST를 따르게 되면 method 가 get일때 json을 못보내기 때문에 비효율적이다.
`http`통신은 `get`을 사용할때 `request body`를 보내는것을 허용한다. 쉽게 말하자면 기술적으로 get일때 json을 보내는 게 가능하다는 말이다. 하지만 시맨틱하게 바라보면 GET일때 뭔가를 보낸다는 건 안맞다.
로이필딩이 `get`일때 `request body`를 추가하는 것에 대해 남긴 코멘트를 첨부한다. 최종 결정은 개발자에게 달려있다.
>Yes. In other words, any HTTP request message is allowed to contain a message body, and thus must parse messages with that in mind. 
Server semantics for GET, however, are restricted such that a body, if any, has no semantic meaning to the request. The requirements on parsing are separate from the requirements on method semantics.
So, yes, you can send a body with GET, and no, it is never useful to do so.
This is part of the layered design of HTTP/1.1 that will become
clear again once the spec is partitioned (work in progress).
....Roy 


### 5. REST의 CRUD메소드는 현실세계의 모든 행위를 표현할 수 없다. 표현이 불가능한 행위가 많다. 


"대부분의 회사가 `TDD`를 한다고 말합니다. 하지만 실상은 API를 먼저 만들고 나중에 슬며시 test를 추가합니다. 레스트 역시 마찬가지 입니다. `HATEOAS`를 API마다 구현하는 건 어려운 일이기 때문에 대부분의 회사가 그부분은 슬며시 건너뜁니다. 저는 그런 방법이 바쁘다고 생각하지 않고, 적극적으로 상황에 맞게 구현하면 된다고 생각합니다. 아무리 생각해도 "
`flask`는 `blueprint`라는 앱을 사용한다. 한국웹의 예제를 빠르게 훑어보니, 대부분의 예제가 `def`를 사용해 API를 만들고 `blueprint`로 라우팅한다. 
그래서 많은 사람들이 `django`는 기본적으로 `classview`를 사용하고 
`flask`는 `def`만 제공한다고  

