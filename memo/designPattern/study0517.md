#디자인 패턴

>디자인 패턴이란

소프트웨어를 설계할 때 특정 맥락에서 자주 발생하는 문제들이 또 발생했을 때 재사용할 수 있는 해결책

>디자인 패턴 구조

-콘텍스트: 문제가 발생하는 여러 상활 기술. 패턴이 적용될 수 있는 상황

-문제: 패턴이 적용되어 해결될 필요가 있는 여러 디자인 이슈 기술. 제약상황과 영향력도 같이 기술

-해결: 문제를 해결하도록 설계를 구성하는 요소들과 그 요소들 사이의 관계, 책임, 현력 관계 기술. 반드시 구체적인 구현 방법이나 언어에 의존하지 않고 다양한 상황에 적용할 수 있는 일종의 템플릿

>싱글턴(singleton)

-콘텍스트: 클래스가 객체를 생성하는 과정을 제아

-문제: 에플리케이션이 전역적으로 접근하고 관리할 필요가 있는 데이터를 포함. 이러한 데이터는 시스템에 유일하며 어떤방식으로 클래스에서 생성되는 객체의 수를 제어하고 클래스의 인터페이스에 접근하는 것을 제어해야 하는가?

-해결: 클래스의 생성자를 public 으로 정의하지 말고 private 나 protected 로 선언해 외부에서 생성자를 이용해 객체를 생성할 수 없도록 만듦

>GoF 디자인 패턴

Gang of four(에리히 감마, 리차드 헬름, 랄프 존슨, 존 블리시디스)가 design patterns: elements of reusable object-oriented software라는 책에서 23가지 패턴을 정리함.
생성, 구조, 행위 3가지로 분류

-생성패턴: 추상팩토리(abstract factory), 빌더(builder), 팩토리 메서드(factory method), 프로토타입(prototype), 싱글턴(singleton)

-구조패턴: 어댑터(adapter), 브리지(bridge), 컴퍼지트(composite), 데커레이터(decorator), 퍼사드(facade), 플라이웨이트(flyweight), 프록시(proxy)

-행위패턴: 책임연쇄(chain of responsibility), 커맨드(command), 인터프리터(interpreter), 이터레이터(iterator), 미디에이터(mediator), 메멘토(memento)
         옵서버(observer), 스테이트(state), 스트래티지(strategy), 템플릿 메서드(template method), 비지터(visitor)

참조 : 자바 객체지향 디자인패턴