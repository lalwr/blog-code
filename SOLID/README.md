# SOLID

> SOLID의 객체 지향 프로그래밍 및 설계의 다섯 가지 기본 원칙을 말한다.



## SRP(Single responsibility principle)

**단일 책임 원칙(single responsibility principle)**이란 모든 클래스는 하나의 책임만 가지며, 클래스는 그 책임을 완전히 캡슐화해야 함을 뜻한다. 
책임을 변경하려는 이유로 정의하고, 어떤 클래스나 모듈은 변경하려는 단 하나 이유만을 가져야 한다.
하나의 목적으로만 class를 작성,변경 유지하게 되면 다른 주체에 대한 변경의 영향을 받지않고 미래에 변경을 할 수 있는 유연성을 얻을수 있습니다. 

마찬가지로 service 클래스를 작성하고 있다면, 메소드 호출의 그 부분만을 포함하고 다른것은 아무것도 포함하지 않아야 합니다. 모듈 관련 유틸리티 전역 함수조차 없어야 합니다. 액세스 할 수 있는 다른 클래스 파일에서 분리합니다

```java
public class Person
{
    private Long personId;
    private String firstName;
    private String lastName;
    private String age;
    private List<Account> accounts;
}
```

```java
public class Account
{
    private Long guid;
    private String accountNumber;
    private String accountName;
    private String status;
    private String type;
}
```



## OCP(Open/closed principle)

**개방-폐쇄 원칙(OCP, Open-Closed Principle)**이란 소프트웨어 개체(클래스, 모듈, 함수 등등)는 확장에 대해 열려 있어야 하고, 수정에 대해서는 닫혀 있어야 함을 뜻한다.

소프트웨어 개발 작업에 이용된 많은 모듈 중에 하나에 수정을 가할 때 그 모듈을 이용하는 다른 모듈을 줄줄이 고쳐야 한다면, 이와 같은 프로그램은 수정하기가 어렵다. 개방-폐쇄 원칙은 시스템의 구조를 올바르게 재조직(리팩토링)하여 나중에 이와 같은 유형의 변경이 더 이상의 수정을 유발하지 않도록 하는 것이다. 개방-폐쇄 원칙이 잘 적용되면, 기능을 추가하거나 변경해야 할 때 이미 제대로 동작하고 있던 원래 코드를 변경하지 않아도, 기존의 코드에 새로운 코드를 추가함으로써 기능의 추가나 변경이 가능하다.

#### 확장에 대해 열려 있다

모듈의 동작을 확장할 수 있다는 것을 의미한다. 애플리케이션의 요구 사항이 변경될 때, 이 변경에 맞게 새로운 동작을 추가해 모듈을 확장할 수 있다. 즉, 모듈이 하는 일을 변경할 수 있다.

#### 수정에 대해 닫혀 있다

모듈의 소스 코드나 바이너리 코드를 수정하지 않아도 모듈의 기능을 확장하거나 변경할 수 있다. 그 모듈의 실행 가능한 바이너리 형태나 링크 가능한 라이브러리를 건드릴 필요가 없다.

인터페이스를 이용하여 디자인을 확장할 수 있다.

```java
public interface Play {
    public void doPlay();
}
```

```java
public class Person() {
    private Play play;
    
    public Person(Play play) {
        this.play = play;
    }
    public void playing() {
        this.play.doPlay();
    }
}
```

```java
public class Soccer implements Play {
    public void doPlay() {
        System.out.println(“play soccer”);
    } 
}
public class Tennis implements Play {
    public void doPlay() {
        System.out.println(“play tennis”);
    } 
}
```

```java
public class DemoPlay() {
    public static void main(String[] args) {
        Person person = new Person(new Soccer()); 
        person.playing();  
        Person = new Person(new Tennis());  
        person.playing();  
    }
}
```



## LSP(Liskov substitution principle)

**리스코프 치환 원칙(LSP, Liskov substitution principle)**이란 자료형 S가 자료형 T의 하위형이라면, 프로그램에서 자료형 T의 객체는 프로그램의 속성을 변경하지 않고 자료형 S의 객체로 교체할 수 있어야 함을 뜻한다. 

하위 클래스의 객체가 부모 클래스의 객체와 동일한 방식으로 작동해야합니다.

```java
class Rectangle {
    private int width;
    private int height;

    public void setHeight(int height) {
        this.height = height;
    }

    public int getHeight() {
        return this.height;
    }

    public void setWidth(int width) {
        this.width = width;
    }

    public int getWidth() {
        return this.width;
    }

    public int area() {
        return this.width * this.height;
    }
}
```

