# Java 7 vs 8 차이점

## Java7

### Language enhancements

#### Diamond Operator

왼쪽에 선언한 타입을 바탕으로 컴파일러가 타입을 추측하여 생략이 가능하다.

```java
 // Java 7이전
 List<String> arr = new ArrayList<String>();

 // Java 7이후
 List<String> arr = new ArrayList<>();
```

#### Using strings in switch statements

Switch문은 Primitive 자료형과 Enumerated 자료형을 사용했었다. Java7 부터는 String 자료형을 사용할 수 있다.



#### Automatic resource management



#### Numeric literals with underscores



#### Improved exception handling



### New file system API (NIO 2.0)

#### Working with Path



#### Fork and Join



#### Supporting dynamism



## Java8

### Lambda Expressions



### Method References



### Streams



### Enhanced Interfaces



### New Date and Time API



# 참고

- https://johanneslee.github.io/articles/page7/



