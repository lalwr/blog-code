# Interface VS Abstract Class



## Abstract Class

- 추상메소드가 하나 이상 존재하는 클래스가 추생클래스다. 
- 추상클래스는 상속을 통해서 자식클래스에 의해서만 객체를 생성할 수 있다.

기존 클래스의 코드를 다시 사용하는 경우 사용한다. 

사람마다 다른 음식을 만들어 먹는다고 할 때 먹는다는 행위의 공통적인 부분은 추상클래스에서 사용하고 달라지는 부분은 자식클래스에 구현하여 사용한다.

```java
public abstract Person
{
   public void eat(Food food)
   {
        // Food를 먹는다.
   }

   public abstract void makeFood();
}
```

```java
public John extends Person
{
   public void makeFood() { System.out.println ("Pasta"); }
}

public Michael extends Person
{
   public void makeFood() { System.out.println ("Salad"); }
}
```



## Interface

-  Abstract Class와 달리 추상메소드와 상수만을 가질 수 있다. (JAVA 8부터는 default메소드를 통해서 구현할 수 있다) 
- 다중상속이 가능하다.
- 모든 멤버변수는 public static final 이다.(생략 가능)
- 모두 메소드는 메서드는 public abstract 이다.(생략 가능)
- 내부에 필드를 가질 수 없다.
- 표준화가 가능하다.

사람마다 먹는 방식이 달라진다면 공통적인 부분이 사라지게 된다. 사람마다 다르게 구현해야 하는 경우 아래와 같이 구현하여 사용한다.

```java
public interface Person
{
   public void eat(Food food);

   public void makeFood();
}
```

```java
public John extends Person
{
   public void eat(Food food) { System.out.println ("fork"); }
   
   public void makeFood() { System.out.println ("Pasta"); }

}

public Michael extends Person
{
   public void eat(Food food) { System.out.println ("Spoon"); }
   
   public void makeFood() { System.out.println ("Salad"); }
}
```

