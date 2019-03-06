# Mysql Engine InnoDB vs MyISAM



## MyISAM

- 트랜잭션을 지원하지 않는다.

- table-level locking을 제공한다.(테이블 단위 잠금)

- 무결성을 보장하지 않는다.

- 쓰기 작업에 비해 훨씬 더 많은 읽기 작업이 필요한 큰 테이블에서 사용한다.

- 데이터를 자주 변경하지 않는 경우 사용한다.

  

## InnoDB

- transaction-safe, 커밋, 롤백, 데이터 복구 기능 제공
- row-level locking 제공(행 단위 잠금)
- 데이터를 clustered index에 저장하여 PK 기반의 query의 I/O 비용을 줄인다.
- FK 제약을 제공하여 데이터 무결성을 보장한다.
- 데이터가 자주 변경될때 사용한다.