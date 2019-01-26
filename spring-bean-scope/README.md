# Spring Bean Scope



## Bean Scopes

Bean 정의를 생성할 때, 해당 Bean 정의된 클래스의 실제 인스턴스를 생성하는 방법을 작성합니다. Bean 정의가 정의가 레시피라는 생각은 중요합니다. 왜냐하면 그것은 클래스처럼 하나의 레시피에서 많은 객체 인스턴스를 만들 수 있다는 것을 의미하기 때문이다.

특정 bean 정의에서 생성 된 객체에 플러그인되는 다양한 종속성 및 구성 값뿐만 아니라 특정 bean 정의에서 생성 된 객체의 범위를 제어 할 수 있습니다. 이 접근법은 강력하고 유연합니다. Java 클래스 수준에서 객체의 범위를 설정하지 않고 구성을 통해 만드는 객체의 범위를 선택할 수 있기 때문입니다. 여러 범위 중 하나에 배치되도록 정의할 수 있다. 스프링 프레임워크는 6개의 범위를 지원하며,  그 중 4개는 웹 인식 `ApplicationContext`를 사용하는 경우에만 사용할 수 있다. [사용자 지정 범위를](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-scopes-custom) 만들 수도 있습니다.

| 범위                                                         | 기술                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [singleton](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-scopes-singleton) | (기본값) 각 Spring IoC 컨테이너에 대해 단일 객체 인스턴스로 범위로 지정합니다. |
| [prototype](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-scopes-prototype) | 단일 빈 정의를 원하는 수의 객체 인스턴스로 확장합니다.       |
| [request](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-scopes-request) | 단일 bean 정의를 단일 HTTP 요청의 라이프 사이클 범위로 확장합니다. 즉, 각 HTTP 요청은 단일 bean 정의의 뒤쪽에서 작성된 bean의 자체 인스턴스를가집니다. 웹 인식 Spring `ApplicationContext`의 맥락에서만 유효합니다. |
| [session](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-scopes-session) | 단일 bean 정의를 HTTP의 수명주기 범위로 확장합니다. 웹 인식 Spring `ApplicationContext`의 맥락에서만 유효합니다. |
| [application](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-scopes-application) | 단일 bean 정의를 `ServletContext`의 수명주기 범위로 확장합니다. 웹 인식 Spring ` ApplicationContext`의 맥락에서만 유효합니다. |
| [websocket](https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#websocket-stomp-websocket-scope) | 단일 bean 정의를 `WebSocket`의 수명주기 범위로 확장합니다. 웹 인식 Spring `ApplicationContext`의 맥락에서만 유효합니다. |



## 범위

- https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-scopes