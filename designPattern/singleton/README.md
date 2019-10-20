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



### **정리**

- 정적 초기화 방법은 구현하기 쉽지만 리소스 및 CPU 시간이 낭비 될 수 있습니다. 클래스 초기화 비용이 리소스 측면에서 적거나 프로그램에 항상 클래스 인스턴스가 필요한 경우에만 사용해야 합니다.
- 정적 초기화 방법에서 정적 블록을 사용하여 예외 처리를 제공하고 인스턴스를 제어 할 수도 있습니다.
- 동기화를 사용하면 멀티 스레딩 환경에서도 싱글 톤 클래스를 만들 수 있지만 성능이 저하 될 수 있으므로 이중 검사 잠금 메커니즘을 사용할 수 있습니다.
- Bill Pugh 구현은 싱글 톤 클래스에 가장 널리 사용되는 방식입니다. 대부분의 개발자는 단순성과 장점으로 인해 선호합니다.



## Singleton을 깰 수 있는 방법



### Reflection

```java
// Java code to explain effect of Reflection 
// on Singleton property 
  
import java.lang.reflect.Constructor; 
  
// Singleton class 
class Singleton  
{ 
    // public instance initialized when loading the class 
    public static Singleton instance = new Singleton(); 
      
    private Singleton()  
    { 
        // private constructor 
    } 
} 
  
public class GFG  
{ 
  
    public static void main(String[] args) 
    { 
        Singleton instance1 = Singleton.instance; 
        Singleton instance2 = null; 
        try
        { 
            Constructor[] constructors =  
                    Singleton.class.getDeclaredConstructors(); 
            for (Constructor constructor : constructors)  
            { 
                // Below code will destroy the singleton pattern 
                constructor.setAccessible(true); 
                instance2 = (Singleton) constructor.newInstance(); 
                break; 
            } 
        } 
      
        catch (Exception e)  
        { 
            e.printStackTrace(); 
        } 
          
    System.out.println("instance1.hashCode():- " 
                                      + instance1.hashCode()); 
    System.out.println("instance2.hashCode():- " 
                                      + instance2.hashCode()); 
    } 
} 
```

```java
Output:- 
instance1.hashCode () :-366712642
instance2.hashCode () :-1829164700
```

이 클래스를 실행 한 후에는 hashCode가 다르다는 것을 알 수 있습니다. 즉, 동일한 클래스의 2 개의 객체가 만들어지고 싱글 톤 패턴이 파괴되었음을 의미합니다.

**리플렉션 문제 극복 : 리플렉션으로** 발생한 문제를 극복하기 위해 java는 열거 형 값이 한 번만 인스턴스화되도록 내부적으로 보장하기 때문에 열거 형이 사용됩니다. java Enum은 전역 적으로 액세스 가능하므로 싱글 톤에 사용할 수 있습니다. 그것의 유일한 단점은 융통성이 없으며 즉, 초기화 지연을 허용하지 않는다는 것입니다.

```java
//Java program for Enum type singleton 
public enum GFG  
{ 
  INSTANCE; 
} 
```

열거 형에는 생성자가 없으므로 Reflection에서이를 사용할 수 없습니다. 열거 형에는 기본 생성자가 있으므로 직접 호출 할 수 없습니다. **JVM은 내부적으로 열거 형 생성자의 생성 및 호출을 처리합니다.** 열거 형은 생성자에게 프로그램에 대한 정의를 제공하지 않으므로 Reflection으로도 액세스 할 수 없습니다. 따라서 리플렉션은 열거 형의 경우 싱글 톤 속성을 손상시킬 수 없습니다.



### 직렬화

직렬화는 단일 클래스의 단일 속성을 손상시킬 수도 있습니다. 직렬화는 바이트 스트림의 오브젝트를 변환하고 파일로 저장하거나 네트워크를 통해 전송하는 데 사용됩니다. 싱글 톤 클래스의 객체를 직렬화한다고 가정합니다. 그런 다음 해당 객체를 직렬화 해제하면 새 인스턴스가 생성되어 싱글 톤 패턴이 중단됩니다.

