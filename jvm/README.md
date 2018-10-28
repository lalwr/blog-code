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

# JVM 구조

### class Loader

RunTime 시점에 클래스를 로딩시키며 클래스의 인스턴스를 생성하면 Class Loader를 통해 메모리에 로드한다.

### Runtime Data Areas

JVM이 프로그램을 수행하기 위해 OS로 부터 별도로 할당 받은 메모리 공간을 말하며, Runtime Data Areas는 크게 5가지 영역으로 나눌 수 있다.

### Execution Engine

Load된 Class의 ByteCode를 실행하는 Runtime Module이다. Class Loader를 통해 JVM 내의 Runtime Data Areas 에 배치된 바이트 코드는 Executin Engine에 의해 실행되며, 실행 엔진은 자바 바이트 코드를 명령어 단위로 읽어서 실행한다.

### Garbage Collector

JVM은 Garbage Collector를 통해 메모리 관리 기능을 자동으로 수행한다. 애플리케이션이 생성한 객체의 생존 여부를 판단하여 더 이상 사용되지 않는 객체를 해제하는 방식으로 메모리를 자동 관리한다.

# Runtime Data Areas

- **Method (Static) Area**: JVM이 읽어들인 클래스와 인터페이스 대한 런타임 상수 풀, 멤버 변수(필드), 클래스 변수(Static 변수), 생성자와 메소드를 저장하는 공간

- **Runtime Constant Pool**
  - 메소드 영역에 포함되지만 독자적 중요성이 있다.
  - 클래스 파일 constant_pool 테이블에 해당하는 영역
  - 클래스와 인터페이스 상수, 메소드와 필드에 대한 모든 레퍼런스를 저장
  - JVM은 런타임 상수 풀을 통해 해당 메소드나 필드의 실제 메모리 상 주소를 찾아 참조

- **메소드 영역/런타임 상수 풀의 사용기간 및 스레드 공유 범위**

  - JVM 시작시 생성
  - 프로그램 종료 시까지
  - 명시적으로 null 선언 시

  - 구성 방식이나 GC 방법은 JVM 벤더마다 다를 수 있다.
  - 모든 스레드에서 공유