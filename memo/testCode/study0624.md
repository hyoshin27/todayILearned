#자바와 JUnit을 활용한 실용주의 단위 테스트

>1-1 첫번째 JUnit 테스트 만들기

테스트를 먼저 실패하고 성공하도록 고치기(정상적으로 동작하는지 증명)

클래스에서 결함이나 한계점을 드러낼 수 있는 테스트를 작성할 수 있는지

>1-2 JUnit 진짜로 써보기

테스트 구조 : 준비, 실행, 단언 (A.A.A Arrange-Act-Assert)

테스트 이름도 테스트의 의도를 담을 수 있는 적절한 이름으로 만들기

테스트 두개가 중복된 로직을 가지고 있다면 @Before 로 이동하기

테스트 클래스에는 static 필드 피하기(이럴 경우 테스트 마다 새로운 인스턴스를 생성해도 공유된다.)

~~~
public ProfileTest(){
    private Profile profile;
    private Question question;
    private Criteria criteria;
    
    @Before
    public void create(){
        profile = new Profile("Bull");
        question = new Question(1, "autonomous work?")
        criteria = new Criteria();
    }
    
    
    @Test
    public void matchAnswersTrue(){
        Answer profileAnswer = new Answer(question, Bool.FALSE);
        profile.add(profileAnswer);
        
        Answer criteriaAnswer = new Answer(question, Bool.TRUE);
        Criterition criterition = new Criterition(criteriaAnswer, Weight.DonCare);
        criteria.add(criterition);
        
        boolean matchers = profile.matches(criteria);
        
        assertTrue(matchers);
    }
    
    @Test
    public void matchAnswersFalse(){    
    //위와 같은 로직이지만 깔끔해 보이는 이유는 준비, 실행, 단언 부분을 한두줄로 압축 했기 때문 @Before는 우리의 관심사가 아니다
        profile.add(new Answer(question, Bool.FALSE));
        
        criteria.add(new Criterition(new Answer(question, Bool.TRUE), Weight.MustMatch));
        
        boolean matchers = profile.matches(criteria);
        
        assertFalse(matchers);
    }
    
}
~~~

