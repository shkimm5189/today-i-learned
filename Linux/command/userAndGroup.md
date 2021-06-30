# 사용자
- 사용자는 시스템에서 UID를 할당받아 로그인이 가능한 자
- 리눅스의 모든 파일 및 디렉토리는 특정 사용자에 의해 소유
- 모든 프로세스는 특정 사용자에 의해 실행

시스템에 등록된 사용자의 정보는 `/etc/passwd`에 저장된다.

``USER:x:UID:GID:GECOS:HOME:SHELL``
// 필드 별 속성정보 정리
## root 사용자
- UID가 0인 사용자
- 시스템에 대한 모든 권한 소유 / 파일 및 디렉토리의 소유자의 권한 수정 할수있다.


## 사용자 전환
`su [username]` : 이전 사용자의 \$PATH를 상속 받음.<br>
`su -` : 루트 계정으로 이동<br>
`su - [username]` : 로그인 사용자의 \$PATH를 사용한다.<br>


> `su`명령어만 사용한다면 해당 유저의 디렉토리에 로그인만 하지만 `su - username`을 사용하면 변경할 유저의 디렉토리까지 같이 변경한다. `echo '$PATH' `로 확인 가능

로그인 내역은 ``/var/log/messages``에 저장 된다.
## sudo
- root의 권한을 빌려오는 것(관리자 권한으로 실행)
- su - 는 root의 패스워드가 필요하지만 sudo는 사용시 해당 계정의 패스워드만 필요
- wheel 그룹의 구성원만 sudo 명령어 를 사용가능
- ``/var/log/secure``에 명령어 사용 흔적 기록됨.

## 사용자 생성
- 새로운 유저를 생성하는 명령어.
```bash
cccr3계정을 생성하시오 홈디렉토리는 /home/guest 쉘은/bin/sh
useradd -d /home/guest -s /bin/sh
```


 ```
 # useradd 할때 기본적인 적용사항
 /etc/default/useradd

 # 비밀번호 수정 기한 설정
 /etc/login.defs
 PASS_MAX_DAYS # 암호를 사용할수있는 최대기한 설정
 PASS_MIN_DAYS
 PASS_MIN_LEN
PASS_WARN_AGE
 ```
## 사용자 설정 변경
- 이미 생성된 유저에 대한 환경 설정을 변경할수있다.
```bash
유저의 uid를 2010으로 변경하고 보조그룹에 wheel을 추가 하시오

usermod -u 2010 -a -G wheel
/etc/passwd 바뀐 UID를 확인하고,
/etc/group 바뀐 Group을 확인한다.
```

```
usermod  -aG
usermod -G  차
```

## 사용자 삭제
```bash
userdel -r [username]

-r 옵션을 같이 사용 해주어야 삭제된 유저가 사용했던 홈 디렉토리까지 같이 삭제된다.
```
> -r 옵션을 사용하지않으면 유저에 대한 정보는 삭제되지만 사용했던 홈디렉토리 정보는 남아있는다. 남겨진 UID로 새로운 사용자를 생성하면 기존 유저의 디렉토리를 새로운 사용자가 소유하기때문에 문제가 생긴다.

# 그룹
- 사용자와 마찬가지로 이름과 GID가 있다.

- 기본 그룹
 1. 사용자는 무조건 하나의 기본그룹을 소유
 2. 기본 그룹은 /etc/passwd의 4번째 필드 GID로 표시
- 보조 그룹
 1. 보조그룹은 반드시 속하지 않아도 됨.
 2. ``/etc/group`` 파일의 마지막 필드에 보조그룹 구성원 표시
## 사용자 패스워드
- `/etc/shadow`에 저장됨.
```
암호화 지정 방식
사용자 PW지정 -> salt값 + 사용자 지정 PW -> 암호
```

```bash
usermod -s /sbin/nologin [username]
쉘에 접속 한다는 것은 일단 어떠한 권한이 생기는 것이기때문에 /sbin/nologin으로 쉘을 설정하면 로그인 자체를 막을수있다.
```
