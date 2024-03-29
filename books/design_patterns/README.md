# Design Patterns

> Head First Design Patterns책을 보면서 내용들을 정리한다



## 객체지향 원칙

- 바뀌는 부분을 캡슐화 한다.
- 상속보다는 구성을 활용한다.
- 구현이 아닌 인터페이스에 맞춰서 프로그래밍한다.
- 서로 상호작용을 하는 객체 사이에서는 가능하면 느슨하게 결합하는 디자인을 사용해야 한다.
- 클래스는 확장에 대해서는 열려있지만 변경에 대해서는 닫혀 있어야한다.(OCP)
- 추상화된 것에 의존하라. 구상 클래스에 의존하지 않도록 한다.
- 친한 친구들하고만 이야기 한다.
- 먼저 연락하지 마라. 내가 연락을 하라.
- 어떤 클래스가 바뀌게 되는 이유는 한가지 뿐이어야 한다.



## 디자인 원칙

- 애플리케이션에서 달라지는 부분을 찾아내고, 달라지지 않는 부분으로부터 분리시킨다.
- 구현이 아닌 인터페이스에 맞춰서 프로그래밍한다.
- 상송보다는 구성을 활용한다.
- 클래스를 바꾸는 이유는 한 까지 뿐이어야 한다.



## 스트래티지 패턴

**스트래티지 패턴**은 알고리즘군을 정의하고 각각을 캡슐화하여 교환해서 사용할 수 있도록 만든다. 스트래티지을 활용하면 알고리즘을 사용하는 클라이언트와는 독립적으로 알고리즘을 변경할 수 있다.

상속을 통해 구현시 일부 서브클래스에서는 수퍼클래스에 메소드가 추가되었을때 적합하지 않는 메소드를 추가 하게된다. 인터페이스를 통해 구현 하여도 코드 재사용성을 할 수 가 없다.





## 옵져버 패턴

**옵져버 패턴은** 한 객체의 상태가 바뀌면 그 객체에 의존하는 다른 객체들한테 연락이 가고 자동으로 내용이 갱신되는 방식으로 일대다 의존성을 정의한다. 옵져버 패턴에서는 주제와 옵저버가 느슨하게 결합되어 있는 객체 디자인을 제공한다.

구체적인 구현에 맞춰서 코딩을 하면 프로그램을 고치지 않고 다른 항목을 추가/제거 할수 없다. 공통된 인터페이스를 사용하는 부분을 캡슐화 할 수 있다.



## 데코레이터 패턴

**데코레이터 패턴은** 객체에 추가적인 요건을 동적으로 첨가한다. 서브클래스를 만드는것을 통해서 기능을 유연하게 확장할 수 있는 방법을 제공한다.

상속만으로 구현하면 요구사항이 변경될때마다 기존 코드를 수정해야 해서 유연하고 관리하기 쉬운 디자인을 만들수 없다. 상속으로 구현하면 행동은 컴파일시에 행동이 결정되고 모든 서브스클래스에서 똑같은 행동을 상속 받아야 한다. 하지만 구성을 통해서 객체의 행동을 확장하면 실행중에 동적으로 행동을 설정할 수 있다. 즉 OCP적인 코드를 구현 할 수 있다.

자바 I/O API는 많은 부분이 데코레이터 패턴을 바탕으로 만들어져있다. 

패턴의 단점으로는 자잘한 클래스가 엄청나게 추가되서 남들이 봤을 때 이해하기 힘든 디자인이 만들어 진다. 가장 큰 장점 으로 데코레이터를 끼워넣어도 클라이언트 쪽에서는 데코레이터를 사용하고 있다는 것을 전혀 알 수 없지만 특정 형식에 의존하는 코드에 데코레이터를 적용하면 엉망이 되어버리고 구성 요소를 초기화하는 데 필요한 코드가 훨씬 복잡해진다.



## 팩토리 패턴

**팩토리 패턴**에서는 객체 생성을 처리(바뀌는 부분) 하는 캡슐화 하여 클래스를 만든다. 팩토리 패턴에서는 각자 독자적인 방법을 통해 사용하면서 기능이 빠지는 경우가 있다. 

**팩토리 메소드 패턴**에서는 객체를 생성하기 위한 인터페이스를 정의하는데, 어떤 클래스의 인스턴스를 만들지는 서브클래스에서 결정하게 만듭니다. 팩토리 메소드 패턴을 이용하면 클래스의 인스턴스를 만드는 일을 서브클래스에게 맡기는 것이다.

