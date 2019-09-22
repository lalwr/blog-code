# Singleton DesignPattern

싱글턴 패턴은 가장 간단한 디자인 패턴중 하나로 인스턴스화를 하나의 객체로 제한하는 디자인 패턴입니다.

예를들어 DB연결에 대한 구현을 한다고 했을때 모든 객체에 별도의 DB연결을 구현하면 많은 비용이 들기 때문에 여러 객체가 공유하는 단일 DB 연결 클래스의 인스턴스 하나로 처리하면 됩니다.



## 생성



### **정적 초기화**

```java
class Singleton 
{ 
    private static Singleton INSTANCE = new Singleton(); 
  
    private Singleton() {} 
  
    public static Singleton getInstance() 
    { 
        return INSTANCE; 
    } 
}
```

정적으록 객체를 초기화하여 인스턴스를 생성하였습니다. JVM은 클래스가 로드 될 때 정적 이니셜 라이저를 실행하므로 스레드 안전을 보장합니다. 구현이 간단한 대신 사용하지 않은 객체라면 자원의 낭비와 CPU 시간이 낭비 될 수 있고 예외 처리가 불가능합니다. 프로그램이 항상 이 클래스의 인스턴스를 사용하거나 인스턴스 작성 비용이 자원 및 시간 측면에서 너무 크지 않은 경우에 사용할 수 있습니다.



### **정적 블록**

```java
class Singleton 
{ 
    private static Singleton INSTANCE = new Singleton(); 
  
    private Singleton() {} 
  
    { 
        INSTANCE = new Singleton(); 
    } 
}
```

정적 초기화와는 다르게 정적 블록으로 만들어서 예외 처리와 같은 생성에 관한 처리를 할 수 있습니다. 클래스 로딩시 객체가 생성되며 초기 설정으로 객체를 생성 할 때 예외가 발생할 때 사용할 수 있습니다.

구현이 간단하며 `getInstance()` 메소드를 구현할 필요가 없고 인스턴스에 직접 액세스하여 사용 할 수 있으며 정적 블록에서 예외를 처리 할 수 있습니다.

하지만 클래스의 인스턴스는 항상 생성되기 때문에 자원의 낭비와 CPU 시간이 낭비됩니다.



### **지연 초기화**

```java
 class Singleton 
{ 
    private static Singleton INSTANCE; 

    private Singleton() {} 
  
    public static Singleton getInstance() 
    { 
        if (INSTANCE == null) 
            INSTANCE = new Singleton(); 
        return INSTANCE; 
    } 
} 
```

클래스를 인스턴스화하지 않고 호출 할 수 있게 `getInstance()`를 선언했습니다. 처음 호출하면 싱글톤 객체가 생성 된 후 동일한 객체가 반환됩니다. 실제로 객체가 필요할 때 까지 생성되지 않고 메소드가 호출될때 생성되어 리소스 낭비를 막을 수 있습니다. 이것을 **lazy 인스턴스**화 라고합니다.

아무도 직접 액세스 할 수 없도록 인스턴스를 비공개로 유지하며 필요한 경우에만 객체가 생성됩니다. 리소스 극복 및 CPU 시간 낭비문제를 해결 할 수 있으며 메소드에서 예외 처리가 가능합니다.

하지만 null 조건을 확인 할 때 인스턴스에 직접 액세스 할 수 없고 멀티스레드 환경에서 안전하지 않습니다. 멀티스레드 환경에서 요청이 왔을때 다수의 객체가 생성될 수 있습니다.



### **동기화**

```java
class Singleton 
{ 
    private static Singleton INSTANCE; 
  
    private Singleton() {} 
   
    public static synchronized Singleton getInstance() 
    { 
        if (INSTANCE == null) 
            INSTANCE = new Singleton(); 
        return INSTANCE; 
    } 
} 
```

멀티 스레드 환경에서도 싱글톤 객체가 유지되도록 thread-safe하게 구현 하였습니다. 스레드로 부터 안전하게 구현하기 위해  `getInstance()`메소드는 여러 스레드가 동시에 접근 할 수 없게 동기화 처리를 했습니다.

게으른 초기화가 가능하며 스레드에 안전합니다. 

하지만 `getInstance()` 메소드는 동기화되어 여러 스레드가 동시에 액세스 할 수 없으므로 성능이 저하됩니다.

성능이 중요하지 않은 경우 사용하는 것이 좋습니다.



### **이중 검사**

```java
class Singleton 
{ 
    private static Singleton INSTANCE; 
  
    private Singleton() {} 
  
    public static Singleton getInstance() 
    { 
        if (INSTANCE == null) 
        { 

            synchronized (Singleton.class) 
            { 
                if (INSTANCE == null) 
                    INSTANCE = new Singleton(); 
            } 
          
        } 
        return INSTANCE; 
    } 
} 
```

동기화 처리에 대한 오버헤드 문제를 극복하기 위해 `getInstance()` 메소드를 동기화 하지 않고 인스턴스를 작성하는 블록을 동기화 하여 최소 수의 스레드가 대기하게 구현하였습니다.

게으른 초기화가 가능하며 스레드에 안전하고 성능 문제를 개선하였습니다.

하지만 처음에는 성능에 영향을 줄 수 있습니다.



### Bill Pugh

Java5 이전에는 메모리 모델에 많은 문제가 있었고 위의 방법으로 인해 다중 스레드 환경의 특정 시나리오에서 오류가 발생했습니다. 따라서 Bill Pugh는 싱글 톤에 사용할 내부 정적 클래스 개념을 제안했습니다.

```java
class Singleton 
{ 
    private static Singleton INSTANCE; 
  
    private Singleton() {} 
  
    // Inner class
    private static class BillPughSingleton 
    { 
      private static final Singleton INSTANCE = new Singleton(); 
    } 
  
    public static GFG getInstance()  
    { 
      return BillPughSingleton.INSTANCE; 
    } 
  
} 
```

Singleton 클래스가 로드 되어도 내부 클래스는 로드된 상태가 아니므로 객체를 만들지 않습니다. 내부 클래스는  `getInstance()` 메소드가 호출 될 떄만 실행됩니다. **지연 초기화**처럼 보이지만 다른 지연 초기화 방법입니다.

동기화를 사용하지 않기 때문에 가장 널리 사용되는 방법입니다.



# Singleton 지키는 방법







## **정리**

- 정적 초기화 방법은 구현하기 쉽지만 리소스 및 CPU 시간이 낭비 될 수 있습니다. 클래스 초기화 비용이 리소스 측면에서 적거나 프로그램에 항상 클래스 인스턴스가 필요한 경우에만 사용해야 합니다.
- 정적 초기화 방법에서 정적 블록을 사용하여 예외 처리를 제공하고 인스턴스를 제어 할 수도 있습니다.
- 동기화를 사용하면 멀티 스레딩 환경에서도 싱글 톤 클래스를 만들 수 있지만 성능이 저하 될 수 있으므로 이중 검사 잠금 메커니즘을 사용할 수 있습니다.
- Bill Pugh 구현은 싱글 톤 클래스에 가장 널리 사용되는 방식입니다. 대부분의 개발자는 단순성과 장점으로 인해 선호합니다.



## 참고

- https://www.geeksforgeeks.org/singleton-design-pattern/
- https://www.geeksforgeeks.org/java-singleton-design-pattern-practices-examples/
- https://www.geeksforgeeks.org/prevent-singleton-pattern-reflection-serialization-cloning/