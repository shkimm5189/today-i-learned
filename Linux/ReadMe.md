# 리눅스

## 1. Inode


## 2. Link (Symbolic, Hard)
|구분|하드 링크|심볼릭링크|
|--|--|--|
|생성 명령어|ln 원본파일 링크파일|ln -s 원본파일 링크파일 |
|생성 종류|파일만 생성|디렉토리,파일 생성|
|링크 기능|원본파일(inode)에 대한 포인터|파일/디렉토리 이름에 대한 링크를 가리킴|
|원본 파일 삭제 시|액세스 가능|액세스 불가|
|Inode 번호|inode 같음|inode 다름|
|용량|추가적인 용량 없음|추가 용량 차지|
|다른 파티션 링크|다른 파티션에 링크 불가|다른 파티션에 링크 가능|

> 링크된 파일에서 내용을 수정해도 그 내용이 원본 파일에 똑같이 반영됨<br>
> > 모든 링크를 하드 링크로 사용하지 않을까 ???
> 파티션을 나눠 관리할경우 하드링크의 경우 링크가 깨지기때문에 심볼릭 링크가 적절할수있다.

### 2.1 링크 사용시기
**심볼릭 링크**
1. 서로 다른 파티션(파일 시스템)에 링크할 경우 사용
2. 디렉토리를 링크할 경우 사용


**하드 링크**
1. 저장 공간 : 하드링크는 진짜로 데이터를 복사한게 아니라 이미 존재한 데이터의 위치만 가리키기에 용량 차지 하지않음.
2. 성능 : 다른 파일을 거치지 않으므로 성능이 약간 좋음.
3. 파일 위치 이동 : 같은 파일 시스템 내에서 다른 위치로 옮기면 하드링크는 동작하지만 소프트링크는 실패
4. 안전성 : 심볼릭 링크보다 데이터 안전성이 우수



## 3. Permission
|퍼미션|문자|파일 |디렉토리|
|--|--|--|--|
|read|r|내용 확인 / 복사 가능|디렉토리 내용 확인 가능 (ls)|
|write|w|파일 내용 수정 가능|실행 권한이 있을 경우 디렉토리내 파일 추가 및 삭제 가능|
|execute|x|실행 파일일 경우 실행가능<br>쉘 스크립트의 경우, r-x권한이 있으면 실행가능|cd 명령어로 접근가능. r권한이 있을경우 ls -l 디렉토리 내용 확인 가능|

> 디렉 토리에 실행 권한만 없을경우(rw-) 해당 디렉토리내 파일 추가/삭제가 가능 한가?
디렉토리에 읽기 권한만 없을경우(-wx) ls -l 가능 한가?

### 3.1 리눅스 read, write, execute (rwx)
**file rwx**
- **read**
file의 data block을 통해 실제 디스크의 데ㅐ이터 섹터로 접근 후 그 내용을 표준 출력 or 지정된 터미널로 출력

- **write**
file의 data block을 통해 실제 디스크의 데이터 섹터 접근후 표준 입력 or 데이터를 접근 한 섹터에 write 하는 과정

- **execute**
file의 data block을 통해 접근한 데이터 섹터의 파일 내용에 access하는 것
<br>

**directory rwx**

- **read**
directory의 data block을 통해 실제 디스크의 데이터 섹터로 접근 후 그 내용을 표준 출력 or 지정된 터미널로 출력

- **write**
directory의 data block을 통해 실제 디스크의 데이터 섹터 접근후 표준 입력 or 데이터를 접근 한 섹터에 write 하는 과정

- **execute**
directory의 data block을 통해 접근한 데이터 섹터의 파일 내용에 access하는 것

read : cat head tail ls<br>
write : mkdir > >> touch cp mv rm rmdir<br>
exxecute : cd<br>


### 3.2 chmod
r(4)w(2)x(1)로 부여가능
- 8진수 모드
``rwx/rwx/rwx  = user/group/others``
``chmod 777 test1``

- 심볼릭 모드
``chmod "mode" test1``


