# Event Sourcing & CQRS

`Event Sourcing`은 데이터 저장방법을 뜻하는 것으로 순차적으로 모든 이벤트를 저장한다. 이벤트 조회시 순차적으로 조회하기 때문에 성능이 좋지 않다. 그래서 `CQRS`와 같이 사용한다.

`CQRS`(Command and Query Responsibility Segregation)는 상태를 변경하는 Command Model과 조회를 담당하는 Query Model을 분리 구현하는것이다. 즉 C (Create –> INSERT), U(Update –> UPDATE), D (Delete –> DELETE) 쿼리와 R (Read –> SELECT) 쿼리를 분리하는 것이다. 

 

