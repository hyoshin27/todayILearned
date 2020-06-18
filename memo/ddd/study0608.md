#DDD start!

>5. 리포지터리의 조회 기능(JPA 중심)

페이징과 개수 구하기

-setFirstResult() : 첫번째 행번호 지정

-setMaxResult() : 읽어올 행 개수

@Immutable, @Subselect, @Synchronize 는 하이버네이트 전용 애노테이션인데 이 태그를 이용하면 테이블이 아닌 쿼리 결과를 @Entity 로 매핑할 수 있다.

뷰를 수정할 수 없듯이 @Subselect 로 조회한 @Entity 역시 수정할 수 없다.

실수로 @Subselect 를 이용한 @Entity 매핑 필드를 수정하면 하이버네이트는 변경 내역을 반영하는 update 쿼리를 실행할 것이다. 그런데 매핑한 테이블이 없으므로 에러가 발생한다.

이런 문제를 방지하기 위해 @Immutable 을 사용한다. @Immutable 을 사용하면 하이버네이트는 해당 엔티티의 매핑 필드/ 프로퍼티가 변경되어도 DB에 반영하지 않고 무시한다.

~~~
//purchase_order 테이블에서 조회
Order order = orderRepository.findById(orderNumber);
order.changeShippingInfo(newInfo);  //상태 변경

//변경내역이 DB에 반영되지 않았는데 purchase_order 테이블에서 조회
List<OrderSummary> summaries = orderSummaryRepository.findByOrderId(userId);

~~~

위코드는 Order 의 상태를 변경한뒤에 OrderSummary 를 조회하고 있다. 특별한 이유가 없으면 하이버네이트는 변경사항을 트랜잭션을 커밋하는 시점에 DB를 반영하므로 Order의
변경내역은 아직 purchase_order 테이블에 반영되지 않은 상태에서 purchase_order 테이블을 사용하는 OrderSummary 를 조회하게 된다. 즉 최신값이 아닌 이전 값이 담기게 된다.
이러한 문제를 해소하기 위한 용도로 사용한 것이 @Synchronize 이다. @Synchronize 는 엔티티를 로딩하기 전에 지정한 테이블과 관련된 변경이 발생하면 플러시를 먼저한다.