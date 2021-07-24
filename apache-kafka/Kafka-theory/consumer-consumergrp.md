Consumer
========

-	topic(name으로 식별된)으로부터 데이터를 읽어옴
-	**데이터는 각 파티션내에서 순서대로 읽음**
-	각 파티션은 병렬 처리되기때문에 파티션의 offset의 순서대로 데이터를 읽을 뿐, 파티션의 우선순위는 알수없다.

> ConsumerGroup: Consumer Instance들을 대표<br> Consumer Instance: 하나의 프로세스

ConsumerGroup
=============

-	Consumer는 Consumer Group에 속하여 데이터를 읽어옴
-	그룹내 consumer는 파티션을 독점하여 읽는다 > partition < comsumer 일경우 일부 consumer는 비활성화 읽을 파티션 갯수와 consumer의 숫자는 맞춰주는게 좋다.

Comsumer offset
===============

-	kafka에는 consumer group이 읽었던 offset을 복원하는 기능이 있음. => ConsumerGroup이 데이터 읽을 때 실시간으로 커밋 됨. (kafka topic에 저장됨.) \__counsumer_offsets 형식

	역할
	----

-	consumer가 down됫을 경우, 파티션의 오프셋을 어디까지 읽었는지 확인 하기위함.

-	복원된 consumer가 작업을 이어나가기 위해 어느 포인트에서 작업을 이어서 진행할지 알수있게됨.