|옵션|설명|
|--|--|
|ugoa|u = user<br>g = group <br>o = others<br>a = all|
|+-=|+ 권한 추가<br>- 권한 삭제 <br> = 입력된 권한만 추가 (입력 안된 권한들은 삭제된다.)|
|rwx|read write execute|

> chmod로 심볼릭 링크와 시스템 콜의 권한을 바꿀수 없다.


## 3.3 UMASK
#### 3.3.1 개요
- XOR 연산을 통해 계산된다.
- 파일이나 디렉토리에 생성시 default Permission을 부여하기 위해서 존재한다. 보안적으로나 관리적인 측면에서 사용을편하게 하기 위해서이다.
- 스페셜 퍼미션은 지정할수없다.
- 터미널에서 지정된 umask는 해당 터미널에서만 사용됨.(휘발성)

특정 사용자의 umask를 영구적으로 지정하려면 사용자의 bash_profile에서 umask값을 지정해야함<br>
파일의 Full Permission = 666 <br>

> 파일의 실행 권한을 뺀이유 : 파일안에 어떤 명령어,위험한 명령어 등이 있을경우 위험하기때문이다(ex. rm -rf /.). 뭐가 들어있는줄알고 실행시킴?

디렉토리의 Full Permission = 777

#### 3.3.2 사용
umask를 지정하는 방법은 chmod와 동일하다.<br>
- 심볼릭모드 : chmod와 사용법이 똑같다
- 숫자모드 : 최대권한(umask적용이 안된 Full Permission)에서 umask 지정값을 빼면된다.

``umask -S``현재 지정된 umask를 심볼릭 모드로 모여준다.<br>
``umask 0655``umask 지정``umask 4`` 아규먼트가 한개,두개일경우 뒤에서부터 권한이 지정.


## 3.4 고급 권한
|종류|사용|내용|
|--|--|--|
|set uid|u+s|실행 권한이 있는 바이너리 파일/스크립트 파일만 사용|
|set gid|g+s||
|sticky bit|o+t||

1. Set Uid
- 바이너리 파일이나 스크립트 파일에 적용
- 실행한 사용자가 아닌 파일 소유자의 권한으로 실행
- user의 실행 권한이 x대신 s로 표시됨.

```
root 권한이 필요없는 프로그램에 소유주가 root로 되어있고 setuid가 설정되있을경우 보안적으로 매우 취약
일반 사용자로 접근하는 경우도 root로 실행이 가능해지기 때문
권한 상승의 문제 때문에 setuid설정은 최소화 해야한다.
```
> S와 s의 차이?? 실행권한의 유무의 차이이다.

> chomod u-s와 chmod u+s의 차이점 확인
/etc/passwd 파일의 경우 소유자가 root이므로 일반 사용자가 passwd명령어를 사용하더라고 파일의 소유자의 root권한으로 실행된다 하지만 s를 제거할경우 일반 사용자의 권한으로 파일이 실행되고 비밀번호는 바뀌지 않는다.


2. Set Gid
- 바이너리 파일이나 스크립트 파일에 적용할수있지만 일반적으로 디렉토리에 설정해놓고 사용하는 경우가 많다.
- 주로 데몬 프로세스가 사용하는 그룹의 권한을 공유하기 위해 사용


3. Sticky bit
- 디렉토리에 기타 사용자 권한이 rwx인 경우 모든 사용자가 해당 디렉토리에서 파일 생성 및 삭제 가능
- 특정 디렉토리를 누구나 자유롭게 사용하고자 할때 사용
- 단, 읽기나 쓰기는 문제가 되지 않을수있지만 삭제나 변경시 문제가 될수있기때문에 sticky bit가 설정되어있다면 이를 막을수있다.

> sticky 비트를 공유 모드라 함.

## 3.5 ACL (파일 세부권한)
- ACL로 좀더 세밀한 설정가능
- 디렉토리에 defaul ACL이 설정되어 있다면 하위 디렉토리가 상속받음.

