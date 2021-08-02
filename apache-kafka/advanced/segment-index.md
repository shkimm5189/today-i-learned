# Partition and Segment

파티션은 오프셋의 모음인 세그먼트로 구성된다.

## 제어설정
1. log.segment.bytes
```
단일 세그먼트의 최대 크기

byte단위 (default 1GB)
```
2. log.segment.ms
```
활성화된 세그먼트가 다 차지 않았을때 기다리는 시간
default 1주일
```
