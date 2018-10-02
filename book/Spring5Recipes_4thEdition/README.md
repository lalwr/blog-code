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

