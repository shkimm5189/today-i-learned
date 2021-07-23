Kafaka
======

등장배경
========

여러 각각의 시스템들이 있을 때 시스템의 마이그레이션에 어려움이 있다. 1. 고려할 사항 - 프로토콜

```
데이터 전송방식을 선택
TCP, HTTP, REST, FTP, JDBC
```

-	데이터 포맷

```
데이터 파싱 방식
Binary, CSV, JSON, Avro
```

-	데이터 스키마

```
데이터 스키마의 생성 및 변경
(앞으로 추가될 사항에 대해 어떻게 유연하게 대처)
```

1.	새로운 시스템을 추가

```
새로 연결 할때마다 부하를 증가 시킴
```

Apache Kafaka
=============

데이터를 발생 시키는 시스템은 apache kafka로 데이터를 보내고 데이터를 받는 시스템은 apache kafka로부터 받는다

source system -> apache kafka -> target system

Kafka 사용 이유
===============

1.	분산됬으며 (Distrubuted), \*탄력적인 아키텍처(Resilient Architecture), 내결함성 > 탄력적 아키텍처? 확장 가능한 아키텍처와 비슷하며, 서비스 통합, 배포, 생성 시간을 단축시키는것.

2.	수평적 확장

	-	100개이상의 브로커 확장 가능
	-	초당 수백만개의 메세지로 확장 가능

3.	고성능 처리

	-	실시간 (real time) 처리 가능 (대기시간 10ms미만)

4.	많은 기업에서 사용

	-	airbnb, LinkedIn, UBER, NETFLIX

사용 사례
=========

1.	Messaging System 메세징 시스템
2.	Actvity Tracking
3.	측정치를 수집하기 위해
4.	애플리케이션 로그 수집
5.	Stream processing
6.	시스템의 종속성 분리 -> DB 오버헤드 줄임
7.	Spark, Flink, Storm, Hadoop 같은 빅데이터기술과 통합
