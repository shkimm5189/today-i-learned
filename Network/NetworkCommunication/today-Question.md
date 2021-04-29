# 4.29
## STP
무슨 프로토콜인가?
왜 사용할까?
단점은? 단점을 보완하기 위해?

PVST per vlan STP vlan마다 STP 계산

vlan 10개 STP 10개
vlan 100개 STP 100개

장점
경로 수만큼 vlan생성해서 로드밸런싱 가능
단점
vlan 많을 수록 부하

CST common STP ,VLAN 수 상관없이 STP 1개 계산

vlan 10개 STP 1개
vlan 100개 STP 1개
장점
vlan수가 많아도 부하 없음
단점
vlan으로 로드밸런싱이 안됨
