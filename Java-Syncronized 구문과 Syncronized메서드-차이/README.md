# Java 에서 Syncronized 구문과 Syncronized 메서드의 차이

synchronizred란 하나의 동일한 객체에 하나의 쓰레드만 접근하도록 하는 것이다. 블록에 접근 하는 다른 쓰례드는 block 처리가 된다. 

두 블록 사이에 차이는 없지만 `synchronized` 메서드와 달리 `synchronized` 문은 고유 잠금을 제공하는 개체를 지정해야합니다.

