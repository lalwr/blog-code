# Mybatis-Spring

> Mybatis-Spring에 대해서 알아본다.



## 설정

마이바티스 스프링 연동모듈을 사용하기 위해서, 클래스패스에 `mybatis-spring-x.x.x.jar`를 포함시켜야 한다.

```xml
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis-spring</artifactId>
  <version>x.x.x</version>
</dependency>
```

마이바티스를 스프링과 함께 사용하려면 스프링의 애플리케이션 컨텍스트에 적어도 두개를 정의해줄 필요가 있다. 두가지는 `SqlSessionFactory`와 한개 이상의 매퍼 인터페이스이다.

마이바티스 스프링 연동모듈에서, `SqlSessionFactoryBean`은 `SqlSessionFactory`를 만들기 위해 사용된다. 

```xml
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
  <property name="dataSource" ref="dataSource" />
</bean>
```

`SqlSessionFactory`는 `DataSource`를 필요로 하는 것을 알아둘 필요가 있다. 어떤 `DataSource`도 상관없지만 다른 스프링의 데이터베이스 연결과 동일하게 설정되어야 한다.

매퍼 인터페이스가 다음처럼 정의되었다고 가정해보자.

```java
public interface UserMapper {
  @Select("SELECT * FROM users WHERE id = #{userId}")
  User getUser(@Param("userId") String userId);
} 
```

UserMapper인터페이스는 다음처럼 `MapperFactoryBean`을 사용해서 스프링에 추가된다.

```xml
<bean id="userMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">
  <property name="mapperInterface" value="org.mybatis.spring.sample.mapper.UserMapper" />
  <property name="sqlSessionFactory" ref="sqlSessionFactory" />
</bean>
```

매퍼는 **반드시** 구현체 클래스가 아닌 인터페이스로 정의되어야 한다. 예를들어, 애노테이션이 SQL을 명시하기 위해 사용되지만 마이바티스 매퍼 XML파일 또한 사용될 수 있다.

한번만 설정하면, 다른 스프링 빈에 주입하는 같은 방법으로 비즈니스/서비스 객체에 매퍼를 직접 주입할 수 있다. `MapperFactoryBean`은 `SqlSession`을 생성하고 닫는 작업을 잘 다룬다. 실행중인 스프링 트랜잭션이 있다면, 트랜잭션이 완료되는 시점에 커밋이나 롤백이 될 것이다. 마지막으로 예외가 발생하면 스프링의 `DataAccessException`예외가 발생한다.



## SqlSessionFactoryBean

마이바티스만 사용하면, `SqlSessionFactory`는 `SqlSessionFactoryBuilder`를 사용해서 생성한다. 마이바티스 스프링 연동모듈에서는, `SqlSessionFactoryBean`가 대신 사용된다.

팩토리 빈을 생성하기 위해, 스프링 XML설정파일에 다음설정을 추가하자.

```xml
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
  <property name="dataSource" ref="dataSource" />
</bean>
```

`SqlSessionFactoryBean` 은 스프링의 `FactoryBean` 인터페이스를 구현(스프링 문서의 3.8절을 보자)한다는 점을 알아야 한다. 이 설정은 스프링이 `SqlSessionFactoryBean` 자체를 생성하는 것이 **아니라** 팩토리에서 `getObject()` 메서드를 호출한 결과를 리턴한다는 것을 의미한다. 이 경우, 스프링은 애플리케이션 시작 시점에 `SqlSessionFactory`를 빌드하고 `sqlSessionFactory` 라는 이름으로 저장한다. 자바에서 코드로 표현하면 아래와 같다.

```java
SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
SqlSessionFactory sessionFactory = factoryBean.getObject();
```

일반적인 마이바티스 스프링 사용법에서는, `SqlSessionFactoryBean`이나 관련된 `SqlSessionFactory`를 직접 사용할 필요가 없다. 대신 세션 팩토리가 `MapperFactoryBean`나 `SqlSessionDaoSupport`를 확장하는 다른 DAO에 주입될것이다.



