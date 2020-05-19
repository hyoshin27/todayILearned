#디자인 패턴

>싱글턴 패턴이란?

싱글턴 패턴은 인스턴스가 오직 하나만 생성되는 것을 보장하고 어디에서는 이 이스턴스에 접근할 수 있도록 하는 디자인 패턴이다.
싱글턴이라는 단어는 '단 하나의 원소만을 가진 집합'이라는 수학 이론에서 유래 되었다.
전역변수 사용하지 않고 객체 하나만 생성하도록 하며 생성된 객체를 어디에서든지 사용.

>싱글턴 패턴을 쓰는 이유

-고정된 메모리 영역을 얻으면서 한번의 new로 인스턴스를 사용하기 때문에 메모리 낭비를 방지할 수 있다.

-싱글톤으로 만들어진 클래스의 인스턴스는 전역 인스턴스이기 때문에 다른 클래스의 인스턴스들이 데이터를 공유하기 쉽다.

-디비 커넥션처럼 공통된 객체를 여러개 생성해서 사용해야하는 상황에서 많이 사용

-안드로이드 앱 같은 경우 액티비티나 클래스별로 주요 클래스들을 일일이 전달하기가 번거롭기 때문에 싱글톤 만들어 접근하는 것이 편함.

-인스턴스가 절대적으로 한개만 존재하는 것을 보증하고 싶을 때

-두번쨰 이용시부터 객체 로딩 시간이 현저하게 줄어들어 성능이 좋아짐


>싱글턴 패턴의 문제점

스레드를 사용할경우 인스턴스가 1개 이상이 생길수 있다. (경합조건) -> 정적 변수에 인스턴스 만들어 바로 초기화, 인스턴스 만드는 메서드에 동기화

싱글톤 인스턴스가 너무 많은 일을 하거나 많은 데이터를 공유 시킬 경우 다른 클래스의 인스턴스 간에 결합도가 높아져 개방폐쇄 원칙을 위배.
~~~
public class User {
    private String name;
    
    public User(Sting name){
        this.name = name;
    }
    
    public void print(){
        Printer printer = Printer.getPrinter();
        printer.print(this.name + " print using" + printer.toString() + ".");
    }
}

public class Printer {
    private static Printer printer = null;
    private int counter = 0;
    
    private Printer(){
    
    }
    
    public synchronized static Printer getPrinter(){
        if(Objects.isNull(printer)){
            printer = new Printer();    //Printer 인스턴스 생성
        }
        return printer;
    }
    
    public synchronized void print(String str){ // 하나의 스레드만 접근 허용
        counter++;
        System.out.println(str + counter);
    }
}

public void Main {
    private static final int USER_NUM = 5;
    
    public static void main(String[] args){
        User[] user = new User[USER_NUM];
        for(int i=0; i < USER_NUM ; i++){
            user[i] = new User((i+1) + "-user");
            user[i].print();
        }
    }
    
}
~~~

결과

~~~
4-thread print using .1
2-thread print using .2
1-thread print using .3
5-thread print using .4
3-thread print using .5
~~~


>싱글턴 패턴과 정적 클래스

정적 메서드로만 이루어진 정적 클래스를 사용해도 싱글턴 패턴 효과를 얻을 수 있다. 차이점은 객체를 전혀 생성하지 않고 메서드 사용한다는 점.
정적 메서드를 사용하는 것이 일반적으로 실행할때 바인딩되는 인스턴스 메서드를 사용하는 것보다 성능면에서 우수하다고 할 수 있다.
하지만 정적 클래스를 사용할 수 없는 경우가 있다 대표적인 경우가 인터페이스.

~~~
public class Printer {
    private static int counter = 0;
    
    public synchronized static void print(String str) { //메서드 동기화
        counter++;
        System.out.println(str + counter);
    }
}

public class UserThread extends Thread {
    public UserThread(String str){  //스레드 생성
        super(name);
    }
    
    public void run(){  //현재 스레드 출력
        Printer.print(Thread.currentThread().getName() + " print using " + ".");
    }
}

public class Main {
    private static final int THREAD_NUM = 5;
    
    public static void main(String[] args){
        UserThread[] user = new UserThread[THREAD_NUM];
        
        for(int i=0; i <THREAD_NUM ; i++){
            user[i] = new UserThread((i+1) + "thread");
            user[i].start();    //스레드 실행
        }
    }

}
~~~

결과
~~~
1-thread print using .1
4-thread print using .2
3-thread print using .3
5-thread print using .4
2-thread print using .5
~~~

>인터페이스를 사용하는 주된 이유?

-대체 구현이 필요한 경우

ㅡMock 객체를 사용해 단위 테스트를 수행하는 경우 

>Enum을 통해 싱글턴 구현

- Thread-safety 와 Serialization 보장

- Reflection 을 통한 공격에도 안전

~~~
public enum SingletonTest {
    INSTANCE;
  
	public static SingletonTest getInstance() {		
		return INSTANCE;
	}
}

~~~

참조 : 자바 객체지향 디자인패턴, 블로그