```bash
ACL 설정
setfacl [op] ENTRY:NAME:PERMS file/dir-name
ACL 확인
getfacl file/dir-name

추가 및 변경
-m

삭제
-x  : 해당 ACL만 삭제
setfacl -x u:user01 fileA  # fileA에 부여된 user01 ACL 삭제
setfacl -x d:g:group2 dirA # dirA에 부여된 group2의 default ACL삭제

-b  : 설정된 모든 ACL 삭제
setfacl -b fileA
-k  : 파일/디렉토리에 설정된 default ACL 제거
setfacl -k fileA
```
|ENTRY|값|
|--|--|
|사용자|u|
|사용자 그룹|g|
|마스크|m|
|기타 사용자|o|
|default ACL|d|

> default acl은 파일에 적용하지않음 (상속이기때문)
기타 사용자나 마스크 일경우 NAME부분은 생략

### 3.5.1 ACL 재귀적 사용
```bash
setfacl -Rm ENTRY:NAME:PERMS file/dir-name # -R 재귀적 옵션 사용 -m 추가
ACL이 지정되지않은 디렉토리가 있을때 그 하위 디렉토리/파일들에 같은 ACL을 적용한다.
하지만 실행 권한이 포함될경우 일반 파일에도 실행권한이 적용될수있으므로 대문자X 로 사용하면 실행권한이 필요한 파일에 대해서만 실행권한을 적용할수있다.

```
### 3.5.2 ACL Mask
- **지정된 유저**와 **그룹**이 사용할 수 있는 최대 권한을 설정할수있다.
- user(본인)은 영향을 받지 않는다.
- AND연산을 한다.
- `#effective`로 출력 되는부분이 실제 사용할수있는 권한이다.
> chmod를 먼저 해서 전체 권한을 설정하고 ACL로 세부 권한을 주는것이 좋다. 번갈아서 사용하다보면 테이블이 꼬일수도있다.

## 4. File Descriptor FD
- 파일 디스크립터 테이블의 인덱스이다.
- 파일 오픈 or 소켓 생성 시 부여(3부터 시작) 가장 작은 값부터 할당됨.
- 프로세스가 열려있는 파일에 시스템 콜을 이용해서 접근시, FD값을 이용해 파일을 지칭할수있다.
- 시스템이 만들어 놓은것을 가리키기 좋게 하기 위해 시스템이 우리들에게 건네주는 숫자일 뿐이다.



## 5. 프로세스
## 6. 작업 예약
### 1. at
- 1회성 작업 예약

작업 목록 확인
```bash
at -l  alias atq
```
### 1.2 at 설치
### cron
- crond 데몬에서 관리
- crontab 에서 명령어 관리

``crontab -e`` vi 편집기 모드로 작업예약<br>
``crontab -l`` 현재 예약된 작업 목록을 보여줌<br>
``crontab -r`` 예약된 모든 작업 삭제<br>


|index|분|시|일|월|요일|
|--|--|--|--|--|--|
|1|*/5|13-17|*|*|*|  
2|20|14|8-14|3,6,9|2|

> 1. 매월 매일 오후 13시 부터 17시까지 5분마다 실행<br>2. 3월 6월 9월 2번째 화요일 14시 20분 마다 실행


## 7. 디스크 관리
### 7.1 디스크 파티션(MBR,GPT)
|MBR|GPT|
|--|--|
|- 주 파티션을 4개 까지 생성가능<br>- 디스크 용량 최대 2TB가지 인식|- 주파티션을 128개까지 생성가능<br>- 디스크 용량 최대 9TB까지 인식|
|Windows 32비트,64비트 사용가능|Windows 32비트 사용불가|

```
fdisk /dev/[장치 명]   // MBR방식
gdisk /dev/[장치 명]   // GPT방식
lsblk // 할당된 파티션 확인
partprobe //할당된 파티션이 안나타날 경우
```

LBA(Logical Block Address) : 섹터의 논리적 주소

### 7.2 파일 시스템
### 7.3 마운트
#### 7.3.1 개념
- 마운트 : 파일 시스템을 생성한 후 파일시스템에 접근할수있는 경로를 생성하는 과정
- 마운트 포인트 : 장치가 연결되는 디렉토리

