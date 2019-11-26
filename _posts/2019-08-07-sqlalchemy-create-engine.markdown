---
layout: post
title: SQLAlchemy, Timeit으로 성능 측정하기
tags: [flask, sqlalchemy, timeit]
---
데이터베이스와 연결하는 코드를 읽어보니, API에서 쿼리를 보낼때 마다 SQLAlchemy [create_engine](https://docs.sqlalchemy.org/en/13/core/engines.html) 을 사용해서 디비와 커넥션 부터 만들고 세션을 만든 다음 끊는 방식으로 되어있었다.
만약 한 API에서 쿼리를 5번 보낸다면 engine을 5번 생성하게 된다. 그런데 engine을 여러번 생성하는게 성능에 큰 차이를 미칠까?
성능을 측정하기 위해서 [timeit](https://docs.python.org/3/library/timeit.html)을 사용했다. timeit은 작은 크기의 파이선 코드의 실행시간을 측정할 때 사용할 수 있는 라이브러리다.


엔진을 한번만 만드는 경우

```bash
python -m timeit -s "from sqlalchemy import create_engine" "engine=create_engine('mysql://root:@localhost:3306/quicket');engine.execute('SELECT 1')"
```

```bash
result:
100 loops, best of 3: 5.55 msec per loop
```
‌

엔진을 실행할때마다 매번 생성할 경우

```bash
python -m timeit -s "from sqlalchemy import create_engine;engine=create_engine('mysql://root@localhost:3306/quicket')" "engine.execute('SELECT 1')"
```
```bash
result:
10000 loops, best of 3: 135 usec per loop
```
    
결과를 봤을때는 `app.run()`으로 코드를 메모리에 올렸을 때 최초 한번 실행하는 게 제일 좋은 방법이다.

