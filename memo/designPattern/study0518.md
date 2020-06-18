#디자인 패턴

>스트래티지 패턴(전략 패턴)

스트래티지 패턴이란

-전략을 쉽게 바꿀 수 있도록 해주는 디자인 패턴. 특히 게임 캐릭터가 자신이 처한 상황에 따라 공격이나 행동하는 방식을 바꾸고 싶을때 매우 유용.
객체들이 할 수 있는 행위 각각에 대해 전략 클래스 생성하고 유사한 행위들을 캡슐화 하는 인터페이스를 정의하여 객체의 행위 동적을 바꾸고 싶을때 행위를 수정하지 않고 전략을 바꿔주기만 함으로써 행위를 유연하게 확정하는 방법 

strategy: 인터페이스나 추상 클래스로 외부에서 동일한 방식으로 알고리즘을 호출하는 방법을 명시

concretestrategy1, concretestrategy2, concretestrategy3: 스트래티지 패턴에서 명시한 알고리즘을 실제로 구현한 클래스

context: 스트래티지 패턴을 이용하는 역할을 수행한다. 필요에 따라 동적으로 구체적인 전략을 바꿀 수 있도록 setter 메소드 제공


~~~
public interface Fruit(){
    public void flavor();
}

public class Strawberry implements Fruit(){
    public void flavor(){
        System.out.println("새콤달콤");
    }
}

public class Melon implements Fruit(){
    public void flavor(){
        System.out.println("달다");
    }
}

public class Basket(String args[]){
    Fruit strawberry = new Strawberry();
    Fruit melon = new Melon();
    
    strawberry.flavor();
    melon.flavor();
}
~~~

이러다 딸기가 맛이 없어졌다고 가정했을때

~~~
public class Strawberry implements Fruit(){
    public void flavor(){
        System.out.println("맛없어");
    }
}

~~~

로 바꿔 주면 된다.

이렇게 수정하는 방법은 SOLID 원칙 중 OCP(open close principle)에 위배된다.
이런 방식은 시스템이 확장 되었을때 유지보수를 어렵게 한다.(매서드 중복의 문제)




참조 : 자바 객체지향 디자인패턴, 블로그
