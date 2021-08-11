Consumer
========

-	topic(name으로 식별된)으로부터 데이터를 읽어옴
-	**데이터는 각 파티션내에서 순서대로 읽음**
-	각 파티션은 병렬 처리되기때문에 파티션의 offset의 순서대로 데이터를 읽을 뿐, 파티션의 우선순위는 알수없다.

> ConsumerGroup: Consumer Instance들을 대표<br> Consumer Instance: 하나의 프로세스
> current offset : 프로듀서가 넣은 마지막 오프셋 지점
cunsumer offsets : 컨슈머가 마지막으로 읽기를 마친 오프셋 (현재 읽고있는 오프셋은 포함안됨.)
-> cunsumer_group마다 offset이 각각 존재함.


>current offsets , cunsumer offset의 차이 = cunsumer lag
지연이 발생한 경우

assignor

ConsumerGroup
=============

-	Consumer는 Consumer Group에 속하여 데이터를 읽어옴
-	그룹내 consumer는 파티션을 독점하여 읽는다 > partition < comsumer 일경우 일부 consumer는 비활성화 읽을 파티션 갯수와 consumer의 숫자는 맞춰주는게 좋다.

>  같은 그룹내에서 특정 파티션에 2개의 컨슈머가 붙을 수 없음

Comsumer offset
===============

-	kafka에는 consumer group이 읽었던 offset을 복원하는 기능이 있음. => ConsumerGroup이 데이터 읽을 때 실시간으로 커밋 됨. (kafka topic에 저장됨.) \__counsumer_offsets 형식


	역할
	----

-	consumer가 down됫을 경우, 파티션의 오프셋을 어디까지 읽었는지 확인 하기위함.

-	복원된 consumer가 작업을 이어나가기 위해 어느 포인트에서 작업을 이어서 진행할지 알수있게됨.

autocommit 은 잘 사용하지않음 (장애 발생시 데이터 유실이 발생할수있음.)

# Delivery semantics
cunsumer는 offset을 commit하는 시점선택(Mannual/Auto Commit)

커밋 방식 전달방법 : configuration Parameter + Sub API Code 로 전달
consumer에만 존재
enable.auto.commit=true(default)



- At most once:
	offset은 메시지가 수신되면 commit (auto commit) ()
- At least once:
	offset은 메시지가 수신되고 처리가 완료 됬을때 commit
- Exactly once:
