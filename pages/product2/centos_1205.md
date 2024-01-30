---
title: 1205
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_1205.html
folder: product2
---


### 01. Network 기초


- Network 기초(제일 중요) → Subnetting → Supernetting


```yaml
IP주소 (집주소) = x.x.x.x
                = 10진수(0~255).10진수(0~255).10진수(0~255).10진수(0~255)
                = 8bit.8bit.8bit.8bit = 32bit
                = Network 주소 자리(동) + Host 주소 자리(번지)
```

- 10진수 한 자리는 8비트로 구성되어 있음.


```yaml

8bit = 00000000
     = 0
8bit = 11111111
     = 128(2^7)  64(2^6)  32(2^5)  16(2^4)  8(2^3)  4(2^2)  2(2^1)  1(2^0)  ← 암기 !
     = 255

```


### 02. Classful Address  ← 암기 !

IPv4 = (?).10진수.10진수.10진수 ← 처음에 따라 클래스가 나뉨.

```yaml

- A Class : 1-126      서브넷 마스크(Subnet Mask) : 255.0.0.0 = /8 (1의 개수가 8개)
- B Class : 128-191    서브넷 마스크(Subnet Mask) : 255.255.0.0 = /16 (2번째자리까지넽웤)
- C Class : 192-223     서브넷 마스크(Subnet Mask) : 255.255.255.0 = /24 (4번째만호스트주소)

```


Subnet Mask

1 = Network 주소 자리

0 = Host 주소 자리


```yaml
ex.

  IP 주소 = 192.168.10.1

  Subnet Mask = 255.255.255.0

  Network 주소 = Host 주소 자리가 전부 0 ex.청학동

  Broadcast 주소 = Host 주소 자리가 전부 1 ex.청학동에 있는 모든 집 주소

  할당 가능한 주소 = IP주소는 할당 가능 주소 중 하나 사용 하고 있는 것

  주소 개수 = Host 개수를 의미함
  주의 : 항상 -2를 함, Network 주소랑 Broadcast 주소는 할당 할 수 없기 때문.

```


### 03. Ex 풀어보기


ex. 01
```yaml
IP 주소 = 192.168.10.1
Subnet Mask = 255.255.255.0
Network 주소 = 192.168.10.0 (Host 주소 자리가 전부 0) ex.청학동
Broadcast 주소 = 192.168.10.255 (Host 주소 자리가 전부 1) ex.청학동에 있는 모든 집 주소
할당 가능한 주소 = 192.168.10.1 - 192.168.10.254 (IP주소는 할당 가능 주소 중 하나 사용 하고 있는 것)
주소 개수 = 2^8 - 2 = ? (Host 개수를 의미함, 주의 : 항상 -2를 함, Network 주소랑 Broadcast 주소는 할당 할 수 없기 때문)
```


ex. 02
```yaml
IP 주소 = 126.255.0.1
Subnet Mask = 255.0.0.0
Network 주소 = 126.0.0.0
Broadcast 주소 = 126.255.255.255
할당 가능한 주소 = 126.0.0.1 - 126.255.255.254
주소 개수 = 2^24 - 2
```


ex. 03
```yaml
IP 주소 = 191.255.0.1
Subnet Mask = 255.255.0.0
Network 주소 = 191.255.0.0
Broadcast 주소 = 191.255.255.255
할당 가능한 주소 = 191.255.0.1 - 191.255.255.254
주소 개수 = 2^16 - 2
```


ex. 04
```yaml
IP 주소 = 223.0.255.254
Subnet Mask = 255.255.255.0
Network 주소 = 223.0.255.0
Broadcast 주소 = 223.0.255.255
할당 가능한 주소 = 223.0.255.1 - 223.0.255.254
주소 개수 = 2^8 - 2
```


ex. 05
```yaml
IP 주소 = 128.0.0.1
Subnet Mask = 255.255.0.0
Network 주소 = 128.0.0.0
Broadcast 주소 = 128.0.255.255
할당 가능한 주소 = 128.0.0.1 - 128.0.255.254
주소 개수 = 2^16 - 2
```






