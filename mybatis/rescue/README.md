# Mybatis

> Mybatis에 대해서 알아본다.



## Mybatis란?

마이바티스는 개발자가 지정한 SQL, 저장프로시저 그리고 몇가지 고급 매핑을 지원하는 퍼시스턴스 프레임워크이다. 마이바티스는 JDBC로 처리하는 상당부분의 코드와 파라미터 설정및 결과 매핑을 대신해준다. 마이바티스는 데이터베이스 레코드에 원시타입과 Map 인터페이스 그리고 자바 POJO 를 설정해서 매핑하기 위해 XML과 애노테이션을 사용할 수 있다.

**모든 마이바티스 애플리케이션은 SqlSessionFactory 인스턴스를 사용한다.** SqlSessionFactory인스턴스는 SqlSessionFactoryBuilder를 사용하여 만들수 있다.



## SqlSessionFactory

SqlSessionFactory 이름에서 보듯이 SqlSession 인스턴스를 만들수 있다. SqlSession 은 데이터베이스에 대해 SQL명령어를 실행하기 위해 필요한 모든 메소드를 가지고 있다. 그래서 SqlSession 인스턴스를 통해 직접 SQL 구문을 실행할 수 있다. 

```java
SqlSession session = sqlSessionFactory.openSession();
try {
  Blog blog = session.selectOne("org.mybatis.example.BlogMapper.selectBlog", 101);
} finally {
  session.close();
}
```

이좀더 좋은 방법으로는 주어진 SQL 구문의 파라미터와 리턴값을 설명하는 인터페이스(예를 들면, BlogMapper.class )를 사용하여 문자열 처리 오류나 타입 캐스팅 오류 없이 좀더 타입에 안전하고 깔끔하게 실행할 수 있다.

```java
SqlSession session = sqlSessionFactory.openSession();
try {
  BlogMapper mapper = session.getMapper(BlogMapper.class);
  Blog blog = mapper.selectBlog(101);
} finally {
  session.close();
}
```

이 시점에 SqlSession이나 Mapper클래스가 정확히 어떻게 실행되는지 궁금할 것이다. 매핑된 SQL 구문에 대한 내용이 가장 중요하다. 다음의 두가지 예제를 통해 정확히 어떻게 작동하는지에 대해서는 충분히 이해하게 될 것이다.

구문은 XML이나 애노테이션을 사용해서 정의할 수 있다. 그럼 먼저 XML 을 보자. 마이바티스가 제공하는 대부분의 기능은 XML을 통해 매핑 기법을 사용한다. 이전에 마이바티스를 사용했었다면 쉽게 이해되겠지만 XML 매핑 문서에 이전보다 많은 기능이 추가되었다. SqlSession을 호출하는 XML 기반의 매핑 구문이다.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.mybatis.example.BlogMapper">
  <select id="selectBlog" resultType="Blog">
    select * from Blog where id = #{id}
  </select>
