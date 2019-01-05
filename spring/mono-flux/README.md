## Reator Core와 Srping5 Mono/Flux

[Reactor Core](https://github.com/reactor/reactor-core)는 반응 형 프로그래밍 모델을 구현하는 Java 8 라이브러리이다.

Reactor를 살펴보기 전에 Reactive Streams Specification를 알아야 하며 **기본적으로 리 액티브 스트림은 비동기 스트림 처리를위한 사양이다.**

`Mono`는 0~1 개의 결과를 처리 `Flux`는 0~N 개의 결과를 처리한다.

Flux 와 Mono는 모두 Reactive Stream [`Publisher`](http://www.reactive-streams.org/reactive-streams-1.0.0-javadoc/org/reactivestreams/Publisher.html) 인터페이스의 구현한다.