### Properties

`SqlSessionFactory`는 JDBC `DataSource`의 필수 프로퍼티가 필요하다. 어떤 `DataSource`라도 상관없고 다른 스프링 데이터베이스 연결처럼 설정되어야만 한다.

하나의 공통적인 프로퍼티는 마이바티스 XML설정파일의 위치를 지정하기 위해 사용되는 `configLocation`이다. 이 프로퍼티를 설정하는 것은 디폴트 설정을 가진 마이바티스 설정을 변경해야 할 경우 뿐이다. 대개는 `<settings>`과 `<typeAliases>` 섹션을 변경하는 경우이다.

설정파일이 마이바티스 설정을 완전히 다룰 필요는 없다. 어떤 환경, 어떤 데이터소스 그리고 마이바티스 트랜잭션 관리자가 **무시**될수도 있다. 

설정파일이 필요한 다른 이유는 마이바티스 XML파일이 매퍼 클래스와 동일한 클래스패스에 있지 않은 경우이다. 이 설정을 사용하면 두가지 옵션이 있다. 첫번째는 마이바티스 설정파일에 `<mappers>` 섹션을 사용해서 XML파일의 클래스패스를 지정하는 것이다. 두번째는 팩토리 빈의 `mapperLocations` 프로퍼티를 사용하는 것이다.

`mapperLocations` 프로퍼티는 매퍼에 관련된 자원의 위치를 나열한다. 이 프로퍼티는 마이바티스의 XML매퍼 파일들의 위치를 지정하기 위해 사용될 수 있다. 디렉터리 아래 모든 파일을 로드하기 위해 앤트(Ant) 스타일의 패턴을 사용할수도 있고 가장 상위 위치를 지정하는 것으로 재귀적으로 하위 경로를 찾도록 할수도 있다. 예를 들어보면 다음과 같다.

```xml
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
  <property name="dataSource" ref="dataSource" />
  <property name="mapperLocations" value="classpath*:sample/config/mappers/**/*.xml" />
</bean>
```



## Transactions

마이바티스 스프링 연동모듈을 사용하는 중요한 이유중 하나는 마이바티스가 스프링 트랜잭션에 자연스럽게 연동될수 있다는 것이다. 마이바티스에 종속되는 새로운 트랜잭션 관리를 만드는 것보다는 마이바티스 스프링 연동모듈이 스프링의 `DataSourceTransactionManager`과 융합되는 것이 좋다.

스프링 트랜잭션 관리자를 한번 설정하면, 대개의 경우처럼 스프링에서 트랜잭션을 설정할 수 있다. `@Transactional` 애노테이션과 AOP스타일의 설정 모두 지원한다. 하나의 `SqlSession`객체가 생성되고 트랜잭션이 동작하는 동안 지속적으로 사용될것이다. 세션은 트랜잭션이 완료되면 적절히 커밋이 되거나 롤백될것이다.

마이바티스 스프링 연동모듈은 한번 셋업되면 트랜잭션을 투명하게 관리한다. DAO클래스에 어떠한 추가적인 코드를 넣을 필요가 없다.



스프링 트랜잭션을 가능하게 하려면, 스프링 XML설정파일에 `DataSourceTransactionManager`를 생성하자.

```xml
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
  <property name="dataSource" ref="dataSource" />
</bean>
```

명시된 `DataSource`는 스프링을 사용할때 일반적으로 사용한다면 어떠한 JDBC `DataSource`도 될수 있다. JNDI룩업을 통해 얻어진 `DataSource`뿐 아니라 커넥션 풀링 기능도 포함한다.

트랜잭션 관리자에 명시된 `DataSource`가 `SqlSessionFactoryBean`을 생성할때 사용된 것과 **반드시** 동일한 것이어야 하는 것만 꼭 기억하자. 그렇지 않으면 트랜잭션 관리가 제대로 되지 않을것이다.



