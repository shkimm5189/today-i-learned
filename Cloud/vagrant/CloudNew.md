
# Vagrant
개발 환경을 쉽게 구성 할수있다. (선언형)
### 주의사항
```
한번 코드로 작성한 인프라는 계속 코드로 관리해야한다.
임의로 수동으로 설정하면 오류가 발생할수있다. (무결성?)
```
## command sheet
```
vagrant init 'Imagename'
vagrant up
vagrnat status  # 상태 확인
vagrant halt  # 종료
vagrant reload # 재부팅
vagrant ssh # 해당 가상화된 머신으로 ssh접속
vagrant destory # 가상머신에서 삭제
```

```
vagrant init Image(OS/VERSION)
-> VagrantFile
```

- https://app.vagrantup.com/boxes/search vagrant init 시에 image를 참고하는 클라우드
> providers : 지원하는 가상화 환경


## Loadbalance (HAPROXY)
- Linux에서 가장 보편적으로 사용하는 오픈소스 로드 밸런
[HAProxy](haproxy.org)
