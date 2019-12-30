---
layout: post
title: 파이선의 덕타이핑(Duck Typing)
tags: [Python, Duck Typing, Language]
categories: [Python]
---

새처럼 생긴 동물이 있다. 정확하게 무슨 동물인지 알아보기 전에,

1. 오리처럼 걷고 
2. 오리처럼 꽥꽥거리면
3. 이 동물은 오리다. 

다음과 같은 논증을 거쳐 ‘이 동물은 오리다’ 라는 판단을 내리는 방법을 덕 테스트(Duck Test)라고 한다. 이 논증법은 프로그래밍 분야에 전파돼 Duck Typingg(덕 타이핑)이라는 개념이 됐다. 이 개념에 따라 파이썬은 본질적으로 다른 클래스라도 객체의 실제 유형이 아니라 특정 메소드와 속성의 존재 유무에 따라 결정한다. 


예제코드 출처 https://en.wikipedia.org/wiki/Duck_typing  
```python
class Duck:
    def fly(self):
        print("Duck flying")

class Airplane:
    def fly(self):
        print("Airplane flying")

class Whale:
    def swim(self):
        print("Whale swimming")

for animal in Duck(), Airplane(), Whale():
    animal.fly()
```
```
output:
    Duck flying
    Airplane flying
    AttributeError: 'Whale' object has no attribute 'fly'
```
Duck, Airplane은 서로 관계가 없는 클래스이지만 내부에 fly()라는 메소드가 있다는 공통점이 있이 때문에 같은 타입으로 본다. 만약 Java 였다면 Duck이라는 클래스를 상속받았다고 명시해야만 같은 타입이 될 수 있다.
