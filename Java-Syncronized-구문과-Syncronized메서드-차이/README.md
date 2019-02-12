# Java 에서 Syncronized 구문과 Syncronized 메서드의 차이

`synchronizred`란 하나의 동일한 객체에 하나의 쓰레드만 접근하도록 하는 것이다.

 `synchronized` 메서드와 달리 `synchronized` 문은 고유 잠금을 제공하는 개체를 지정해야합며 해당 객체를 하나의 쓰레드만 사용 가능하다.

```java
public void aa(){
    synchronized(this){
        i++;
    }
    
}
```

 `synchronized` 메서드에 선언하면 하나의 쓰레드만 해당 메소드를 실행할 수 있다.

```java
public synchronized void aa(){
	i++;    
}
```