#### 7.3.2 mount 사용
```bash
mount [op] partion mount-point

[op]
-t 마운트할 파일시스템 지정
-o 마운트시 세부옵션 지정, 없이 하면 default값 지정

-o 옵션 지정 시 tip
  - nosuid : suid 의 경우 보안상 문제를 야기 할수있다. setUid가 꼭필요한 경우가 아니면 nosuid를 사용
  - noatime : 파일 시스템의 inode에서 접근 시간 정보가 수정되도록 설정한다.
   파일정보 변경이 반영되어 성능에 영향을 미친다. 따라서, 파일 입출력이 줄어들어 성능이 개선될수있다.
  하지만,접근시간 없데이트가 안된다면 보안 감사및 추적이 불가능해지므로 사용에 주의가 필요함.
  - sync,async(default) : 파일시스템 데이터 변경 방식을 지정하는것.
  동기식은 이전데이터의 기록을 확인 한뒤에 데이터 기록 -> 디스크의 입출력 속도 저하되고 빈도가 증가하기때문에 디스크 수명에 영향을 끼칠수있음.
  비동기식 이전 데이터 기록을 확인하지않고 데이터를 버퍼로 전송함.
  데이터가 즉시 기록되지않을 가능성이 있어서 데이터가 손실될수있다.

마운트된 파일시스템의 목록과 마운트된 옵션 확인
mount | grep '장치명'

lost-found : 파일 시스템의 오류를 체크할때 발견되 파일들이 복구되는 디렉토리, 마운트 해제 시 보이지않음.
```


### 7.4 Swap(스왑) 메모리
#### 7.4.1 swap영역을 구성하는 방식
- 장치를 추가 해서 사용하는 방식
```
swap 파티션 구성
fdisk -> mkswap(mkfs와 같음) -> swapon (mount와 같다)

swap file 구성
mkdir -> dd swap (영역의 모든 데이터를 0으로 초기화)
-> mkswap -swapon

영구적인 적용을 위해 /etc/fstab에 저
```
- 디렉토리를 스왑 디렉토리로 구성하는 방식
swap file 구성
```bash
mkdir /swapdir # 스왑 디렉토리 구성

dd if=/dev/zero of=/swapdir/swapfile bs=1M count=1024 # /dev/zero파일을 swapfile로 1024M 크기만큼 복사

chmod 600 /swapdir/swapfile # 사용자만 읽고 쓰면된다.

mkswap  /swapdir/swapfile  # swapfile을 swap영역으로 설정

swapon /swapdir/swapfile # 파일/장치를 페이징과 스와핑을 허용
```


> /dev/zero
읽기를 위해 가능한 많은 널문자를 제공하는 특수 파일,일반적인 용도 데이터 스토리지를 초기화 하기위한 문자스트림 제공
fdisk 했을때 linux의 스왑메모리 헥사 코드는 82

### 7.5 논리 볼륨 관리
### 7.5.1 개요
- 논리 볼륨의 구성 순서
> 물리 볼륨 -> 볼륨 그룹 -> 논리 볼륨

- 기존의 파티션으로 디스크를 관리 -> 유연하게 사용할수없었다.
- 디스크를 유연하게 사용할수 있게 만들어 준다.
- 데이터를 유지한 상태로 볼륨 확장, 제거 가능
- RAID 적용가능


