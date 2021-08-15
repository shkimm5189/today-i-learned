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

#
