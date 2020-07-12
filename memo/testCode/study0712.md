#자바와 JUnit을 활용한 실용주의 단위 테스트

>3-11 테스트 리팩토링

테스트 코드를 리팩토링하고 이해도를 최대화하며 유지보수 비용을 최소화 하자

~~~
List<String> result = getList();
assertThat(result, is(notNullValue())); //어떤 변수를 역으로 참조하기 전에 null을 검사하는건 안전하지만 테스트에서는 군더더기다
assertThat(result.size() >= 1); 
~~~

~~~
assertThat(result.size(), equalTo(0));
//비어있음을 의미하므로 아래와 같이 줄일수 있다.
assertTrue(result.isEmpty());
~~~

프로그래밍에서 상수로 선언하지 않은 숫자 리터럴을 '매직넘버'라고 부르며 되도록이면 사용금지. 상수로 표현하면 의미를 분명하게 전달할 수 있다.

테스트와 무관한 세부사항은 @Before, @After로 옮겨서 테스트하고자 하는 정보만 깔끔하게 전달하자

좋은 테스트는 테스트를 이해하는데 다른 함수를 파헤지지 않도록 해야한다.