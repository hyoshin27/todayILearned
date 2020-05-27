#디자인 패턴

>데커레이터 패턴

데커레이터 패턴은 기본 기능에 추가할 수 있는 기능의 종류가 많은 경우 각 추가 기능을 Decorator 객체를 조합함으로써 추가 기능의 조합을 설계하는 방식이다.

객체의 결합을 통해 기능을 동적으로 유연하게 확장 할 수 있게 해주는 패

Component : 기본 기능을 뜻하는 ConcreteComponent 와 추가 기능을 뜻하는 Decorator 의 공통 기능을 정의한다. 즉 클라이언트는 Compomnent 를 통해 실제 객체를 사용한다.

ConcreteComponent : 기본 기능을 구현하는 클래스다.

Decorator : 많은 수가 존재하는 구체적은 Decorator 의 공통 기능을 제공한다.

ConcreteDecoratorA, ConcreteDecoratorB : Decorator 의 하위 클래스로 기본 기능에 추가 되는 개별적인 기능을 뜻한다.

>구현한 때 고려할 점

1.Component 는 베이스가 되는 역할이므로 작고 가볍게 정의한다.

- 가급적 인터페이스만을 정의한다.

-무언가 저장하는 변수는 정의하지 않는다.

-저장할 것이 있다면 서브클래스에서 하자.

2.상속 구조를 통해 Decorator 과 Component 가 같은 인터페이스를 갖게 해야 한다.

3. 코드를 수정하지 않고 Decorator 를 조합해 기능을 추가 할 수 있도록 한다.

4.구현하려는 내용이 객체의 겉을 변경하려고 하는지 속을 변경하려고 하는지 고려하자.(속을 변경하는 것이라면 전력패턴이 더 적절하다.)

5.Decorator가 다른 Decorator에 대해 참고할 필요가 있다면 Decorator 패턴의 사용 의도에 어긋나는 작업일 수 있다.

6.재귀적으로 기능을 갖게 하는 방법 외에도 Decorator 를 추가할 때마다 얻은 아이템을 List로 갖고 있을 수 있다.

추가 참조: https://johngrib.github.io/wiki/decorator-pattern/