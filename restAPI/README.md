# REST AND REST API



## REST

REST란 **Representational State Transfer**의 약자다. 자원의 이름으로 자원의 상태를 주고 받는 것을 의미힌다.  

HTTP URI를 통해 Resource를 명시하고, HTTP Method(Post, Get, Put, Delete)를 통해 해당 Resource에 대한 CRUD Operation을 적용한다. 즉, REST는 ROA(Resource Oriented Architecture) 설계의 중심에 Resource가 있고 HTTP Method를 통해 Resource를 처리하도록 설계된 아키텍쳐를 의미한다.

### CRUD Operation

- Create : 생성(POST)
- Read : 조회(GET)
- Update : 수정(PUT)
- Delete : 삭제(DELETE)
- HEAD: header 정보 조회(HEAD)



### 장점

- OPENAPI를 제공하기 쉽다.
- 멀티플랫폼 지원 및 연동이 용이하다.
- 원하는 타입으로 데이터를 주고받을수 있다. (XML, JSON, RSS )
- 기존 웹 인프라를(HTTP)를 그대로 사용가능하다 ( 방화벽, 장비 요건 불필요 )
- 사용하기 쉽다
- 세션을 사용하지 않는다. 각각의 요청에 독립적이다



### 단점

- 표준이 없어서 관리가 어렵다.
- 사용할 수 있는 메소드가 4가지 밖에 없다.
- HTTP통신 모델에 대해서만 지원한다.



### 특징

- 클라이언트/서버 구조(Server-Client) : 일관적으로 독립되어야 한다.
- 무상태(Stateless) : 각요청 간 클라이언트의 Context는 서버에 저장되어서는 안 된다.
- 캐시가능(Cacheable) : WWW에서와 같이 클라이언트는 응답을 Caching 할 수 있어야 한다.
- 계층화(Layered System) : 클라이언트는 보통 대상 서버에 직접 연결 또는 중간 서버를 통해 연결되는지 모른다.
- Code on demand(option) : 자바 애플릿/ 자바스크립의 제공으로 서버가 클라이언트가 실행 시킬 수 있는 로직을 전송하여, 기능을 확장 할수 있다.
- 인터페이스 일관성(uniform interface) : 아키텍처를 단순화하고, 작은 단위로 분리하여, 클라이언트-서버 파트별로 독립적으로 개선 될 수 있도록 한다.
  - identification of resources : Resource가 URI 로 식별되어야 한다.
  - manipulation of resources through representations : representations 전송을 통해서 resources를 조작해야한다.
  - self-descriptive messages : 메시지 스스로 설명해야 한다.
  - hypermedia as the engine of application state(HATEOAS) : application의 상태는 Hyperlink를 이용해서 전이 되어야한다. (지키는 곳이 많이 없다)
- 자체 표현구조(Self-Descriptiveness) : API 메시지만 보고도 어떤 API인지를 이해 할수 있는 자체 표현 구조를 가진다.



## REST API

- REST를 기반으로 API를 구현한 것이다.



### 특징

- 확장성과 재사용성이 높아 유지보수 및 운용을 편리하게 할 수 있다.



### **규칙**

#### URI는 정보의 자원을 표현해야 한다.

- resource는 동사보다는 명사를, 대문자보다는 소문자를 사용한다.

- resource의 도큐먼트 이름으로는 단수 명사를 사용해야 한다.

- resource의 컬렉션 이름으로는 복수 명사를 사용해야 한다.

- resource의 스토어 이름으로는 복수 명사를 사용해야 한다.

  

#### 자원에 대한 행위는 HTTP Method(GET, PUT, POST, DELETE 등)로 표현한다.

- URI에 HTTP Method가 들어가면 안된다.
  - Ex) `GET /members/delete/1` -> `DELETE /members/1`
- URI에 행위에 대한 동사 표현이 들어가면 안된다.(즉, CRUD 기능을 나타내는 것은 URI에 사용하지 않는다.)
  - Ex) `GET /members/show/1` -> `GET /members/1`
  - Ex) `GET /members/insert/2` -> `POST /members/2`
- 경로 부분 중 변하는 부분은 유일한 값으로 대체한다.(즉, :id는 하나의 특정 resource를 나타내는 고유값이다.)
  - Ex) student를 생성하는 route: `POST /students`
  - Ex) id=12인 student를 삭제하는 route: `DELETE /students/12`



#### 하이픈(- )은 URI 가독성을 높이는데 사용

불가피하게 긴 URI경로를 사용하게 된다면 하이픈을 사용해 가독성을 높인다.



#### 밑줄(_ )은 URI에 사용하지 않는다.

밑줄은 보기 어렵거나 밑줄 때문에 문자가 가려지기도 하므로 가독성을 위해 밑줄은 사용하지 않는다.



#### URI 경로에는 소문자가 적합하다.

- URI 경로에 대문자 사용은 피하도록 한다.
- RFC 3986(URI 문법 형식)은 URI 스키마와 호스트를 제외하고는 대소문자를 구별하도록 규정하기 때문이다.



#### 파일확장자는 URI에 포함하지 않는다.

- REST API에서는 메시지 바디 내용의 포맷을 나타내기 위한 파일 확장자를 URI 안에 포함시키지 않는다.
- Accept header를 사용한다.
- Ex) `http://restapi.example.com/members/soccer/345/photo.jpg (X)`
- Ex) `GET / members/soccer/345/photo HTTP/1.1 Host: restapi.example.comAccept: image/jpg (O)`



#### 리소스 간에는 연관 관계가 있는 경우

- /리소스명/리소스 ID/관계가 있는 다른 리소스명
- Ex) `GET : /users/{userid}/devices (일반적으로 소유 ‘has’의 관계를 표현할 때)`



## RESTful의 개념

### RESTful이란

- RESTful은 일반적으로 REST라는 아키텍처를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어이다.
  - ‘REST API’를 제공하는 웹 서비스를 ‘RESTful’하다고 할 수 있다.
- RESTful은 REST를 REST답게 쓰기 위한 방법으로, 누군가가 공식적으로 발표한 것이 아니다.
  - 즉, REST 원리를 따르는 시스템은 RESTful이란 용어로 지칭된다.



### RESTful의 목적

- 이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것이다.
- RESTful한 API를 구현하는 근본적인 목적이 성능 향상에 있는 것이 아니라 일관적인 컨벤션을 통한 API의 이해도 및 호환성을 높이는 것이 주 동기이니, 성능이 중요한 상황에서는 굳이 RESTful한 API를 구현할 필요는 없다.



### RESTful 하지 못한 경우

Ex1) CRUD 기능을 모두 POST로만 처리하는 API
Ex2) route에 resource, id 외의 정보가 들어가는 경우(/students/updateName)



## 추천 동영상

- [그런 REST API로 괜찮은가](https://www.youtube.com/watch?v=RP_f5dMoHFc&t=222s)



## 참고

- https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html