```java
class Square extends Rectangle {
    @Override
    public void setWidth(int width) {
        this.width = width;
        this.height = width;
    }

    @Override
    public void setHeight(int height) {
        this.width = height;
        this.height = height;
    }
}
```

```java
class Test
{
	public static void main (String args[])
	{
		Rectangle r = new Square();
        
		r.setWidth(3);
		r.setHeight(5);

		System.out.println(r.getArea());
		// 15가 아닌 25가 출력된다.
	}
}
```

이 원칙은 열기 닫기 원칙의 확장 일 뿐이며 새로운 파생 클래스가 동작을 변경하지 않고 기본 클래스를 확장하는지 확인해야한다.

## ISP(Interface segregation principle)

**인터페이스 분리 원칙(ISP, Interface segregation principle)**은 클라이언트가 자신이 이용하지 않는 메서드에 의존하지 않아야 한다는 원칙이다. 인터페이스 분리 원칙은 큰 덩어리의 인터페이스들을 구체적이고 작은 단위들로 분리시킴으로써 클라이언트들이 꼭 필요한 메서드들만 이용할 수 있게 한다. 이와 같은 작은 단위들을 *역할 인터페이스*라고도 부른다.인터페이스 분리 원칙을 통해 시스템의 내부 의존성을 약화시켜 리팩토링, 수정, 재배포를 쉽게 할 수 있다.

마우스에 대한 기능을 구현 하기위해서 사용하지 않는 메소드도 구현 하게 된다.

```java
public interface ComputerMotionListener
{
    public void mouseDragged();
 
    public void mouseMoved(); 
    
    public void keboardInput();
}
```

```java
public class MouseMotionListenerImpl implements ComputerMotionListener
{
    @Override
    public void mouseDragged() {
        System.out.println("mouse drag");
    }
 
    @Override
    public void mouseMoved() {
        System.out.println("mouse drag" );
    }
    
    @Override
    public void keboardInput() {
        
    }
}
```

인터페이스를 분리하자.

```java
public interface MouseMotionListener
{
    public void mouseDragged();
 
    public void mouseMoved(); 
}
```

```java
public interface KeyboardMotionListener
{   
    public void keboardInput();
}
```

```java
public class MouseMotionListenerImpl implements MouseMotionListener
{
    @Override
    public void mouseDragged() {
        System.out.println("mouse drag");
    }
 
    @Override
    public void mouseMoved() {
        System.out.println("mouse drag" );
    }
}
```

## DIP(Dependency inversion principle)

**의존 관계 역전 원칙(DIP, Dependency inversion principle)**은 소프트웨어 모듈들을 분리하는 특정 형식을 지칭한다. 이 원칙을 따르면, 상위 계층(정책 결정)이 하위 계층(세부 사항)에 의존하는 전통적인 의존 관계를 반전(역전)시킴으로써 상위 계층이 하위 계층의 구현으로부터 독립되게 할 수 있다. 이 원칙은 다음과 같은 내용을 담고 있다.

첫째, 상위 모듈은 하위 모듈에 의존해서는 안된다. 상위 모듈과 하위 모듈 모두 추상화에 의존해야 한다.
둘째, 추상화는 세부 사항에 의존해서는 안된다. 세부사항이 추상화에 의존해야 한다.

상위 Develop 모듈이 하위 모듈에 의존적이다.

```java
public class JavaDeveloper
{   
    public void develop(){
    	System.out.println("java develop");
    };
}
```

```java
public class JavScriptDeveloper
{   
    public void develop(){
    	System.out.println("javaScript develop");
    };
}
```

```java
public class Project
{
    private JavaDeveloper javaDeveloper = new JavaDeveloper();
    private JavScriptDeveloper javScriptDeveloper = new JavScriptDeveloper();
    
    public void projectDevelop() {
        javaDeveloper.develop();
        javScriptDeveloper.develop();
    }
}
```

추상화에 의존하게 수정하자. OCP도 준수하게 된다.

```java
public interface Developer {
    void projectDevelop();
}
```

```java
public class JavaDeveloper implements Developer
{   
    @Override
    public void develop(){
    	System.out.println("java develop");
    };
}
```





```java
public class JavScriptDeveloper implements Developer
{   
    @Override
    public void develop(){
    	System.out.println("javaScript develop");
    };
}
```

```java
public class Project {
    
    private List<Developer> developers;
    
    public Project(List<Developer> developers) {
        this.developers = developers;
    }
    
    public void projectDevelop() {
        developers.forEach(d -> d.develop());
    }
}
```



## 참고

- [SOLID (객체 지향 설계)](https://ko.wikipedia.org/wiki/SOLID_(%EA%B0%9D%EC%B2%B4_%EC%A7%80%ED%96%A5_%EC%84%A4%EA%B3%84))