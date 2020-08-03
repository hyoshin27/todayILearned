#자바의 정석 

>6.객체지향 프로그래밍1

객체지향언어의 특징

-코드의 재사용성이 높다.

-코드의 관리가 용이하다.

-신뢰성이 높은 프로그래밍을 가능하게 한다.

클래스와 객체

-클래스 : 객체를 정의해 놓은 것. 객체를 생성할 때 쓰임. ex)붕어빵틀, TV 설계도

-객체 : 실제로 존재하는 것. 사물 또는 개념. ex)붕어빵, TV

객체와 인스턴스

-인스턴스와 객체는 같은 의미이다. 클래스로부터 객체를 만드는 과정을 인스턴스화라고 하고 만들어진 객체를 인스턴스라고 한다.

속성과 기능

-객체는 속성과 기능으로 이루어져있다. 속성(=멤버변수) 특징이고 기능(=메서드)은 그 속성을 컨드롤하는 동작이다.

변수의 종류

-클래스 변수 : static 이 붙은 것. 인스턴스 변수 앞에 static이 붙은 것이다. 
클래스 변수는 모든 인스턴스가 공통된 저장공간을 공유하기 때문에 모든 인스턴스들이 공통으로 유지해야 하는 값일 경우 사용한다.

-인스턴스 변수 : static 이 안 붙은 것. 클래스 영역에 선언되며 클래스의 인스턴스를 생성할 때 만들어진다. 독립된 저장공간을 가지므로 서로 다른 값을 가질 수 있다.

-지역변수 : 매서드 내에 선언된 변수. 메서드가 종료되면 사라진다.

메서드를 사용하는 이유

-높은 재사용성

-중복된 코드의 제거

-프로그램의 구조화

return 문

~~~
void test(){
    System.out.println("hi");
    //return; 반환타입이 void라 생략가능 컴파일러가 자동 추가
}

~~~

JVM 메모리 구조

-Method area : 프로그램 실행중 어떤 클래스가 사용되면 JVM은 .class 파일을 읽어 이곳에 저장한다. 클래스와 함께 클래스 변수도 저장

-call stack : 메서드 작업에 필요한 메모리 공간 제공. 메서드가 호출되면 메서드를 위한 메모리가 할당되며 작업을 마시면 반환되어 비워진다.

-heap : 인스턴스가 생성되는 공간.
 
오버로딩

-한 클래스 내에 같은 이름의 메서드를 여러 개 정의하는 것.

-메서드 이름이 같고 매개변수의 개수 또는 타입이 달라야 한다.

~~~
long add(int a, long b);
long add(long a, int b);
~~~

int 와 long 이 하나씩 선언되어 있지만 순서가 다르다. 이렇게 되면 사용자가 순서를 외우지 않아도 되는 장점이 있지만 add(3,3) 일 경우 
어떤것이 두 메서드 중 어느 메소드가 호출된지 알 수 없기 때문에 메소드를 호출하는 곳에서 컴파일 에러가 난다.

생성자란?

- 인스턴스가 생성될때 호출되는 인스턴스 초기화 메서드. 생성자는 클래스 이름과 같아야하고 리턴 값이 없다.

초기화 블럭
~~~
class BlockTest{
    static {    //클래스 초기화 블럭
        System.out.println("static { }");
    }
    
    {   //인스턴스 초기화 블럭
        System.out.println("인스턴스 { }");
    }
    
    public BlockTest(){
        System.out.println("생성자");
    }
    
    public static void main(String args[]){
        System.out.println("호출1");
        BlockTest bt = new BlockTest();
        
        System.out.println("호출2");
        BlockTest bt2 = new BlockTest();
    
    }
    /*결과
    static { }
    호출1
    인스턴스 { }
    생성자
    호출2
    인스턴스 { }
    생성자*/
}
~~~




