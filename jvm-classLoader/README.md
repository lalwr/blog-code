# JVM Class Loader

클래스 로더는 **런타임 중에 JVM**에 동적으로 **Java 클래 를 로드** 합니다. JRE의 일부입니다. JVM은 클래스 로더 덕분에 Java 프로그램을 실행하기 위해 기본 파일이나 파일 시스템을 알 필요가 없습니다.

Java 클래스는 한 번에 메모리에 로드되지 않지만 응용 프로그램에서 필요할 때 로드됩니다. 클래스 로더는 클래스를 메모리에 로드 할 책임이 있습니다.



## 내장 클래스 로더의 유형 Class Loader

```java
public void printClassLoaders() throws ClassNotFoundException {
 
    System.out.println("Classloader of this class:"
        + PrintClassLoader.class.getClassLoader());
 
    System.out.println("Classloader of Logging:"
        + Logging.class.getClassLoader());
 
    System.out.println("Classloader of ArrayList:"
        + ArrayList.class.getClassLoader());
}

Class loader of this class:sun.misc.Launcher$AppClassLoader@18b4aac2
Class loader of Logging:sun.misc.Launcher$ExtClassLoader@3caeaf62
Class loader of ArrayList:null
```

세 가지 다른 클래스 로더가 있습니다. 

- application Class Loader
- extension Class Loader
- bootstrap Class Loader(null로 표시됨)

`application Class Loader`는 예제 메서드가 있는 클래스를 로드합니다. application 또는 extension Class Loader는 **클래스 경로에 자체 파일을 로드합니다.**

`extension Class Loader`는 **표준 코어 Java 클래스의 확장 인 클래스를 로드합니다.**

**bootstrap Class Loader 또는 primordial lass Loader는 다른 모든 클래스의 부모입니다.**

ArrayList의 경우 출력에서 null이 표시되는 것은 bootstrap Class Loader가 Java가 아닌 **원시 코드로 작성 되었기 때문에 Java 클래스로 표시되지 않기 때문입니다.** 이러한 이유 때문에 bootstrap Class Loader의 동작은 JVM간에 다를 수 있습니다.

이제 각각의 클래스로더에 대해 알아봅시다.



### Bootstrap Class Loader

Java 클래스는 *java.lang.ClassLoader* 의 인스턴스에 의해 로드됩니다 . 그러나 클래스 로더는 클래스 자체입니다. 따라서, *java.lang.ClassLoader* 자체를 누가로드 할 것인가가 문제가 됩니다.

이것은 부트 스트랩이나 원시 클래스 로더가 그림으로 들어가는 곳입니다.

주로 JDK 내부 클래스, 일반적으로 *rt.jar* 및 *$ JAVA_HOME / jre / lib 디렉토리* 에있는 다른 핵심 라이브러리 를로드하는 역할을 담당합니다 . 또, **Bootstrap 클래스 로더는, 다른 모든 ClassLoader 인스턴스 의 부모로서 기능 합니다** .

**이 부트 스트랩 클래스 로더는 핵심 JVM의 일부이며** 위의 예에서 지적한대로 **원시 코드** 로 **작성됩니다** . 플랫폼마다,이 클래스 로더의 구현이 다른 경우가 있습니다.



### Extension Class Loader

**확장 클래스 로더는 부트 스트랩 클래스 로더의 하위 클래스이며 플랫폼에서 실행되는 모든 응용 프로그램에서 사용할 수 있도록 표준 코어 Java 클래스의 확장을로드하는 작업을 담당**합니다.

확장 클래스 로더는 JDK extensions 디렉토리 (일반적으로 $ JAVA_HOME / lib / ext 디렉토리 또는 java.ext.dirs 시스템 등록 정보에 언급 된 다른 디렉토리)에서로드됩니다.



### **System Class Loader**

반면에 시스템 또는 응용 프로그램 클래스 로더는 모든 응용 프로그램 수준 클래스를 JVM으로 로드하는 작업을 처리합니다. **classpath 환경 변수, -classpath 또는 -cp 명령 행 옵션에서 찾은 파일을 로드합니다.** 또한 Extensions 클래스 로더의 자식입니다.



## Class Loader는 어떻게 작동하나?

클래스 로더는 Java Runtime Environment의 일부입니다. JVM이 클래스를 요청할 때 클래스 로더는 클래스를 찾고 클래스 이름을 사용하여 런타임에 클래스 정의를로드하려고 시도합니다.

**java.lang.ClassLoader.loadClass () 메소드는 런타임에 클래스 정의를 로드** 합니다. 정규화 된 이름을 기반으로 클래스를로드하려고합니다.

클래스가 아직로드되어 있지 않은 경우는, 그 요구를 요청한 클래스 로더에 위양합니다. 이 프로세스는 재귀 적으로 발생합니다.

