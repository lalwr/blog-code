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

![image-20200606005912316](.\images\jvm)

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



