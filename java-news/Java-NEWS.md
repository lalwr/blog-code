> JAVA9 버전 이후 알고 있으면 좋을 내용을 정리한 내용입니다.



# Java 관련 News



## Java 버전 관리

Java 9 이후의 Java 버전 번호는 지금 그대로 Java 10, Java 11, Java 12과 연번으로 늘어나게되었습니다.

Java 9 릴리스하면 Java 버전 번호가 출시 년도와 자료 달을 합친 숫자, 예를 들어 2018 년 3 월에 출시되는 Java 10은 "Java 2018.3"이된다는 설명도있었습니다. 그 자취로서 공식 사이트에서도 버전 표기가 "Java 10 (18.3)」나 「Java 11 (18.9) '과 버전 번호와 출시 년월 병렬 표기되어 있습니다.

그러나 현재의 정책은 지금까지대로 순차적으로 버전 번호를 나타가는 것이라고합니다. 버전 번호 및 릴리스 세월을 병기 한 「Java 10 (18.3) "는 중 버전 번호 만 바꾸게되어 나가게 될 것입니다.



## 버전 업주기는 6 개월마다

Java 버전 업 사이클은 전술 한 바와 같이 2018 년 3 월에 출시되는 Java 10에서 6 개월마다 메이저 버전 업이 이루어집니다. 즉 2018 년 3 월에 Java 10 2018 년 9 월에 Java 11 2019 년 3 월에 Java 12, 2019 년 9 월에 Java 13 이어져가는 것입니다.

그리고 그 사이의 1 월, 4 월, 7 월, 10 월 연 4 회, 버그 수정과 보안에 대응 한 마이너 버전이 제공 될 예정입니다.

버전 업의 기본 정책은 기능의 추가 변경 사항은 메이저 버전 업에서 열린 마이너 버전 업에서는 기능에 영향이없는 버그 수정과 보안 대응 변경 만 이뤄질 것으로되어 있습니다.



## 왜 6 개월마다 메이저 버전 업을 할 것인가?

왜 Java는 6 개월마다 메이저 버전 업으로 출시 모델을 변경하는 것입니까?

Java는 지금까지 큰 기능 추가에 따라 메이저 버전 업을 해왔습니다. 큰 새로운 기능의 개발에 오랜 시간이 걸리고 때로는 개발 일정 지연도 고통도 왔습니다.

예를 들어, Java 6에서 7로 메이저 버전 업에는 4 년 8 개월, Java 7에서 8로 2 년 8 개월, Java 8-9로는 3 년 6 개월 걸려 있습니다.

이것은 즉, Java의 진화는 몇 년 밖에 일어나지 않았다는 것을 보여줍니다.

안정이 중시되는 엔터프라이즈 시스템 개발 분야에서 사용되는 경우가 많다 Java에서 몇 년마다의 느린 진화의 속도는 바람직한 것이기도했습니다. 그러나 최근에는 Java는 다른 언어 나 기술과 비교하면 진화가 느린 다소 오래된 것으로 보인다하도록지고도있었습니다.

기존 출시 모델을 바꾸게 6 개월마다 메이저 버전 업을 할 시간 기반 릴리스 모델로 Java를 전환 한 것은 이러한 반성에 서 Java를 지금까지보다 빠른 속도로 전진 시키려고하고 있기 때문입니다 .



## 상용의 Oracle JDK와 오픈 소스 OpenJDK는 같은 것이된다

상용 라이센스 Oracle JDK는 무료 버전에는없는 Java 프로파일 러 Flight Recorder와 Mission Control과 같은 도구가 제공되고 있습니다.

오라클은 현재 이러한 도구를 오픈 소스 Java 구현 인 OpenJDK도 포함하는 작업을 진행하고 있습니다.

예정으로는 Java 11이 출시되는 2018 년 9 월에이 작업을 완료하고 상업 유통의 Oracle JDK와 오픈 소스 구현 OpenJDK는 기능, 품질면에서 같은 일이 될 예정입니다.



## Oracle JDK의 무료 배포는 다음 JDK 10 마지막

현재 Java 구현으로 오라클에서 Oracle JDK가 공개되어 있으며, 무료로 다운로드하여 이용할 수 있습니다. 많은 개발자, 사용자가 Oracle JDK를 사용하는 것입니다.

그러나 Oracle JDK의 무상 공개는 Java 10까지로되어 있습니다. 2018 년 9 월에 출시 될 Java 11 이상 Oracle JDK는 오라클과 유상 지원 계약을 맺은 사용자에게만 제공되는 것입니다. Oracle JDK 11은 무상 공개되지 않습니다.

대신, 전술 한 바와 같이 Java 11에서 Oracle JDK와 동일한 기능과 품질을 갖춘 오픈 소스 구현 인 OpenJDK는 출시 된 바이너리가 무료로 제공됩니다. Java 11에서 OpenJDK는 기존 무상으로 제공되고 있던 Oracle JDK를 대체하는 것입니다.