</mapper>
```

한 개의 매퍼 XML 파일에는 많은 수의 매핑 구문을 정의할 수 있다.

```java
Blog blog = (Blog) session.selectOne("org.mybatis.example.BlogMapper.selectBlog", 101);
```

이건 마치 패키지를 포함한 전체 경로의 클래스내 메소드를 호출하는 것과 비슷한 형태이다. 이 이름은 매핑된 select구문의 이름과 파라미터 그리고 리턴타입을 가진 네임스페이스과 같은 이름의 Mapper 클래스와 직접 매핑될 수 있다. 이건 위에서 본것과 같은 Mapper 인터페이스의 메소드를 간단히 호출하도록 허용한다. 위 예제에 대응되는 형태는 아래와 같다.

```java
BlogMapper mapper = session.getMapper(BlogMapper.class);
Blog blog = mapper.selectBlog(101);
```

두번째 방법은 많은 장점을 가진다. 먼저 문자열에 의존하지 않는다는 것이다. 이는 애플리케이션을 좀더 안전하게 만든다. 두번째는 개발자가 IDE 를 사용할 때 매핑된 SQL 구문을 사용할때의 수고를 덜어준다. 세번째는 리턴타입에 대해 타입 캐스팅을 하지 않아도 된다. 그래서 BlogMapper 인터페이스는 깔끔하고 리턴 타입에 대해 타입에 안전하며 이는 파라미터에도 그대로 적용된다.

BlogMapper와 같은 Mapper클래스에는 몇가지 트릭이 있다. 매핑된 구문은 XML 에 전혀 매핑될 필요가 없다. 대신 자바 애노테이션을 사용할 수 있다. 예를들어 위 XML 예제는 다음과 같은 형태로 대체될 수 있다.

```java
package org.mybatis.example;
public interface BlogMapper {
  @Select("SELECT * FROM blog WHERE id = #{id}")
  Blog selectBlog(int id);
}
```



## 스코프(Scope) 와 생명주기(Lifecycle)

> **객체 생명주기와 의존성 삽입 프레임워크**
>
> 의존성 삽입 프레임워크는 쓰레드에 안전하도록 해준다. 트랜잭션 성질을 가지는 SqlSessions과 매퍼들 그리고 그것들을 직접 빈에 삽입하면 생명주기에 대해 기억하지 않아도 되게 해준다. DI프레임워크와 마이바티스를 사용하기 위해 좀더 많은 정보를 보기 위해서는 MyBatis-Spring이나 MyBatis-Guice 하위 프로젝트를 찾아보면 된다.

### SqlSessionFactoryBuilder

이 클래스는 인스턴스화 되어 사용되고 버려질 수 있다. SqlSessionFactory 를 생성한 후 유지할 필요는 없다. 그러므로 SqlSessionFactoryBuilder 인스턴스의 가장 좋은 스코프는 메소드 스코프(예를들면 메소드 지역변수)이다. 여러개의 SqlSessionFactory 인스턴스를 빌드하기 위해 SqlSessionFactoryBuilder를 재사용할 수도 있지만 유지하지 않는 것이 가장 좋다.

### SqlSessionFactory

한번 만든뒤 SqlSessionFactory는 애플리케이션을 실행하는 동안 존재해야만 한다. 그래서 삭제하거나 재생성할 필요가 없다. 애플리케이션이 실행되는 동안 여러 차례 SqlSessionFactory 를 다시 빌드하지 않는 것이 가장 좋은 형태이다. 재빌드하는 형태는 결과적으로 “나쁜냄새” 가 나도록 한다. 그러므로 SqlSessionFactory 의 가장 좋은 스코프는 애플리케이션 스코프이다. 애플리케이션 스코프로 유지하기 위한 다양한 방법이 존재한다. 가장 간단한 방법은 싱글턴 패턴이나 static 싱글턴 패턴을 사용하는 것이다. 또는 구글 쥬스나 스프링과 같은 의존성 삽입 컨테이너를 선호할 수도 있다. 이러한 프레임워크는 SqlSessionFactory의 생명주기를 싱글턴으로 관리할 것이다.

### SqlSession

각각의 쓰레드는 자체적으로 SqlSession인스턴스를 가져야 한다. SqlSession인스턴스는 공유되지 않고 쓰레드에 안전하지도 않다. 그러므로 가장 좋은 스코프는 요청 또는 메소드 스코프이다. SqlSession 을 static 필드나 클래스의 인스턴스 필드로 지정해서는 안된다. 그리고 서블릿 프레임워크의 HttpSession 과 같은 관리 스코프에 둬서도 안된다. 어떠한 종류의 웹 프레임워크를 사용한다면 HTTP 요청과 유사한 스코프에 두는 것으로 고려해야 한다. 달리 말해서 HTTP 요청을 받을때마다 만들고 응답을 리턴할때마다 SqlSession 을 닫을 수 있다. SqlSession 을 닫는 것은 중요하다. 언제나 finally 블록에서 닫아야만 한다.

### Mapper 인스턴스

Mapper는 매핑된 구문을 바인딩 하기 위해 만들어야 할 인터페이스이다. mapper 인터페이스의 인스턴스는 SqlSession 에서 생성한다. 그래서 mapper 인스턴스의 가장 좋은 스코프는 SqlSession 과 동일하다. 어쨌든 mapper 인스턴스의 가장 좋은 스코프는 메소드 스코프이다. 사용할 메소드가 호출되면 생성되고 끝난다. 명시적으로 닫을 필요는 없다.



## ObjectFactory

매번 마이바티스는 결과 객체의 인스턴스를 만들기 위해 ObjectFactory를 사용한다. 디폴트 ObjectFactory 는 디폴트 생성자를 가진 대상 클래스를 인스턴스화하는 것보다 조금 더 많은 작업을 한다. ObjectFactory 의 디폴트 행위를 오버라이드하고자 한다면 만들 수 있다.



## Environments

마이바티스는 여러개의 환경으로 설정할 수 있다. 여러가지 이유로 여러개의 데이터베이스에 SQL Map을 적용하는데 도움이 된다. 예를들어, 개발, 테스트, 리얼 환경을 위해 별도의 설정을 가지거나 같은 스키마를 여러개의 DBMS 제품을 사용할 경우들이다. 그외에도 많은 경우가 있을 수 있다.

**중요하게 기억해야 할 것은 다중 환경을 설정할 수는 있지만 SqlSessionFactory 인스턴스마다 한개만 사용할 수 있다는 것이다.**

두개의 데이터베이스에 연결하고 싶다면 SqlSessionFactory 인스턴스를 두개 만들 필요가 있다. 세개의 데이터베이스를 사용한다면 역시 세개의 인스턴스를 필요로 한다. 기억하기 쉽게

- **데이터베이스별로 하나의 SqlSessionFactory**



## **TransactionManager**

마이바티스는 두가지 타입의 TransactionManager를 제공한다.

- JDBC - 이 설정은 간단하게 JDBC 커밋과 롤백을 처리하기 위해 사용된다. 트랜잭션의 스코프를 관리하기 위해 dataSource 로 부터 커넥션을 가져온다.
- MANAGED - 이 설정은 어떤것도 하지 않는다. 결코 커밋이나 롤백을 하지 않는다. 대신 컨테이너가 트랜잭션의 모든 생명주기를 관리한다. 디플트로 커넥션을 닫긴 한다. 하지만 몇몇 컨테이너는 커넥션을 닫는 것 또한 기대하지 않기 때문에 커넥션 닫는 것으로 멈추고자 한다면 “closeConnection”프로퍼티를 false로 설정하라. 

> 마이바티스를 스프링과 함께 사용하는 경우에는 구성할 필요가 없습니다 스프링 모듈 자체의 설정 때문에 어떤 TransactionManager 이전에 설정된 구성을 무시합니다.



## 장/단점

### 장점

#### 매핑구문

마이바티스의 가장 큰 장점은 매핑구문이다. 이건 간혹 마법을 부리는 것처럼 보일 수 있다. SQL Map XML 파일은 상대적으로 간단하다. 더군다나 동일한 기능의 JDBC 코드와 비교하면 아마도 95% 이상 코드수가 감소하기도 한다. 마이바티스는 SQL을 작성하는데 집중하도록 만들어졌다.

#### 동적SQL

마이바티스의 가장 강력한 기능 중 하나는 동적 SQL을 처리하는 방법이다. JDBC나 다른 유사한 프레임워크를 사용해본 경험이 있다면 동적으로 SQL 을 구성하는 것이 얼마나 힘든 작업인지 이해할 것이다. 간혹 공백이나 콤마를 붙이는 것을 잊어본 적도 있을 것이다. 동적 SQL 은 그만큼 어려운 것이다.



### 단점

#### 사용법

DB별로 사용법이 다르다.







## 참고

- http://www.mybatis.org/mybatis-3/ko/index.html