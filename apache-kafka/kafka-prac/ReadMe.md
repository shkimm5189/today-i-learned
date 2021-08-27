# 환경 구성
```
local - mac OS
ide : Intellij CE / gradle
java ver : 1.8


VM - Ubuntu
zookeeper, kafka server
```

gradle dependencies
```
kafka-clients : ver 2.8.0
slf4j-simple : ver 1.7.25
```

# why use gradle


```
<dependencies>에 관련된 의존성을 추가해서 사용해야한다.

관련 의존성을 추가하는 여러 옵션들이 있지만 대표적으로 compile과 implementaion이 있다.

일단, implementaion 옵션을 사용해서 추가 해주도록하자.

```
[dependencies configure docs](https://docs.gradle.org/current/userguide/dependency_management_for_java_projects.html#sec:configurations_java_tutorial)

# Producer

# Consumer
#### auto_offset_reset_config
topic의 offset 정보가 존재하지 않을때, 설정된 값으로 consuming 정책을 결정함.
- latest : 가장 마지막 오프셋부터
- ealist : 가장 처음 오프셋부터
- none : 해당 컨슈머 그룹이 가져가고자 하는 consumer offset 정보가 없다면 exception 발생

# Kafka Bi-Directional Compatibilty (클라이언트의 양방향성)
카프카 브로커와 카프카 클라이언트의 버전이 달라도 사용하는데 문제 없다.

카프카 브로커의 버전이 낮고 클라이언트 버전이 최신 버전이여도 운영가능.