결국 부모 클래스 로더가 클래스를 찾지 못하면 자식 클래스는 java.net.URLClassLoader.findClass () 메서드를 호출하여 파일 시스템 자체에서 클래스를 찾습니다.

마지막 하위 클래스 로더가 클래스를로드 할 수 없으면 java.lang.NoClassDefFoundError 또는 java.lang.ClassNotFoundException을 발생시킵니다.

ClassNotFoundException이 Throw되었을 때 출력 예제를 보자.

```java
java.lang.ClassNotFoundException: com.baeldung.classloader.SampleClassLoader    
    at java.net.URLClassLoader.findClass(URLClassLoader.java:381)    
    at java.lang.ClassLoader.loadClass(ClassLoader.java:424)    
    at java.lang.ClassLoader.loadClass(ClassLoader.java:357)    
    at java.lang.Class.forName0(Native Method)    
    at java.lang.Class.forName(Class.java:348)
```

java.lang.Class.forName ()을 호출 할 때 일련의 이벤트를 거치면 부모 클래스 로더를 통해 클래스를 로드하고 java.net.URLClassLoader.findClass ()를 통해 클래스를 로드하려고 시도한다는 것을 알 수 있습니다. 클래스 그 자체.

클래스를 찾지 못하면 ClassNotFoundException을 발생시킵니다.

클래스 로더에는 세 가지 중요한 기능이 있습니다.



### **Delegation Model**

클래스 로더는, 위양 모델에 따라 **클래스 또는 자원의 검색을 요구하면, ClassLoader 인스턴스는 그 클래스 또는 자원의 검색을 한 클래스 로더에 위양합니다.**

애플리케이션 클래스를 JVM에로드하라는 요청이 있다고 가정 해 보겠습니다. 시스템 클래스 로더는 먼저 해당 클래스의 로딩을 상위 확장 클래스 로더에 위임합니다.이 로더는 상위 클래스 로더를 부트 스트랩 클래스 로더에 위임합니다.

부트 스트랩과 확장 클래스 로더가 클래스 로딩에 실패한 경우에만 시스템 클래스 로더가 클래스 자체를로드하려고 시도합니다.



### Unique Classes

위임 모델의 결과로 **우리는 항상 위임을 시도하면서 고유 한 클래스**를 보장하기 쉽습니다.

상위 클래스 로더가 클래스를 찾을 수 없으면 현재 인스턴스는 그 자체를 시도합니다.



### **Visibility**

게다가, **자식의 클래스 로더는, 요청한 클래스 로더에 의해 로드 된 클래스에 대해서 표시됩니다.**

예를 들어, 시스템 클래스 로더에 의해 로드 된 클래스는 확장 클래스 및 부트 스트랩 클래스 로더에 의해로드 된 클래스에 대한 가시성을 갖지만 그 반대는 마찬가지 입니다.

이를 설명하기 위해 클래스 A가 응용 프로그램 클래스 로더에 의해 로드되고 클래스 B가 확장 클래스 로더에 의해 로드되면 Application 클래스 로더가 로드하는 다른 클래스와 관련하여 A 및 B 클래스가 모두 표시됩니다.

그럼에도 불구하고 클래스 B는 확장 클래스 로더에 의해로드 된 다른 클래스가 보이는 한 눈에 보이는 유일한 클래스입니다.



## **Custom ClassLoader**

내장 클래스 로더는 파일이 이미 파일 시스템에 있는 대부분의 경우에 충분합니다.

그러나 로컬 하드 드라이브나 네트워크에서 클래스를 로드해야 하는 경우 사용자 정의 클래스 로더를 사용해야 할 수도 있습니다.

사용자 지정 클래스 로더에 대한 다른 사용 사례에 대해 설명하고 작성 방법을 시연합니다.



### Custom Class Loaders Use-Cases

사용자 지정 클래스 로더는 런타임에 클래스를 로드하는 것 외에도 다음과 같은 몇 가지 사용 사례에 유용합니다.

1. 기존 바이트 코드 수정(예: 웨이빙 에이전트)
2. 사용자의 필요에 따라 동적으로 클래스를 만듭니다. 예를 들어 JDBC에서는 동적 클래스 로드를 통해 서로 다른 드라이버 구현 간의 전환이 수행됩니다.
3. 동일한 이름 및 패키지를 가진 클래스에 대해 서로 다른 바이트 코드를 로드하는 동안 클래스 버전 지정 메커니즘을 구현합니다. 이 작업은 URL 클래스 로더(URL을 통해 병 로드) 또는 사용자 정의 클래스 로더를 통해 수행할 수 있습니다.