마이바티스 `SqlSession`은 트랜잭션을 제어하는 메서드를 제공한다. 하지만 마이바티스 스프링 연동모듈은 빈을 스프링이 관리하는 `SqlSession`이나 스프링이 관리하는 매퍼에 주입한다. 이 말은 스프링이 **항상** 트랜잭션을 관리한다는 뜻이다.

스프링이 관리하는 `SqlSession`에서는 `SqlSession.commit()`, `SqlSession.rollback()` 또는 `SqlSession.close()` 메서드를 호출할수가 없다. 그럼에도 불구하고 이 메서드들을 사용하면 `UnsupportedOperationException` 예외가 발생한다. 이러한 메서드는 주입된 매퍼 클래스에서는 사용할 수 없다.

JDBC연결의 자동커밋 설정을 어떻게 하더라도 스프링 트랜잭션 밖의 `SqlSession` 데이터 메서드나 매퍼 메서드의 실행은 자동으로 커밋된다.

트래잭션을 수동으로 제어하고자 한다면 스프링 레퍼런스의 10.6절을 참고하자. 다음의 코드는 스프링 레퍼런스 10.6.2절에서 언급된 내용으로 `PlatformTransactionManager`를 사용해서 수동으로 트랜잭션을 다루는 방법을 보여준다.



## SqlSession

마이바티스에서는 `SqlSession`를 생성하기 위해 `SqlSessionFactory`를 사용한다. 세션을 한번 생성하면 매핑구문을 실행하거나 커밋 또는 롤백을 하기 위해 세션을 사용할수 있다. 마지막으로 더 이상 필요하지 않은 상태가 되면 세션을 닫는다. 마이바티스 스프링 연동모듈을 사용하면 `SqlSessionFactory`를 직접 사용할 필요가 없다. 왜냐하면, 스프링 트랜잭션 설정에 따라 자동으로 커밋 혹은 롤백을 수행하고 닫혀지는, 쓰레드에 안전한 `SqlSession` 개체가 스프링 빈에 주입될 수 있기 때문이다.



### SqlSessionTemplate

`SqlSessionTemplate`은 마이바티스 스프링 연동모듈의 핵심이다. `SqlSessionTemplate`은 `SqlSession`을 구현하고 코드에서 `SqlSession`를 대체하는 역할을 한다. `SqlSessionTemplate`은 쓰레드에 안전하고 여러개의 DAO나 매퍼에서 공유할수 있다.

`getMapper()`에 의해 리턴된 매퍼가 가진 메서드를 포함해서 SQL을 처리하는 마이바티스 메서드를 호출할때 `SqlSessionTemplate`은 `SqlSession`이 현재의 스프링 트랜잭션에서 사용될수 있도록 보장한다. 추가적으로 `SqlSessionTemplate`은 필요한 시점에 세션을 닫고, 커밋하거나 롤백하는 것을 포함한 세션의 생명주기를 관리한다. 또한 마이바티스 예외를 스프링의 `DataAccessException`로 변환하는 작업또한 처리한다.

`SqlSessionTemplate`은 마이바티스의 디폴트 구현체인 `DefaultSqlSession` 대신 **항상** 사용된다. 왜냐하면 템플릿은 스프링 트랜잭션의 일부처럼 사용될 수 있고 여러개 주입된 매퍼 클래스에 의해 사용되도록 쓰레드에 안전하다. 동일한 애플리케이션에서 두개의 클래스간의 전환은 데이터 무결성 이슈를 야기할수 있다.

`SqlSessionTemplate`은 생성자 인자로 `SqlSessionFactory`를 사용해서 생성될 수 있다.

```xml
<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
  <constructor-arg index="0" ref="sqlSessionFactory" />
</bean>
```



### SqlSessionDaoSupport

