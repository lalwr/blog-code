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

```java
// Java 7이전
switch (num){
     case 1 : return 1;
     case 2 : return 2;
     case 3 : return 3;
 }

// Java 7이후
switch (fruit){
    case "apple" : return "apple";
    case "banana" : return "banana";
}
```

#### Automatic resource management

예전에는 close() 메소드를 호출하여 수동으로 리소스를 회수해야 했다. Java7 부터는 Try문 안에 리소스를 선언하면 자동으로 리소스를 회수한다.

```java
// Java 7이전
FileOutputStream fos = null;
try {
    fos = new FileOutputStream("file.txt");
} catch (FileNotFoundException e) {

}finally {
    try {
        fos.close();
    } catch (IOException e) {

    }
}

// Java 7이후
try(FileOutputStream fos2 = new FileOutputStream("file.txt")){

}catch(Exception e){

}
```

#### Numeric literals with underscores

```java
// Java 7이전
int thousand =  1_000;

// Java 7이후
int million  =  1_000_000;
```



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