### 04. Network 주소가 같은/다른 경우

- 클래스별 고정/가변

고정/가변

A 0/ <br/>
B 10/ <br/>
C 110/ <br/>
D 1110/ <br/>
E 1111/ <br/>

- 실제 할당은 A, B, C 밖에 없음. (D, E는 따로 빼둔 것.)
- A, B, C가 중요.

- IPv4에서의 통신방법 <- 암기

```yaml
unicast(일대일), multicast(일대그룹), broadcast(일대다수)
```


- 네트워크 주소란 ? 호스트 자리가 다 0인 경우.

- 허브 : PC를 묶어주는 장치
- 스위치 : PC를 묶어주는 장치

1. 네트워크 주소 자리가 같아야지만 통신이 가능한 경우,

```yaml
  PC-PC / PC-HUB-PC / PC-SWITCH-PC / ROUTER-ROUTER
```
  (네트워크 주소가 다른 경우 PC끼리 통신이 안 됨.)

ex. A클래스 일 경우, 네트워크 주소인 첫 번재 자리만 같으면 PC끼리 통신이 가능함.
호스트 주소는 당연히 번지니까 달라야 함.


1. 네트워크 주소 자리가 달라야지만 통신이 가능한 경우,

```yaml
  PC-ROUTER-PC
```

  - 여기서 라우터는 다른 네트워크 주소를 연결해 주는 장치임.
  - 라우터는 팔이 2개 이기 때문에 주소가 2개 필요함.
  - 라우터에 연결 된 것들은 반드시 네트워크 주소가 달라야 함.



1. 결론 : 모든 건 네트워크 주소가 중요하다 !






### 참고.

```yaml

A Class : 1-126                                255./0.0.0=/8
0/0000000(0) - 0/1111111(127)   <예>0.x.x.x(X),  127.0.0.1(Loopback주소=자기자신)

B Class : 128-191                            255.255./0.0=/16
10/000000(128) - 10/111111(191)

C Class : 192-223                            255.255.255./0=/24
110/00000(192) - 110/11111(223)

D Class: 224-239 (Multicast 주소)   <예> 224.0.0.90=인사부,   224.1.1.100=총무부
1110/0000(224) - 1110/1111(239)

E Class: 240-255 (연구용)
1111/0000(240) - 1111/1111(255)
```
※ IPv4 통신 방법 : unicast(1:1), multicast(1:Group), broadcast(1:다)

▣ Network주소가 같은 경우
```yaml
PC-PC,  PC-HUB-PC, PC-SWITCH-PC, Router-Router

PC1----------------------------PC2
192.168.10.1/24                      192.168.20.1/24 = x
126.255.0.1/8                           126.0.255.1/8 =o
191.0.255.1/16                         191.255.0.1/16 = x
223.0.0.1/24                              223.0.1.2/24 = x
192.168.10.1/24                       192.168.10.2/24 = 0
```
▣ Network주소가 다른 경우(Router)
```yaml
PC1-----------------(Router)-----------------PC2
1.1.1.1               1.1.1.2     1.1.1.3                  1.1.1.4  = x
1.1.1.1               1.1.1.2      2.1.1.2                 2.1.1.1  = 0
```








### 05. Router에 주소 주는 방법.

![1](https://user-images.githubusercontent.com/117553252/211124567-b5ecf71c-b7c4-43e6-a908-e62ac3e7bbe5.png)


```yaml

>enable
#conf t

(config)# interface e0/0
(config-if)# ip address 192.168.10.2 255.255.255.0
(config-if)# no shutdown
(config-if)# exit

(config)# interface e0/1
(config-if)# ip address 192.168.20.2 255.255.255.0
(config-if)# no shutdown
(config-if)# end

```



- 라우터 명령어

  1. enable
  1. conf t
  1. interface e0/0 : e0/0으로 들어가라
  1. ip address : ip 주소 입력
  1. no shutdown : 활성화
  1. exit : 나가기
  1. end : 끝내기 -> #으로 돌아옴.
  
