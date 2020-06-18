# Java Performance Fundamental



## Java virtual Machine

### Java Architecture

- 연관된 기술
  - The Java Programming Language
  -  The Java Class File Format
  - The Java Application Programming Interface (Java API)
  - The Java Virtual Machine (JVM)
- Java Source Code는 JDK 에 내장되어 있는 'Javac'라는 Compiler를 통해 class 확장자를 가지는 Binary 파일이 생성
- class 파일을 수행하기 위해서는 적어도 JRE, Java Runtime Environment라는 것이 필요
- 'Java'는 Java Virtual Machine을 하나의 프로세스로 올리며 Class 파일의 로딩도 수행
- Class 파일을 분석하여 JRE 내 에 있는 Java Application Program Interface(Java API)와 더불어 프로그램 수행
- Java라는 확장자를 가진 Source 파일로 Class 파일을 생성하는 시점을 가리켜 Compile Time
- Compile을 거쳐 생성된 Class 파일을 수행하는 시점이 RunTime

![](.\images\jvm.png)

- JVM은 단독으로 수행되는 것이 아니라 Java Application Programming Interface 와 동적으로 연결되어 실행



### The Java Programming Language

#### Dynamic Linking

Java의 Class 파일은 실행 가능한 형태로 변경된 것이 아닌 JVM 이 읽을 수 있는 형태로 번역된 것이며 JVM 위에서 실행 가능한 형태로 변형된다. 실행을 위한 Linking 작업은 이때 일어나게 된다. Class 파일은 실행시 Link를 할 수 있도록 Symbolic Reference 만을 가지고 있다. 

Symbolic Reference는 Runtime시점에서 메모리상에서 실제로 존재하는 물리적인 주소로 대치되는 작업인 Linking이 일어나게 된다. Link 작업은 필요할 때마다 동적으로 이루어지며 Dynamic Linking이라고 한다.

Dynamic Linking 기술 때문에 Class 파일의 크기를 작게 유지할 수 있다. Java의 철학을 구현한다는 의미로 확대되며 Java는 플랫폼에 독립적이기 떄문에 Compile 된 파일만 있으면 수행이 가능하다. 

Java는 Runtime Memory를 직접 핸들링 하지 않아 생산성이 높고 객체지향의 특성인 Source Code의 재사용, ArrayBound Check 등과 같은 엄격한 Type Rule, Object Reference Check등을 통해 프로그램의 안정성의 제고 등도 Java의 장점이다.



### The Java Class File Format

Java는 Compiler를 통해 Class 파일로 재생성 되는 과정을 거치며 네가지 특징이 있다.

- Compact 한 형태
- ByteCode 로의 변경
- Platform 독립적
- Network Byte Order의 사용

Java의 철학을 가장 대변 하는 것이 Class File Format이다. Class 파일은 Btyecode 를 Binary 형태로 담아놓은 것이며 Bytecode는 JVM이 읽을 수 있는 언어를 의미한다. 

JVM은 Class를 로딩한 후 Bytecode을 읽어 실행히 가능하도록 해석(Interpret)하는 과정을 거친다. Class가 Load된 후에는 JIT Compiler 나 Hotspot Compiler 와 같은 **Execution Engine**을 거쳐 실행된다. JVM이 Btyecode를 읽는 한 JVM이 어디에 설치되어 있는지는 상관이 없다. **'Run Everywhere'**가 실현된다.

Bytecode는 Source Code를 단순히 JVM의 언어로 번역해 놓은 것이 때문에 Source Code와 비슷한 크기를 가진다. 작은 크기를 유지할 수 있는 이유는 Class 파일에 실제로 라이브러리를 포함하고 있지 않고, 단지 Symbolic Reference만을 가지고 있기 때문이다.