`SqlSessionDaoSupport`는 `SqlSession`을 제공하는 추상클래스이다. `getSqlSession()`메서드를 호출해서 다음처럼 SQL을 처리하는 마이바티스 메서드를 호출하기 위해 사용할 `SqlSessionTemplate`을 얻을 수 있다.

```java
public class UserDaoImpl extends SqlSessionDaoSupport implements UserDao {
  public User getUser(String userId) {
    return (User) getSqlSession().selectOne("org.mybatis.spring.sample.mapper.UserMapper.getUser", userId);
  }
}
```

대개 `MapperFactoryBean`은 추가적인 코드가 필요없기 때문에 이 클래스를 선호한다. 하지만 DAO에서 마이바티스가 필요하지 않고 구현된 클래스가 필요하지 않을때만 유용하다.

`SqlSessionDaoSupport`는 `sqlSessionFactory` 와 `sqlSessionTemplate` 프로퍼티를 셋팅할 필요가 있다. 두개의 프로퍼티를 모두 셋팅하면 `sqlSessionFactory`는 무시된다.

`SqlSessionDaoSupport`의 하위클래스인 `UserDaoImpl`가 있다고 하면 스프링에서는 다음처럼 설정될 수 있다.

```xml
<bean id="userMapper" class="org.mybatis.spring.sample.mapper.UserDaoImpl">
  <property name="sqlSessionFactory" ref="sqlSessionFactory" />
</bean>
```



## Mappers

데이터 접근 객체인 DAO를 만든것보다 직접 `SqlSessionDaoSupport` 나 `SqlSessionTemplate` 를 사용하자. 마이바티스 스프링 연동모듈은 다른 빈에 직접 주입할 수 있는 쓰레드에 안전한 매퍼를 생성할 수 있다.

```xml
<bean id="fooService" class="org.mybatis.spring.sample.mapper.FooServiceImpl">
  <property name="userMapper" ref="userMapper" />
</bean>
```

한번 주입하고나면 매퍼는 애플리케이션 로직에서 사용할수 있는 준비가 된다.

```java
public class FooServiceImpl implements FooService {

  private UserMapper userMapper;

  public void setUserMapper(UserMapper userMapper) {
    this.userMapper = userMapper;
  }

  public User doSomeBusinessStuff(String userId) {
    return this.userMapper.getUser(userId);
  }
}
```

이 코드를 보면 `SqlSession`이나 마이바티스 객체가 보이지 않는다. 게다가 세션을 생성하거나 열고 닫을필요도 없어보인다. 마이바티스 스프링 연동모듈이 알아서 처리할 것이다.



### mapper등록

매퍼를 등록하는 방법은 기존의 전통적인 XML설정법을 사용하거나 새로운 3.0 이후의 자바설정(일명 @Configuration)을 사용하느냐에 따라 다르다.

#### XML설정 사용

매퍼는 다음처럼 XML설정파일에 `MapperFactoryBean`을 두는 것으로 스프링에 등록된다.

```xml
<bean id="userMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">
  <property name="mapperInterface" value="org.mybatis.spring.sample.mapper.UserMapper" />
  <property name="sqlSessionFactory" ref="sqlSessionFactory" />
</bean>
```

UserMapper가 매퍼 인터페이스와 같은 경로의 클래스패스에 마이바티스 XML매퍼 파일을 가지고 있다면 `MapperFactoryBean`이 자동으로 파싱할것이다. 매퍼 XML파일을 다른 클래스패스에 두는게 아니라면 마이바티스 설정파일에 매퍼를 지정할 필요가 없다. 좀더 세부적인 정보는 `SqlSessionFactoryBean`의 `configLocation` 프로퍼티를 살펴보자.

`MapperFactoryBean`은 `SqlSessionFactory` 나 `SqlSessionTemplate`가 필요하다. `sqlSessionFactory` 와 `sqlSessionTemplate` 프로퍼티를 셋팅하면 된다. 둘다 셋팅하면 `SqlSessionFactory`가 무시된다. 세션 팩토리 셋은 `SqlSessionTemplate`이 필요하고 `MapperFactoryBean`는 팩토리를 사용할것이다.