#### 7.5.2 물리 볼륨
- 파티션 단위로 생성.
```bash
pvcreate partion1 partion2 ...
```
#### 7.5.3 볼륨 그룹
- 최소 한개 이상의 물리볼륨이 필요
- 물리 볼륨의 집합으로 구성
- 볼륨 그룹 생성시 PE의 크기 지정 (Physical Extent, 기본 2^2M에서 2^8M 까지 가능,<br>안쓰는 PE모아서 따로 그룹을 지정해서 사용할수있다. (동적할당)
> PE는 볼륨 그룹에 소속된 물리 볼륨을 분할하는 최소 단위로
디스크 내에서 연속된 공간을 차지한다.

```bash
vgcreaete [op] vg-name pv-name
-s : PE의 크기를 지정 할수있다 (선언 안하면 default값 : 4M)

vgextend vg-name pv-name
```

#### 7.5.4 논리 볼륨
- lvextend 추가적으로 확장시 추가된 볼륨에는 파일 시스템이 적용되지 않아 마운트가 제대로 안될수있음. -r 옵션을 사용하면 파일 확장 명령어를 따로 입력하지 않아도됨.
- lvreduce 축소시에는 논리적으로 문제가 생길수있다. (확장은 쉽지만 축소는 어렵다.)
```bash
lvcreate [op] vg-name

lv 생성
lvcreate -n lv-name -L size vg-name

-l : 크기 지정
ex) PE size가 4M인 그룹에 lvcreate -l 100
  400M할당됨
-L : 크기 지정
ex) PE사이즈 상관없이 절대값으로 지정 lvcreate -L 1G
  1G할당
-n : 볼륨 그룹의 이름 지정

lvextend  # lv확장 -r 옵션을 사용하여 파일시스템을 같이 적용해준다
lvreduce  # lv 용량 감소
```
> 논리 볼륨을 적용 시 파티셔닝 -> pvcreate -> vgcreate ->lvcreate ->  mkfs ->  /etc/fstab 등록




## 8. Systemd
```
init
- 쉘 스크립트 기반으로 동작
- /etc/inittab : 실행시 시작한 runlevel 저장
- /etc/init.d : 동작 시킬 스크립트 파일 저장
- /etc/rc?.d : 해당 런레벨로 지정할시 실행되는 프로그램 저장

runlevel : systemd의 'target' 개념과 대치된다.

현재,init는 Systemd로 대체 되었다.
```
- unit 단위로 서비스를 관리한다.
  - 서비스 유닛 - http, sshd, ftp,
  - 소켓 유닛 - 프로세스간의 통신을 위해 생성


```
systemctl status service   # 상태 확인
systemctl start service    # 재부팅시 설정 사라짐(휘발성)
                    # 즉시 반영된다.

systemctl enable service   # 설정 영구 반영
                    # 바로 설정되지 않고 재부팅되면 반영된다.
                    # 심볼릭 링크가 생성됨

systemctl list-units  # 현재 동작하는 유닛
systemctl -all list-units # 모든 유닛 보기
systemctl -t service list-units # 서비스 타입 유닛 보기
systemctl -t target list-units # 타겟 타입 유닛 보기
systemctl -t service  # 타입 지정

systemctl stop service
systemctl restart service  # 서비스 종료 후 재 실행 (PID 변경)
systemctl reload service   # 서비스가 진행중인 상태에서 설정을 반영 가능 (PID 변경 안됨)

systemctl mask service       # /dev/null 에 심볼릭 링크 지정
systemctl unmask service     # mask 해제
systemctl list-dependencies service # service의 의존성 확인
```

## 9. 부트 프로세스

부트되는 과정
1. POST (Power On Self Test)
2. 부트로더 메모리에 적재, grub2를 적재하여 가능한 커널 목록 출력
3. initramfs 압축을 해제하여 /sysroot에 압축 해제
4. 필요한 파일을 메모리에 적재
5. default.target
6. multi-user.target (CLI)
7. graphical.target (GUI)


#### GUI CLI 변경
systemctl get-default
systemctl set-default <target-unit>
systemctl isolate <target-unit>
> target-unit : multi-user.target / graphical.target


#### root passswd 변경

1. 부트할때 initrd16 위에
rd.break 삽입 # ram disk break
2. mount -o remount,rw /sysroot
3. chroot /sysroot
4. touch /.autorelabel # 보안 때문에 변경 사항을 반영하겠다는 뜻
> 라벨링 ??



## 9. 네트워크 관리
```
ip addr show        # ip 확인
ip link set [network-name] down  # 인터페이스 비활성화

ip a add 192.168.122.100/24 dev [network-name] # network-name에 ip주소 추가
ip a del 192.168.122.100/24 dev [network-name] # ip주소 삭제

ip route show  # 연결된네트워크의 라우터 게이트웨이 확인
ip route add default via ip주소 dev [network-name]


/etc/resolv.conf # DNS 서버 ip
```
> ip 명령어는 이제 사용하지 않는다.

#### 9.2 네트워크 관리자  (nmcli)
- 네트워크와 관련된 모든 설정을 관리하는 서비스
- nmcli
```
네트워크 설정을 다시 해준다 -> DHCP 서버에서 동적 할당 해준걸 정적 할당으로 바꾼다.  

nmcli ip connection show # 시스템에 존재하는 네트워크 확인
nmcli ip connection show network-name # network-name의 세부사항 확인
```
#### nmcli로 네트워크 정적 할당
```bash
nmcli connection add con-name [이름] type [유형] ifname [활성화된 인터페이스 이름]
```
#### nmcli로 네트워크 정적 할당
- static 지정 순서
ip/SubnetMask설정 -> gateway 설정 -> dns 설정 -> 메소드 설정 )
```bash
nmcli ip con add con-name [이름] type [유형] ifname [활성화된 인터페이스] # type유형은 일반적으로 ethernet
nmcli con modify [이름] ipv4.address IP주소/prefix
nmcli con modify [이름] ipv4.dns 8.8.8.8 # 구글의 DNS서버
nmcli con modify [이름] ipv4.gateway GATEWAY주소
nmcli con modify [이름] ipv4.method manual # auto: 동적 manual : 정적
# 메소드 지정 안하면static으로 해도 DHCP 서버로 요청함
nmcli connection up [이름]  # 인터페이스에 매핑


nmcli connection delete [이름] # []이름] 네트워크 삭제
```
#### 네트워크 설정 파일 ifcfg
경로 ``etc/sysconfig/network-scripts/``의 하위 파일인 ifcfg 파일을 설정<br>
nmcli 명령어를 사용해도 되고 이 파일을 설정해도 같은 결과
> ipv4.method 의 변수이름은 BOOTPROTO이다.
정적 설정일 경우 none 동적 설정일 경우 dhcp
#### hostname 변경
```
hostname              # 호스트네임 확인
hostnamectl set-hostname name # hostname 변경
```


