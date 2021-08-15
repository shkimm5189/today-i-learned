# min.sync.replicas
- acks=all 설정 시, 메세지를 보낼 떄 데이터 손실없이 write를 성공하기 위한 최소 복제본의 수.
- Acks=all은 min.sync.replicas와 함께 사용 된다.
- topic에서 설정함 (config/server.properties)
- deafault = 1
