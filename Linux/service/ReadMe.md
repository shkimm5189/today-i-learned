# Linux Service
# 1. SELinux

## 동작모드
1. Enforcing
- 커널 모듈을 메모리에 로드.
- 활성화 상태
- 권한,컨텍스트와 부울을 확인하고 필요시 차단
2. Permissive
- 커널 모듈을 메모리에 로드
- 활성화 되긴 하지만 정책을 강요하진않는다.
- 정책 위반시 경고 메시지만 남김.
3. Disabled
- 커널 모듈을 메모리에 로드하지않는다.
- 완전한 비활성화 상태
> 컨텍스트 : 파일/디렉토리에 대한 접근제어
부울 : 서비스 동작별 제어(스위치)
포트 : 네트워크 포트별 서비스 할

> /etc/selinux/config 에서 동작모드를 설정할수있다.
특히, disabled로 되어있다면 커널에 올라가지 않아 setenforce명령어 자체가 동작하지않는다.

## selinux 명령어 (chcon)
1. chcon
2. restorecon
3. semanage fcontext


각 파일에 대한 컨텍스트 설정
semanage fcontext -a -t "context" "dir/.*" 기본값 변경

restorecon -RFv "dir" : 해당 디렉토리와 그 하위 파일의 컨텍스트를 기본값으로 설정

포트에 대한 레이블 설정
semanage port -a -t "Label" port -p tcp/udp 특정 포트 번호에 이름 부여


# 2. NFS(Network File Service) 스토리지

## 2.1 개요
## 2.2 서비스의 서버 구성 흐름
NFS를 사용하기위해선 Server-Client 방식으로 운영한다.
1. 패키지 설치
``yum install nfs-utils``
2. 서비스 구성
    - 디렉토리를 구성한다.
    - /etc/exports 공유디렉토리 대상 옵션셜정
    - 서비스 설정파일
3. 서비스 활성화
    -
4. 방화벽 구성
    - nfs 추가

## 2.3 서비스의 클라이언트 구성 흐름
1. 패키지 설치
2. mount
    - mount : 재부팅되면 설정 초기화
    - /etc/fstab : 부팅시 연결 (영구적 설정)
    - autofs : 사용 시 연결 (네트워크리소스 줄임)

## 2.4

## 2.5 autofs
- 자동 마운트
- ``yum install autofs -y``
- 마스터 맵으로 직접 맵, 간접 맵파일을 구성할수있다.
> 흐름
마스터맵으로 직접 맵, 간접맵을 지정한뒤 어떤것을 매핑할지 연결한다.

### 마스터맵
``/etc/auto.master.d/file-name.autofs``<br>
file-name은 마음대로 지정해도되지만 반드시 `.autofs`의 확장자를 가져야한다. <br>
- 직접 맵 필드:  /-  직접맵파일

- 간접 맵: *   간접맵파일

> 나의 경우 알아보기 쉽게 하기위해 직접 매핑이 되는 파일은 direct.autofs 간접 매핑이되는 파일은 indricet.autofs로 지정하였다.

### 직접, 간접 맵파일
``auto.file-name``<br>
file-name은 마음대로 지정해도 되지만 반드시 ``auto.``은 지켜야한다. <br>

- 직접 맵파일 : mount-point  mount-op  serverIP주소:/공유할 대상 경로

- 간접 맵파일 :  *   mount-op  serverIP주소:/공통경로/&

> 나의 경우 직접 맵파일은 auto.dricet 간접 맵파일은 auto.indirect로 설정하였다.
```
Server
공유할 디렉토리/파일 생성 mkdir

/etc/exports 설정
-> 공유 디렉토리    cilentip주소(마운트 옵션)

# nfs-server on
systemctl start nfs-server

# firewall setting
firewall-cmd --add-service=nfs --permanent

# /etc/exports 설정 반영
exportfs -r
```


```
Client
# autofs 설치
yum install autofs -y
# 마운트 포인트 디렉토리 생성
mkdir

# 마스터맵 파일 설정
/etc/auto.master.d/
file-name.autofs
# 직접, 간접 맵 파일 설정
auto.direct

systemctl restart autofs

/etc/fstab 설치
```

nfs와 smaba의 차이
