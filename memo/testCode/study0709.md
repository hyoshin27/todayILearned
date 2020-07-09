#자바와 JUnit을 활용한 실용주의 단위 테스트

>3-10 목 객체 사용

스텁(stub) : 테스트 용도로 하드코딩한 값을 반환되는 구현체 

람다식
~~~
Http http = (String url) -> "{\"address\" : \"{
                        +"\"city\": \"anyang\"," 
                        +"\"country\": \"korea\"}"
                        +"}";
~~~ 

익명 내부 클래스
~~~

Http http = new Http (){
    @Override
    public String get(String url) throw IOException{
        return "{\"address\" : \"{
               +"\"city\": \"anyang\"," 
               +"\"country\": \"korea\"}"
               +"}";
    }
}
~~~ 

사용해보기
~~~
public class AddressRetriever {
    private Http http;
    
    public AddressRetriever(Http http){
        this.http = http;
    }
    
    public JSONObject retrieve(){
        String response = http.get("http://test.com");
        return (JSONObject) new JSONParser().parse(response);
    }
}

public class AddressRetrieverTest {
    @Test
    public void stubTest() throw IOException, ParseException{    
        Http http = (String url) -> "{\"address\" : \"{
                                +"\"city\": \"anyang\"," 
                                +"\"country\": \"korea\"}"
                                +"}";
        AddressRetriever addressRetriever = new AddressRetriever(http); //스텁 저장
        JSONObject object = addressRetriever.retrieve(); //항상 정해진 값 전달
        
        Map<String, Object> map = new ObjectMapper().readValue(object.toJSONString(), Map.class);
        Map<String, String> address = (Map<String, String>) map.get("address");
        String city = (String) address.get("city");
        
        assertThat(city, equalTo("anyang"));                  
    }
}

when().thenReturn()은 대표적인 목 설정

목 Mock: 모조품(가짜 데이터)

@Mock 을 사용하여 목 인스턴스 생성

@InjectMock 을 붙인 대상 인스턴스 변수 선언

대상 인스턴스를 인스턴스화한 후에는 MockitoAnnotations.initMocks(this) 호출. 

목은 단위 테스트 커버리지 구멍을 만든다. 통합 테스트를 작성하여 이 구멍을 막아야 한다.
~~~