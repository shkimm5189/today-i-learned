# ROOT PASSWD 변경


# 레포지토리  
```bash
[id]
name=???
baseurl=
enabled=1     # 활성화 여부
gpgcheck=0

```
# NTP
```
utilty.network.example.co.kr 을 ntp서버 목록에 추가되도록 구성하세요.

vi /etc/chrony.conf

server utilty.network.example.co.kr iburst
systemctl restart chrony.service
```

#grep
```
/etc/sudoers 에서 주석을 제외한 내용을 출력해서 sudoerstest file에 저장하세요

grep -v ^# /etc/sudoers > sudoerstest

grep ^[^#] /etc/sudoers > sudoerstest
```

1.
```
ip 주소 : 192.168.111.111
netmask 255.255.255.0
ip이름주소 192.168.111.2
gateway 192.168.111.2

호스트네임 Client
```
2.
```
group yellow 생성
user red,blue를 기본그룹 yellow생성
user orange를 보조그룹 yellow를 추가하여 생성
useradd -G yellow orange
user green을 uid 1234로 만듦
useradd -u 1234 green
user purple를 로그인 안되는 쉘로 설정
useradd -s /sbin/nologin purple
```
3.
```
repogitory http://ftp.daum.net/centos/7/os/$basearch/ 로 설정하세요

.repo 설정으로 해주세요
yum install httpd-manual

```
[id]
name=
baseurl=
exabled=
gpgcheck=





4. grep systemd /var/lof/message > /home/test.context

5.
```
find / -user unbound -


주로 사용함 -name -perm -user
```

6.
```
압축 cvf
해제 xvf
확인 tf
```

7.
```
비표준 포트 100에서 실행중인 웹서버 콘텐층를 제공하는 error발생

다음 수행할수있도록 변경을 허용하라

웹서버는 시스템 부팅 시간에 자동으로 시작되어야함.

what? firewall, selinux
```

8.
```
2.centos.server.ntp.org의 NTP 클라이언트가 되도록 구성

/etc/chrony.conf

server 2.centos.server.ntp.org iburst

chronyc sources /확인
systemctl restart chronyd
```

9.
```  
/home/green/ 디렉터리의 소유자를 red 소유그룹을 blue로 변경
해당 디렉토리를 소유그룹만 읽고 쓰고 엑세스 할수있도록 변경
디렉터리에서 만들어진 파일에는 자동으로 blue에 소유권이 있도록 설정 # chown -R 로 했지만 그러면 만들어지는 파일에는 적용이 안된다. setGID 해야함.

```

10.
```
/etc/passwd 파일을 /home/yellow/에 복사
해당 파일을 소유자와 소유그룹을 yellow로 변경
해당 파일을 누구든지 쓰거나 실행할수없도록 변경 #뭔소린지?
chmod a-wx file

해당 파일을 다른 사용자들은 읽고 쓰고 엑세스 할수 없도록 변경
chmod o-rwx file

해당파일을 유저 orange는 일고 쓸수있도록 변경
해당파일을 유저 green은 읽고 쓸수없도록 변경

다른 모든 사용자

chmod a-r /var/tmp/passwd
```
11.
```
 yellow 그룹에 속한 모든 사용자가 암호를 입력하지않고 명령을 실행할수있도록 yellow 그룹에 sudo 권한 구성

visudo
%yellow  ALL=(ALL) NOPASSWD: ALL

그냥 외우자 실제 시험에선 예시가 없을수있다.
```
12.
```
새로운 사용자를 추가할 경우, 사용자의 암호가 최소 20일은 사용하도록 설정
/etc/login.defs

매분마다 blue권한으로 logger test 실행
/etc/cron.d 에 생성해도됨
crontab

/var/log/cron
*/1 * * * *
```
13.
```

```

