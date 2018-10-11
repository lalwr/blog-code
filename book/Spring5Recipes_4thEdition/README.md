# 2장 스프링 코어

### @Scope를 붙여 POJO 스코프 지정하기

|    스코프     |                             설명                             |
| :-----------: | :----------------------------------------------------------: |
|  Singletion   |          Ioc 컨테이너당 빈 인스턴스 하나를 생성한다          |
|   prototype   |           요청할 때마다 빈 인스턴스를 새로 만든다            |
|    request    | HTTP 요청당 하나의 빈 인스턴스를 생성한다. 웹 애플리케이션 컨텍스트에만 해당 |
|    session    | HTTP 세션당 빈 인스턴스를 하나를 생성한다. 웹 애플리케이션 컨텍스트에만 해당 |
| globalSession | 전역 HTTP 세션당 빈 인스턴스 하나를 생성한다. 포털 애플리케이션 컨텍스트에만 해당 |

### 외부 리소스(텍스트, XML, 프로퍼티, 이미지 파일)

아래의 파일을 생성한다

```properties
#discounts.properties
specialcustomer.discount=0.1
summer.discount=0.15
endofyear.discount=0.2
```

```java
package com.apress.springrecipes.shop.config;

import com.apress.springrecipes.shop.Battery;
import com.apress.springrecipes.shop.Disc;
import com.apress.springrecipes.shop.Product;
import org.springframework.beans.factory.annotation.Value; 
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
import org.springframework.context.support.PropertySourcesPlaceholderConfigurer;

@Configuration 
@PropertySource("classpath:discounts.properties") @ComponentScan("com.apress.springrecipes.shop") 
public class ShopConfiguration {
    
    @Value("${endofyear.discount:0}")
	private double specialEndofyearDiscountField;
    
    @Bean
	public static PropertySourcesPlaceholderConfigurer
        propertySourcesPlaceholderConfigurer() {
		return new PropertySourcesPlaceholderConfigurer(); 
    }
   
    @Bean
    public Product dvdrw() {
        Disc p2 = new Disc("DVD-RW", 3.0, specialEndofyearDiscountField); 						p2.setCapacity(700);
        return p2;
    } 
    
}

```

- `@PropertySource` 어노테이션을 통해 properties 정보를 가지고 올수 있다
- @PropertySource 를 통해 프로퍼티 파일을 로드하려면 `PropertySourcesPlaceholderConfigurer` 를 @Bean으로 등록해야 한다
- `@Value`어노테이션을 통해 placeholder 표현식을 넣어 프로퍼티 값을 자바 변수에 할당할 수 있다. 기본값 할당도 가능

# 3장 스프링 MVC

## 유저 로케일 해석하기

- 스프링 MVC 애플리케이션에서 유저 로케일은 `LocaleResolver`  인터페이스를 구현한 로케일 리졸버가 식별
- 로케일을 해석하는 기준에 따라 여러 `LocaleResolver`  구현체가 스프링 MVC에 준비
- 직접 이 인터페이스를 구현해서 커스텀 로케일 리졸버를 만들어 사용 가능

로케일 리졸버는 웹 애플리케이션 컨텍스트에 `LocaleResolver`  형 빈으로 등록한다. `DispatcherServlet`이 자동 감지하려면 로케일 리졸버 빈을 `LocaleResolver`  라고 명명한다.

> 참고로 로케일 리졸버는 `DispatcherServlet`당 하나만 등록할수 있다.

스프링 mvc가 제공하는 다양한 `LocaleResolver`  를 살펴보고 인터셉터로 유저 로케일을 어떻게 변경하는지 알아보자

### HTTP 요청 헤더에 따라 로케일 해석하기

`AcceptHeaderLocaleResolver`는 스프링의 기본 로케일 리졸버로서 accept-language 요청 헤더값에 따라 로케일을 해석한다

### 세션 속성에 따라 로케일 해석하기

`SessionLocaleResolver`는 유저 세션에 사전 정의된 속성에 따라 로케일을 해석한다. 세션 속성이 없으면 accept-language 헤더로 기본 로케일을 결정한다

```java
@Bean
public LocaleResolver localeResolver () {
    SessionLocaleResolver localeResolver = new SessionLocaleResolver();
    localeResolver.setDefaultLocale(new Locale("en"));
    return localeResolver;
}
```

로케일 관련 세션 속성이 없을 경우 설정 가능

### 쿠키에 따라 로케일 해석하기

`CookieLocaleResolver`는 유저 브라우저의 쿠키값에 따라 로케일을 해석한다. 해당 쿠키가 없으면  accept-language 요청 헤더로 기본 로케일을 결정한다

```java
@Bean
public LocaleResolver localeResolver() {
    return new CookieLocaleResolver();
}
```

쿠키 설정을 커스터 마이징 할 수 있다

```java
@Bean
public LocaleResolver localeResolver() {
    CookieLocaleResolver cookieLocaleResolver = new CookieLocaleResolver();
    cookieLocaleResolver.setCookieName("language");
    cookieLocaleResolver.setCookieMaxAge(3600);
    cookieLocaleResolver.setDefaultLocale(new Locale("en"));
    return cookieLocaleResolver;
}
```

