# Zookepeer
- Zookepeer는 브로커를 관리함. => 브로커 목록 유지
- 파티션의 리더를 선정하는데 도움을 줌
- kafka에 상태 변화(CRUD)가 있을때 서로 알림
- **Zookepeer없이 kafka는 동작하지않는다.**
- Kafka에서 zookepeer의 메타데이터 관리
- leader(쓰기 관리)와 follwer(읽기 관리)
- v0.10이상은 consumer offset을 저장하지않음.
  consumer offset은 topic에 저장됨.






모든 주키퍼는 같은 데이터를 가지고 있음.

정족수가 항상 만족이 되야 정상적으로 동작한다. (ex) 5개 클러스터가 있다. follwer 1개가 down 상태이고 4개의 클러스터가 2개,2개로 네트워크가 나눠졌다.(split brain) 이럴 경우, 두 개의 네트워크 클러스터가 정족수를 만족하지 못하므로 정상작동하지 못함. )

zookeeper는 qurom architecture 로 구성되어있기때문에 홀수로 구성되어야함. (정족수)

zookeeper노드의 좀더 높은 안정성을 위한다면 권장 5대 (비용적인 측면이 증가됨. -> 최소 3대 )



ROI(Return of Invest)

TCO(Total Cost of Onwership) 총 소유 비용

zookeeper에 대한 세팅은 직접 다루진 않을것이다.