## 10. 시스템 로그
systemd 시스템은 rsyslog, system-journald두 데몬에 의해 관리 됨
systemd-journald 부팅 시작되는 순간부터 파일에 모든 로그 수집하여 rsyslog로 전달

systemd-journald -> rsyslogd -> logfile
rsyslog : 후처리를 통해 각 파일 별로 로그를 저장함
|파일|설명|
|--|--|
|/var/log/messages|대부분의 로그 기록|
|/var/log/secure|인증과 관련된 기록|
|/var/log/mailog|메일과 관련된 기록|
|/var/log/cron|주기적인 작업과 관련된 기록|
|/var/log/boot.log|부팅과정에 발생된 로그 기록|


logrotate : cron을 통해 주기적으로 실행되는 작업
/etc/logrotate.conf : 일괄 처리 방식 지정
/etc/logrotate.d/???  ???에 대한 개별 처리방식 지정


rsyslog
/etc/rsyslog.conf 파일에 rsyslog에 의해 전달되는 로그의 규칙들이 정의 되어있음
필터(filter)와 액션(action) 필드로 구성됨.
> 필터는 기능과 우선순위 형식으로 되어있다.
즉, 기능 우선순위 액션으로 구성

```bash
logger "내용"

/var/log/messages에 추가

logger -p authpriv.err "hello"
이 경우 /var/log/secure에 저장
우선 순위에 따라 메세지를 보낼수있다

```



## 소프트웨어 패키지
예전의 패키지 설치 방식
- 압축 or 아카이브 -> 파일 추출 -> 컴파일 -> 설치or 사용

- RPM 패키지 설치 (deb파일)
- YUM (apt or apt-get)
- DNF(8버전)

repogitory (저장소)
- 패키지를 저장해놓은 서버
- /etc/yum.repos.d 디렉토리에 저장 확장자는 repo 사용
- gpgkey 사용 여부
  - gpgcheck = 0 or 1 (0 미사용 1사용)

