# Spring Framework의 3대 핵심 기술

> Spring 3대 핵심 기술에 대해서 알아본다.

## IoC/DI

### IoC(Inversion of Control)

IoC는 의존성 주입 (DI)이라고도합니다. 객체 인스턴스에 설정된 특성을 통해서만 객체가 종속성을 정의하는 프로세스입니다. 그런 다음 컨테이너는 Bean을 작성할 때 이러한 종송석을 주입합니다. 이 프로세스는 근본적으로 클래스의 직접적인 생성이나 서비스로 케이터 패턴과 같은 메커니즘을 사용하여 종속성의 인스턴스 또는 위치를 제어하는 빈 자체의 역(반전 제어의 이름)입니다.

`org.springframework.beans`및 `org.springframework.context`패키지는 Spring 프레임 워크의 IoC 컨테이너의 기초입니다.[`BeanFactory`](https://docs.spring.io/spring-framework/docs/5.1.3.RELEASE/javadoc-api/org/springframework/beans/factory/BeanFactory.html) 인터페이스는 모든 유형의 개체를 관리 할 수있는 고급 구성 메커니즘을 제공합니다. [`ApplicationContext`](https://docs.spring.io/spring-framework/docs/5.1.3.RELEASE/javadoc-api/org/springframework/context/ApplicationContext.html) 의 하위 인터페이스입니다 `BeanFactory`는 아래의 항목을 추가합니다.

- Spring의 AOP 기능과의보다 쉬운 통합
- 메시지 리소스 처리 (국제화에 사용)
- Event publication
- `WebApplicationContext` 웹 응용 프로그램에서 사용 하기 위한 것과 같은 응용 프로그램 계층 별 컨텍스트 

즉, `BeanFactory`구성 프레임 워크 및 기본 기능을 제공하고 `ApplicationContext`엔터프라이즈 별 기능을 추가합니다. `ApplicationContext`는 `BeanFactory` Spring의 IoC 컨테이너에 대한 설명에서 독점적으로 사용되는 완전한 슈퍼셋입니다 .

Spring에서는 애플리케이션의 백본을 형성하고 Spring IoC 컨테이너에 의해 관리되는 객체를 Bean이라고 부른다. Bean은 Spring IoC 컨테이너에 의해 인스턴스화, 어셈블링 및 관리되는 객체이다. 그렇지 않으면 빈은 단순히 응용 프로그램의 많은 객체 중 하나입니다. 빈들과 그 사이의 의존성은 컨테이너가 사용하는 컨피규레이션 메타 데이터에 반영됩니다.



### DI(Dependency Injection)

DI (Dependency Injection)는 생성자 인수, 팩토리 메소드에 대한 인수 또는 구성 후 객체 인스턴스에 설정된 등록 정보를 통해서만 객체가 종속성 (즉, 작동하는 다른 객체)을 정의하는 프로세스입니다. 팩토리 메서드에서 반환됩니다. 그런 다음 컨테이너는 Bean을 작성할 때 이러한 종속성을 주입합니다. 이 프로세스는 기본적으로 클래스 또는 서비스 로케이터 패턴의 직접 작성을 사용하여 자체 의존성의 인스턴스화 또는 위치를 제어하는 빈 자체의 역 (따라서 Inversion of Control)입니다.

코드는 DI 원칙을 사용하여보다 명확하며 객체가 종속성을 제공받을 때 디커플링이 더 효과적입니다. 개체는 종속성을 찾지 않으며 종속성의 위치와 클래스를 알지 못합니다. 따라서 종속성이 인터페이스 또는 추상 기본 클래스에있을 때 클래스 테스트가 쉬워 져 단위 테스트에서 스텁 또는 모의 구현을 사용할 수 있습니다.

DI는 크게 두 가지로 나뉩니다. [생성자 기반 종속성 주입](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-constructor-injection) 과 [Setter 기반 종속성 주입](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-setter-injection) 입니다.

#### 생성자 기반 종속성 삽입

생성자 기반 DI는 컨테이너가 여러 종속 변수를 나타내는 여러 개의 인수로 생성자를 호출함으로써 수행됩니다. 정적 Bean을 생성하기위한 특정 인수를 가진 팩토리 메소드를 호출하는 것은 거의 동일하며, 생성자와 정적 팩토리 메소드 에 대한 인수를 유사하게 취급합니다. 다음 예제는 생성자 삽입으로만 종속성 주입 될 수있는 클래스를 보여줍니다.

```java
public class SimpleMovieLister {

    // the SimpleMovieLister has a dependency on a MovieFinder
    private MovieFinder movieFinder;

    // a constructor so that the Spring container can inject a MovieFinder
    public SimpleMovieLister(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // business logic that actually uses the injected MovieFinder is omitted...
}
```

이 클래스에는 특별한 것이 없다는 것을 주목하세요. 이것은 컨테이너 관련 인터페이스, 기본 클래스 또는 주석에 대한 의존성이없는 POJO입니다.

##### 생성자 인수 해결

생성자 인수 확인 일치는 인수의 유형을 사용하여 발생합니다. bean 정의의 생성자 인수에 잠재적 인 모호성이 없으면 bean 정의에서 생성자 인수가 정의되는 순서는 bean이 인스턴스화 될 때 해당 인수가 적절한 생성자에 제공되는 순서입니다. 다음 클래스를 고려하십시오.

```java
package x.y;

public class ThingOne {

    public ThingOne(ThingTwo thingTwo, ThingThree thingThree) {
        // ...
    }
}
```

 `ThingTwo`및 `ThingThree`클래스가 상속으로 관련이없는, 잠재적 모호성은 존재하지 않는다. 따라서 다음 구성이 올바르게 작동하므로 구성 `<constructor-arg/>` 요소 인수 또는 인덱스를 명시 적으로 요소 에 지정할 필요가 없습니다 .

```xml
<beans>
    <bean id="thingOne" class="x.y.ThingOne">
        <constructor-arg ref="thingTwo"/>
        <constructor-arg ref="thingThree"/>
    </bean>

    <bean id="thingTwo" class="x.y.ThingTwo"/>

    <bean id="thingThree" class="x.y.ThingThree"/>
</beans>
```

다른 bean이 참조 될 때, 그 타입은 알려지고 일치가 발생할 수있다 (앞의 예제에서와 같이). `<value>true</value>`Spring이 값의 타입을 결정할 수 없기 때문에 간단한 타입이 사용될 때, 도움없이 타입별로 일치시킬 수 없습니다. 다음 클래스를 고려하세요.

```java
package examples;

public class ExampleBean {

    // Number of years to calculate the Ultimate Answer
    private int years;

    // The Answer to Life, the Universe, and Everything
    private String ultimateAnswer;

    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }
}
```

##### 생성자 인수 유형 일치

앞의 시나리오에서 컨테이너는 `type`속성 을 사용하여 생성자 인수의 유형을 명시 적으로 지정하면 단순 유형과 일치하는 유형을 사용할 수 있습니다 . 다음 예제와 같이

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg type="int" value="7500000"/>
    <constructor-arg type="java.lang.String" value="42"/>
</bean>
```

##### 생성자 인수 인덱스

다음 예제와 같이 속성을 사용하여 생성자 인수의 인덱스를 명시 적으로 지정할 수 있습니다 .

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg index="0" value="7500000"/>
    <constructor-arg index="1" value="42"/>
</bean>
```

여러 간단한 값의 모호성을 해결하는 것 외에도 인덱스를 지정하면 생성자가 동일한 유형의 인수가 두 개인 모호성이 해결됩니다.

> 인덱스는 0부터 시작합니다.

##### 생성자 인수 이름

다음 예제와 같이 값 모호성 제거에 생성자 매개 변수 이름을 사용할 수도 있습니다.

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg name="years" value="7500000"/>
    <constructor-arg name="ultimateAnswer" value="42"/>
</bean>
```

이 작업을 수행하려면 Spring에서 생성자의 매개 변수 이름을 찾을 수 있도록 디버그 플래그를 사용하도록 코드를 컴파일해야합니다. 디버그 플래그로 코드를 컴파일 할 수 없거나 컴파일하지 않으려면 [@ConstructorProperties](https://download.oracle.com/javase/6/docs/api/java/beans/ConstructorProperties.html) JDK 어노테이션을 사용 하여 생성자 인수의 이름을 명시 적으로 지정할 수 있습니다 . 샘플 클래스는 다음과 같이 보일 것입니다.

```java
package examples;

public class ExampleBean {

    // Fields omitted

    @ConstructorProperties({"years", "ultimateAnswer"})
    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }
}
```

##### 세터 기반 의존성 주입

Setter 기반 DI는 인수가없는 생성자 또는 빈을 인스턴스화하기위한 인수가없는 정적 팩토리 메소드를 호출 한 후 bean에서 setter 메소드를 호출하는 컨테이너에 의해 수행됩니다.

다음 예제에서는 순수한 setter 주입을 사용하여 종속성 주입 만 할 수있는 클래스를 보여줍니다. 이 클래스는 일반적인 Java입니다. 컨테이너 고유의 인터페이스, 기본 클래스 또는 주석에 대한 종속성이없는 POJO입니다.

```java
public class SimpleMovieLister {

