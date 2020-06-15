#DDD start!

>10. 이벤트

기능을 추가할 때마다 파라미터가 함께 추가되면 다른 로직이 더 많이 섞이고 트랜잭션 처리가 더 복잡해진다.

이벤트 생성주체 -> 이벤트 디스패쳐(이벤트 퍼블리셔) -> 이벤트 핸들러(이벤트 구독자)

이벤트가 필요한 데이터를 담고 있지 않으면 이벤트 핸들러는 리포지터리, 조회 api, 직접 DB 접근 등의 방식을 통해 필요한 데이터를 조회해야 한다.

이벤트의 용도 : 트리거, 서로 다른 시스템 간의 데이터 동기화

이벤트를 사용하면 서로 다른 도메인 로직이 섞이는 것을 방지할 수 있다.

이벤트의 흐름(응용서비스와 동일한 트랜잭션 범위에서 핸들러의 handle()이 샐행됨. 즉 도메인의 상태변경과 이벤트 핸들러는 같은 트랜잭션 범위에서 실행)

-이벤트 처리에 필요한 이벤트 생성

-이벤트 발생 전에 이벤트 핸들러를 Events.handle() 메서드를 이용해서 등록

-이벤트를 발생하는 도메인 기능을 실행

-도메인은 Events.raise()를 이용해서 이벤트 발생

-Events.raise()는 등록된 핸들러의 canHandle()을 이용해서 이벤트를 처리할 수 있는지 확인

-핸들러가 이벤트 처리를 할 수 있으면 handle()메서드를 이용해서 이벤트 처리

-Events.raise() 실행을 끝내고 리턴

-도메인 기능 실행을 끝내고 리턴

-Events.reset()을 이용해서 ThreadLocal을 초기화

~~~
@Aspect
@Order(0)   //우선순위 0으로 지정. 트랜잭션 관련 AOP 보다 우선순위를 높여 먼저 실행되도록
@Component
public class EventsResetProcessor{
    
    //서비스 메서드의 중첨 실행 개수를 저장하기 위한 ThreadLocal 변수 생성
    private ThreadLocal<Integer> nestedCount = new ThreadLocal<Integer>(){
        @Override
        protected Integer initialValue(){
            return new Integer(0);
        }
    };
    
    @Around("@target(org.springframework.stereotype.Service) and within(com.myshop..*) ")   // @Around Aspect를 이용하여 AOP 구현
    public Object doReset(ProceedingJoinPoint joinPoint) throws Throwable {
        nestedCount.set(nestedCount.get() + 1); //중접 실행횟수 1증가
        try{
            return joinPoint.proceed(); //대상메서드 실행
        }finally{
            nestedCount.set(nestedCount.get() - 1); //중접 실행횟수 1감소
            if(nestedCount.get() == 0){
                Events.reset(); //중첩 실행 횟수가 0이면 이벤트 초기화
            }
        }
    }
}
~~~


