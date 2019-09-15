# Spring TEST



## 통합테스트



### 장점

모든 Bean을 올려서 테스트

- 쉽게 테스트 가능
- 운영환경과 유사한 테스트
- 전체적인 Flow 테스트 가능



### 단점

- 모든 Bean을 올리기 때문에 시간이 오래걸리고 무거움
- 테스트 단위가 커서 테스트 실패시 디버깅이 어려움
- Rollback 처리가 안되는 테스트 진행을 하기 어려움



### TEST

```java
@RunWith(SpringRunner.class)
@SpringBootTest(
  classes = {CommonConfig.class},
  properties = "classpath:application-test.yml"),
	webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT
)
@Transactional
public class IntegrationTest{
  
  @Autowired 
  MockMvc mvc;
  
  @Autowired
  private TestRestTemplate restTemplate;
  
}
```

- 요청부터 응답까지 전체 Flow를 TEST
- `@SpringBootTest`는 실제 ApplicatonText를 로드
  - classes 속성에 `@Configuration` 을 사용하는 클래스가 있다면 내부에서 `@Bean` 어노테이션을 통해서 생성되는 빈도 모두 등록
  - `webEnvironment`을 통해 웹 환경 설정 가능
- 웹 통합 테스트 가능
  - `@TestRestTemplate`
    - Servlet Container를 생성
    - Web Environment 설정을 기반으로 Port 실행
    - Clinet 입장에서 `RestTemplate`을 사용하듯이 테스트
  - `@MockMvc`
    - Servlet Container를 생성하지 않음
    - 서버 입장에서 구현한 API를 통해 비즈니스 로직이 문제없이 수행되는지 테스트
- 기존에 정의했던 Configuration을 커스터마이징하고 싶은 경우 
  - `@TestConfiguration` 
    - ComponentScan을 통해서 감지되기 때문에 만일 `@SpringBootTest`의 classes 속성을 이용하여 특정 클래스만을 지정했을 경우에는 `@TestConfiguation`은 감지되지 않음
      - classes 속성에 직접 `@TestConfiguration`을 추가
      - `@Import` 을 통해 직접 사용할 TestConfiguration을 명시
        - 여러 테스트에서 공유하여 사용 가능
- `@ActiveProfiles`을 통해 원하는 프로파일 환경값 설정 가능
- `@Transactional` 을 사용하면 자동으로 **rollback 처리**
  - `WebEnvironment.RANDOM_PORT`, `DEFINED_PORT`를 사용하면 실제 테스트 서버는 별도의 스레드에서 테스트를 수행하기 떄문에 **트랜잭션이 롤백되지 않음**



## Controller Test



### 장점

- 관심사가 아닌 것들은 Mocking을 통해 외부 의존성을 줄임
- 테스트 속도가 빠름
- `WebApplication` 관련된 Bean들만 등록
- 통합 테스트보다 속도가 빠름
- 통합 테스트를 진행하기 어려운 테스트를 할 수 있음
  - 외부 API
  - Rollback 처리가 힘들거나 불가능한 경우
  - 실제로 호출 하면 안되는 API



### 단점

- 의존성 객체를 Mocking하기 때문에 완벽한 테스트는 아님
- Mocking 처리하기 위한 시간이 소요
- Mocking 라이브러리에 대한 학습 비용 발생
- Mock 기반 으로 테스트하기 때문에 실제 환경에서는 결과가 다를 수 있음



### TEST

```java
@RunWith(SpringRunner.class)
@WebMvcTest(TestController.class)
public class TestController {

    @MockBean 
    private MockService MockService;
  
    @Autowired
    private MockMvc mvc;

    //..

}
```

- `@MockBean`을 선언하면 Mock 객체를 빈으로써 등록
- `@MockBean`으로 선언한 객체와 같은 이름과 타입으로 이미 빈으로 등록되어있다면 해당 빈은 선언한 Mock 빈으로 대체
- `@WebMvcTest `  Scan 대상
  - @Controller
  - @ControllerAdvice
  - @JsonComponent
  - Filter
  - WebMvcConfigurer and HandlerMethodArgumentResolver
  - `MockMvc`를 자동으로 설정하여 Bean으로 등록
- MockMvc를 자동으로 지원
  -  HTTP 서버 없이 Controller 테스트 가능
  - Async Test를 할 경우 `MvcResult`로 받아서 `asyncDispatch`로 감싸줄 필요가 있음