**Symbolic Reference**는 참조하고자 하는 대상의 이름만으로 참조관계를 구성한 것을 의미한다. 특정 메모리 번지로 참조관계를 구성한 것이 아닌 참초하는 대상의 이름만을 지칭한다. Class 파일이 JVM에 올라가면 Symbolic Reference는 그 이름에 맞는 객체의 주소를 찾아 연결하는 작업을 수행한다. 이러한 작업을 **Dynamic Liking**이라고 한다. Java는 Dynamic Linking 덕분에 Class 파일은 Compact 한 형태를 유지할 수 있다.

Class File Format의 마지막 특징은 Network Byte Order를 사용한다. ByteOrder는메모리의 주소 값을 할당하는 방식을 의미한다. 메모리 주소의 번지수를 점점 작게 부여하는 것을 뜻한다. Intel 계열의 CPU 는 Little Endian 이란느 ByteOrder를 사용하고 Unix 머신에서 주로 사용되는 CPU 는 Big Endian 방식을 사용한다. 두 CPU에 맞게 생성된 프로그램들은 서로 호환이 되지 않는 문제가 있다.

**Network Byte Order**는 서로 다른 계열의 CPU 끼리 데이터를 전송 받을때의 문제점을 해결하기 위해 정해진 약속이다. 다른 계열에게 Byte Order를 고려하지 않고 보낸다면 서로의 데이터는 주소가 반대로 되어 있어 제대로 전송되지 않을 것이다. 그렇기 때문에 Network를 통해 데이터를 전송할 때는 통일된 방식을 따르기로 약속을 했는데 그것이 Network Byte Order 이다. Big Endian을 사용하기로 약속이 되어있다.

이러한 고려는 Java가 설계 부터 Network를 활용하겠다는 것을 알 수 있다. 



### The Java Application Interface

Java API는 Runtime Library의 집합이라고 할 수 있다. 앞에서 Class 파일을 수행하기 위해서는 JER가 필요하다고 했다.  JRE는 Java 실행 환경이다. Java Virtual Machine과 Java API, Native Method 등이 포함되어 있다.

JavaAPI는 OS과 Java 프로그램을 이어주는 역할을 한다. Native Method를 통해 OS 자원과 연계되어 있고 다른 한편으로는 Java 프로그램과 맞딱드리고 있다. Interface의 역할을 하고 있는 셈이다.

![](.\images\java_api.png)

Java를 사용하면 파일 시스템의 특정 정보를 읽기 위해서 Platform에 대해 고민 할 필요가 없다. java.io.InputStream을 사용하기만 하면 원하는 정보를 가지고 올 수 있다.

Class 파일 내에 있는 java.io.InputStream의 Symbolic Reference를 이용하여 Runtime시 해당 Instance에 접근하게 된다. 실제 File에 대한 접근은 Native Method를 통해 OS에 명령을 전달한다. 이후 OS는 실제로 File IO가 실행되고, File IO의 결과가 Native Method를 통해 Java API로 전달되서 프로그램이 실행된다.

Java 프로그램은 단지 Reference를 호출할 뿐이다. 이는 Java Interface를 이용한다는 의미한다. OS와 관계없이 JER가 알아서 수행한다. JRE는 OS나 머신데 따라 설치해야 하지만 설치된 JRE는 모든 Java 프로그램에 동일한 실행환경을 제공한다.



### The Java Virtual Machine(JVM)

Java의 4가지 구송요소 중 핵심적인 것은 Java Virtual Machine이다. JVM은 Java를 위한 것이며 물리적인 형태가 아닌 Software 로 하나의 개념으로 존재한다. 이것이 **VIRTUAL** 이 뜻하는 것이다. 그리고 JVM 독자적으로 작동할 수 있는 메커니즘과 구조를 가지고 있다. 이는 하나의 축약된 컴퓨터와 같다. 이러한 의미에서 **MACHINE**이라는 단어을 사용하게 된 것이다.

'JVM이 무엇일까? ' JVM은 하나의 개념, 스펙에 지나지 않는다. JVM은 설계도를 만들어 제공하지 않는다. 단지 이렇게 저렇게 해야 한다는 식의 정이만 존재한다. 이러한 표준화된 정의를 기반으로 JVM 벤더들은 자신들의 JVM을 구현한다.

