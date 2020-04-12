---
title: 디자인 패턴
layout: page
tags:
- Design pattern
categories:
- Design pattern
---

1. 생성패턴  
	1.1 추상팩토리  
	1.2 빌더  
	1.3 팩토리 매서드  
	1.4 프로토타입  
	1.5 싱글턴  
	
2. 구조패턴
    2.1 어댑터
    2.2 브릿지
    2.3 컴퍼짓 
    2.4 데코레이터 
    2.5 퍼사드
    2.6 플라이웨이트
    2.7 프록시
    
3. 행위패턴



#### 1. 생성패턴(Creational Pattern)

객체 생성에 관련된 패턴으로, 객체 생성과 조합을 캡슐화해 특정 객체가 생성되거나 변경되도 프로그램 구조에 영향을 받지 않도록 유연성을 제공한다.

##### 1.1 추상팩토리(Abstract Factory)
서로 연관되거나 의존적인 객체의 조합으로 만든 인터페이스

##### 1.2 빌더(Builder)

##### 1.3 팩토리 매서드(FactoryMethods)
객체 생성 처리를 서브 클래스로 분리해 처리

##### 1.4 프로토타입(Prototype)

##### 1.5 싱글턴(Singleton)
전역변수를 사용하지 않고 객체를 하나만 생성해, 어디서든 해당 객체를 참조할 수 있도록 하는 패턴

  

##### 2. 구조패턴(Structural Pattern)

클래스나 객체를 조합해 더 큰 구조를 만든다. 

##### 2.1 어댑터(Adaptor)
어댑터 패턴은 클래스의 인터페이스를 클라이언트가 사용하는 다른 인터페이스로 변환한다. 호환되지 않는 인터페이스 때문에 같이 쓸 수 없는 클래스들을 함께 사용할 수 있게 한다.

##### 2.2 브릿지(Bridge)

##### 2.3 컴퍼짓(Composite) 
여러개 객체로 구성된 복합객체와 단일 객체를 다룸

##### 2.4 데코레이터(Decorator) 
객체를 결합해 기능을 동적으로 유연하게 확장

##### 2.5 퍼사드(Facade)

##### 2.6 플라이웨이트(Flyweight)

##### 2.7 프록시(Proxy)

#### 3. 행위패턴(Behavioral Pattern)
객체나 클래스 사이의 알고리즘이나 책임 분배에 관련된 패턴으로, 한 객체가 수행할 수 없는 작업을 여러개 객체로 분배하거나, 분배하면서 객체 사이의 결합도를 최소화한다. 어댑터 패턴에는 객체 어댑터와 클래스 어댑터 두가지가 있다.

##### 책임연쇄(Chain of Responsibility)

##### 커맨드(Command) 
기능을 캡슐화해 여러 기능을 실행할 수 있는 재사용성 높은 클래스를 만드는 패턴

##### 인터프리터(Interpreter)

##### 이터레이터(Iterator)

##### 미디에이터(Mediator)

##### 메멘토(Memento)

##### 옵저버(Observer) 
한 객체의 상태 변화에 따라 다른 객체의 상태도 연동되는 패턴

##### 스테이트(State)
객체의 상태에 따라 객체의 행위 내용을 변경하는 패턴

##### 스트레트지(Strategy)
행위를 클래스로 캡슐화해 동적으로 행위를 바꾸는 패턴

##### 템플릿 메서드(Template Method)
전체 구조는 바꾸지 않으면서 특정 단계에서 수행하는 내역을 바꾸는 패턴

##### 비지터(Visitor)

##### 파사드(facade)
파사드 패턴은 여러 서브 시스템을 간편하게 사용해 줄 수 있도록 하는 패턴
간단하게 서브시스템 3개와 facade, client 를 구현한다.

`subsystems.py`

```python
class SubSystemOne(object):
    def do_something(self, name: str):
        print(f'operate 1 {name}')


class SubSystemTwo(object):
    def do_something(self, name: str):
        print(f'operate 2 {name}')


class SubSystemThree(object):
    def do_something(self, name: str):
        print(f'operate 3 {name}')
```



`facade_service.py`

```python
class FacadeService(object):
    def __init__(self):
        self.sub_system_one = SubSystemOne()
        self.sub_system_two = SubSystemTwo()
        self.sub_system_three = SubSystemThree()

    def operate(self, name: str):
        self.sub_system_one.do_something(name)
        self.sub_system_two.do_something(name)
        self.sub_system_three.do_something(name)
```



`client.py`

```python
class Client(object):
    def main(self):
        facade_service = FacadeService()
        facade_service.operate('Client')
```






#### References

* https://gmlwjd9405.github.io/2018/07/06/design-pattern.html
* https://imasoftwareengineer.tistory.com/29
