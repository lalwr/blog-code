# Java Performance Fundamental



## Java virtual Machine

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