오라클 주식회사 Cloud Platform 사업 추진 본부 수석 매니저 이토 타카시 씨는 "오라클은 JDK 11에서 OpenJDK는 널리 일반적으로 사용하기 JDK이며, 지금까지 유지해온 JDK의 기능 · 품질을 겸비한 Java 개발 키트로 자리 매김 "고 설명하고 있습니다.



## 무료 OpenJDK에 오라클의 지원은 제공되지 않고 유지 보수 기간은 6 개월

이어 지원 정책에 대해.

물론 무료로 제공되는 Oracle JDK 10과 OpenJDK 11 등 오라클의 지원은 제공되지 않습니다.

Java 9 이상에게 무료로 제공되는 Oracle JDK와 Open JDK의 지원 정책에서 중요한 점은 주 버전 당 유지 보수 기간이 다음 주 버전이 나올 때까지의 사이 인 것입니다 (Java 8에 대해 는 예외로 다른 규칙이 적용됩니다. 이에 대해서는 후술합니다).

구체적으로는 2018 년 3 월에 출시되는 Java 10의 구현 인 Oracle JDK 10은 출시 이후 4 월과 7 월에 버그 수정 및 보안 설비의 마이너 버전 업이 진행될 예정입니다. 그리고 출시 6 개월 후 9 월에는 다음의 주요 버전 인 Java 11과 그 구현 인 OpenJDK 11이 출시됩니다.

메이저 버전 업 후에는 이전 주요 버전에 대한 마이너 버전 업을하지 않게됩니다.

즉, OpenJDK 11이 출시되면 Oracle JDK 10에 대한 패치가 릴리스되지 않습니다. 2018 년 10 월과 2019 년 1 월에 출시 될 예정 패치는 OpenJDK 11에 대한 마이너 버전 업입니다.

그리고 2019 년 3 월에 Java 12이 등장하고 그 구현 인 OpenJDK 12이 출시되면 다음 4 월과 7 월에 출시되는 마이너 버전 업은 OpenJDK 12에 대응 한 것이되어, OpenJDK 11에 대한 버그 수정 등은 제공되지 않습니다.

(여기에서 추기 2018/2/8 14시 25 분)

그러나 OpenJDK는 오픈 소스 프로젝트의 판단으로 LTS를 설정하고 LTS에 대한 유지 보수 버전을 계속해서 출시하는 움직임이 있다는 지적을 받았습니다.

다음은 2017 년 11 월 Java의 공식 트윗. 미국 오라클의 Java Platform Group 수석 건축가의 Mark Reinhold 씨가 OpenJDK의 LTS 소개하고 있습니다.

Twitterで画像を見る

