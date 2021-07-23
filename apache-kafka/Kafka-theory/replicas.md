replication
===========

-	통상적으로 3개의 복제본을 서정
-	하나의 브로커가 다운 됬을때 다른 브로커의 파티션이 서비스 제공하려는게 목적 (failover)

ISR
---

Is Sync replica<br> ISR내의 모든 follwer들은 어느것이라도 leader가 될수있다.

-	파티션의 복제본의 그룹
-	하나의 Leader - 다수의 follwer 로 구성됨

```
  leader :follwer중에 일정기간 뒤쳐지면(장애 발생) 해당 follwer는 ISR에서 제외 시킴
  follwer : leader와 동일한 데이터 유지를 위해 짧은 주기로 leader로부터 데이터 갱신
```

-	zookepeer로 리더를 결정

All Down
--------

모든 브로커가 다운 상태일경우 대응 방법 2가지 1. 마지막까지 leader였던 broker가 up이 되고 leader가 될때까지 기다림

```
조건) 마지막 leader였던 broker가 반드시 up 되야하고 가장빨리 되야함. 
데이터 손실 적음
```

1.	ISR과 상관없이 제일 빨리 UP 되는 topic이 leader가 됨.

```
kafka의 default 값

제일 빨리 up된 leader는 down이후의 데이터가 없으므로 데이터 손실됨.
장애 대응이 빠름
```
