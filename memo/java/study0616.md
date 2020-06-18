#for-loop 와 stream().forEach()의 차이

>Streams

자바 8에서 추가한 스트림은 람다를 활용할 수 있는 기술 중에 하나. 스트림은 '데이터의 흐름' 이다. 
컬렉션 인스턴스에 함수 여러 개를 조합해서 원하는 결과를 필터링 하고 가공된 결과를 얻을수 있다. 
병렬처리 가능(parallelStream)

빈 스트림
~~~
public Stream<String> 빈_스트림(List<String> list){
    return Objects.isNull(list) || list.isEmpty()? Stream.empty() : list.stream();
}

public void 빌더(){
    Stream<String> builderStream = Stream.<String>builder()
    .add("test1")
    .add("test2")
    .build();
}

public void generate는_무한이라_최대사이즈_정해줘야함(){
    Stream<String> builderStream = Stream.generate(()->"test").limit(5);    //[test,test,test,test,test]
}

public void iterate는_초기값과_해당값_계산_얘도_무한이라_최대사이즈_정해줘야함(){
    Stream<String> builderStream = Stream.iterate(10, n -> n+2).limit(5);    //[10,12,14,16,18]
}

public void 기본타입_스트림(){
    IntStream result1 = IntStream.range(1,5);   //[1,2,3,4] 마지막부분 포함 안됨
    IntStream result2 = IntStream.rangeClosed(1,5);   //[1,2,3,4,5] 마지막부분 포함
}

public void test(){
    Strean<String> text1 = Stream.of("a","b","c");
    Strean<String> text2 = Stream.of("aa","bb","cc");
    
    //합치기
    Strean<String> concat = Stream.concat(text1,text2);
    
    //필터링
    Strean<String> filter = concat.stream()
    .filter(text -> text.contains("a"));    //["a","aa"]
    
    //매핑
    Strean<String> mapping = concat.stream()
    .map(String::toUpperCase);    //["A","AA"]
    
    //계산
    long count = IntStream.rangeClosed(1,5).count();    //5
    long sum = IntStream.rangeClosed(1,5).sum();    //15
    
    //최대 최소는 비어있을경우 표현이 안되기 때문에 Optional 처리
    OptionalInt min = IntStream.rangeClosed(1,5).min();    //1
    OptionalInt max = IntStream.rangeClosed(1,5).max();    //5
    
    

}
~~~



|구분     |stream().forEach()     |Iterable.forEach       |
|--------|-----------------------|-----------------------|
|순서     |순서 보장x                |순서가 보장              |
|side effect (부작용)|작업중 간섭 안됨 |제한이 적음              |
|Synchronized collection 상황에서 차이|컬렉션의 스플리터 사용   잠금 없음   비간섭적인 일반규칙에 의존|컬렉션 잠금을 한번 가져와서 베소드 진행까지 갖고 있음|



