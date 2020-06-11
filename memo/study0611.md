#DDD start!

>8. 애그리거트 트랜잭션 관리

애그리거트에 대해 사용할 수 있는 트랜잭션 처리 방법에는 선점(Pessimistic), 비선점(Optimistic)

선점 잠금 : 먼저 애그리거트를 구한 스레드가 애그리거트 사용이 끝날 때까지 다른 스레드가 해당 애그리거트를 수정 하는 것을 막는 방식

JPA 의 EntityManager 는 LockModeType 을 인자로 받는 find 메서드를 제공하는데 LockModeType.PESSIMISTIC_WRITE 를 값으로 전달하면 해당 엔티티와 매핑된 테이블을 이용해서 선점방식을 적용할수 있다.

~~~
Order order = entityManager.find(Order.class, orderNo, LockModeType.PESSIMISTIC_WRITE);
~~~

교착상태 

1. 스레드1 : A애그리거트 선점잠금 구함
2. 스레드2 : B애그리거트 선점잠금 구함
3. 스레드1 : B애그리거트 선점잠금 시도
4. 스레드2 : A애그리거트 선점잠금 시도

3,4번은 더이상 다음단계로 진행하지 못하고 교착 상태에 빠지게 됨

이런 문제가 발생하지 않도록 잠금을 구할때 최대 대기 시간을 지정해야한다. JPA 에서 선점 잠금을 시도할 때 최대 대기 시간을 지정하려면 다음과 같이 힌트를 사용하면 된다.

~~~
Map<String, Object> hints = new HashMap<>();
hints.put("javax.persistence.lock.timeout", 2000);
Order order = entityManager.find(Order.class, OrderNo, LockModeType.PESSIMISTIC_WRITE, hints);
~~~

지정한 시간내에 잠금을 구하지 못하면 익셉션을 발생 시킨다. DBMS 마다 힌트 적용 유무가 다르니 체크하자

비선점 잠금 : 잠금을 해서 동시에 접근하는 것을 막는 대신 변경한 데이터를 실제 DBMS 에 변경가능 여부 확인하는 방식

JPA 는 버전을 이용한 비선점 잠금 기능을 지원한다. @Version 에 애노테이션을 붙이고 매핑되는 테이블에 버전을 저장할 칼럼을 추가하기만 하면 된다.

~~~
@Entity
@Table(name="purchase_order")
@Access(AccessType.FIELD)
public class Order {
    @EmbeddedId
    private OrderNo number;
    
    @Version
    private long version;
}

~~~