[![Twitterで画像を見る](https://pbs.twimg.com/media/DOGTWsrXcAA_7H8?format=jpg&name=small)](https://twitter.com/java/status/928185028714160128/photo/1)



이것이 실행되면 Java 11, Java 17 등 LTS가 설정된 버전 OpenJDK에 대해서는 몇 년간 (슬라이드에서는 다음 LTS의 메이저 버전이 등장하기까지의 3 년간), 무료 OpenJDK에서도 유지 보수 패치가 제공 될 수 있습니다.

OpenJDK는 오라클과는 독립적 인 오픈 소스 프로젝트로 소프트웨어 제공 정책을 수립 실행 해 나가는 것이되기 때문에, 본건에 대해서는 OpenJDK 측의 발표를 기다려야합니다.

며칠 전에도 OpenJDK의 관계자로부터 LTS에 대한 [발언이 있었다](https://twitter.com/kis/status/961463005375426561) 라는 것입니다 만, 아직 OpenJDK 커뮤니티에서 논쟁이 계속되고 있다고합니다.



유상의 Oracle JDK는 장기 지원 버전마다 3 년 동안 메이저 버전 업

Java 11에서 유상 지원 사용자에게만 제공된다 Oracle JDK는 무료 OpenJDK와는 다른 버전 업 및 지원 정책에서 제공됩니다.

2018 년 9 월에 등장하는 Java 11부터 Java는 3 년마다 "LTS"(Long Term Support)라는 장기 지원에 대응 한 메이저 버전이 등장합니다. 그 LTS 버전마다 Oracle JDK의 메이저 버전 업을하는 것입니다.

첫 번째 LTS되는 Java는 2018 년 9 월에 등장하는 Java 11에서 구현으로 Oracle JDK 11이 출시됩니다. 다음 LTS는 3 년 후인 2021 년 9 월에 등장하는 Java 17에서 구현으로 Oracle JDK 17이 출시됩니다. Oracle JDK의 메이저 버전 업판은 3 년마다 발표되는 것입니다.

LTS 버전의 지원 기간은 「Premier Support '로 출시 5 년 "Extended Support'로 다시 3 년이 추가 된 'Sustaining Support"를 계약하면 추가 지원 기간을 연장 할 수 있습니다. 3 년이 경과 한 다음 메이저 버전이 등장했다고해서 지원이 끝나는 것은 아닙니다.

그리고 지원 기간 동안 계속 1 월, 4 월, 7 월, 10 월의 3 개월마다 버그 수정과 보안 픽스를 대상으로 한 마이너 버전 업의 제공을받을 수있는 것입니다.

(추기 2018/2/8 : 마이너 버전 업 패치의 제공은 Premier Support와 Extended Support까지 .Sustaining Support에서는 패치 제공하지 않습니다)

다음은 오라클이 무료 버전의 JDK 및 유료 버전 JDK의 지원 정책에 대한 설명에 사용되는 그림을 허가 전재 한 것입니다.

![Java의 새로운 지원 정책](http://www.publickey1.jp/2018/java101112b.gif)

흰색 마커는 무료 버전 Oracle JDK의 출시시기 및 지원 기간을 나타내며, 파란색 마커는 무료 버전 OpenJDK의 출시시기 및 지원 기간을 나타내고 빨간색 마커는 유료 버전 Oracle JDK의 출시시기 및 지원 기간을 보여줍니다.

무료 OpenJDK 6 개월마다 메이저 버전 업이 등장하고 그 때마다 지원 기간이 종료하는 것을 알 수 있습니다.

유상의 Oracle JDK는 3 년마다 LTS 버전이 등장하고 8 년 이상 동안이 있음을 알 수있을 것입니다.



## Java 8의 지원 기간은 Java 11의 등장 이후까지 계속

마지막은 Java 8의 지원 기간입니다.

Java 9에서는 Java의 모듈화를 실현하는 Project Jigsaw가 구현 되었기 때문에, Java 9 이전과 이후로 지금까지 메이저 버전 업과 비교해도 큰 차이가 있습니다.

따라서 Java 8에서 Java 9 또는 그 이후 버전으로 마이그레이션하는 데 시간이 걸린다고 생각합니다.



Java 8의 지원 기간은 그것을 고려하여 설치되어 있습니다. 구체적으로는 최초의 LTS 버전 인 Java 11 2018 년 9 월에 나와 4 개월 후 2019 년 1 월까지는 무료 버전 Oracle JDK의 공식 업데이트가 제공됩니다. 또한 개인 Java SE를 사용하는 (non-corporate desktop use) 경우 2020 년 12 월말까지 계속 Java 8의 업데이트를받을 수 있다고 설명되어 있습니다.



# JAVA9



## **110: HTTP 2 Client**



HTTP/2 와 WebSocket을 구현하는 새로운 HTTP 클라이언트 API가 제공되어 기존 HttpURLConnection API를 대체할수 있다.

```java
// Sync Call

HttpResponse response = HttpRequest

               .create(new URI("http://www.ocado.com"))

               .body(noBody())

              .GET()**.send();**

int responseCode = response.responseCode();

String responseBody = response.body(asString());

System.out.println(responseBody);



// Async Call

HttpRequest request = HttpRequest

               .create(new URI("http://www.ocado.com"))

               .body(noBody())

               .GET();

CompletableFuture<HttpResponse> future = **request.sendAsync();**

Thread.sleep(10);

if (!future.isDone()) {

    future.cancel(true);

    System.err.println("timeout");

    return;

}



HttpResponse response = future.get();
```



## **224: HTML5 Javadoc**

이전 버전의 JDK에서는 HTML 4.01 형식을 사용하였으나 javadoc 도구를 개선하여 HTML5 마크업으로 생성이 가능하게 되었다.



## **226: UTF-8 Property Files**

ResourceBundle 클래스의 properties file 에 대한 기본적인 파일 인코딩이 기존 ISO-8859-1 에서 UTF-8 로 변경되었다.

시스템 프로퍼티에서 설정도 가능하다.

```properties
java.util.PropertyResourceBundle.encoding=ISO-8859-1
```



## **238: Multi-Release JAR Files**

JAR 파일 형식을 확장하여 여러 Java 버전의 클래스 파일을 하나의 JAR 안에 공존시킬수 있다.

특정한 버전에서 작동하는 특정 클래스를 jar 안에 넣을수 있다.



**Before - 특정 java 버전에서만 작동하는 A.class**

jar root

  **- A.class**

  \- B.class

  \- C.class

  \- D.class



**Multi-Release JAR Files 아래에서는 java9 와 10 일때 사용되는 A.class, B.class**

jar root

  **- A.class**

  **- B.class**

  \- C.class

  \- D.class

  **- META-INF**

​     **- versions**

​        **- 9**

​           **- A.class**

​           **- B.class**

​        **- 10**

​           **- A.class**



만약 위 Multi-Release 가 java9 에서 실행된다면 **java9 버전의 A,B 클래스와 일반 C,D 클래스가 실행**될 것이고

java10 에서 실행 된다면 **java10 버전의 A 클래스와 java9 버전의 B 클래스 그리고 일반 C,D 클래스가 실행**된다.



## **266: More Concurrency Updates (reactive stream)**

응용 프로그램의 동시성 및 병렬처리 지원을 위해 병행성 관련 라이브러리들 지원이 늘어난다.

개선된 CompletableFuture 과 reactive stream 을 도입하였다.



java.util.concurrent.Flow

java.util.concurrent.Flow.Publisher

java.util.concurrent.Flow.Subscriber

java.util.concurrent.Flow.Processor



Reactive Stream 은 Observer Pattern 과 유사하다.



### **CompletableFuture.**



#### **delay**

```java
future.completeAsync(supplier, CompletableFuture.delayedExecutor(3, TimeUnit.SECONDS))

		.thenAccept(result -> System.out.println("accept: " + result));
```



#### **Timeout**

```java
// TIMEOUT = 3;
// doWork() takes 5 seconds to finish

CompletableFuture<String> future = 

		doWork("JavaSampleApproach")

		.orTimeout(TIMEOUT, TimeUnit.SECONDS)

		.whenComplete((result, error) -> {

			if (error == null) {

				System.out.println("The result is: " + result);

			} else {

				System.out.println("Sorry, timeout in " + TIMEOUT + " seconds");

			}

	});
```





### **Reactive Stream.**

#### **Subscriber**

```java
import java.util.concurrent.Flow.Subscriber;

import java.util.concurrent.Flow.Subscription;



import com.journaldev.reactive.beans.Employee;



public class MySubscriber implements Subscriber<Employee> {



	private Subscription subscription;

	

	private int counter = 0;

	

	@Override

	public void onSubscribe(Subscription subscription) {

		System.out.println("Subscribed");

		this.subscription = subscription;

		this.subscription.request(1); //requesting data from publisher

		System.out.println("onSubscribe requested 1 item");

	}



	@Override

	public void onNext(Employee item) {

		System.out.println("Processing Employee "+item);

		counter++;

		this.subscription.request(1);

	}



	@Override

	public void onError(Throwable e) {

		System.out.println("Some error happened");

		e.printStackTrace();

	}



	@Override

	public void onComplete() {

		System.out.println("All Processing Done");

	}



	public int getCounter() {

		return counter;

	}





}
```



#### **Test Stream Data**

```java
public class Employee {



	private int id;

	private String name;

	

	public int getId() {

		return id;

	}

	public void setId(int id) {

		this.id = id;

	}

	public String getName() {

		return name;

	}

	public void setName(String name) {

		this.name = name;

	}

	

	public Employee(int i, String s) {

		this.id = i;

		this.name = s;

	}

	

	public Employee() {

	}

	

	@Override

	public String toString() {

		return "[id="+id+",name="+name+"]";

	}

}
```



```java
import java.util.ArrayList;
import java.util.List;
import com.journaldev.reactive.beans.Employee;


public class EmpHelper {



	public static List<Employee> getEmps() {



		Employee e1 = new Employee(1, "Pankaj");

		Employee e2 = new Employee(2, "David");

		Employee e3 = new Employee(3, "Lisa");

		Employee e4 = new Employee(4, "Ram");

		Employee e5 = new Employee(5, "Anupam");

		

		List<Employee> emps = new ArrayList<>();

		emps.add(e1);

		emps.add(e2);

		emps.add(e3);

		emps.add(e4);

		emps.add(e5);

		

		return emps;

	}



}


```

### **Reactive Stream Test**

```java
import java.util.List;
import java.util.concurrent.SubmissionPublisher;
import com.journaldev.reactive.beans.Employee;

public class MyReactiveApp {



	public static void main(String args[]) throws InterruptedException {



		**// Create Publisher**

		**SubmissionPublisher<Employee> publisher = new SubmissionPublisher<>();**



		**// Register Subscriber**

		**MySubscriber subs = new MySubscriber();**

		**publisher.subscribe(subs);**



		List<Employee> emps = EmpHelper.getEmps();



		// Publish items

		System.out.println("Publishing Items to Subscriber");

		emps.stream().forEach(i -> publisher.submit(i));



		// logic to wait till processing of all messages are over

		while (emps.size() != subs.getCounter()) {

			Thread.sleep(10);

		}

		// close the Publisher

		publisher.close();



		System.out.println("Exiting the app");



	}



}
```



## **267: Unicode 8.0**

Java 8 은 Unicode 6.1을 지원한다.

Java 9 는 10,555자,  29개의 스크립트 및 42개 블록의 유니코드 8.0 표준을 지원한다.**269: Convenience Factory Methods for Collections**



## **269: Convenience Factory Methods for Collections**

불변 Collection 을 만들수있는 메소드가 생겼다.



**Before** 

```java
Set<String> set = new HashSet<>();

set.add("a");

set.add("b");

set.add("c");

set = Collections.unmodifiableSet(set);



Set<String> set = Collections.unmodifiableSet(new HashSet<>(Arrays.asList("a", "b", "c")));



Set<String> set = Collections.unmodifiableSet(new HashSet<String>() {{

    add("a"); add("b"); add("c");

}});



java 8 stream 사용

Set<String> set = Collections.unmodifiableSet(Stream.of("a", "b", "c").collect(toSet()));
```



**After**

```java
Set<String> set = Set.**of**("a", "b", "c");

List.**of**(a, b, c);

Set.**of**(d, e, f, g);

Map.**of**()

Map.**of**(k1, v1)

Map.**ofEntries**(

    entry(k1, v1),

    entry(k2, v2),

    entry(k3, v3),

    // ...

    entry(kn, vn));


```



## **289: Deprecate the Applet API**

웹 브라우저 공급 업체들이 Java 브라우저 플러그인에 대한 지원을 제거함에 따라 Applet API 사용은 급속도로 무의미해지고 있다.

따라서 Applet API는 더이상 사용하지 않는것을 권장한다.

**새로운 Java Web Start 또는 설치 가능한 응용 프로그램과 같은 대체기술을 제공한다.**



## **Java9 의 크게 달라진점 중 한가지인 아래와 관련된 모듈 시스템.**

201: Modular Source Code

220: Modular Run-Time Images

260: Encapsulate Most Internal APIs

261: Module System; see also JSR 376

282: jlink: The Java Linker



java 모듈화 프로젝트인 [Jigsaw 프로젝트](http://openjdk.java.net/projects/jigsaw/)는 링크에서 자세하게 알수 있다.



요러한 선언으로 프로젝트 최상위에 module-info.java 라는 이름으로 생성된다.

```java
module com.foo.bar {


    requires org.baz.qux.one;

    requires org.baz.qux.two;

    requires ...;



    exports com.foo.bar.alpha;

    exports com.foo.bar.beta;

    exports ...;



    // not exports com.foo.bar.support; //hidden package

}
```



## **그외 추가적인 코어라이브러리 업데이트와 개선점들.**

### **Private Interface Method**



Java8 에서는 인터페이스에 **default method** 및 **static method** 라는 두 가지 새로운 기능을 제공했었다.

단지 특정 기능을 처리하는 내부method 일뿐인데도 외부에 공개되는 public method 로 만들어야 하는 단점을 가지고 있었다.

그러한 요구 사항과 interface를 구현하는 다른 interface 또는 class가 해당 method에 액세스하거나 상속 할수 있는것을 원하지 않는 요구사항이 있다. 이에 Java9 Private Interface Method 에서는 **interface 에 private method / private static method라는 새로운 기능을 제공**하여 문제를 해결한다. 이제 중복 코드를 피하고 interface에 대한 캡슐화를 유지 할 수 있다.

```java
public interface IMyInterface {

 

	private void method1(String arg) {

		// do something

	}

 

	private static void method2(Integer arg) {

		// do something

	}



}
```



**try-with-resource 향상**



기존의 java7 부터 제공된 try-with-resource 구문도 그냥저냥 사용했지만 사실 불편함이 많았다.

```java
//Before

void tryWithResourcesByJava7**() throws IOException {

BufferedReader reader1 = new BufferedReader(new FileReader("test.txt"));

try (BufferedReader reader2 = reader1) {

// do something

}

}





//final or effectively final이 적용되어 참조 변수를 구문에 바로 사용할 수 있게 되었다.

//After

// final or effectively final이 적용되어 reader 참조를 사용할 수 있음

void tryWithResourcesByJava9() throws IOException {

    BufferedReader reader = new BufferedReader(new FileReader("test.txt"));

try (reader) {

        // do something

    }



}
```



## **Improve Diamond Operator**

Java7 에는 코드를 보다 읽기 쉽게 만드는데 도움되는 "<>"(다이아몬드 연산자) Diamond Operator 라는 새로운 기능이 있었지만 **익명 클래스(Anonymous Inner Class)에는 제한적**이었다.

```java
MyHandler<Integer> intHandler = new MyHandler<>(10) { //Anonymous Class };  
//오류

MyHandler<?> handler = new MyHandler<>("One hundred") { // Anonymous Class }; 
//오류
```

**이제는 익명 클래스에서  Diamond Operator 를 사용할수 있다.**



```java
MyHandler<Integer> intHandler = new MyHandler<>(1) {


		@Override

		public void handle() {

			// handling code...

		}

	};



	MyHandler<? extends Integer> intHandler1 = new MyHandler<>(10) {



		@Override

		void handle() {

			// handling code...

		}

	};

		

	MyHandler<?> handler = new MyHandler<>("One hundred") {



		@Override

		void handle() {

			// handling code...

		}

	};



}
```



## **Stream Improvements**

Java 9 Stream 은 **iterate(), takeWhile(),dropWhile(), ofNullable()**  과 같은 새로운 추가 메소드를 사용하여 비동기 프로그래밍에 대한 몇 가지 유용한 개선 사항을 제공한다.



### **Iterate**

```java
IntStream
	.iterate(1, i -> i < 20, i -> i * 2)
	.forEach(System.out::println);
```

### **takeWile**

```java
//for ordered Stream
Stream.of(1, 2, 3, 4, 5, 6).takeWhile(i -> i <= 3).forEach(System.out::println);


// The result is:
// 1
// 2
// 3

// for unordered Stream
Stream.of(1, 6, 5, 2, 3, 4).takeWhile(i -> i <= 3).forEach(System.out::println);


// The result is:
// 1
```

###  

### dropWhild

```java
//for ordered Stream
Stream.of(1, 2, 3, 4, 5, 6).dropWhile(i -> i <= 3).forEach(System.out::println);

// It drops (1,2,3), the result is:
// 4
// 5
// 6

 
//for unordered Stream
Stream.of(1, 6, 5, 2, 3, 4).dropWhile(i -> i <= 3).forEach(System.out::println);
 
// It drops (1), the result is:
// 6
// 5
// 2
// 3
// 4

```



### **ofNullable**

```java
// numbers [1,2,3,4]
// mapNumber [1 - one, 2 - two, 3 - three]		

List<String> newstringNumbers = numbers.stream()
		.flatMap(s -> Stream.ofNullable(mapNumber.get(s)))
		.collect(Collectors.toList());

// null 값을 제외하고 스트림을 가져옴
// The result is:
// [one, two, three]
```



## **Optional class Stream**

옵셔널 클래스 스트림이 개선되었다. orElse 구문 사용등에서 깔끔하지 못한부분이 해결되었다. 



옵셔널로 스트림 메소드가 제공됨에 따라

```java
Stream<Integer> stream = Optional.of(1).stream();
```

Java 9 는 새로운 Optional::stream 으로 Optional 객체 지연작업을 제공하며 zero 또는 하나 이상의 요소 스트림을 반한한다. 또한 빈요소를 자동으로 확인하고 제거한다.

```java
// streamOptional(): [(Optional.empty(), Optional.of("one"), Optional.of("two"), Optional.of("three")]

List<String> newStrings = streamOptional()
				.flatMap(Optional::stream)
				.collect(Collectors.toList());

				

// Result: newStrings[one, two, three]
```

isPresent() 및orElse() 를 사용하여 코드를 보다 명확하게 만들고 **“else” 케이스를 처리하는 대신 Java9의 ifPresentOrElse() 메소드를 사용**할 수 있다.

```java
Optional<Integer> result3 = getOptionalEmpty();

result3.ifPresentOrElse(
		x -> System.out.println("Result = " + x),
		() -> System.out.println("return " + result2.orElse(-1) + ": Result not found."));
				

// return -1: Result not found.
```

or() 메소드는 값이 존재하는지 검사하고 값이 있는 경우 해당 Optional 을 리턴하고 그렇지 않은 경우 supplying function에서 생성한 다른 Optional 값을 리턴한다.

```java
Optional<Integer> result = getOptionalEmpty() // Empty Optional object
		.or(() -> getAnotherOptionalEmpty()) // Empty Optional object
		.or(() -> getOptionalNormal())  // this return an Optional with real value 42
		.or(() -> getAnotherOptionalNormal());  // this return an Optional with real value 99

				

// Result: Optional[42]
```





# JAVA10



## **286: Local-Variable Type Inference**

눈에띄는 새로운 점은 로컬변수를 선언할때 타입추론을 이용하여 명시적으로 타입선언 없이도 변수를 선언할수 있게 되었다.

ES5 의 var 변수와 lombok 의 var 처럼 variable 을 줄여 var 로 변수 선언이 가능하다.

다이아몬드 연산자에 아무 타입도 선언해주지 않거나 반복문 안에서 사용되는 var 변수의 초깃값을 주지 않는다면 Object 로 인식한다.



```java
var list = new ArrayList<String>();  // infers ArrayList<String>

var stream = list.stream();          // infers Stream<String>
```



주의 해야할 점은 변수를 리터럴로 선언할때와 for 문 for each 구문에서만 사용이 가능하다.

```java
for(var value : list) {

	System.out.println(value);

}



for(var i=0; i<list.size(); i++) {

	System.out.println(i);

}
```



## **304: Garbage-Collector Interface**

가비지콜렉터 인터페이스를 도입하였다. 앞으로 여러 용도에 맞는 가비지 콜렉터가 등장하고 교체해서 쓸수 있을것이라고 한다.



## **313: Remove the Native-Header Generation Tool (javah)**

{JAVA_HOME}/bin 하위의 JDK 에서 제공해주던 많은 Tool 기능중 코드에 native 메소드 사용시 JNI 헤더를 생성해주는 툴이 삭제되었습니다.

삭제된 이유는 **JDK8 부터 javac에서 JNI 헤더 파일 생성을 지원**해주기 때문이다.





## **314: Additional Unicode Language-Tag Extensions**

java.util.Locale 및 관련 API 들을 향상시켜서 [BCP 47 언어 태그](https://ko.wikipedia.org/wiki/IETF_%EC%96%B8%EC%96%B4_%ED%83%9C%EA%B7%B8)의 추가적인 유니코드를 확장 구현 하였다.





## **310: Application Class-Data Sharing**



기존의 Class-Data Sharing(CDS) 기능을 확장해 애플리케이션 클래스를 공유 아카이브에 배치하고 서로 다른 자바 프로세스들이 공유할 수 있도록 개선함으로써, startup 시간을 단축시키고 메모리 사용량을 최적화 시켰습니다. 



**기존에 AppCDS 기능은 상업용으로 Oracle JDK에서만 제공되었으나, 오픈소스화 되어 Open JDK에도 사용할 수 있게 되었다.**



## **312: Thread-Local Handshakes**

GC 가 발생하기 위해 흔히 말하는 "stop-the-world" 가 발생하는데, GC를 수행하기 위해 GC를 실행하는 쓰레드를 제외하고 모든 쓰레드가 작업을 멈추는 것을 의미한다.



이전과 같이 모든 쓰레드들이 동시에 멈추지 않고 쓰레드를 개별로 멈출수 있게 되었고 VM safepoint 수행 없이도 개별 쓰레드에서 콜백 실행이 가능해졌다.



# JAVA11



## Nest 기반 접근 제어

[Nest-based access controls](https://openjdk.java.net/jeps/181). Nest 는 접근 제어 컨텍스트로 논리적으로는 같은 클래스를 분리된 클래스로 컴파일할 수 있게 해줍니다. 그러면 다른 클래스의 private 멤버에 getter/setter 없이 바로 접근 가능합니다. 여러 클래스를 하나의 클래스처럼 묶어줄 수 있는 기술로 보입니다.



## 새로운 가비지 컬렉터

[ZGC: A Scalable Low-Latency Garbage Collector (Experimental)](https://openjdk.java.net/jeps/333). 성능을 향상시킨 새로운 가비지 컬렉터(Carbage Collector)입니다. 메모리를 자동으로 정리해주는 가비지 컬렉터는 자바의 장점 중 하나이지만, 가비지 컬렉터가 동작할 때 JVM이 애플리케이션을 멈추기 때문에 자바의 단점이기도 합니다. ZGC는 이 시간을 10ms 미만으로 줄이고 15% 이하의 성능 페널티를 목표로 합니다.



## Flight Recorder

[Flight Recorder](https://bugs.openjdk.java.net/browse/JDK-8203664). 자바 애플리케이션과 HotSpot JVM의 문제 해결을 위한 오버헤드가 낮은 데이터 수집 프레임워크입니다. 이전에는 유료 기능이었지만 오픈소스로 공개되었습니다.



## 새로운 표준 HTTP 라이브러리

[HTTP Clinet(Standard)](https://openjdk.java.net/jeps/321). `java.net.http` 패키지의 새로운 모듈로 flow 기반의 HTTP/1.1과 HTTP/2를 지원합니다. 자바 9과 자바 10에서 사용되었던 `jdk.incubator.http` 패키지가 표준화되어 `java.net.http` 패키지로 추가되었습니다.



## TLS 1.3

[Transport Layer Security (TLS) 1.3](https://openjdk.java.net/jeps/332). [TLS](https://futurecreator.github.io/2018/07/12/https-and-ssl-tls/)는 [이전 포스트](https://futurecreator.github.io/2018/07/12/https-and-ssl-tls/)에서 살펴봤던 것처럼 SSL(Secure Socket Layer)의 표준 이름이죠. TLS의 새로운 버전을 구현했습니다.



## 람다에서의 var 변수

[Local-Variable Syntax for Lambda Parameters](https://openjdk.java.net/jeps/323). 자바 10에서 도입된 var 타입 추론을 업데이트했습니다.



##  기능

사라진 기능도 간단하게 살펴보겠습니다.

- **Java EE and CORBA Modules** : 앞으로 EE 나 CORBA 모듈이 필요한 경우 명시적으로 의존을 추가해야 합니다.
- **Web Start** : 특별한 대안 없이 삭제되었습니다.
- **Applets** : 한동안 대부분 deprecated 되었다가 완전히 삭제되었습니다.
- **JavaFX** : FX 라이브러리가 OpenJFX 프로젝트로 옮겨가면서 코어에서 삭제되었습니다.



# JAVA12



## **CDS 및 가비지 수집**

64비트 플랫폼에서 기본 클래스 목록을 사용해 기본 클래스 데이터 공유(Class Data-SharingCDS) 아카이브를 생성하도록 JDK 빌드 프로세스가 향상된다. 목적은 기본 시작 시간을 포함하고 CDS의 이점을 활용하기 위해 *-Xshare:dump*를 실행할 필요가 없도록 하는 데 있다. 또한 JDK 빌드를 수정해서 이미지 링크 후 *java-xshare:dump*를 실행한다.

가비지 수집 힙 시간을 튜닝해 일반적인 사례의 메모리 레이아웃을 개선하기 위한 부가적인 명령줄 옵션도 포함될 수 있다. 애플리케이션 클래스와 다양한 가비지 수집 구성이 포함된 맞춤형 클래스 목록과 같은 고급 요구사항이 있는 사용자는 계속해서 맞춤형 CDS 아카이브를 만들 수 있다.  



## ARM 포트 수 감소

자바 12에서 ARM 프로세서를 위한 포트는 2개가 아닌 1개가 된다. 계획은 32비트 ARM 및 64비트 aarch64를 보존하면서 arm64 포트와 관련된 모든 소스를 없애는 것이다. 이 포트를 제거하면 기여자들은 하나의 64비트 ARM 구현에 집중하고 2개 포트를 유지 관리하는 데 따르는 중복 작업을 없앨 수 있다. 지금은 JDK에 2개의 64비트 ARM 포트가 있다.



## 스위치 익스프레션(Switch expressions) 

베타 스위치 익스프레션 기능은 switch 구문을 확장해 구문 또는 식으로 사용할 수 있도록 한다. 이를 통해 코딩을 간소화할 수 있다. 두 형식 모두 "전통적인" 또는 "간소화된" 범위 및 흐름 제어 동작을 사용할 수 있게 된다. 이런 변화를 통해 "일상적인" 코딩이 더 간소화되고 switch에서 패턴 매칭(pattern matching)을 사용하기 위한 기반을 마련했다.

자바 제작자들이 패턴 매칭을 지원하도록 전환함에 따라 기존 switch 문의 불일치가 장애물이 된다. 여기에는 switch 블록의 기본 흐름 제어 동작, 블록이 단일 범위로 취급되는 switch 블록의 기본 범위 지정, 문으로만 작동하는 switch 등이 포함된다.

현재 자바의 switch 문 동작은 기본적으로 C++와 같은 언어와 거의 비슷하고 fallthrough 의미 체계를 지원한다. 이 흐름 제어는 저수준 코드를 작성하는 데 유용하다. 그러나 switch가 더 높은 수준의 컨텍스트에서 사용되면 특유의 잦은 오류로 인해 유연함으로 얻는 이점이 무의미해진다.



## 원시 문자열 리터럴

베타 원시 문자열 리터럴은 \n과 같은 이스케이프 시퀀스 또는 /uXXX 형식의 유니코드 이스케이프를 해석하지 않으면서 여러 줄의 소스 코드를 포괄할 수 있다. 자바 제작자들이 염두에 두고 있는 이 기능의 목적은 여러 가지다.

- 자바 이외의 문법을 대상으로 하는 문자열을 입력할 때 자바 단순화(Java Simplification) 없이 읽을 수 있는 형식으로 문자 시퀀스를 표현하기가 쉬워짐
- 새 라인을 나타내는 특수한 표식 없이 몇 줄의 소스코드를 포괄하는 문자열 입력
- 동일한 문자열을 기존 문자열 리터럴로 표현할 수 있는 기능(플랫폼별 줄바꿈 문자 제외)
- 이스케이프의 현재 javac 문자열 리터럴 해석을 복제하고 왼쪽 여백 자르기를 관리하기 위한 라이브러리 지원

자바 12 개발자들은 현실적인 자바 코드에는 유니코드 이스케이핑, 백슬래시 또는 새 줄을 특별히 처리하는 과정 없이 리터럴 문자열을 있는 그대로 캡처하기 위한 메커니즘이 필요하다고 말한다. 원시 문자열 리터럴은 자바 이스케이프와 자바 줄바꿈 문자 사양을 접어두고 대부분의 상황에서 기존 문자열 리터럴보다 더 읽기 쉽고 유지 관리가 용이한 문자 시퀀스를 제공한다.

자바는 원시 문자열에 대한 언어 수준의 지원이 없는 극소수 언어 가운데 하나다. 이 제안으로 새로운 문자열 연산자가 도입되지는 않을 것이다. 또한 원시 문자열 리터럴은 문자열 보간을 직접 지원하지도 않는다. 기존 문자열 보간은 변화 없이 그대로 유지된다.



## 기본 벤치마크 모음

자바 개발 키트(JDK) 12에는 플랫폼의 소스코드에 추가되는 기본적인 벤치마크 모음이 포함될 것으로 보인다. 목적은 개발자가 더 쉽게 기존 벤치마크를 실행하거나 새 벤치마크를 제작하도록 하는 것이다.

2014년 7월에 만들어져 2018년 11월 초에 업데이트된 마이크로벤치마크 모음 제안의 기반에는 자바 및 다른 JVM 언어로 작성된 벤치마크 제작을 위한 자바 마이크로벤치마크 하네스(Java Microbenchmark Harness, JMH)가 있다. 이 모음은 하나의 디렉터리에 JDK 소스코드와 함께 위치하며, 개발자는 손쉽게 새 벤치마크를 추가할 수 있다.

새 JDK 기능을 위한 벤치마크를 제공하거나 JDK의 모든 요소를 포괄하는 일체의 벤치마크 집합을 만드는 것을 목표로 하지는 않는다.

자바 12 계획에는 빌드 시간 증가, 새 벤치마크에서 잠재적인 테스트 불안정성, 소스코드 저장소 크기 증가와 같은 위험 요소도 있다. 그러나 벤치마킹 모음은 일반적인 JDK 빌드에는 필요 없고 별도의 빌드 타겟이 된다. 잠재적인 테스트 불안정성에 대처하기 위해 새 벤치마크에는 철저한 테스트가 필요할 것이다.



### JEP 325 : Switch 표현식

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

#### 기술

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

- <https://www.oss.kr/news/show/f6d76af9-b1ee-4fef-bf4c-90ecce8eabc7>
- <https://futurecreator.github.io/2018/09/29/java-11-released/>
- https://jusungpark.tistory.com/
- <https://exien.tistory.com/85>
- https://jusungpark.tistory.com/57