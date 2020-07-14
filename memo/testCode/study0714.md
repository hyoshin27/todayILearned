#자바와 JUnit을 활용한 실용주의 단위 테스트

>4-13 까다로운 테스트

관심사 분리하기

-애플리케이션 로직은 스레드, 데이터베이스 or 문제를 일으킬 수 있는 다른 의존성과 분리

-느리거나 휘발적인 코드는 목으로 대체하여 단위테스트의 의존성 끊기

-필요한 경우 통합 테스트를 작성하되 단순하고 집중적으로 만들기

 
실행 전후 데이터베이스 비우기(@Before)

테스트가 끝나면 롤백(@After)

~~~
public class QuestionControllerTest{
    private QuestionController controller;
    
    @Before
    public void create(){
        controller = new QuestionController();
        controller.deleteAll();
    }
    
    @After
    public void cleanUp(){  //데이터 확인하려면 after 주석처리 후 확인
        controller.deleteAll();
    }
    
    @Test
    public findQuestion(){
        int id = controller.addBooleanQuestion("question text");
        Question question = controller.find(id);
        
        assertThat(question.getText, equalTo("question text"));
    }
    
    @Test
    public findMatchinfQuestion(){
        controller.addBooleanQuestion("alpha 1");
        controller.addBooleanQuestion("alpha 2");
        controller.addBooleanQuestion("beta 1");
        
        List<Question> questions = controller.findMatchingText("alpha");
        
        assertThat(questions.stream().map(Question::getText).collect(Collectors.toList()), equalTo(Arrays.asList("alpha 1", "alpha 2")));
    }
}
~~~

