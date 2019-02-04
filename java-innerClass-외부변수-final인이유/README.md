# Java 의 Inner Class 에서 외부 변수에 접근하기 위해서는 final을 사용하는 그 이유

Inner class에서 접근하는 외부 로컬 변수는 반드시 final 이어야 한다. 

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

`JDK 1.8`부터는 자동적으로 외부 로컬 변수에 `final`을 붙여 주기 때문에 생략이 가능하다.

왜 **외부 로컬 변수는 반드시 final**이어야 할까?