### 유저 로케일 변경하기

LocaleChangeInterceptor를 핸들러 매핑에 적용할 수 있다. 이 인터셉터의 주특기는 현재 HTTP 요청에 특정한 매개변수가 존재하는지 감지하는 일이다

```java
package com.apress.springrecipes.court.web.config;

import org.springframework.web.servlet.i18n.CookieLocaleResolver;
import org.springframework.web.servlet.i18n.LocaleChangeInterceptor;
import org.springframework.web.servlet.view.InternalResourceViewResolver;
import java.util.Locale;

// Other imports omitted
@Configuration
public class I18NConfiguration implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(measurementInterceptor());
        registry.addInterceptor(localeChangeInterceptor());
        registry .addInterceptor(summaryReportInterceptor())
        	.addPathPatterns("/reservationSummary*");
    }
    
    @Bean
    public LocaleChangeInterceptor localeChangeInterceptor() {
        LocaleChangeInterceptor localeChangeInterceptor = new LocaleChangeInterceptor();
        localeChangeInterceptor.setParamName("language");
        return localeChangeInterceptor;
    }
    
    public CookieLocaleResolver localeResolver() {
        CookieLocaleResolver cookieLocaleResolver = new CookieLocaleResolver();
        cookieLocaleResolver.setCookieName("language");
        cookieLocaleResolver.setCookieMaxAge(3600);
        cookieLocaleResolver.setDefaultLocale(new Locale("en"));
        return cookieLocaleResolver;
    }
    ...
}
```

URL의 language 매개변수를 이용해 유저 로케일을 바꿀 수 있다

- http://localhost:8080/court/welcome?language=en_US
- http://localhost:8080/court/welcome?language=de

## 이름으로 뷰 해석하기

- 핸들러가 요청 처리를 마치고 논리 뷰 이름을 반환하면 DispatcherServlet은 화면에서 테이터를 표시하도록 뷰 템플릿에 제어권을 넘긴다

- 스프링 MVC에서 뷰는 웹 애플리케이션 컨텍스트에 하나 이상 선언된 뷰  리졸버 빈을 해석
- 뷰 리졸버 빈은 DispatcherServlet이 자동 감지할 수 있도록 ViewResolver 인터페이스를 구현
- 스프링 MVC에는 다양한 전략에 맞게 뷰를 해석할 수 있는 ViewResolver 구현체가 몇 가지 있다

### 템플릿명과 위치에 따라 뷰 해석하기

```java
@Bean
public InternalResourceViewResolver viewResolver() {
    InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
    viewResolver.setPrefix("/WEB-INF/jsp/");
    viewResolver.setSuffix(".jsp");
    return viewResolver;
}
```

- `InternalResourceViewResolver`는 클래스패스에 JSTL 라이브러리가 있으면 기본적으로 JstlView형 뷰 객체로 해석하기 때문에 JSTL 태그가 포함된 JSP 템플릿 형태의 뷰는 View를 생략해도 된다
- `InternalResourceViewResolver`는 RequestDispatcher가 포워딩할 수 있는 내부적인 리소스 뷰(내부 JSP 파일 또는 서블릿)만 해석할 수 있다

### XML 구성 파일에 따라 뷰 해석하기

`XmlViewResolver`는 기본적으로 /WEB-INF/views.xml 파일에서 뷰 빈을 읽는다

```java
@Configuration
public class ViewResolverConfiguration implements WebMvcConfigurer, ResourceLoaderAware {
    private ResourceLoader resourceLoader;
    
    @Bean
    public ViewResolver viewResolver() {
        XmlViewResolver viewResolver = new XmlViewResolver();
        viewResolver.setLocation(resourceLoader.getResource("/WEB-INF/court-views.nl"));
        return viewResolver;
    }
    
    @Override
    public void setResourceLoader(ResourceLoader resourceLoader) {
   		this.resourceLoader=resourceLoader;
    }
}
```

- ResourceLoaderAware 인터페이스의 구현 클래스는 location 프로퍼티가 Resource형 인수를 받으므로 리소스를 로드해야 한다
- XML 구성 파일에서는 스프링이 String을 Resource로 자동 변환하지만 자바로 구성하면 몇가지 작업이 더 필요하다. 

court-view.xml 구성 파일에 클래스명과 프로퍼티를 설정하여 각 뷰를 일반 스프링 빈으로 선언

```jsp
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <bean id="welcome"
    class="org.springframework.web.servlet.view.JstlView">
    <property name="url" value="/WEB-INF/jsp/welcome.jsp" />
    </bean>
    
    <bean id="reservationQuery"
    class="org.springframework.web.servlet.view.JstlView">
    <property name="url" value="/WEB-INF/jsp/reservationQuery.jsp" />
    </bean>
    
    <bean id="welcomeRedirect"
    class="org.springframework.web.servlet.view.RedirectView">
    <property name="url" value="welcome" />
    </bean>
    
</beans>
```

### 리소스 번들에 따라 뷰 해석하기

