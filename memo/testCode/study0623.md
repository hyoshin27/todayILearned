#Mockito



~~~
@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
public class ApiControllerTest {

    private MockMvc mockMvc;

    @Autowired
    private ObjectMapper mapper;

    @InjectMocks
    ApiController apiController;    //@InjectMocks 으로 선언한 곳에 @Mock 주입

    @Mock
    private Service serviceMock;
    
    List<Integer> list = new ArrayList(); 

    @Before
    public void setup() {
        mockMvc = MockMvcBuilders.standaloneSetup(apiController)
                .setControllerAdvice(new ExceptionHandlerExceptionResolver())   //ControllerAdvice(전역 에러처리를 도와주는 Bean
                .build();
        
        list.add(1)       
    }
    
    @Test
    public void 일반_테스트() throws Exception{
        mockMvc.perform(get("/api/test")
                .content(mapper.writeValueAsString(list))
                .contentType(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk());
    }
    
    @Test
    public void 에러_테스트() throws Exception{
        mockMvc.perform(get("/api/test")
                .content(mapper.writeValueAsString(list))
                .contentType(MediaType.APPLICATION_JSON))
                .andReturn();
    }
        
    @Test(expected = Exception.class)
    public void 정확한_에러확인() throws Exception{

        mockMvc.perform(get("/api/test")
                .content(mapper.writeValueAsString(list))
                .contentType(MediaType.APPLICATION_JSON))
                .andExpect((result) -> assertTrue(
                        result.getResolvedException()
                                .getClass()
                                .isAssignableFrom(AdRuntimeException.class)
                        )
                );
    }
    
    @Test(expected = Exception.class)
    public void 값_확인() throws Exception{

        mockMvc.perform(get("/api/test")
                .content(mapper.writeValueAsString(list))
                .contentType(MediaType.APPLICATION_JSON))
                .andExpect(jsonPath("$.id").value("1"));

    } 
    
      
}
~~~
