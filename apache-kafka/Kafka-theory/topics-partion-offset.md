Topics
======

kafka에서 topics란?
-------------------

-	모든 것의 기초가 됨.
-	특정 데이터 스트림.
	-	(제약 조건 없는) 데이터 베이스의 테이블과 유사함.

```
  원하는 만큼의 topic이 존재할수있음
```

-	이름으로 식별. (기본키?)

```
  이름을 정하는 규칙이 있음 하지만 맘대로 정해도 되긴 함.
  일반적으로 데이터 스트림으로 지정
```

-	Topics는 partition(파티션)으로 분할 됨.

	Partition
	---------

	하나의 토픽에 여러 파티션이 지정될 수 있음.

	-	파티션은 오름차순 (0 부터 시작)
	-	`offset`이라 불리는 증가 값(ID)을 얻는다.
	-	토픽은 생성시 필요한 파티션 갯수를 지정함(변경 가능)
	-	각 파티션은 독립적임`
		파티션간에는 다른 메세지가 있어야하고 같은 오프셋이더라도 파티션이 다르면 다른 메세지다.
		`
	-	오프셋의 정렬 순서는 파티션 내에서만 보장됨.`
		파티션이 다르지만 같은 오프셋은 서로 상관 관계가 없다.
		`
	-	파티션에 저장된 값은 소멸 주기가 있다.`
		default : 1주일
		오프셋 값은 계속 증가 (mysql의 autoincrement)
		`
	-	파티션에 저장된 데이터들은 **변경 불가능** (불변성 보장)
	-	key를 지정하지않으면 랜덤으로 파티션에 저장된다.

LAG
current offset : write시에 최종으로 쓰여진 오프셋
cunsumer offset : consumer에서 read한 최종 오프셋
LAG 차이가 적을수록 실시간성
### Leader for partition (파티션의 리더)

일반적인 가장 좋은 경우 best case**한 번에 하나의 브로커에 하나의 파티션의 리더 지정.** > 하나의 브로커에 다수의 리더 파티션이 존재 할수있지만 좋지 않은 방법이다 주로 하나의 브로커에 하나의 리더 파티션이 존재하게 만든다. (partition reassign)<br>- 브로커당 실제 기능을 하는 것은 하나의 파티션이고 그 외 파티션들은 다른 파티션들의 복제(replication)이다. <br>- 동작하지 않는 파티션들은 데이터 동기화만 하면됨.