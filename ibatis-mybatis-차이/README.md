# Ibatis VS Mybatis

Apache project팀에서 google code팀으로 이동하면서 명칭이 변경되었다. 



## MyBatis란?

- 객체 지향 언어인 자바의 관계형 데이터베이스 프로그래밍을 더욱 쉽게 도와주는 프레임 워크
- 자바에서는 관계형 데이터베이스 프로그래밍을 하기 위해 JDBC를 제공 
- MyBatis는 JDBC를 더욱 편하게 사용하기 위해 개발되었다.

### 특징 

- SQL문이 코드로부터 완전히 분리 : 기존에는 DAO파일에 모든 SQL 문을 작성하였다. 하지만 MyBatis에서는 Mapper 파일에 SQL 코드를 입력해 놓고 DAO 파일에서 필요할 때마다 가져와서 사용할 수 있다. 
- 생산성 : 코드가 짧아진다. 
- 유지 보수성 향상 : Mapper 파일에만 SQL 코드를 입력하고 나중에 SQL 코드를 변경할 때 이곳에서 유지보수만 하면, DAO에서는 아무런 영향을 받지 않는다. 왜냐하면, DAO에서는 Mapper 파일에서작성된 SQL 코드를 갖다 쓰기만 하기 때문이다.



## 차이점

### 자바 버전

iBatis에서는 JDK 1.4 이상, MyBatis에서는 JDK 1.5 이상 사용 가능하다.

### 내부 구조

- ibatis : com.ibatis.*
- mybatis : org.apache.ibatis.* 

### sqlMap.xml 내부 구조 변경

- parameterMap 이 parameterType으로 대체되었다.

- dtd가 변경 (“http://mybatis.org/dtd/mybatis-3-mapper.dtd”> 
- SqlMapConfig : sqlMap 이 Mapper 로 변경되었다.
- Configration : resultClass 이 resultType 로 변경되었다.

### 태그 변경

\<isEqual> , \<isNull> 등의 구문이 \<if>로 통합 되고 trim, foreach 태그가 새로 추가되었다.

### MyBatis lib 별도 제공

### Annotation 도입 

- sqlMapClient DI 설정 불필요하다.
- 간편해졌다.
- Bean id sqlSesstionFactory, sqlSesstionTemplate만 지정하면 된다.

###  rowHandler 대체 

- xml및 대량 데이터 처리를 위해 사용되었던 rowHandler가 삭제되었다.
- rowHandler 이 resulthandler로 변경되었다.
- 자바 annotation을 사용하여 xml을 사용하지 않고 자바로만 할 수 있게되었다.
- 자바 선언 보다 xml 선언이 우선순위를 가진다.

### 네임스페이스 방식 변경 

- 경로를 모두 명시해야한다.
- ibatis : \<sqlMap namespace="User"> 
- MyBatis : \<mapper namespace="myBatis.mapper.UserMapper"> 
- 네임스페이스 사용은 필수, userStatementNameSpace설정은 제거한다.

### 동적 SQL 

If, choose(when, otherwise), trim (whre,set), foreach 를 사용할 수 있다.