``sudo yum repoplist all`` yum-repos.d에 있는 repo들 등록<br>

``yum-config-manager --add-repo="http주소"`` http주소의 repo 추가 <br>
``var/log/yum.log``yum 설치로그 기록 파일
> .bak 파일 :컴퓨터로 작업중 생길수있는 전원 차단과 같은 갑자기 컴퓨터가 꺼질 경우를 대비해 자동으로 만드는 파일, 자동으로 디렉토리의 내용을 백업해주는 파일이다.

## open SSH
- 보안상의 이유로 telnet 보다는 SSH사용이 권장됨
> why? SSH는 데이터에 대한 암호화를 지원

- 비대칭키 암호화와 대칭키 암호화 알고리즘 사용.
- /etc/ssh 에 암호지정

```
접속 시 사용자 지정 안하면 접속이 안될수있다.
ssh user@ip주소  형식으로 접속한다.

설정파일 수정 후 재시작
/etc/ssh/sshd_config
```
설정파일 관련 설정<br>
``#PermitRootLogin yes``<br>
- 원격 접속자의 루트 접속의 허용 여부를 설정, 보통 막아두도록하자
- without-passwd 일경우 키 기반 인증 사용자만 접속 가능하도록함

``PasswordAuthentication yes``<br>
- 패스워드 사용여부 설정
- no일 경우 키 기반 인증만 사용가능함
> 키 기반 인증 사용자를 등록할때 no일 경우, 등록을 할수없으니 등록시에는 yes로 설정하자.

### ssh 키 인증
```
server에서 /etc/ssh/sshd_config파일을 수정하고
사용하고자 하는 client에서

ssh-keygen

사용하여 키 파일을 생성하고
접속하고자 하는 서버에

ssh-copy-id test@ip주소

입력하여 전달하도록한다.
```

#### 권장 팁
- root 접속은 모두 제한
- sudo 권한이있는 사용자만 키기반 인증설정
- 키 기반 만 접속허용

### scp 로컬 파일 복사
- cp + sshd
`` scp [option] origin destination
``



## NTP 서버
- 네트워크를 이용하여 정확한 시간 정보를 받아옴
- 사용 목적: 시스템의 정확한 시간이 필요할때
  시스템 간의 시간을 동기화가 필요할때
```
chronyd 는 /etc/chrony.conf 설정 파일을 참조하여 동작환경 설정


```


## 방화벽 firewall
- 네트워크 패킷을 필터링하는것은 Netfilter라는 커널 모듈이고 iptables는 Netfilter의 제어 도구일 뿐이다.
- 리눅스에서 제공하는 방화벽 서비스
  1. iptables
  2. firewalld

### 1. iptables
- 예전부터 사용된 오래된 방식 (여전히 사용)
- 규칙 변경시 서비스 재시작이 필요함 -> 오픈스택이나 가상화 같은 환경에서는 제약이 있음

### 2. firewalld
#### 2.1 특징
- XML 파일 형태로 보관
- Runtime 및 Permanent 설정
- 사전 정의된 영역
- 사전 정의된 서비스
- D-Bus사용

##### 경로
- /usr/lib/firewalld
```
기본 구성과 관련된 파일과 대비용 파일 저장
```
- /etc/firewalld
```
firewalld에 실제로 적용되는 설정파일과 규칙 저장
```
xml 파일을 직접 수정하여 방화벽 규칙 수정가능.

사전에 정의 된 영역
|||
|--|--|
|trusted(변경 불가)|- 모든 패킷 허용<br>- 사용에 주의 필요|
|block|- 모든 패킷 거부<br>- 내부에서 외부로의 반환 패킷은 허용<br>- 거부된 원인을 알려준다|
|drop|- 내부로 들어오는 모든 패킷 폐기<br>- ICMP 에러도 폐기<br>- 외부로 연결은 가능<br>- block과 차이|
|dmz|- 내부로 들어오는 패킷 거부<br>- 외부로의 연결,ssh 서비스 허용|
|public |- default zone<br>- 내부로 들어오는 패킷 거부|
