# Event Sourcing & CQRS

일반적인 CRUD 모델에서는 하나의 데이터 베이스에서 모든걸 진행하다보니 동시성 문제가 나타날 수 있다. 이러한 문제점을 해결하기 위해 `CQRS`패턴을 사용한다.

`CQRS`(Command and Query Responsibility Segregation)는 상태를 변경하는 Command Model과 조회를 담당하는 Query Model을 분리 구현하는것이다. 즉 C (Create –> INSERT), U(Update –> UPDATE), D (Delete –> DELETE) 쿼리와 R (Read –> SELECT) 쿼리를 분리하는 것이다. 더 나아가서는 읽기 전용 디비와 쓰기 전용 디비를 분리하는 방법이 있다.

`Event Sourcing`은 데이터 저장방법을 뜻하는 것으로 순차적으로 모든 이벤트를 디비에 저장한다. 데이터 베이스 연산은 삽입만 가가능하며 이벤트 조회시 순차적으로 조회하기 때문에 성능이 좋지 않다. 그래서 `CQRS`와 같이 사용한다.

**CQRS패턴에 이벤트 소싱은 필수가 아니지만 이벤트 소싱에 CQRS는 필수**