예를 들어 **브라우저는 사용자 정의 클래스 로더를 사용하여 웹 사이트에서 실행 가능한 콘텐츠를 로드합니다.** 브라우저는 별도의 클래스 로더를 사용하여 다른 웹 페이지에서 애플릿을 로드할 수 있습니다. 애플릿을 실행하는 데 사용되는 애플릿 뷰어에는 로컬 파일 시스템을 찾는 대신 원격 서버의 웹 사이트에 액세스하는 ClassLoader가 포함되어 있습니다.

그런 다음 HTTP를 통해 원시 바이트 코드 파일을 로드하고 이를 JVM 내부의 클래스로 바꿉니다. 이 **애플릿의 이름이 같은 경우에도 서로 다른 클래스 로더에 의해 로드되는 경우 다른 구성요소로 간주**됩니다.

이제 사용자 지정 클래스 로더가 관련된 이유를 이해했습니다. 이제 ClassLoader 하위 클래스를 구현하여 JVM 로딩 방식의 기능을 확장하고 요약하겠습니다.



### Creating our Custom Class Loader

예를 들어 사용자 지정 클래스 로더를 사용하여 파일에서 클래스를 로드해야 한다고 가정해 보겠습니다.

**ClassLoader 클래스를 확장하고 findClass() 메서드를 재정의해야 합니다.**

```java
public class CustomClassLoader extends ClassLoader {
 
    @Override
    public Class findClass(String name) throws ClassNotFoundException {
        byte[] b = loadClassFromFile(name);
        return defineClass(name, b, 0, b.length);
    }
 
    private byte[] loadClassFromFile(String fileName)  {
        InputStream inputStream = getClass().getClassLoader().getResourceAsStream(
                fileName.replace('.', File.separatorChar) + ".class");
        byte[] buffer;
        ByteArrayOutputStream byteStream = new ByteArrayOutputStream();
        int nextValue = 0;
        try {
            while ( (nextValue = inputStream.read()) != -1 ) {
                byteStream.write(nextValue);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        buffer = byteStream.toByteArray();
        return buffer;
    }
}
```

위의 예에서는 기본 클래스 로더를 확장하고 지정된 파일에서 바이트 배열을 로드하는 사용자 지정 클래스 로더를 정의했습니다.



##  *java.lang.ClassLoader에 대한 이해*

java.lagng의 몇 가지 필수적인 방법에 대해 토론해 보겠습니다. ClassLoader 클래스는 작동 방식을 보다 명확하게 보여줍니다.



### The *loadClass()* Method

```java
public Class<?> loadClass(String name, boolean resolve) throws ClassNotFoundException {
```

이 메서드는 지정된 이름 매개 변수를 사용하여 클래스를 로드합니다. 이름 매개 변수는 정규화된 클래스 이름을 나타냅니다.

Java 가상 시스템은 loadClass() 메서드를 호출하여 클래스 참조 설정이 true로 확인되도록 합니다. **그러나 클래스를 해결할 필요가 항상 있는 것은 아닙니다. 클래스가 있는지 여부만 확인하면 확인 매개 변수가 false로 설정됩니다.**

이 방법은 클래스 로더의 진입점 역할을 합니다.

java.lang의 소스 코드에서 loadClass() 메서드의 내부 작업을 이해할 수 있습니다. ClassLoader는 다음과 같습니다.

```java
protected Class<?> loadClass(String name, boolean resolve)
  throws ClassNotFoundException {
     
    synchronized (getClassLoadingLock(name)) {
        // First, check if the class has already been loaded
        Class<?> c = findLoadedClass(name);
        if (c == null) {
            long t0 = System.nanoTime();
                try {
                    if (parent != null) {
                        c = parent.loadClass(name, false);
                    } else {
                        c = findBootstrapClassOrNull(name);
                    }
                } catch (ClassNotFoundException e) {
                    // ClassNotFoundException thrown if class not found
                    // from the non-null parent class loader
                }
 
                if (c == null) {
                    // If still not found, then invoke findClass in order
                    // to find the class.
                    c = findClass(name);
                }
            }
            if (resolve) {
                resolveClass(c);
            }
            return c;
        }
    }
```

메서드의 기본 구현에서는 다음 순서로 클래스를 검색합니다.

1. findLoadedClass(String) 메서드를 호출하여 클래스가 이미 로드되었는지 확인합니다.
2. 상위 클래스 로더에서 loadClass(String) 메서드를 호출합니다.
3. findClass(String) 메서드를 호출하여 클래스를 찾습니다.



### The *defineClass()* Method

```java
protected final Class<?> defineClass(
  String name, byte[] b, int off, int len) throws ClassFormatError
```

이 방법은 바이트 배열을 클래스의 인스턴스로 변환하는 역할을 합니다. 그리고 class 사용하기 전에 해결해야 합니다.

데이터에 유효한 클래스가 없는 경우 ClassFormatError가 발생합니다.

또, 이 방법이 최종으로 표시되어 있기 때문에 무효화할 수 없습니다.



### The *findClass()* Method

