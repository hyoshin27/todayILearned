#자바와 JUnit을 활용한 실용주의 단위 테스트

>1-3 JUnit 단언 깊게 파기

JUnit은 테스트에 넣을 수 있는 정적 메서드 호출이다.

두가지 주요 단언 스타일 제공 - 전통적인 스타일의 단언은 JUnit의 원래 버전에 포함되어 있고 새롭고 좀 더 표현력이 좋은 단언은 matchers

assertTrue : 참 거짓을 확인하지만 테스트코드는 특정 사례에 해당하기 떄문에 검증하는 기댓값을 명시적으로 지정하는 것이 낫다.

assertThat(실제 표현식, 검증하고자하는 값) : assertThat(account.getBalance(), equalTo(100)) 에서 equalTo가 matchers. 
matchers는 왼쪽에서 읽을수 있게 가독성을 높여 준다. 계좌 잔고가 100과 같아야 한다.
assertThat(account.getBalance()>0, is(true)) 처럼 장황하게 사용할바에는 assertTrue 를 사용하자

부정 :
assertThat(account.getName(), not( equalTo("TOM") ) )

널체크 : 널이 아닌 값을 자주 체크하는 것은 설계문제이거나 지나치게 걱정하는 것이다. 많은 경우 이런 검사는 불필요하고 가치가 없다.
assertThat(account.getName(), is( not( nullValue() ) ) 
assertThat(account.getName(), is( notNullValue() ) )

JUnit 햄크레스트매처 이용 효과

- 객체 타입을 검사

- 두 객체의 참조가 같은 인스턴스인지 검사

- 다수의 매처를 결합하여 둘 다 혹은 둘 중에 어떤 것이든 성공하는지 검사

- 어떤 컬렉션이 요소로 포함하거나 조건에 부합하는지 검사

- 어떤 컬렉션이 아이템 몇개를 모두 포함하는지 검사

- 어떤 컬렉션에 있는 모든 요소가 매처를 준수하는지 검사
