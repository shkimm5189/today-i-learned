# 개념


# 가상화
CPU 성능이 너무 좋아져서 뭐가 좋다고 하기 어려울 수있지만 한번 다루고 넘어야함.
### 전가상화(Full-Virtualization)
게스트 OS명령이 Binary Translation 과정을 거쳐 하이퍼바이저의 중재를 통해 하드웨어로 전달, Binary Translation 과정에서 지연 발생.

### 반가상화(Para-Virtualization)
전가상화와 달리 Binary Translation 과정 없이 Hyper Call이라는 인터페이스를 통해 게스트 OS명령이 하이퍼 바이저를 거쳐 하드웨어로 전달.
Binary Translation과정이 없기 때문에 전가상화보다 빠름.

### 하드웨어 지원 가상화 (Hardware-assited Virtualization) -HVM
- 현재 퍼블릭 클라우드가 사용하는 머신
- Binary Translation을 하이퍼 바이저가 아닌 CPU에서 처리하므로써 성능을 큰폭으로 향상시킴.

# 하이퍼바이저 종류
### Type 1 (native or bare-metal)
운영체제가 프로그램을 제어하듯, 하이퍼 바이저가 해당 하드웨어에서 직접 실행되며 게스트 운영체제는 하드웨어 위에서 2번째 수준으로 실행

클라우드용, 실무용
### Type 2 (hosted)
일반 프로그램과 같이 호스트 운영체제에서 실행되며 VM내부에서 동작되는 게스트 운영체제는 하드웨어에서 3번째 수준으로 실행

실습용

host os가 존재함.

# 클라우드 종류

### Public Cloud

### Private Cloud

### on-Premise 환경

위의 모든 종류를 합쳤을 때 Hybrid Cloud라 할수있다.

cloud 끼리만 합쳤을때 Multi Cloud


# 퍼블릭 클라우드 핵심 서비스
1. Compute Service : CPU, RAM 용량 설정
2. Storage Service : HDD, SSD 용량 설정
3. Network Service : NIC, S/W 설계
4. Image Service : OS 운영체제 설정
5. Identity(Security) Service : Username, Password, 역할, 방화벽


# AWS 서비스 - EC2
## Regin (지역)
ap-northeast-2 (서울)

availability zone 가용성 영역(데이터 센터)

키페어 생성후 .cer파일 다운로드
.cer 파일에 400 권한 부여 후
ssh -i 20210621.cer ec2-user@web01.cccr.shop 로그인


# 로드 밸런서
- Elastic Load balancing
- 로드밸런싱 부하분산

# CLB (Classic Load Balancer)
- 기본적인 로드 밸런서
- L4 스위치 역할을 해서 포워딩하는 역할
- 보안 그룹을 CLB를 별도로 나눠서 관리
- 리스너에 HTTPS를 사용해야 보안적으로 개선됨 (경고 메세지 나옴)
- health check


## EBS (Elastic Block Storage)
-

### EBS Snapshot
- 타임머신 기능, 복사, 백업
> 퍼블릭 클라우드에서 백업이란? 해당 스토리지를 복사해서 외부로 뺄수있다. 서로 다른 가용 영역으로 복사를 통해 데이터를 옮기는 것
virtual 머신에서는 타임머신 기능을 통한 되돌리기 기능이고 넗은 의미의 백업의 개념까지는 아니다.

- 서로 다른 가용 영역사용하는 인스턴스로 블록 스토리지를 보낼수있다.
```
senario 1
2a 추가볼륨 > 스냅샷 > 2c 볼륨 생성 > web02에 연결
2a영역에서 사용된 볼륨을 스냅샷을 만들고, 2c 볼륨으로 만들어 web02인스턴스에 연결

주의사항 : 파일시스템이 적용 된 스냅샷 스토리지를 다른 인스턴스에 마운트 할때 mkfs명령어를 다시 사용하면 초기화 된다. 그냥 마운트만 하면된다.
```

```
senario2
web01 스냅샷을 복사해서 도쿄 region으로 백업하자 (현재 seoul)
web01 볼륨 > 스냅샷 > 복사(도쿄로 백업) > 도쿄에서 인스턴스 생성
```


## 보안 그룹
```
default 보안 그룹은 생성하지않음. => 해커의 타켓 1순위
직접 만드는게 기본 상식
efs로 보안 그룹을 생성

인바운드 규칙
접근 허용
아웃 바운드 규칙
방화벽에서 나가는 규칙은 대부분 신경쓰지 않는다
-> 그래도 설정하면 보안적으로 좋겠지?
```



### S3 ( 클라우드의 확장 가능한 객체 스토리지 )
- block > file > object
- 상품 이지미 호스팅(iso같은 이미지가 아니라 jpg)



### VPC
1. 1개의 InterNet Gateway는 1개의 VPC에만 연결할수있다.
VPC Architect
![]()


인스턴스로 AMI생성시 스냅샷이 같이 생성되는데 설정한 OS,Service등이 스냅샷에 저장된다.

```
SSH는 내IP(출발지)로 설정하자 다만, IP가 바뀌었을경우 접근하기 힘듦
```



# Azure
```
region이 2개이다.
한국 중부(서울), 힌국 남부(부산)
단점 : 하나의 리전에 가용 영역이 1개뿐이다.
why? 가용성이 떨어진다. 다른 region에는 그렇지 않다.
```
[](azureregion)

```
역할 할당시
소유자
소유자 권한이 있을 경우 root계정을 지울수가 있다.
그러니 기여자 계정으로 만들자
기여자
```


```
L4, L7 스위치 구축시 퍼블릭 아이피가 없어야한다. (퍼블릭 IP를 가질경우 로드밸런서의 역할이 의미가 없어짐)
why? 퍼블릭아이피를 가진다면 스위치를 거치지않고 직접적으로 접근이 가능하기 때문에 원칙적으로 서버는 퍼블릭 IP를 가져서는 안된다.
-> 백엔드에있는 서버들은 퍼블릭IP가 없어야함.

```


```

AWS ELB-CLB Round Robin 방식 (순차적으로 접속) - 실습용

Azure L4 Least Connection 방식(최소 연결 접속 되는 것) - 실무용

```
