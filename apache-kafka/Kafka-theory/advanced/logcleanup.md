# log cleanup policy
Kafka는 기본적으로 데이터를 만료시킨다. = log cleanup

log retention(보관 기간)이 지난 데이터의 제거를 어떻게 할건가


## cleanup policy

1. log.cleanup.policy=delete

모든 유저에대한 topic의 기본값
```
age 1주일 기준으로 데이터를 삭제
max size of log (default -1 == infinite) = 로그의 크기가 무한대이라는 뜻이다.
=> retention과 관계있는 옵션이다.
```

2. log.cleanup.policy=compact
consumer offset에 대한 topic 기본값
```
메세지의 키를 기준 (모든 중복 키 삭제)

```

## cleanup when why?
로그정리는 파티션 세그먼트에서 발생함.

디스크에 있는 데이터 사이즈조절, 쓸모없는 데이터 삭제

- 로그 정리가 발생할때
```
세그먼트의 크기가 작고 갯수가 많아질수록 cleanup이 자주 발생하게됨.

로그 정리는 cpu와 RAM의 리소스를 사용하기때문에 너무 많이 발생하면 성능저하

기본값을 잘 세팅하면 많이 발생하지 않지만 그래도 많이 발생함.

log.cleaner.backoff.ms로 확인 가능하며 15초 마다 정리
```
### delete log
1. log.retention.hours
데이터를 유지하는 기간 (default 168(1주일))


```
높은 값으로 적용시, 디스크 공간을 많이 차지하지만 데이터륾많이 가질수있음

낮은 값으로 적용시, 보관 데이터가 적다는것이지만 다운 시간이 길어지면 데이터 손실이 발생할수있음.
```
> Topic의 보관 주기와 Topic의 consumer offset의 보관주기가 다를경우 문제가 생길 수 있다.


2. log.retention.bytes
각 파티션의 최대크기(byte)
```
존재하지만 따로 설정하지않으면 기본적으로 적용되지않음
```