## Service Test



### 장점

- 하고자 하는 테스트에만 집중
- 관심사가 아닌 것들은 Mocking을 통해 외부 의존성을 줄임
- 테스트 속도가 빠름



### 단점

- 의존성 객체를 Mocking하기 때문에 완벽한 테스트는 아님
- Mocking 처리하기 위한 시간이 소요
- Mocking 라이브러리에 대한 학습 비용 발생



### TEST

```java
@RunWith(MockitoJUnitRunner.class)
public class MockServiceTest {

  @Mock
  private MockRepository mockRepository;
  
  @Spy
  private SpyRepository SpyRepository;

  @InjectMocks
  private MockService mockService;

  //..
}
```

- `@Mock`을 사용하여 mock객체를 만들어 원하는 응답의 데이터를 설정
- `@InjectMocks`은 **@Mock**이나 **@Spy** 어노테이션을 사용하는 Mock 객체를 자신의 멤버 클래스와 일치하면 주입
- `@Spy`이 선언된 Mock 객체는 목 메소드를 선언하지 않았다면 실제 메소드가 호출





# Repository Test



### 장점

- **Repository** 관련된 Bean들만 등록
- 통합 테스트보다 속도가 빠름
- 테스트 범위가 작음



### 단점

- 테스트 범위가 작기 떄문에 실제 환경과 다를 수 있음



### TEST

```java
@RunWith(SpringRunner.class)
@DataJpaTest
@AutoConfigureTestDatabase(replace = Replace.NONE)
public class RepositoryTest {
  
  @Autowired
  private TestRepository TestRepository; 

}

```

- `@DataJpaTest` 을 사용하여 **Repository**에 대한 Bean만 등록
  - `@Entity` Class를 Scan
  - `@Transactional` 어노테이션을 포함
    - `@Transactional` 기능이 필요하지 않으면 `@Transactional(propagation = Propagation.NOT_SUPPORTED)` 설정
  - `TestEntityManager` 생성
- `@DataJpaTest`는 기본적으로 **in-memory embedded database**에 대한 테스트를 진행
  - real database를 사용하고자 하는 경우`@AutoConfigureTestDatabase`  사용
- `JpaRepository`에서 기본적으로 기본적으로 재공하는 메서드는 테스트 하지 않음
- 단위 테스트마다 DB가 자동적으로 Rollback
- `@JdbcTest`
  - Spring Data JPA를 사용하지 않고 순수 데이터베이스 테스트 할 떄 사용
  -  기본적으로 **in-memory embedded database**가 설정되며, 테스트를 위한 `JdbcTemplate`을 생성



## Json Test

`@JsonTest` 을 사용하면 JSON serialization과 deserialization을 테스트

```java
@RunWith(SpringRunner.class)
@JsonTest
public class ArticleJsonTest {
  
  @Autowired
  private JacksonTester<Member> json;

}
```

- `@JsonTest` 는 `ObjectMapper`와 `@JsonComponent` 빈을 포함한 Jackson의 테스트를 위한 모듈들을 자동으로 설정
- `JacksonTester`, `GsonTester`, `BasicJsonTester` 등 제공



## Client Test

- 서버가 아닌 클라이언트 입장에서 Test할 때 사용
  - Apache HttpClient나 Spring의 RestTemplate을 사용하여 외부 서버에 웹 요청할 때 



### 장점

- `RestTemplate` 같은 객체를 Mock 객체로 바꿔서 테스트하는 것보다 리얼 환경에 가깝게 단위 테스트를 수행



```java
@RunWith(SpringRunner.class)
@RestClientTest(TestService.class)
public class TestServiceWithRestClientTest {
    
    @Autowired
    private MockRestServiceServer server;

}
```

- `@RestClientTest`는 요청에 반응하는 가상의 Mock 서버를 만듬
  - 내부 코드에서 웹 요청이 발생할 경우 `@RestClientTest`로 인해서 생성된 가상의 서버가 응답



## 참고

- https://meetup.toast.com/posts/124
- [https://www.popit.kr/spring-guide-%ED%85%8C%EC%8A%A4%ED%8C%85-%EC%A0%84%EB%9E%B5/](https://www.popit.kr/spring-guide-테스팅-전략/)
- https://jojoldu.tistory.com/34