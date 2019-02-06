# Java 의 내부 Class 에서 외부 변수에 접근하기 위해서는 final을 사용하는 그 이유

내부 class에서 접근하는 인스턴스 변수는 반드시 final이어야 한다. `JDK 1.8`부터는 자동으로 인스턴스 변수에 `final`을 붙여 주기 때문에 생략할 수 있고 내부 클래스는 외부 환경에 대한 레퍼런스를 가지고 있어서 인스턴스 변수에 접근할 수 있다. (clouser개념)

```java
//Inner class
public class InnerTest {
    public static void main(String[] args)
    {
        InnerTest test = new InnerTest();
        int obj = test.func();
        System.out.println(obj);
    }

    public int func()
    {
        final int x = 3; //jdk 1.8부터 final 생략 가능
        class LocalInner
        {
            public int toInt()
            {
                return x;
            }
        }
        return new InnerTest().toInt();
    }

}

//anonymous class
void InnerTest() {
    final int x = 0; //jdk 1.8부터 final 생략 가능
    Runnable runnable = new Runnable() {
        @Override
        public void run() {
            System.out.println(x);
        }
    };
}

//lamda
void InnerTest() {
    int x = 0; 
    Runnable runnable = () -> System.out.println(x);
}
```

final 을 붙이면 `Constant Pool`에 저장되기 때문에 메소드가 종료돼도 참조가 가능하므로 `InnerTest`는 정상적으로 실행된다. 만약에 **x** 변수의 값을 수정하려고 하면 **effectively final** 컴파일 에러가 나온다.

어떠한 side effect 때문에 반드시 **final**이어야 할까?

Thread Safe 하지 않기 때문이다.

배열 또는 참조할 수있는 객체를 사용 할 수 있지만, 값이 변경되는 것을 볼 수 있다.

```java
int[] a = {1};
Runnable r = () -> {
    System.out.println(a[0]);
    a[0] = 1;
};
Runnable r2 = () -> {
    System.out.println(a[0]);
    a[0] = 0;
};

int count = 100;
Thread[] threads = new Thread[count];
Thread[] threads2 = new Thread[count];

for (int i = 0; i < count; i++) {
    threads[i] = new Thread(r);
    threads2[i] = new Thread(r2);
}

for (int i = 0; i < count; i++) {
    threads[i].start();
    threads2[i].start();
}
```

java8의 병렬 처리를 구현한 parallelStream을 사용하여 확인할 수도 있다.

```java
int[] sum = {0};
IntStream.range(0, 100).parallel().forEach(i ->
  sum[0]+=i
);
System.out.println(sum[0]);
```

4950이 나와야 정상이지만 실행할 때 마다 값이 변경되는 것을 확인할 수 있다.