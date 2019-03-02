# Connection timeout vs Read timeout



## 차이점

**Connection Timeout**은 초기 연결을 할때에 대한 시간 초과다. TCP 연결 핸드 쉐이크를 완료하는 것을 뜻한다.

**Read timeout**은 데이터를 읽는 동안 대기하는 시간 초과다. 서버가 마지막 바이트 다음에 바이트 **timeout** 초를 보내지 못하면 오류가 발생한다.



## 참고

- https://stackoverflow.com/questions/3069382/what-is-the-difference-between-connection-and-read-timeout-for-sockets