```java
// Java code to explain effect of  
// Serilization on singleton classes 
import java.io.FileInputStream; 
import java.io.FileOutputStream; 
import java.io.ObjectInput; 
import java.io.ObjectInputStream; 
import java.io.ObjectOutput; 
import java.io.ObjectOutputStream; 
import java.io.Serializable; 
  
class Singleton implements Serializable  
{ 
    // public instance initialized when loading the class 
    public static Singleton instance = new Singleton(); 
      
    private Singleton()  
    { 
        // private constructor 
    } 
} 
  
  
public class GFG  
{ 
  
    public static void main(String[] args)  
    { 
        try
        { 
            Singleton instance1 = Singleton.instance; 
            ObjectOutput out 
                = new ObjectOutputStream(new FileOutputStream("file.text")); 
            out.writeObject(instance1); 
            out.close(); 
      
            // deserailize from file to object 
            ObjectInput in  
                = new ObjectInputStream(new FileInputStream("file.text")); 
              
            Singleton instance2 = (Singleton) in.readObject(); 
            in.close(); 
      
            System.out.println("instance1 hashCode:- "
                                                 + instance1.hashCode()); 
            System.out.println("instance2 hashCode:- " 
                                                 + instance2.hashCode()); 
        }  
          
        catch (Exception e)  
        { 
            e.printStackTrace(); 
        } 
    } 
} 
```

```java
Output:- 
instance1 hashCode:- 1550089733
instance2 hashCode:- 865113938
```

보시다시피, 두 인스턴스의 hashCode가 다르므로 싱글 톤 클래스의 객체가 2 개 있습니다. 따라서 클래스는 더 이상 싱글 톤이 아닙니다.

**직렬화 문제 극복 :** 이 문제를 극복하려면 readResolve () 메소드를 구현해야합니다.

```java
// Java code to remove the effect of  
// Serialization on singleton classes 
import java.io.FileInputStream; 
import java.io.FileOutputStream; 
import java.io.ObjectInput; 
import java.io.ObjectInputStream; 
import java.io.ObjectOutput; 
import java.io.ObjectOutputStream; 
import java.io.Serializable; 
  
class Singleton implements Serializable  
{ 
    // public instance initialized when loading the class 
    public static Singleton instance = new Singleton(); 
      
    private Singleton()  
    { 
        // private constructor 
    } 
      
    // implement readResolve method 
    protected Object readResolve() 
    { 
        return instance; 
    } 
} 
  
public class GFG  
{ 
  
    public static void main(String[] args)  
    { 
        try
        { 
            Singleton instance1 = Singleton.instance; 
            ObjectOutput out  
                = new ObjectOutputStream(new FileOutputStream("file.text")); 
            out.writeObject(instance1); 
            out.close(); 
          
            // deserailize from file to object 
            ObjectInput in  
                = new ObjectInputStream(new FileInputStream("file.text")); 
            Singleton instance2 = (Singleton) in.readObject(); 
            in.close(); 
          
            System.out.println("instance1 hashCode:- "
                                           + instance1.hashCode()); 
            System.out.println("instance2 hashCode:- "
                                           + instance2.hashCode()); 
        }  
          
        catch (Exception e) 
        { 
            e.printStackTrace(); 
        } 
    } 
} 
```

```java
Output:- 
instance1 해시 코드 :-1550089733
instance2 해시 코드 :-1550089733
```

두 해시 코드 위에서 동일하므로 다른 인스턴스는 생성되지 않습니다.



### 복제 

복제는 중복 개체를 만드는 개념입니다. clone을 사용하여 객체의 복사본을 만들 수 있습니다. 우리는 싱글 톤 객체의 복제를 중단 한 다음, 싱글 톤 클래스의 두 인스턴스가있는 사본을 생성해야하므로 클래스는 더 이상 싱글 톤이 아니라고 가정합니다.

