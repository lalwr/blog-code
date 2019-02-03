## Transaction

트랜잭션은 데이터베이스의 컨텍스트에서 데이터 검색 또는 업데이트를 위해 독립적으로 실행되는 논리 단위입니다. 관계형 데이터베이스에서 데이터베이스 트랜잭션은 원자성, 일관성, 고립성, 내구성이 있어야하며 ACID 약어로 요약됩니다.

# 트랜잭션은 트랜잭션의 시작 또는 끝을 나타내는 COMMIT 또는 ROLLBACK SQL 문에 의해 완료됩니다. ACID 약어는 다음과 같이 데이터베이스 트랜잭션의 속성을 정의합니다.

- 원자성(Atomicity) : 트랜잭션은 완전히 완료, 저장 (커밋) 또는 완전히 실행 취소 (롤백)되어야합니다. 소매점 데이터베이스의 판매는 원가를 설명하는 시나리오를 설명합니다. 예를 들어 판매가 재고 감소와 현금 유입 기록으로 구성됩니다. 둘 다 함께 일어나거나 일어나지 않습니다.
- 일관성(Consistency) : 트랜잭션은 트랜잭션 이전의 상태와 완전히 일치해야합니다. 즉, 트랜잭션은 데이터베이스의 제약 조건을 깨뜨릴 수 없습니다. 예를 들어 데이터베이스 테이블의 전화 번호 열에 숫자 만 포함 할 수있는 경우 일관성은 알파벳 문자를 입력하려고하는 모든 트랜잭션이 커밋되지 않도록 지시합니다.
- 격리성(Isolation) : 원래 트랜잭션이 커밋 또는 롤백될 때까지 트랜잭션 데이터를 다른 트랜잭션에서 사용할 수 없습니다.
- 지속성(Durablility) : 트랜잭션이 성공적으로 완료된 경우, 결과는 영구적으로 반영되어야 한다.
