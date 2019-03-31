# Java 12



## 출시일

2019년 3월 19일



## 개선 사항

자바 12의 새로운 개선 사항에 대해 알아봅시다.



### 스위치 익스프레션(Switch expressions) 

베타 스위치 익스프레션 기능은 switch 구문을 확장해 구문 또는 식으로 사용할 수 있도록 한다. 이를 통해 코딩을 간소화할 수 있다. 두 형식 모두 "전통적인" 또는 "간소화된" 범위 및 흐름 제어 동작을 사용할 수 있게 된다. 이런 변화를 통해 "일상적인" 코딩이 더 간소화되고 switch에서 패턴 매칭(pattern matching)을 사용하기 위한 기반을 마련했다.

자바 제작자들이 패턴 매칭을 지원하도록 전환함에 따라 기존 switch 문의 불일치가 장애물이 된다. 여기에는 switch 블록의 기본 흐름 제어 동작, 블록이 단일 범위로 취급되는 switch 블록의 기본 범위 지정, 문으로만 작동하는 switch 등이 포함된다.

현재 자바의 switch 문 동작은 기본적으로 C++와 같은 언어와 거의 비슷하고 fallthrough 의미 체계를 지원한다. 이 흐름 제어는 저수준 코드를 작성하는 데 유용하다. 그러나 switch가 더 높은 수준의 컨텍스트에서 사용되면 특유의 잦은 오류로 인해 유연함으로 얻는 이점이 무의미해진다.

예를 들어, 다음 코드에서 많은 `break`문이 불필요하게 장황하게 만들고, 시각적 인 노이즈로 인해 오류를 디버그하기가 어려울 수 있습니다. 누락 된 `break`문으로 우발적 인 오류 가 발생한다는 의미입니다.

```java
switch (day) {
    case MONDAY:
    case FRIDAY:
    case SUNDAY:
        System.out.println(6);
        break;
    case TUESDAY:
        System.out.println(7);
        break;
    case THURSDAY:
    case SATURDAY:
        System.out.println(8);
        break;
    case WEDNESDAY:
        System.out.println(9);
        break;
}
```

레이블의 일치하는 경우 레이블 오른쪽의 코드 만 실행된다는 것을 나타내는 "case L ->"형식의 새 스위치 레이블을 소개합니다. 예를 들어 이전 코드를 다음과 같이 작성할 수 있습니다.

```java
switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> System.out.println(6);
    case TUESDAY                -> System.out.println(7);
    case THURSDAY, SATURDAY     -> System.out.println(8);
    case WEDNESDAY              -> System.out.println(9);
}
```

이 예제는 또한 여러 사례 레이블을 사용합니다. 단일 스위치 레이블에 쉼표로 구분 된 여러 레이블을 지원하는 것이 좋습니다.

"case L ->"스위치 레이블의 오른쪽에있는 코드는 표현식, 블록 또는 (편의상) throw 문으로 제한됩니다. 이것은 구문이 지역 변수를 도입 할 때 블럭에 포함되어야하고 따라서 스위치 블럭의 다른 구문의 범위에 포함되어서는 안되는 결과를 낳습니다. 이렇게하면 로컬 변수의 범위가 전체 스위치 블록 인 "전통적인"스위치 블록의 또 다른 성가심을 제거 할 수 있습니다.

```java
switch (day) {
    case MONDAY:
    case TUESDAY:
        int temp = ...
        break;
    case WEDNESDAY:
    case THURSDAY:
        int temp2 = ...     // Why can't I call this temp?
        break;
    default:
        int temp3 = ...     // Why can't I call this temp?

```

많은 기존 switch 문은 본질적으로 스위치 식을 시뮬레이션합니다. 각 구문은 공통 대상 변수에 할당하거나 값을 반환합니다.

```java
int numLetters;
switch (day) {
    case MONDAY:
    case FRIDAY:
    case SUNDAY:
        numLetters = 6;
        break;
    case TUESDAY:
        numLetters = 7;
        break;
    case THURSDAY:
    case SATURDAY:
        numLetters = 8;
        break;
    case WEDNESDAY:
        numLetters = 9;
        break;
    default:
        throw new IllegalStateException("Wat: " + day);
}
```

이를 문장으로 표현하는 것은 우회적이고 반복적이며 오류가 발생하기 쉽습니다. 저자는 우리가 매일 numletters의 값을 계산해야 한다는 것을 표현하고자 했습니다. 스위치 식을 사용하여 보다 명확하고 안전하게 다음과 같이 직접 말할 수 있어야 합니다.

```java
int numLetters = switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> 6;
    case TUESDAY                -> 7;
    case THURSDAY, SATURDAY     -> 8;
    case WEDNESDAY              -> 9;
};
```

차례대로 스위치를 확장하여 표현식을 확장하면 흐름 분석 확장 (표현식은 항상 값을 계산하거나 갑작스럽게 완료해야 함)과 같은 일부 추가 요구 사항이 발생하고 스위치 표현식의 일부 경우에는 값을 산출하는 대신 예외가 발생합니다.

#### Description

"전통적인"스위치 블록에 추가하여 새로운 "단순화 된"양식과 새로운 "case L ->"스위치 레이블을 추가 할 것을 제안합니다. 레이블이 일치하면 화살표 레이블의 오른쪽에있는 있는 표현식 또는 문만 실행되며, 통과하지 않습니다. 예를 들어 다음과 같은 방법을 사용할 수 있습니다.

