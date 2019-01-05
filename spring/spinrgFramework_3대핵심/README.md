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



## AOP(Aspect-Oriented Programming)

Aspect-Oriented Programming (AOP)은 프로그램 구조에 대한 또 다른 사고 방식을 제공함으로써 객체 지향 프로그래밍 (OOP)을 보완합니다. OOP의 모듈성의 핵심 단위는 클래스입니다. 반면 AOP에서는 모듈화 단위가 측면입니다. 여러 측면에서는 여러 유형 및 객체를 포괄하는 관심 사항 (예 : 트랜잭션 관리)을 모듈화 할 수 있습니다. (이러한 우려는 AOP 문헌에서 "크로스 커팅 (crosscutting)"문제로 종종 불린다.)

Spring의 핵심 컴포넌트 중 하나는 AOP 프레임 워크이다. Spring IoC 컨테이너는 AOP에 의존하지 않지만 (원하지 않으면 AOP를 사용할 필요가 없다), AOP는 Spring IoC를 보완하여 매우 뛰어난 미들웨어 솔루션을 제공한다.

> Spring 2.0+ AOP
>
> Spring 2.0은 [스키마 기반 접근 방식]((https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#aop-schema))이나 [@AspectJ 주석 스타일](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#aop-ataspectj)을 사용하여 사용자 정의 측면을 작성하는보다 간단하고 강력한 방법을 소개했다. 이 두 스타일은 스프링 AOP를 사용하면서 AspectJ pointcut 언어를 완벽하게 유형화 된 조언과 사용법을 제공합니다.
>
> Spring 2.0+ 스키마와 @ AspectJ 기반 AOP 지원에 대해 설명한다.

AOP는 Spring Framework에서 다음과 같은 용도로 사용됩니다.

- 선언적 엔터프라이즈 서비스를 제공합니다. 특히 EJB 선언적 서비스를 대체합니다. 가장 중요한 이러한 서비스는 [선언적 트랜잭션 관리](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#transaction-declarative)입니다.
- 사용자가 AOP와 함께 OOP 사용을 보완하면서 사용자 정의 측면을 구현할 수 있게 합니다.



### AOP개념

- Aspect : 여러 등급에 걸쳐 영향을 미치는 모듈화입니다. 트랜잭션 관리는 엔터프라이즈 Java 애플리케이션에서 교차하는 우려의 좋은 예입니다. Spring AOP에서 aspect는 정규 클래스 ([스키마 기반 접근법]((https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#aop-schema))) 또는 `@Aspect` 주석으로 주석 된 일반 클래스 ([@AspectJ 스타일](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#aop-ataspectj))를 사용하여 구현된다.

- 조인 포인트 (Join point) : 메소드 실행이나 예외 처리와 같은 프로그램 실행 중 포인트. Spring AOP에서 조인 포인트는 항상 메소드 실행을 나타낸다.

- Advice : 특정 조인 포인트에서 한 측면에 의해 취해진 행동. 다양한 유형의 조언에는 "주변", "이전"및 "후"조언이 포함됩니다. (조언 유형은 나중에 논의됩니다.) Spring을 포함한 많은 AOP 프레임 워크는 인터셉터로 조언을 모델링하고 조인 포인트 주위에 인터셉터 체인을 유지합니다.

- Pointcut : 조인 포인트와 일치하는 술어. 조언은 pointcut 표현식과 관련이 있으며 pointcut (예 : 특정 이름의 메소드 실행)과 일치하는 모든 join point에서 실행됩니다. Pointcut 표현식과 일치하는 조인 포인트의 개념은 AOP의 핵심이며, Spring은 기본적으로 AspectJ pointcut 표현식 언어를 사용한다.

- 소개 : 유형을 대신하여 추가 메소드 또는 필드를 선언합니다. Spring AOP를 사용하면 새로운 인터페이스 (및 해당 구현)를 조언 된 객체에 소개 할 수 있습니다. 예를 들어 소개를 사용하여 캐싱을 단순화하기 위해 Bean이 `IsModified` 인터페이스를 구현하도록 할 수 있습니다. (소개는 AspectJ 커뮤니티에서 inter-type 선언으로 알려져있다.)

- 대상 객체 (Target object) : 하나 이상의 애스펙트에 의해 Advice을받는 객체. "Advice 대상"이라고도합니다. 런타임 AOP를 사용하여 Spring AOP가 구현되므로이 객체는 항상 프록시 객체입니다.

- AOP 프록시 : 애스펙트 계약을 구현하기 위해 AOP 프레임 워크에 의해 생성 된 객체 (메소드 실행 권고 등). Spring Framework에서 AOP 프록시는 JDK 동적 프록시 또는 CGLIB 프록시입니다.

- Weaving : 다른 응용 프로그램 유형 또는 개체와 측면을 연결하여 권고 된 개체를 만듭니다. 이것은 컴파일 타임 (예 : AspectJ 컴파일러 사용),로드 시간 또는 런타임에 수행 할 수 있습니다. Spring AOP는 다른 순수 자바 AOP 프레임 워크와 마찬가지로 런타임에 위빙을 수행합니다.

Spring AOP에는 다음과 같은 유형의 조언이 포함됩니다.

- Before advice : 조인 포인트 이전에 실행되지만 예외를 throw하지 않는 한 실행 흐름이 조인 포인트로 진행되지 못하도록하는 조언.

- After returning advice : 조인 포인트가 정상적으로 완료된 후에 실행할 조언 (예 : 예외가 발생하지 않고 메서드가 반환되는 경우).

- After throwing advice : 예외를 throw하여 메소드가 종료 될 경우 실행될 조언.

- After (finally) advice : 조인 포인트가 종료되는 방법에 관계없이 실행될 조언 (정상 또는 예외적 복귀).

- around advice : 메소드 호출과 같은 조인 포인트를 둘러싼 조언. 이것은 가장 강력한 조언입니다. around advice는 메소드 호출 전과 후에 사용자 정의 동작을 수행 할 수 있습니다. 또한 조인 포인트로 진행할지 또는 자체 반환 값을 반환하거나 예외를 throw하여 권고 된 메소드 실행을 바로 수행할지 여부를 선택하는 작업도 담당합니다.

around advice는 가장 일반적인 종류의 advice이다. AspectJ와 같은 Spring AOP는 모든 유형의 advice 타입을 제공하기 때문에 필요한 행동을 구현할 수있는 가장 강력한 advice 타입을 사용할 것을 권장한다. 예를 들어 메소드의 반환 값으로 캐시를 업데이트하기 만하면 around advice보다 after returning advice를 구현하는 것이 더 좋다. 가장 구체적인 조언 유형을 사용하면 오류 가능성이 적은 간단한 프로그래밍 모델을 제공 할 수 있습니다. 예를 들어 around advice에 사용되는 `JoinPoint`에서 `proceed ()` 메소드를 호출 할 필요가 없으므로 호출을 실패 할 수 없습니다.

Spring 2.0에서는 모든 advice 매개 변수가 정적으로 타입 화되어서 Object 배열보다는 적절한 타입의 advice 매개 변수 (예를 들어 메소드 실행으로부터의 반환 값의 타입)로 작업한다.

포인트 컷에 의해 조인 된 조인 포인트의 개념은 AOP의 핵심이며, AOP는 인터셉터만을 제공하는 오래된 기술과 구별됩니다. Pointcuts는 조언이 객체 지향 계층 구조와 독립적으로 타겟팅 될 수 있도록합니다. 예를 들어 선언적 트랜잭션 관리를 제공하는 around advice를 여러 객체에 걸쳐있는 메소드 세트 (예 : 서비스 계층의 모든 비즈니스 조작)에 적용 할 수 있습니다.

Spring AOP는 순수 자바로 구현된다. 특별한 컴파일 과정이 필요 없습니다. Spring AOP는 클래스 로더 계층을 제어 할 필요가 없으므로 서블릿 컨테이너 또는 애플리케이션 서버에서 사용하기에 적합합니다.

Spring AOP는 현재 메소드 실행 join point 만 지원한다 (Spring 빈에서 메소드의 실행을 조언). 필드 차단은 구현되지 않았지만 필드 차단에 대한 지원은 핵심 Spring AOP API를 손상시키지 않고 추가 될 수 있습니다. 필드 액세스를 조언하고 조인 포인트를 업데이트해야하는 경우 AspectJ와 같은 언어를 고려해야한다.

AOP에 대한 Spring AOP의 접근 방식은 다른 대부분의 AOP 프레임 워크와 다르다. 목표는 가장 완벽한 AOP 구현을 제공하는 것이 아닙니다 (Spring AOP는 상당히 유능한 편이지만). 오히려 AOP 구현과 Spring IoC 간의 긴밀한 통합을 제공하여 엔터프라이즈 애플리케이션의 일반적인 문제를 해결하는 것을 목표로합니다.

따라서, 예를 들어 Spring Framework의 AOP 기능은 일반적으로 Spring IoC 컨테이너와 함께 사용된다. 측면은 일반적인 bean 정의 구문을 사용하여 구성됩니다 (강력한 "자동 프록시"기능이 가능함). 이것은 다른 AOP 구현과의 중요한 차이입니다. 매우 정교한 객체 (일반적으로 도메인 객체)에 대한 조언과 같이 Spring AOP를 사용하여 쉽고 효율적으로 작업 할 수는 없습니다. 이런 경우에는 AspectJ가 최선의 선택이다. 그러나 Spring AOP는 AOP가 적용될 수있는 엔터프라이즈 Java 애플리케이션의 대부분의 문제에 대해 우수한 솔루션을 제공한다는 것이 우리의 경험입니다.



## PSA(**Portable Service Abstraction**)

PSA는 환경과 세부 기술의 변화에 관계없이 일관된 방식으로 기술에 접근할 수 있도록 해주는 기술이다. Spring에서 작동 하는 라이브러리는 POJO 원칙을 지키는 형태로 추상화가 되어있음을 뜻한다. 
외부라이브러리를 사용하더라고 동일한 인터페이스와 구동이 되도록 설계되어 있어서 의존성을 고려하지 않아도 된다. 



## 참고

- [Spring Framework Documentation(Version 5.1.3.RELEASE)](https://docs.spring.io/spring/docs/current/spring-framework-reference/)