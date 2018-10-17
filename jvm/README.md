# JVM(Java Virtual Machine)

- Java로 개발한 프로그램을 컴파일하여 만들어지는 바이트코드를 실행시키기 위한 가상머신
- javac 컴파일러를 통해 바이트코드로 변한되며, JRE에 있는 classloader를 통해 JVM으로 적재되고 JIT 컴파일 방식으로 실행
- JVM은 독립적이므로 JVM이 실행되는 환경이라면 어디서든지 실행이 가능
- JVM은 크게 Class Loader, Runtime Data Areas, Excution Engine 3가지로 구성
- 특징
  - 스택 기반의 가상 머신
  - 심볼릭 레퍼런스
  - 가비지 컬렉션
  - 기본 자룧령을 명확하게 정의하여 플랫폼 독립성 보장
  - 네트워크 바이트 오더

