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





