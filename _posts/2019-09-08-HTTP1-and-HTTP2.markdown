---
layout: post
title: HTTP1.1와 HTTP2.0 차이점
tags: [HTTP, Protocol, ComputerScience]
---
Http 는 request에 대한 response를 받아야만 다음 request를 처리할 수 있다.

### HTTP/1.1
Http 1.0은 `connection`을 매번 생성했다. 그래서 요청한 리소스 개수에 비례해서 `latency`가 길어진다. 이러한 문제를 해결하기 위해 1.1부터 `persistent connection(connection 재사용)`, `pipelining(여러개의 request 를 미리 날림)` 등의 기술이 등장했다. 하지만 다음과 같은 문제가 있었다.

    1. HOL Blocking(Head Of Line Blocking)  
    pipelining을 사용해 여러개의 request를 미리 날리더라도, 첫번째 리소스를 요청하고 응답이 지연되면 당연히 두번쨰, 세번째 리소스는 첫번째 리소스의 응답처리가 완료되기까지 지연된다.
    
    2. RTT(Round Trip Time) 증가  
     매요청별로 connection을 생성하기 때문에 3-way handshake가 반복적으로 발생해 성능 저하를 초래한다.
     
    3.무거운 Header  
     매 요청시마다 중복된 헤더값을 전송한다.

요약: HTTP/1.1은 동시전송이 불가능하다. 요청과 응답이 순차로 이뤄진다. header가 무겁다. 1.1 은 plain text 를 리턴한다.


### HTTP/2.0
HTTP 2.0은 한 `connection`으로 여러개의 메세지를 주고 받는다. `from`, `message`, `stream`개념이 도입됐고 응답은 순서에 상관없이 `stream`으로 주고 받는다.

    1. Stream Prioritization  
     리소스의 의존관계(우선순위)를 설정해 렌더링이 늦어지는 문제를 해결한다.

    2. Server Push 
    요청을 하지 않아도 클라이언트에게 리소스를 보내줄 수 있다. 클라이언트 요청을 최소화해 성능이 향상된다. push_promise를 통해 서버가 전송한 리소스에 대해서는 클라이언트는 요청하지 않는다.

    3. Header Compression
     Header Table, Huffman encoding 기법으로 Header 정보를 압축한다. 중복이 아닌 header 는 인코딩 해 전송한다. 
Header 압축 명세: [RFC 7531](https://http2.github.io/http2-spec/compression.html)  

요약: HTTP/2.0은 한 `connection`으로 여러개의 메세지를 주고 받는다. 응답의 순서를 기다리지 않고 처리한다. 클라이언트가 요청하지 않은 리소스를 미리 보내주고 header를 압축한다. 2.0은 response를 binary로 인코딩해 리턴한다.

