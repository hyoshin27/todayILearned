#getOrDefault, putIfAbsent, computeIfAbsent

>getOrDefault

값이 null 이면 default 값 리턴

Map<String,List<String>> map = new HashMap<>();


~~~~
@Test
public void test(){
    Map<String, List<String>> map = new HashMap<>();
    List<String> list = map.getOrDefault("1", new ArrayList<>());

    System.out.println(Objects.nonNull(list));
    System.out.println(Objects.nonNull(map.get("1")));
}
~~~~

결과

~~~
true
false
~~~

>putIfAbsent

없으면 null 

~~~
@Test
public void test(){
    Map<String, List<String>> map = new HashMap<>();
    List<String> list = new ArrayList<>();
    List<String> list2 = new ArrayList<>();

    list.add("one");
    list2.add("two");

    map.put("1", list2);

    map.putIfAbsent("1", list).add("999");

    map.putIfAbsent("2", list2).add("999"); //여기에서 NullPointerException

    System.out.println("map1  값 :" + map.get("1").toString());
    System.out.println("map2  값 :" + map.get("2").toString());
}
~~~

>computeIfAbsent

키에 해당하는 값이 존재하지 않을때 람다식 안에 사용 
~~~
@Test
public void test(){
     Map<String, List<String>> map = new HashMap<>();
    List<String> list = new ArrayList<>();
    List<String> list2 = new ArrayList<>();

    list.add("one");
    list2.add("two");

    map.put("1", list2);

    map.computeIfAbsent("1", value -> list).add("111");
    map.computeIfAbsent("2", value -> list2).add("999");

    System.out.println("map1  값 :" + map.get("1").toString());
    System.out.println("map2  값 :" + map.get("2").toString());
}
~~~

결과
~~~
map1  값 :[two, 111, 999]
map2  값 :[two, 111, 999]
~~~

>computeIfPresent

computeIfAbsent 반대로 키에 해당하는 값이 존재할때 람다식 안에 사용 

~~~
@Test
public void test(){
    Map<String, List<String>> map = new HashMap<>();
    List<String> list = new ArrayList<>();
    List<String> list2 = new ArrayList<>();

    list.add("one");
    list2.add("two");

    map.put("1", list);
    map.put("2", list2);

    map.computeIfPresent("1", (key, value) -> new ArrayList<>()).add("111");
    map.computeIfPresent("2", (key, value) -> list2).add("999");

    System.out.println("map1  값 :" + map.get("1").toString());
    System.out.println("map2  값 :" + map.get("2").toString());
}
~~~

결과
~~~
map1  값 :[111]
map2  값 :[two, 999]
~~~

결론 : map 안의 값을 확인하고 값을 추가할때는 getOrDefault로 해서 계속 생성비용을 늘리는 것보다 
해당 값이 없을때만 생성 할 수 있는 computeIfAbsent 권

~~~
public void test(){
    Map<String, List<String>> map = new HashMap<>();

    map.put("1", makeArray("1"));
    List<String> list = map.getOrDefault("1", makeArray("2"));
    list.add("one");

    map.put("2",list);
    System.out.println(map.toString());

}

private List<String> makeArray(String key){
    System.out.println(key + " : 호출시작");
    return new ArrayList<>();
}
~~~

결과

~~~
1 : 호출시작
2 : 호출시작
{1=[one], 2=[one]}
~~~

~~~
@Test
public void test(){
    Map<String, List<String>> map = new HashMap<>();

    List<String> list = new ArrayList<>();
    list.add("one");

    map.put("1",list);


    map.computeIfAbsent("1", value -> makeArray("1")).add("end");
    map.computeIfAbsent("2", value -> makeArray("2")).add("two");

    System.out.println(map.toString());

}

private List<String> makeArray(String key){
    System.out.println(key + " : 호출시작");
    return new ArrayList<>();
}
~~~

결과

~~~
2 : 호출시작
{1=[one, end], 2=[two]}
~~~