#Mockito

IntelliJ에서 테스트코드 파일 생성 단축키 : command + shift + t


>@Rule

규칙을 뜻하는데 사용하게 된다면 public으로 사용해야 한다.

~~~
@RunWith(MockitoJUnitRunner.class)
public class Test {

    @Rule
    public ExpectedException ex = ExpectedException.none();
    
}
~~~

>MockitoAnnotations.initMocks()
 
자동으로 초기화 시킴. @RunWith(MockitoJUnitRunner.class)와 같다.

~~~
public class Test {

    @Before
    public void init() {
        MockitoAnnotations.initMocks(this);
    }

}
~~~


>@Mock 

mockito 에서 가장 많이 사용되는 주석. 인스턴스를 생성하고 주입할 수 있다. 

~~~
@RunWith(MockitoJUnitRunner.class)
public class Test {

    @Mock
    List<String> list;

    @Test
    public void 가짜_데이터_만들기() {
        list.add("one");
        verify.add(list).add("one");
        assertEquals(0, list.size());
    
        when(list.size()).thenReturn(100);
        assertEquals(100, list.size());
    }
}
~~~

>@Spy

진짜 인스턴스를 mock하는 것

~~~
@RunWith(MockitoJUnitRunner.class)
public class Test {

    @Spy
    List<String> list = new ArrayList();

    @Test
    public void 스파이_데이터_만들기() {
        list.add("one");
        list.add("two");
        
        verify.add(list).add("one");
        verify.add(list).add("two");
        
        assertEquals(2, list.size());
    
        doReturn(100).when(list).size();
        assertEquals(100, list.size());
    }
}
~~~

>@Captor

~~~
@RunWith(MockitoJUnitRunner.class)
public class Test {

    @Mock
    List<String> list;
    
    @Captor
    ArgumentCaptor argCaptor;

    @Test
    public void captor() {
        list.add("one");

        verify.add(list).add(argCaptor.capture());
        
        assertEquals("one", argCaptor.getValue());
    }
}
~~~

참고 : https://www.baeldung.com/mockito-series