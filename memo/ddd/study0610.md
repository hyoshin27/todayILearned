#DDD start!

>7. 도메인 서비스

-한 애그리거트에 넣기 애매한 도메인 기능을 억지로 구현하면 코드가 길어지고 외부에 대한 의존이 높아지게 된다. 
이것은 코드를 복잡하게 하여 수정이 어려워지고 도메인의 개념도 찾기가 어렵다. 이럴때는 도메인 서비스를 별도로 구현한다.

-도메인 서비스가 도메인 영역의 애그리거크나 밸류와 같은 다른 구성요소와 비교할때 다른점이 있다면 상태 없이 로직만 구현한다는 점.

-도메인의 서비스를 구현하는데 필요한 상태는 애그리거트나 다른 방법으로 전달 받는다.

-특정 기능이 응용서비스인지 도메인 서비스인지 감을 잡기 어려울때는 해당 로직이 애그리거트의 상태를 변경하거나 애그리거트의 상태 값을 계산하는지 검사해본다.
ex)계좌이체 : 계좌의 상태를 바꿈 계제금액로직: 주문금액 계산 -> 도메인로직이면서 한 애그리거트에 넣기 적합하지 않으므로 도메인 서비스로 구현

도메인 서비스의 구현이 특정 구현 기술에 의존적이거나 외부 시스템의 API를 실행한다면 도메인 영역의 도메인 서비스는 인터페이스로 추상화 해야한다.
이를 통해 도메인 영역이 특정 구현에 종속되는 것을 방지할 수 있고 도메인 영역에 대한 테스트가 수월해진다.

