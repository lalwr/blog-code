# Java Performance Fundamental



## Java virtual Machine

### Java Architecture

- 연관된 기술
  - The Java Programming Language
  -  The Java Class File Format
  - The Java Application Programming Interface (Java API)
  - The Java Virtual Machine (JVM)
- Java Source Code는 jdk 에 내장되어 있는 'javac'라는 Comiler를 통해 class 확장자를 가지는 Binary 파일이 생성
- class 파일을 수행하기 위해서는 적어도 JRE, Java Runtime Environment라는 것이 필요
- 'java'는 Java Virtual Machine을 하나의 프로세스로 올리며 Class 파일의 로딩도 수행
- Class 파일을 분석하여 JRE 내 에 있는 Java Application Program Interface(Java API)와 더불어 프로그램 수행
- java라는 확장자를 가진 Source 파일로 Class 파일을 생성하는 시점을 가리켜 Compile Time
- Compile을 거쳐 생성된 Class 파일을 수행하는 시점이 RunTime

![image-20200606005912316](.\images\jvm)

- JVM은 단독으로 수행되는 것이 아니라 JAva Application Programming Interface 와 동적으로 연결되어 실행



### The Java Programming Language

#### Dynamic Linking

Java의 Class 파일은 실행 가능한 형태로 변경된 것이 아닌 JVM 이 읽을 수 있는 형태로 번역된 것이며 JVM 위에서 실행 가능한 형태로 변형된다. 실행을 위한 Linking 작업은 이때 일어나게 된다. Class 파일은 실행시 Link를 할 수 있도록 Symbolic Reference 만을 가지고 있다. 

Symbolic Reference는 Reuntime시점에서 메모리상에서 실제로 존재하는 물리적인 주소로 대치되는 작업인 Linking이 일어나게 된다. Link 작업은 필요할 때마다 동적으로 이루어지며 Dynamic Linking이라고 한다.

Dynamic Linking 기술 때문에 Class 파일의 크기를 작게 유지할 수 있다. Java의 철학을 구현한다는 의미로 확대되며 Java는 플랫폼에 독립적이기 떄문에 Compile 된 파일만 있으면 수행이 가능하다. 

