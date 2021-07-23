# Kafaka

# 등장배경
여러 각각의 시스템들이 있을 때 시스템의 마이그레이션에 어려움이 있다.
1. 고려할 사항
- 프로토콜
```
데이터 전송방식을 선택
TCP, HTTP, REST, FTP, JDBC
```
- 데이터 포맷
```
데이터 파싱 방식
Binary, CSV, JSON, Avro
```
- 데이터 스키마
```
데이터 스키마의 생성 및 변경
(앞으로 추가될 사항에 대해 어떻게 유연하게 대처)
```
2. 새로운 시스템을 추가
```
새로 연결 할때마다 부하를 증가 시킴
```

# Apache Kafaka
데이터를 발생 시키는 시스템은 apache kafka로 데이터를 보내고 데이터를 받는 시스템은 apache kafka로부터 받는다

source system -> apache kafka -> target system
