#디자인 패턴 - 스테이트 패턴

>상태란?

-객체가 시스템에 존재하는 동안 객체가 가질 수 있는 조건이나 상황. 객체가 어떤 액티비티를 수행하거나 특정 이벤트가 발생하길 기다리는 것

>UML 상태 머신 다이어그램에서 의사상태?

- 시작, 종료, 히스토리, 선택, 교차, 포크, 조인, 진입점, 진출점

>ex)형광등
~~~
public class Light {
    private static int ON = 0;
    private static int OFF = 1;
    private static int SLEEPING = 2;
    private int state;
    
    public Light(){
        state = OFF;
    }
    
    public void offButtonPushed(){
        if(state == OFF){
            System.out.println("반응 없음");
        }else if(state == SLEEPING){
            System.out.println("LIGHT OFF");
            state = OFF;
        }else{
            System.out.println("LIGHT OFF");
            state = OFF;
        
        }
    }
    
    public void oNButtonPushed(){
        if(state == ON){
            System.out.println("취침등 상태");
            state = SLEEPING;
        }else if(state == SLEEPING){
            System.out.println("LIGHT ON");
            state = ON;
        }else{
            System.out.println("LIGHT ON");
            state = ON;
        
        }
    }
}

~~~

이러한 경우 새로운 상태가 추가되면 상태 변화에 관한 모든 메서드를 수정해야 하는 번거로움이 있다.

>해결책

~~~
public interface State(){
    public void onButtenPushed(Light light);
    public void offButtenPushed(Light light);
}

public class On implements State {
    private static On on = new On();
    private On(){
    }
    
    public void onButtenPushed(Light light){
        System.out.println("반응 없음");
    }
    
    public void offButtenPushed(Light light){
        System.out.println("LIGHT OFF");
        light.setState(Off.getInstance());
    }
}

public class Off implements State {
    private static Off off = new Off();
    private Off(){
    }
    
    public void onButtenPushed(Light light){
        System.out.println("LIGHT ON");
        light.setState(On.getInstance());
    }
    
    public void offButtenPushed(Light light){
        System.out.println("반응 없음");
    }
}

public class Light {
    private State state;
    
    public Light(){
        state = new Off();
    }
    
    public void setState(State state){
        this.state = state;
    }
    
    public onButtenPushed(){
        state.onButtenPushed(this);
    }
    
    public offButtenPushed(){
        state.offButtenPushed(this);
    }
}
~~~

스테이트 구현 과정은 전략 패턴과 매우 비슷하지만 전략 패턴은 상속을 대체하려는 목적 스테이트 패턴은 조건문 대체 목적으로 사용된다.