#### 자바설정 사용

스프링의 Java Config기능을 사용하면 다음처럼 `SqlSessionTemplate` 에서 직접 매퍼를 얻을 수 있다.

```java
    @Bean
    public SqlSessionFactory sqlSessionFactory() throws Exception {
      SqlSessionFactoryBean sqlSessionFactory = new SqlSessionFactoryBean();
      sqlSessionFactory.setDataSource(dataSource());
      return (SqlSessionFactory) sqlSessionFactory.getObject();
    }

    @Bean
    public UserMapper userMapper() throws Exception {
      SqlSessionTemplate sessionTemplate = new SqlSessionTemplate(sqlSessionFactory());
      return sessionTemplate.getMapper(UserMapper.class);
    }
```

마이바티스의 디폴트 `SqlSession`에서 매퍼를 리턴받을수 없다. 디폴트 `SqlSession`은 쓰레드에 안전하지 않고 생성된 `SqlSession`이 닫힐때까지만 살아있기 때문이다. 대신 샘플코드에서 보여주는 것처럼 `SqlSessionTemplate` 를 사용해야만 한다.



### mappers 스캔

하나씩 매퍼를 모두 등록할 필요가 없다. 대신 클래스패스를 지정해서 마이바티스 스프링 연동모듈의 자동스캔기능을 사용할 수 있다.

자동스캔을 사용하는데는 3가지 방법이 있다.

- `<mybatis:scan/>` 엘리먼트 사용
- `@MapperScan` 애노테이션 사용
- 스프링 XML파일을 사용해서 `MapperScannerConfigurer`를 등록

`<mybatis:scan/>` 와 `@MapperScan` 모두 마이바티스 스프링 연동모듈 1.2.0에서 추가된 기능이다. `@MapperScan` 은 스프링 버전이 3.1이상이어야 한다.

`<mybatis:scan/>` XML엘리먼트는 스프링에서 제공하는 `<context:component-scan/>` 엘리먼트와 매우 유사한 방법으로 매퍼를 검색할 것이다.

샘플 XML설정을 아래에서 볼수 있다.

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:mybatis="http://mybatis.org/schema/mybatis-spring"
  xsi:schemaLocation="
  http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
  http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring.xsd">
 
  <mybatis:scan base-package="org.mybatis.spring.sample.mapper" />

