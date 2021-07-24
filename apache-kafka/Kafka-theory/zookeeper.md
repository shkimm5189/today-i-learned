# Zookepeer
- Zookepeer는 브로커를 관리함. => 브로커 목록 유지
- 파티션의 리더를 선정하는데 도움을 줌
- kafka에 상태 변화(CRUD)가 있을때 서로 알림
- **Zookepeer없이 kafka는 동작하지않는다.**
- Kafka에서 zookepeer의 메타데이터 관리
- leader(쓰기 관리)와 follwer(읽기 관리)
- v0.10이상은 consumer offset을 저장하지않음.
  consumer offset은 topic에 저장됨.



직접 다루진 않을것이다. 