- ResourceBundleViewResolver는 클래스패스 루트에 있는 리소스 번들에서 뷰 빈을 읽는다
- 리소스 번들의 장점을 활용해 로케일별로 리소스 번들을 따로따로 로드할 수 있다

```java
@Bean
public ResourceBundleViewResolver viewResolver() {
    ResourceBundleViewResolver viewResolver = new ResourceBundleViewResolver();
    viewResolver.setBasename("court-views");
    return viewResolver;
}
```

ResourceBundleViewResolver의 베이스 이름을 court-view라고 지정하면 리소스 번들은 court-views.properties 파일이 된다

```properties
welcome.(class)=org.springframework.web.servlet.view.JstlView
welcome.url=/WEB-INF/jsp/welcome.jsp
reservationQuery.(class)=org.springframework.web.servlet.view.JstlView
reservationQuery.url=/WEB-INF/jsp/reservationQuery.jsp
welcomeRedirect.(class)=org.springframework.web.servlet.view.RedirectView
welcomeRedirect.url=welcome
```

### 여러 리졸버를 이용해 뷰 해석하기

웹 애플리케이션 뷰가 여러 개면 한 가지 뷰 해석 전략만으로는 역부족하다

내부 JSP 또는 InternalResourceViewResolver가 해석한다 해도 ResourceBundleViewResolver로 해석해야 할 뷰도 있을수 있다

```java
@Bean
public ResourceBundleViewResolver viewResolver() {
    ResourceBundleViewResolver viewResolver = new ResourceBundleViewResolver();
    viewResolver.setOrder(0);
    viewResolver.setBasename("court-views");
    return viewResolver;
}

@Bean
public InternalResourceViewResolver internalResourceViewResolver() {
    InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
    viewResolver.setOrder(1);
    viewResolver.setPrefix("/WEB-INF/jsp/");
    viewResolver.setSuffix(".jsp");
    return viewResolver;
}
```

- 둘 이상의 전략이 등장할 때에는 적용 순서가 중요하다
- 뷰 리졸버 빈의 order 프로퍼티값으로 우선순위를 지정한다(낮을수록 우선순위가 높다)
- 뷰의 존재 여부와 상관없이 `InternalResourceViewResolver`는 항상 뷰를 해석하므로 우선순위를 가장 낮게 할당해야 한다

이제 InternalResourceViewResolver로 해석할 수 없는 뷰만 court-vies.properties 리소스 번들 파일에 기재하면 된다

```properties
welcomeRedirect.(class)=org.springframework.web.servlet.view.RedirectView
welcomeRedirect.url=welcome
```

### 리다이렉트 접두어 붙이기

InternalResourceViewResolver가 웹 애플리케이션 컨텍스트에 구성되어 있을 경우 뷰 이름이 `redirect:` 접두어를 붙이면 리다이렉트 뷰로 해석할 수 있다.

# 4장 스프링 REST

### 고려사항

- 애플리케이션 데이터 자체를 REST 서비스로 발행
- 애플리케이션에서 쓸 데이터를 서드파티 REST서비스에서 가져오기

### XML 기반의 REST 서비스 발행

#### MarshallingVIew로 XML만들기

- 먀살링
  - 메모리에 있는 객체를 특정한 데이터 형식으로 변환하는 과정
- 스프링 MVC에서는 콘텐트 협상을 통해 View를 결정
- 같은 데이터를 다양한 표현형으로 나타낼려면 뷰를 계속 추가해야 한다



# 10장 스프링 트랜잭션 관리

- **원자성(Atomicity)** : 트랜잭션은 연속적인 액션들로 이루어진 원자성 작업이다. 트랜잭션의 액션은 전부 다 수행되거나 아무것도 수행되지 않도록 보장한다.
- **일관성(Consistency)** : 트랜잭션의 액션이 모두 오나료되면 커밋되고 데이터 및 리소스는 비즈니스 규칙에 맞게 일관된 상태를 유지한다.
- **격리성(Isolation)** : 동일한 데이터를 여러 트랜잭션이 동시에 처리할 경우 데이터가 변질되지 않게 하려면 각각의 트랜잭션을 격리해야 한다.
- **지속성(Durability)** : 트랜잭션 완료 후 그 결과는 설령 시스템이 실패 하더라도 살아남아야 한다. 보통 트랜잭션 결과물은 퍼시스턴스 저장소의 씌어진다.

`PlatformTransactionManager`는 기술 독립적인 트랜잭션 관리 메서드를 캡슐화한, 스프링 트랜잭션 관리 추상화의 핵심 인터페이스다. 스프링에서 어떤 트랜잭션 고나리 전략을 구사하는지는 별로 중요하지 않다. 스프링에는 여러가지 트랜잭션 관리 API에 적용 가능한,  이 인터페이스의 구현체가 이미 탑재되어 있다.

스프링 JDBC 템플릿에서 발생항 예외는 모두 `DataAccessException` 하위형이므로 이런 종류의 예외가 나면 트랜잭션 관리자가 트랜잭션을 롤백하도록 설정해야한다.