    // the SimpleMovieLister has a dependency on the MovieFinder
    private MovieFinder movieFinder;

    // a setter method so that the Spring container can inject a MovieFinder
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // business logic that actually uses the injected MovieFinder is omitted...
}
```

`ApplicationContext`는 자신이 관리하는 빈에 대한 생성자 기반 및 설정자 기반 DI를 지원합니다. 또한 생성자 방식을 통해 이미 일부 종속성이 주입 된 후에도 setter 기반 DI를 지원합니다. `PropertyEditor` 인스턴스와 함께 사용하여 하나의 형식에서 다른 형식으로 속성을 변환하는 `BeanDefinition` 형식으로 종속성을 구성합니다. 그러나 대부분의 Spring 사용자는 이러한 클래스를 직접 (즉 프로그래밍 방식으로) 작동하지 않고 `XML bean` 정의, 주석이 달린 구성 요소 (즉, `@Component`, `@Controller` 등으로 주석 된 클래스) 또는 `@Bean` 메소드를 사용하여 Java 기반 `@Configuration` 클래스. 이러한 소스는 내부적으로 `BeanDefinition`의 인스턴스로 변환되어 전체 Spring IoC 컨테이너 인스턴스를로드하는 데 사용된다.



## AOP





## PSA





## 참고

- [Spring Framework Documentation(Version 5.1.3.RELEASE)](https://docs.spring.io/spring/docs/current/spring-framework-reference/)