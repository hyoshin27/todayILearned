#자바와 JUnit을 활용한 실용주의 단위 테스트

>4-12 테스트 주도 개발

TDD 사이클

-실패하는 테스트코드 작성하기

-테스트 통과시키기

-이전 두 단계에서 추가되거나 변경된 코드 개선하기 

1. 시스템에 추가하고자 하는 동작을 정의하는 테스트코드 작성하기
~~~
public class ProfileTest(){
    @Test
    public void checkProfile(){
        new Profile();
    }
}

public class Profile(){

}
~~~

2.실패하는 테스트코드 작성하기
~~~
public class ProfileTest(){
    @Test
    public void checkProfile(){
        Profile profile = new Profile();
        
        Answer answer = new Answer(Bool.TRUE);
        boolean result = profile.matches(answer);
        
        assertTrue(result);
    }
}

public class Profile(){
    public boolean matches(Answer answer){
        return false;
    }
}
~~~

3. 정리
~~~
public class ProfileTest(){
    
    Profile profile;

    @Before
    public void createProfile(){
        profile = new Profile();
    }
    
    @Test
    public void checkProfile(){
        
        Answer answer = new Answer(Bool.TRUE);
        boolean result = profile.matches(answer);
        
        assertFalse(result);
    }
}

public class Profile(){
    public boolean matches(Answer answer){
    //로직 작성
        return false;
    }
}
~~~