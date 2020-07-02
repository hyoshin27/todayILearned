#자바와 JUnit을 활용한 실용주의 단위 테스트

>3-8 깔끔한 코드로 리펙토링 하기

중복된 코드의 문제점 : 유지보수 비용이 증가, 변경에 대한 리스크도 증가 

~~~
Answer answer = answer.get(criterion.getAnswer().getQuestionText());

~~~

위 코드는 디메테르 법칙에도 위반하고 깔끔하지도 않음. criterion , criterion.getAnswer() 이 null 이면 널포인터 오류 날 확률 높음. 

디메테르의 법칙에 위반 : 다른 객체로 전파되는 연쇄적인 메서드 호출 피하기. 객체 내부의 어디까지 알아야 하나.

~~~
public void matches(Criteria criteria){
    
    for(Criterion criterion:criteria){
        Answer answer = answerMatching(criterion)
   
    }
}

private Answer answerMatching(Criterion criterion){

    return answer.get(criterion.getAnswer().getQuestionText())
}
~~~

어떤 경우든 코드를 수동으로 변경하면 실수하기 쉬우니 자동화된 리팩토링 도구를 사용하거나 테스트를 하자. 
단위 테스트는 기본 원칙을 깨지 않고 코드를 깔끔하게 유지해주는 보호장치를 제공한다.