```java
// JAVA code to explain cloning  
// issue with singleton 
class SuperClass implements Cloneable 
{ 
  int i = 10; 
  
  @Override
  protected Object clone() throws CloneNotSupportedException  
  { 
    return super.clone(); 
  } 
} 
  
// Singleton class 
class Singleton extends SuperClass 
{ 
  // public instance initialized when loading the class 
  public static Singleton instance = new Singleton(); 
  
  private Singleton()  
  { 
    // private constructor 
  } 
} 
  
public class GFG 
{ 
  public static void main(String[] args) throws CloneNotSupportedException  
  { 
    Singleton instance1 = Singleton.instance; 
    Singleton instance2 = (Singleton) instance1.clone(); 
    System.out.println("instance1 hashCode:- "
                           + instance1.hashCode()); 
    System.out.println("instance2 hashCode:- " 
                           + instance2.hashCode());  
  } 
} 
```

```
Output:- 
instance1 해시 코드 :-366712642
instance2 해시 코드 :-1829164700
```

두 개의 다른 hashCode는 싱글 톤 클래스의 두 가지 다른 객체가 있음을 의미합니다.

**복제 문제 극복 :** 이 문제를 극복하려면 clone () 메소드를 대체하고 CloneNotSupportedException 인 클론 메소드에서 예외를 처리하십시오. 이제 사용자가 싱글 톤 객체의 복제본을 만들려고 할 때마다 예외가 발생하므로 클래스는 싱글 톤으로 유지됩니다.

```java
// JAVA code to explain overcome  
// cloning issue with singleton 
class SuperClass implements Cloneable 
{ 
  int i = 10; 
  
  @Override
  protected Object clone() throws CloneNotSupportedException  
  { 
    return super.clone(); 
  } 
} 
  
// Singleton class 
class Singleton extends SuperClass 
{ 
  // public instance initialized when loading the class 
  public static Singleton instance = new Singleton(); 
  
  private Singleton()  
  { 
    // private constructor 
  } 
  
  @Override
  protected Object clone() throws CloneNotSupportedException  
  { 
    throw new CloneNotSupportedException(); 
  } 
} 
  
public class GFG 
{ 
  public static void main(String[] args) throws CloneNotSupportedException  
  { 
    Singleton instance1 = Singleton.instance; 
    Singleton instance2 = (Singleton) instance1.clone(); 
    System.out.println("instance1 hashCode:- " 
                         + instance1.hashCode()); 
    System.out.println("instance2 hashCode:- " 
                         + instance2.hashCode());  
  } 
} 
```

```java
Output:-
Exception in thread "main" java.lang.CloneNotSupportedException
	at GFG.Singleton.clone(GFG.java:29)
	at GFG.GFG.main(GFG.java:38)
```

이제 싱글 톤 클래스의 복제본 생성을 중지했습니다. 예외를 발생시키지 않으려면 clone 메소드에서 동일한 인스턴스를 반환 할 수도 있습니다.

```java
// JAVA code to explain overcome  
// cloning issue with singleton 
class SuperClass implements Cloneable 
{ 
  int i = 10; 
  
  @Override
  protected Object clone() throws CloneNotSupportedException  
  { 
    return super.clone(); 
  } 
} 
  
// Singleton class 
class Singleton extends SuperClass 
{ 
  // public instance initialized when loading the class 
  public static Singleton instance = new Singleton(); 
  
  private Singleton()  
  { 
    // private constructor 
  } 
  
  @Override
  protected Object clone() throws CloneNotSupportedException  
  { 
    return instance; 
  } 
} 
  
public class GFG 
{ 
  public static void main(String[] args) throws CloneNotSupportedException  
  { 
    Singleton instance1 = Singleton.instance; 
    Singleton instance2 = (Singleton) instance1.clone(); 
    System.out.println("instance1 hashCode:- " 
                           + instance1.hashCode()); 
    System.out.println("instance2 hashCode:- "
                           + instance2.hashCode());  
  } 
} 
```

```java
Output:-
instance1 해시 코드 :-366712642
instance2 해시 코드 :-366712642
```



## 참고

- https://www.geeksforgeeks.org/singleton-design-pattern/
- https://www.geeksforgeeks.org/java-singleton-design-pattern-practices-examples/
- https://www.geeksforgeeks.org/prevent-singleton-pattern-reflection-serialization-cloning/