</beans>
```

`base-package` 속성은 매퍼 인터페이스 파일이 있는 가장 상위 패키지를 지정하면 된다. 세미콜론이나 콤마를 구분자로 사용해서 한개 이상의 패키지를 셋팅할 수 있다. 매퍼는 지정된 패키지에서 재귀적으로 하위 패키지를 모두 검색할 것이다.

`<mybatis:scan/>`이 자동으로 주입할 수 있는 `MapperFactoryBean`를 생성하기 때문에 `SqlSessionFactory` 나 `SqlSessionTemplate` 를 명시할 필요가 없다. 하지만 한개 이상의 `DataSource`를 사용한다면 자동주입이 생각한데로 동작하지 않을수도 있다. 이 경우 사용할 빈 이름을 지정하기 위해 `factory-ref` 나 `template-ref` 속성을 사용할수 있다.

`<mybatis:scan/>`은 마커(marker) 인터페이스나 애노테이션을 명시해서 생성되는 매퍼를 필터링할수도 있다. `annotation` 프로퍼티는 검색할 애노테이션을 지정한다. `marker-interface` 프로퍼티는 검색할 상위 인터페이스를 지정한다. 이 두개의 프로퍼티를 모두 지정하면, 매퍼는 두 조건을 모두 만족하는 인터페이스만을 추가한다. 디폴트로 이 두가지 프로퍼티는 모두 null이다. 그래서 base-package프로퍼티에 설정된 패키지 아래 모든 인터페이스가 매퍼로 로드될 것이다.

발견된 매퍼는 자동검색된 컴포넌트를 위한 스프링의 디폴트 명명규칙 전략(스프링 메뉴얼의 3.14.4를 보라)을 사용해서 빈이름이 명명된다. 빈 이름을 정하는 애노테이션이 없다면 매퍼의 이름에서 첫글자를 소문자로 변환한 형태로 빈 이름을 사용할 것이다. @Component 나 JSR-330의 @Named 애노테이션이 있다면 애노테이션에 정의한 이름을 그대로 사용할 것이다. `annotation` 프로퍼티를 `org.springframework.stereotype.Component`, `javax.inject.Named`(자바SE 1.6을 사용한다면) 또는 개발자가 스스로 작성한 애노테이션으로 셋팅할 수 있다. 그러면 애노테이션은 마커와 이름을 제공하는 역할로 동작할 것이다.

중요 `<context:component-scan/>` 가 매퍼를 검색해서 등록을 하지 못할수도 있다. 매퍼는 인터페이스고 스프링에 빈으로 등록하기 위해서는 각각의 인터페이스를 찾기 위해 스캐너가 `MapperFactoryBean` 를 생성하는 방법을 알아야만 한다.

#### @MapperScan

@Configuration 라고 불리는 스프링의 자바설정을 사용한다면 `<mybatis:scan/>`보다는 `@MapperScan`를 사용하길 선호할것이다.

`@MapperScan` 애노테이션은 다음처럼 사용된다.

```java
@Configuration
@MapperScan("org.mybatis.spring.sample.mapper")
public class AppConfig {

  @Bean
  public DataSource dataSource() {
    return new EmbeddedDatabaseBuilder().addScript("schema.sql").build()
  }

  @Bean
  public SqlSessionFactory sqlSessionFactory() throws Exception {
    SqlSessionFactoryBean sessionFactory = new SqlSessionFactoryBean();
    sessionFactory.setDataSource(dataSource());
    return sessionFactory.getObject();
  }
}
```

애노테이션은 앞서 본 `<mybatis:scan/>` 에서 설명하는 것과 동일하게 동작한다. `markerInterface` 와 `annotationClass` 프로퍼티를 사용해서 마커 인터페이스와 애노테이션 클래스를 명시하게 한다. `sqlSessionFactory` 와 `sqlSessionTemplate` 프로퍼티를 사용해서 `SqlSessionFactory` 나 `SqlSessionTemplate`을 제공할 수도 있다.



#### MapperScannerConfigurer

`MapperScannerConfigurer`는 평범한 빈처럼 XML애플리케이션 컨텍스트에 포함된 `BeanDefinitionRegistryPostProcessor` 이다. `MapperScannerConfigurer`를 셋업하기 위해 다음의 스프링 설정을 추가하자.

```xml
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
  <property name="basePackage" value="org.mybatis.spring.sample.mapper" />
</bean>
```

`sqlSessionFactory` 나 `sqlSessionTemplate`를 지정할 필요가 있다면 빈참조가 아닌 **빈이름**이 필요하다. `value` 프로퍼티는 빈 이름을 지정하고 `ref` 는 빈 참조를 지정하기 때문에 `value` 프로퍼티를 사용하자.

```
<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
```

중요 `sqlSessionFactoryBean` 과 `sqlSessionTemplateBean` 프로퍼티는 마이바티스 스프링 연동모듈 1.0.2 버전 이상에서만 사용이 가능하다. 하지만 `MapperScannerConfigurer`는 잦은 에러를 발생시키는 `PropertyPlaceholderConfigurer`보다 앞서 실행되기 때문에 이 프로퍼티들은 사용하지 말길 바란다(deprecated). 대신 새롭게 추가된 프로퍼티인 `sqlSessionFactoryBeanName` 과 `sqlSessionTemplateBeanName` 를 사용하도록 권한다.



## 참고

- http://www.mybatis.org/spring/ko/getting-started.html