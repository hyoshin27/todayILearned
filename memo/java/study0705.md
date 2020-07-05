#Call by value 와 Call by reference

>Call by Value(값에 의한 호출)

인자로 받은 값을 복사하여 처리. 복사되어 처리 되기 때문에 원래 값에 영향을 받지 않아 안전하다.

복사하여 처리되기 때문에 메모리량이 늘어난다.

지역변수처럼 처리

>Call by reference(참조에 의한 호출)

인자로 받은 값의 주소를 참조하여 직접 값에 영향을 준다.

~~~
@Test
public void test(){
    List<String> list = new ArrayList<>();

    list.add("a");
    add(list);
    System.out.println("===1 : "+ list);
    clear(list);
    System.out.println("===2 : "+ list);
}

private void add(List<String> list){
    list.add("b");
}

private void clear(List<String> list){
    list = null;
}
~~~

b가 추가 되어 collection 은 Call by reference 라고 생각할 수 있지만 list에 다른 객체를 넣었을 때 바뀌어야 Call by reference 가 되는 것이다.

