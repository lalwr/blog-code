# Generic이란

제네릭(Generic)은 클래스 내부에서 사용할 데이터 타입을 외부에서 지정하는 기법이다.

```java
class GenericDemo<T>{
    public T info;
}

public class Demo {
    public static void main(String[] args) {
        GenericDemo<String> demo1 = new GenericDemo<String>();
        GenericDemo<Integer> demo2 = new GenericDemo<Integer>();
    }
}
```

인스턴스를 생성할 때 `<>`사이에 어떠한 타입을 사용하느냐에 따라 타입이 달라진다. 위의 예제는 demo1은 String 타입의 객체이고 demo2는 Integer 타입의 객체로 생성된다. 즉 클래스를 정의 할 때는 타입을 정하지 않고 인스턴스를 생성할 때 타입을 결정할 수 있다.



## 사용하는 이유

### 타입 안전성

```java
class itBook {
    public String name;
    itBook(String name){ this.name = name; }
}
class rankBook {
    public int rank;
    rankBook(int rank){ this.rank = rank; }
}
class itBookStore {
    public itBook info;
    itBookStore(itBook info){ this.info = info; }
}
class rankStore {
    public rankBook info;
    rankStore(rankBook info){ this.info = info; }
}
class Book {
    public static void main(String[] args) {
        itBook itBook = new itBook("TDD");
        itBookStore sp = new itBookStore(itBook);
        System.out.println(sp.info.name); // TDD
        rankBook rankBook = new rankBook(1);
        rankStore rankStore = new rankStore(rankBook);
        System.out.println(rankStore.info.rank); // 1
    }
}
```

`itBookStore`와 `rankStore` 클래스가 중복 구조를 가지고 있으니 중복을 제거해보자.

```java
class itBook {
    public String name;
    itBook(String name){ this.name = name; }
}
class rankBook {
    public int rank;
    rankBook(int rank){ this.rank = rank; }
}
class Store {
    public Object info;
    Store(Object info){ this.info = info; }
}
class Book {
    public static void main(String[] args) {
        Store store = new Store("TDD");
        itBook itBook = (itBook) store.info;
        System.out.println(itBook.name);
    }
}
```

위의 코드는 성공적으로 컴파일 되지만 실행을 해보면 아래와 같은 오류를 확인할 수 있을 것이다. `java.lang.ClassCastException: java.lang.String cannot be cast to itBook`

Store의 클래스 타입이 Object여서 어떠한 타입이 와도 컴파일 시에는 에러가 발생하지 않고 런타임에 에러가 발생한다. 런타임에 발생하는 에러는 심각한 상황을 만드는 경우도 있으니 조심해야 한다. 컴파일 언어의 기본은 모든 에러는 컴파일에 발생할 수 있도록 유도해야 한다.

**제네릭**하게 변경해 보자.

```java
class itBook {
    public String name;
    itBook(String name){ this.name = name; }
}
class rankBook {
    public int rank;
    rankBook(int rank){ this.rank = rank; }
}
class Store<T> {
    public T info;
    Store(T info){ this.info = info; }
}
class Book {
    public static void main(String[] args) {
        Store<itBook> store = new Store<itBook>(new itBook("TDD"));
        itBook itBook = store.info;
        System.out.println(itBook.name);

        Store<rankBook> store2 = new Store<rankBook>(new rankBook(1));
        rankBook rankBook = store2.info;
        System.out.println(rankBook.name);
    }
}
```

`store` 는 정상작동하고 `store2`는 컴파일 에러가 발생한다. **name** 필드가 없기 때문이다. 우리는 여기서 아래와 같은 항목을 알 수 있다.

- 컴파일 단계에서 오류 검출
- 중복의 제거와 타입 안전성을 동시에 추구



## 복수의 제네릭

```java
class itBook {
    public String name;
    itBook(String name){ this.name = name; }
}
class Store<T, S> {
    public T info;
    public S rank;
    Store(T info, S rank){
        this.info = info;
        this.rank = rank;
    }
}
class Book {
    public static void main(String[] args) {
        itBook itBook = new itBook("TDD");
        int rank = 1;
        Store<itBook, Integer> store = new Store<>(itBook, rank);
        System.out.println(store.info.name); // TDD
        System.out.println(store.rank); // 1
    }
}
```

참고로 제네릭은 **Reference Type**에서만 사용 가능하고 **primitive type**은 사용할 수 없으니 참고하자.

## 제네릭 생략

```java
class Book {
    public static void main(String[] args) {
        itBook itBook = new itBook("TDD");
        int rank = 1;
        Store<itBook, Integer> store = new Store<>(itBook, rank);
        Store store2 = new Store(itBook, rank);

    }
}
```

제네릭 타입은 생략이 가능하다. 타입을 알고 있기 때문이다.



## 메서드 사용

```java
class Store<T, S> {
    public T info;
    public S rank;
    Store(T info, S rank){
        this.info = info;
        this.rank = rank;
    }
    public <U> void print(U info){
        System.out.println(info);
    }
}
class Book {
    public static void main(String[] args) {
        itBook itBook = new itBook("TDD");
        int rank = 1;
        Store<itBook, Integer> store = new Store<>(itBook, rank);
        store.<itBook>print(itBook);
        store.print(itBook);
    }
}
```

메소드에서도 사용 가능하다.



## 제네릭 제한

```java
abstract class info {
    public abstract String getName();
}
class itBook extends info {
    public String name;
    itBook(String name){ this.name = name; }

    @Override
    public String getName() {
        return this.name;
    }
}
class Store<T extends info> {
    public T info;
    Store(T info){
        this.info = info;
    }
}
class Book {
    public static void main(String[] args) {
        Store store = new Store(new itBook("TDD"));
        Store<String> store2 = new Store("TDD"); //error
    }
}
```

제네릭으로 올 수 있는 데이터 타입을 특정 클래스의 자식으로 제한할 수 있다. **extends가** 아닌 **implements**에서도 가능하다.



## 참고

- https://opentutorials.org/course/2517/14153

