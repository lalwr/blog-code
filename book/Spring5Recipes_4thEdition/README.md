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

# 유저 로케일 해석하기

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

