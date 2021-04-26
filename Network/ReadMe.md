# 네트워크 정리
네트워크를 정리하면서 이해할 수 있게 해보자 by saintKim
# OSI-7 계층(TCP/IP 모델)
| Layer | 이름                   |      | TCP/IP 모델 | 데이터 단위 | protocol |
| ----- | ---------------------- | ---- | ----------- | ----------- | -------- |
| 7계층 | Application Layer (L7) |      |             | 메세지      |          |
|6계층| Presentaion Layer (L6)| 표현 계층 |  Application |메세지
|5계층| Session Layer (L5) |||메세지|
|4계층| Transport Layer (L4)| 전송 계층 |Trasport Layer |세그먼트|TCP,UDP|
|3계층| Network Layer (L3)| |Internet| 패킷|IP,ICMP,IGMP,ARP,RARP|
|2계층| Data Link Layer (L2)| ||프레임|
|1계층| Physical Layer (L1)| 물리 계층 | network Access|비트|



**TCP/IP 프로토콜은 이론보다 실용성에 중점을 둔 프로토콜**

  ##  1계층(Physical Layer)
 ```
 주로 전기 신호를 전달하는데 초점이 맞춰짐.
 전기신호를 잘 보내는 것이 목적
 
1계층의 장비는 주소의 개념이 없기에 전기 신호가 들어온 포트를 제외한 모든 포트에 같은 전기 신호를 전송.

데이터 단위 : 비트

 주요 장비 : Hub,Repeater,Cable,Connetor,Tranceiver, Tap
 ```
  ## 2계층(DataLink Layer)
```
주소 정보를 정의하고 정확한 주소로 통신되도록 하는데 초점.
출발지와 도착지를 확인하고 데이터 처리.

주소 체계가 생기기 때문에 여러 통신이 한꺼번에 이루어지는 것을 구분하기 위한 기능이 주로 정의.
이더넷 기반의 네트워크의 L2에서는 에러를 탐지하는 역할 수행.
무작정 데이터를 던지는 것이 아니라 수신자가 현재 데이터를 받을 수있는 상태인지 확인이 선행된다(Flow Control). 

사용 프로토콜 : 이더넷 ...

데이터의 단위 : 데이터 프레임

MTU : 어떤 데이터링크에서 하나의 프레임 또는 패킷에 담아 운반 가능한 최대 크기
- 데이터 링크계층에서는 자신이 속한 해당 네트워크에 대한 MTU값을 알아야한다.
- 그 상위계층 프로토콜들에게 해당 MTU 값을 알려주어야함

구성 요소
네트워크 인터페이스 카드(NIC), 스위치
 * 스위치 : MAC 주소를 보고 통신해야 할 포트를 지정해 내보내는 능력을 가짐. 
가장 중요한 것은 MAC 주소라는 주소 체계가 있다는 것.

```
* [MAC주소](www.naver.ocm)
* 
## 3계층 (Network Layer)
IP 프로토콜(IP주소)

## 4계층 (Transport Layer)


## 참고 자료
> IT엔지니어를 위한 네트워크 입문 / 고재성,이상훈
> Network Top-down approach