14.
```
/etc/containers/registries.conf

registries 변수 확인

podman search
podman pull
podman run
podman start
podman ps

사용할수있는 레지스트리에서 aa-bb-cc 받고 이름을 dd로 실행하세요
레지스트리에서 사용할수있는 것 확인
podman search registry.access.redhat.com/aa-bb-cc

사용할것 가져오기
podman pull registry.access.redhat.com/rhel8/aa-bb-cc # rhel8 반드시 넣을것 외우장

podman run --name dd registry.access.redhat.com/rhel8/aa-bb-cc

podnab start  dd

podman ps # 실행중인것 확인
기존 사용자 yellow에 대해서만 실행해야 하는  # yellow에 로그인을해서 레지스트리 받고 실행해야함.
systemd 서비스로 실행하도록 구성

loginctl enable-linger # 일반 사용자도 컨테이너를 생성하게 해줌 -> loginctl show-user user 명령어 후 Link=yes 가 되어있어야함
# 일반사용자로 이동
mkdir -p .config/systemd/user

podman generate systemd --name dd --files

서비스는 container-aa로 이름이 지정되어야 하며 재부팅후 자동 실행

# systemctl --user enable container-aa.service 


https://blog.majecty.com/wikis/2019-01-13-a-systemd-user.html systemd

```
15.
```
논리볼륨 이름은 one 볼룸 그룹이름은 num 50 확장영역크기
볼륨 그룹 num의 논리 볼륨에는 8M 확장영역크기가 있어야한다.
vfat파일 시스템으로 새 논리 볼륨을 포맷 /mnt/one 자동 마운트
```

16.
```
논리 볼륨 one 의 크기를 200M 확장
lvextend -L +200M /dev/num/one -r
파일 시스템 크기도 같이 확장

resize2fs /dev/num/one


#만들어진것을 확장

```



rd
%group ALl=(ALL) NOPASSWD: yes
17.
```
300M swap 파티션 추가

swapon --show
```

18.
tuned : 시스템을 최적화 해주는 서비스
yum insatll tuned
tuned-adm

```
1. balanced로 tuned 프로필을 선택하고 기본으로 설정
tuned-adm profile balanced
2. 시스템이 권장하는 tuned프로필을 선택하고 기본으로 설정
tuned-adm recommend
tuned-adm profile ...

확인
tuned-adm active

systemctl enable tuned
```

19.
vdo -> 디스크 압축
장점 : 공간을 효율적으로 사용
단점 : 성능이 저하  -> 30~40% 저하
백업 스토리지 만들때 사용
순서
1. 0만 포함하는 데이터 블록을 메타 데이터화 -> 사이즈 다운
2. 중복 데이터 블록 제거 -> 동일한 복사본을 만들면 참조형식으로 변경
3. 데이터 블록 압축


```
반드시 설치

yum install vdo


vdo create -n <vdo name> --device <block device> --vdoLogicalSize <Mega>

# vdo 리스트 확인
vdo list

# vdo 상태 확인
vdo status -n <vdo name>

# vdo 삭제
vdo remove -n <vdo name>

/dev/mapper/vdoname  <- 장치 이름


# 영구 마운트
# 옵션 부분은 반드시 외워야함.
/dev/mapper/vdo1 /mnt/vdo xfs defaults,x-systemd.requires=vdo.service 0 0

```

```
1. 파티션되지 않은 디스크 사용
2. 볼륨이름 vv 논리 크기 50G
3. xfs 파일시스템 사용하여 포맷
4. /vbread 에서 부팅시간에 마운트
```




fucking podman
```
yum module install container-tools

root에서 ssh student@localhost에서 진행해야함

podman serach registry.access.redhat.com/???/rsyslog

registry.access.redhat.com/rhel7/rsyslog
???부분을 찾기위해 서치해야함

--사용자에서 진
podman pull registry.access.redhat.com/rhel7/rsyslog

podman run --name mylog registry.access.redhat.com/rhel7/rsyslog


안되면 podman restart mylog

podman restart mylog

podman ps 확인

-- root 변경

loginctl enable-linger

```


.config/systemd/user


podman gernerate --name dd --files

systemctl
