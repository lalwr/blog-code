# 자바 8 인 액션

## 1장 자바 8을 눈여겨봐야 하는 이유

- 스트림 처리
  - 한 번에 한 개씩 만들어지는 연속적인 데이터 항목들의 모임
- 동작 파라미터화로 메서드에 코드 전달
- 병렬성과 공유 가변 데이터
- 메서드와 람다를 일급 시민으로
  - 메서드 레퍼런스
  - 람다: 익명함수
- 메서드 전달에서 람다로
- 스트림
  - 내부 반복
- 멀티코어
- 멀티스레딩 문제 해결
- 디폴트 메서드
- Optional을 통한 NullPointer 예외 해결

## 2장 동작 파라미터화 코드 전달하기

- 변화하는 요구사항에 대응
- 동작 파라미터화
- 익명 클래스
- 람다 표련식 미리보기
- 실전 예제





## 3장 람다 표현식

### 메서드 래퍼런스

- 가독성을 높일 수 있다.

#### 메서드 래퍼런스 만드는 방법

1. 정적 메서드 래퍼런스
2. 다양한 형식의 인스턴스 메서드 래퍼런스
3. 기존 객체의 인스턴스 메서드 래퍼런스

```java
1.
(args) -> ClassName.staticMethod(args) #람다
ClassName::staticMethod #메서드 레퍼런스

2.
(args0, rest) -> ClassName.instanceMethod(rest) #람다
ClassName::instanceMethod #메서드 레퍼런스

3.
(args) -> expr.intanceMethod(args)
expr::intanceMethod
```

```java
String::length 는 (String s) -> s.uoUpperCase()
expensiveTransaction::getValue 는 () -> expensiveTransaction.getValue()
    
Function<String, Integer> stringToInteger = (String s) -> Integer.parseInt(s);
Function<String, Integer> stringToInteger = Ineger::parseInt;

BiPredicate<List<String>, String> contains = (list, element) -> list.contains(element);
BiPredicate<List<String>, String> contains = List::contains;
```

### 생성자 레퍼런스

두 인수를 갖는 생성자

```java
ClassName::new

#람다식
Supplise<Apple> c1 = Apple::new
Apple a1 = c1.get();
#예제
Supplise<Apple> c1 = () -> new Appler();
Apple a1 = c1.get();

#람다식
Function<Integer, Apple> c2 = Apple::new;
Apple a2 = c2.apply(110);
#예제
Function<Integer, Apple> c2 = (weight) -> new Apple(weight);
Apple a2 = c2.apply(110);
```

세개의 인수를 갖는 생성자

```
#람다식
BiFunction<String, Integer, Apple> c3 = Apple::new;
Apple c3 = c3.apply( "green" , 110);
#예제
BiFunction<String, Integer, Apple> c3 
	= (color, weight) -> new Apple(color, weight);
Apple c3 = c3.apply( "green" , 110);




```

