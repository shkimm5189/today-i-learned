# Kafka Guarantees
## 전체 흐름
1. 메세지는 topic partition에 순서대로 추가되고 전송됨
2. consumer는 topic partition에 저장된 순서대로 메세지를 읽는다.
3. 파티션의 수가 일정하게 유지되는 한 같은 키는 항상 같은 파티션을 참조한다.