팩토리 메소드 선언을 통해서 객체 생성을 처리하며, 객체를 생성하는 작업을 서브클래스에 캡슐화시킬 수 있습니다. 슈퍼클래스에 있는 클라이언트 코드와 서브스클래스에 있는 객체 생성 코드를 분리시킬 수 있습니다.
모든 팩토리 패턴에서는 객체 생성을 캡슐화합니다. 
팩토리 메소드 패턴에서는 서브클래스에서 어떤 클래스를 만들지를 결정하게 함으로써 객체 생성을 캡슐화합니다. 팩토리 패턴에서의 단점을 보완할 수 있다.

**의존성 디자인 원칙**이란 추상화된 것에 의존하도록 만들어라. 구상 클래스에 의존하도록 만들지 않도록 한다. 이 원칙은 고수준 구성요소가 저수준 구성요소에 의존하면 안 된다는 것이 내포되어 있습니다. 원칙을 위배되는 객체지향 디자인은 아래와 같습니다.

- 어떤 변수에도 구상 클래스에 대한 레퍼런스를 저장하지 말자.
- 구상 클래스에서 유도된 클래스를 만들지 말자.
- 베이스 클래스에 이미 구현되어 있던 메소드를 오버라이드하지 말자.

**추상팩토리 패턴에서는** 서로 연관된, 의존적인 객체들로 이루어진 제품군을 생성하기 위한 인터페이스를 제공합니다. 객체 구성을 활용하여 객체 생성이 팩토리 인터페이스에 선언한 메소드들에서 구현됩니다. 



## 싱글턴 패턴

**싱글턴 패턴은** 특정 클래스에 대해서 객체 인스턴스가 하나뿐인 객체를 만들 수 있게 해주고 어디서든지 그 인스턴스에 접근할 수 있도록 하기위한 패턴이다. 

생성자를 Privete 접근 권한자로 생성해 자신의 클래스 안에서만 인스턴스를 생성할 수 있다. 
클래스 로더가 여러 개 있으면 여러개의 인스턴스가 생길 수 있다.

인스턴스를 필요할때 생성하면 멀티스레드 환경에서는 객체가 한개의 객체보다 더 생성될 수 있어서 객체가 생성되는 로직에 동기화 처리를 할 수 있지만 성능이 100배정도 저하된다. 그래서 처음부터 인스턴스를 만들어 버리는 방법과 DCL을 통해서 인스턴스가 생성되는 처음에만 동기화 처리를 하는 방법이 있다. DCL방법은 1.4이전 버전에서는 쓸 수 없다.



## 커맨드 패턴

**커맨더 패턴**은 요구 사항을 객체로 캡슐화 할 수 있으며, 매개변수를 써서 여러가지 다른 요구사항을 집어넣을수도 있습니다. 또한 요청 내역을 큐에 저장하거나 로그로 기록할 수도 있으며, 작업취소 기능도 지원 가능합니다.

요청을 하는 객체와 그 요청을 행하는 객체를 분리시킬 수 있습니다. 분리시키는 과정의 중심에는 커맨드 객체가 있으며, 이 객체가 행동이 들어있는 리시버를 캡슐화 합니다. 커맨드 패턴을 활용하여 로그 및 트랜잭션 시스템을 구현하는 것도 가능합니다.



## 어댑터 패턴과 퍼사드 패턴

**어댑터 패턴**은 한 클래스의 인터페이스를 클라이언트에서 사용하고자 하는 다른 인터페이스로 변한합니다. 인터페이스가 호환되지 않아 쓸 수 없었던 클래스들을 같이 사용할 수 있게 해줍니다.

**퍼사드 패턴**은 서브시스템에 있는 일련의 인터페이스에 대한 통합 인터페이스를 제공합니다. 퍼사드 패턴에서는 서브시스템을 더 쉽게 사용할 수 있게 해 주는 고수준 인터페이스를 정의합니다. 



## 템플릿 메소드 패턴

**템플릿 매소드 패턴**에서는 메소드에서 알고리즘의 골격을 정의합니다. 알고리즘의 여러 단계 중 일부는 서브클래스에서 구현할 수 있습니다. 템플릿 메소드를 이용하면 알고리즘의 구조는 그대로 유지하면서 서브스클래스에서 특정 단계를 재정의 할 수 있습니다.



## 이터레이터와 컴포지트 패턴