```java
protected Class<?> findClass(
  String name) throws ClassNotFoundException
```

이 메서드는 정규화된 이름을 가진 클래스를 매개 변수로 찾습니다. 클래스를 로드하기 위해 위임 모델을 따르는 사용자 지정 클래스 로더 구현에서 이 방법을 재정의해야 합니다.

또한 상위 클래스 로더가 요청된 클래스를 찾을 수 없는 경우 loadClass()가 이 방법을 호출합니다.

클래스 로더의 상위 로더가 클래스를 찾지 못할 경우 기본 구현에서 ClassNotFoundException을 실행합니다.



### The *getParent()* Method

```java
public final ClassLoader getParent()
```

이 메서드는 위임에 대한 상위 클래스 로더를 반환합니다.

섹션 2에 나와 있는 것과 같은 일부 구현에서는 null을 사용하여 부트스트랩 클래스 로더를 나타냅니다.



### The *getResource()* Method

```java
public URL getResource(String name)
```

이 방법은 지정된 이름의 리소스를 찾으려고 합니다.

먼저 리소스에 대한 상위 클래스 로더에 위임합니다. **상위 로더가 null이면 가상 시스템에 내장된 클래스 로더의 경로가 검색됩니다.**

실패하면 findResource(String)를 호출하여 리소스를 찾습니다. 입력으로 지정된 리소스 이름은 클래스 경로에 상대적이거나 절대적일 수 있습니다.

리소스를 읽을 수 있는 URL 개체를 반환하거나, 리소스를 찾을 수 없거나 호출자가 리소스를 반환할 수 있는 적절한 권한이 없는 경우 null을 반환합니다.

Java는 클래스 경로에서 리소스를 로드합니다.

마지막으로, **Java에서 리소스 로딩은** 환경이 리소스를 찾도록 설정된 경우 코드가 실행되고 있는 위치에 관계없이 위치 **독립적인 것으로 간주됩니다.**



## Context Classloaders

일반적으로 컨텍스트 클래스 로더는 J2SE에 도입된 클래스 로딩 위임 체계에 대한 대체 방법을 제공합니다.

앞서 살펴본 것처럼 **JVM의 클래스 로더는 모든 클래스 로더가 부트스트랩 클래스 로더를 제외한 단일 상위 로더를 갖도록 계층적 모델을 따릅니다.**

그러나 JVM 코어 클래스가 애플리케이션 개발자가 제공하는 클래스나 리소스를 동적으로 로드해야 하는 경우 문제가 발생할 수 있습니다.

예를 들어, JNDI에서는 핵심 기능이 rt.jar의 부트스트랩 클래스에 의해 구현됩니다. 그러나 이러한 JNDI 클래스는 독립 공급업체에서 구현한 JNDI 공급자를 로드할 수 있습니다(애플리케이션 클래스 경로에 배포됨). 이 시나리오에서는 부트스트랩 클래스 로더(상위 클래스 로더)가 애플리케이션 로더(하위 클래스 로더)에 표시되는 클래스를 로드해야 합니다.

**J2SE 대표단은 여기서 작동하지 않으며, 이 문제를 해결하려면 다른 수업 로드 방법을 찾아야 합니다. 또한 스레드 컨텍스트 로더를 사용하여 달성할 수 있습니다.**

java.lang.Thread의 클래스에는 특정 스레드에 대한 ContextClassLoader를 반환하는 메서드 getContextClassLoader()가 있습니다. ContextClassLoader는 리소스 및 클래스를 로드할 때 스레드의 생성자가 제공합니다.

값이 설정되지 않으면 기본적으로 상위 스레드의 클래스 로더 컨텍스트로 설정됩니다.



## 결론

Java 프로그램을 실행하려면 클래스 로더가 필수적입니다. 이 내용의 일부로 좋은 소개를 제공해 드렸습니다.

부트스트랩, 확장 및 시스템 클래스 로더 등 다양한 유형의 클래스 로더에 대해 설명했습니다. Bootstrap은 모든 Bootstrap의 상위 역할을 하며 JDK 내부 클래스를 로드하는 역할을 합니다. 반면 확장 및 시스템은 각각 Java 확장 디렉토리와 클래스 경로에서 클래스를 로드합니다.

그런 다음 클래스 로더의 작동 방식에 대해 이야기하고 위임, 가시성, 고유성 등의 몇 가지 기능에 대해 논의한 후 맞춤형 로더를 만드는 방법에 대해 간략하게 설명했습니다. 마지막으로 컨텍스트 클래스 로더에 대한 소개를 제공했습니다.

코드 샘플은 항상 [GitHub](https://github.com/eugenp/tutorials/tree/master/core-java/src/main/java/com/baeldung/classloader)에서 찾을 수 있습니다.



## 참고

- https://www.baeldung.com/java-classloaders