```java
static void howMany(int k) {
    switch (k) {
        case 1 -> System.out.println("one");
        case 2 -> System.out.println("two");
        case 3 -> System.out.println("many");
    }
}

howMany(1); //one
howMany(2); //tow
howMany(3); //many
```

switch 문을 확장하여 표현식으로 추가로 사용할 수 있습니다. 일반적인 경우 스위치 식은 다음과 같습니다.

```java
T result = switch (arg) {
    case L1 -> e1;
    case L2 -> e2;
    default -> e3;
};
```

스위치 식은 폴리 표현식입니다. 대상 유형을 알 경우 이 유형은 각 구문으로 푸시됩니다. 스위치 표현식 유형은 알려진 경우 대상 유형이며, 그렇지 않은 경우 각 케이스 구문의 유형을 결합하여 독립형 유형을 계산합니다.

대부분의 스위치 표현식은 "case L ->" 스위치 라벨의 오른쪽에 하나의 표현식을 가집니다. 전체 블록이 필요한 경우 인수를 수행하기 위해 중단 문을 확장했으며, 이는 동봉 스위치 식 값이 됩니다.

스위치 표현식은 폴리 표현식입니다. 목표 유형이 알려지면이 유형이 각 팔로 밀어 넣어집니다. 스위치 표현식의 유형은 알 수있는 경우 목표 유형입니다. 그렇지 않은 경우 독립 실행 형은 각 사례 암의 유형을 결합하여 계산됩니다.

대부분의 스위치 표현식은 "case L ->"스위치 레이블의 오른쪽에 단일 표현식을 갖습니다. 전체 블록이 필요할 경우 break 문을 확장하여 인수를 취합니다.이 인수는 둘러싸는 스위치 식의 값이됩니다.

```java
int j = switch (day) {
    case MONDAY  -> 0;
    case TUESDAY -> 1;
    default      -> {
        int k = day.toString().length();
        int result = f(k);
        break result;
    }
};
```

switch 표현식은 switch 문과 마찬가지로 "case L :"스위치 레이블 (fall-through 의미 체계를 의미 함)이있는 "전통적인"스위치 블록을 사용할 수도 있습니다. 이 경우 value with break 문을 사용하여 값이 산출됩니다.

```java
int result = switch (s) {
    case "Foo": 
        break 1;
    case "Bar":
        break 2;
    default:
        System.out.println("Neither Foo nor Bar, hmmm...");
        break 0;
};
```

두 가지 형태의 중단 (값이 있거나 없음)은 메소드에서 리턴의 두 가지 형태와 유사합니다. 두 가지 형태의 리턴은 즉시 메소드의 실행을 종료합니다. 비 - 공법 (non-void method)에서, 메소드의 호출자에게 주어진 값이 추가로 제공되어야한다. (구분 표현식 값과 구분 레이블 양식 간의 모호성은 비교적 쉽게 처리 할 수 있습니다.)

스위치 식의 경우는 철저해야합니다. 가능한 모든 값에 대해 일치하는 스위치 레이블이 있어야합니다. 실제로 이는 일반적으로 기본 절이 필요하다는 것을 의미합니다. 그러나 모든 알려진 경우를 다루는 enum 스위치 식 (결국 봉인 된 형식의 식을 전환 함)의 경우 컴파일러에서 열거 형 정의가 컴파일 시간과 런타임 사이에 변경되었음을 나타내는 기본 절을 삽입 할 수 있습니다. (이것은 개발자가 오늘날 손으로하는 일이지만, 컴파일러가 삽입하는 것은 방해가 적어 손으로 작성된 것보다 더 자세한 설명적인 오류 메시지를 가질 가능성이 높습니다.)

또한 스위치 식은 값으로 정상적으로 완료되거나 예외를 throw해야합니다. 이것은 여러 가지 결과를 낳습니다. 첫째, 컴파일러는 모든 스위치 레이블에 대해 일치하는 경우 값을 산출 할 수 있는지 확인합니다.

```java
int i = switch (day) {
    case MONDAY -> {
        System.out.println("Monday"); 
        // ERROR! Block doesn't contain a break with value
    }
    default -> 1;
};
i = switch (day) {
    case MONDAY, TUESDAY, WEDNESDAY: 
        break 0;
    default: 
        System.out.println("Second half of the week");
        // ERROR! Group doesn't contain a break with value
};
```

또 다른 결과는 제어 명령문 인 break, return 및 continue가 다음과 같은 switch 표현식을 뛰어 넘을 수 없다는 것입니다.

```java
for (int i = 0; i < MAX_VALUE; ++i) {
  int k = switch (e) { 
    case 0:  
      break 1;
    case 1:
      break 2;
    default: 
      continue z; 
      // ERROR! Illegal jump through a switch expression 
  };
  ...
}
```

기회의 대상으로 float, double 및 long과 같이 이전에 허용되지 않은 기본 유형 (및 상자 유형)에 대한 전환을 지원하도록 스위치를 확장 할 수 있습니다.



## 참고

- <https://www.oracle.com/technetwork/java/javase/12-relnote-issues-5211422.html#Removed>
- <http://openjdk.java.net/projects/jdk/12/>
-  <http://www.itworld.co.kr/news/111430>