**이터레이터 패턴**은 컬렉션 구현 방법을 노출시키지 않으면서도 그 집합체 안에 들어있는 모든 항목에 접근할 수 있게 해 주는 방법을 제공해 줍니다. 내부적인 구현 방법을 오부로 노출시키지 않으면서도 집합체에 있는 모든 항목에 접근할 수 있습니다.

**컴포지트 패턴**은 객체들을 트리구조로 구성하여 부분과 전체를 나타내는 계층구조로 만들 수 있습니다. 이 패턴을 이용하면 클라이언트에 개별 객체와 다른 객체들로 구성된 복합 객체를 똑같은 방법으로 다룰 수 있습니다. 



## 스테이트 패턴

**스테이트 패턴**을 이용하면 객체의 내부 상태가 바뀜에 따라서 객체의 행동을 바꿀 수 있습니다. 마치 객체의 클래스가 바뀌는 것과 같은 결과를 얻을 수 있습니다. 스트래티지 패턴과 다이어그램은 똑같지만 차이점이 있다. 스테이트 패턴을 사용할 때는 상태 객체에 일련의 행동이 캡슐화 됩니다. 상황에 따라 여러 상태 객체 중 한객체에게 모든 행동을 맡깁니다. 그 객채의 내부 상태에 따라 현재 상태를 나타내는 객체가 바뀌게 되고, 그 결과 객체의 행동도 자연스럽게 바뀌게 됩니다. 클라이언트는 상태 객채에 대해서 거의 아무것도 몰라도 됩니다. 하지만 스트래티지 패턴을 사용할 때는 일반적으로 어떤 전략 객체를 사용할지를 지정해 줍니다. 주로 실행시에 전략 객체를 변경할 수 있는 유연성을 제공하기 위한 용도로 쓰입니다. 

일반적으로 스트래티지 패턴은 서브클래스를 만드는 방법을 대신하여 유연셩을 극대화하기 위한 용도로 쓰입니다. 상속을 이용해서 클래스의 행동을 정의하면 변경해야 할 때 마음대로 변경하기 힘듭니다. 하지만 스트래티지 패턴을 사용하면 구성을 통해 행동을 정의하는 객체를 유연하게 바꿀 수 있습니다. 



## 프록시 패턴

**프록시 패턴**은 다른 객체를 대변하는 객체를 만들어서 주 객체에 대한 접근을 제어할 수 있습니다. 데코레이터 패턴의 구조하고 비슷하지만 그 용도가 다르다는 차이점이 있습니다. 데코레이터 패턴에서는 객체에 행동을 추가하지만 프록시 패턴에서는 접근을 제어합니다. 



## 컴파운드 패턴

**컴파운드 패턴**은 두 개 이상의 패턴을 결합하여 일반적으로 자주 등장하는 문제들에 대한 해법을 제공합니다. 



## 패턴 분류

### 생성 관련 패턴

객체 인스턴스 생성을 위한 패턴으로, 클라이언트와 그 클라이언트에서생성해야 할 객체 인스턴스 사이의 연결을 끊어주는 패턴입니다.

- 싱글턴
- 추상 팩토리
- 팩토리 메소드

### 구조 관련 패턴

클래스 및 객체들을 구성을 통해서 더 큰 구조로 만들 수 있게 해주는 것과 관련된 패턴입니다.

- 데코레이터
- 컴포지트
- 어댑터
- 퍼사드
- 프록시

### 행동 관련 패턴

클래스와 객체들이 상호작용하는 방법 및 역할을 분담하는 방법과 관련된 패턴입니다.

- 템플릿 메소드
- 커맨드
- 이터레이터
- 옵저버
- 스테이트
- 스트래티지

## 클래스 패턴

클래스 사이의 관계가 상속을 통해서 어떤 식으로 정의되는지를 다룹니다. 클래스 패턴에서는 컴파일시에 관계가 결정됩니다.

- 템플릿 메소드
- 팩토리 메소드
- 어댑터

## 객체 패턴

객체 사이의 관계를 다루며, 객체 사이의 관계는 보통 구성을 통해서 정의도비니다. 객체 패턴에서는 일반적으로 실행 중에 관계가 생성되기 때문에 더 동적이고 유연합니다.

- 컴포지트
- 데코레이터
- 프록시
- 스트래티지
- 추상 팩토리
- 이터레이터
- 옵저버
- 퍼사드
- 스테이트
- 싱글턴
- 커맨드




## 참고

- https://github.com/lalwr/Head-First-Design-Patterns
- [HEAD FIRST DESIGN PATTERNS](http://book.naver.com/bookdb/book_detail.nhn?bid=1882446)