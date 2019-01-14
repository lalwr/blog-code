## Spring TEST



## 설정

```xml
<properties>
    <spring.version>4.3.5.RELEASE</spring.version>
    <servlet.version>3.1.0</servlet.version>
</properties>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>${spring.version}</version>
</dependency>
<dependency>
    <groupId>com.jayway.jsonpath</groupId>
    <artifactId>json-path</artifactId>
    <version>2.4.0</version>
</dependency>
<dependency>
    <groupId>org.hamcrest</groupId>
    <artifactId>hamcrest-library</artifactId>
    <version>2.1</version>
    <scope>test</scope>
</dependency>
<dependency>
	<groupId>javax.servlet</groupId>
	<artifactId>javax.servlet-api</artifactId>
	<version>${servlet.version}</version>
</dependency>
```

```java
@RunWith(SpringJUnit4ClassRunner.class)
@WebAppConfiguration
@ContextConfiguration(locations = {
    "classpath:spring/config/*-config.xml",
    "classpath:spring/*-servlet.xml"
})
public class MainWebTest {

    @Autowired
    
    private WebApplicationContext wac;

    private MockMvc mock;

    @Before

    public void beforeTest() {
        mock = MockMvcBuilders.webAppContextSetup(wac).build();
    }

    @Test
    public void testSample() throws Exception {
        RequestBuilder req = MockMvcRequestBuilders.post("/getTest")
                .param("id", "test");

        mock.perform(req).andExpect(jsonPath("$.id").value("test"));
    }

}
```

