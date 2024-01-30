---
title: 0112
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_0112.html
folder: product2
---


### Ex.Split-Horizon<br/>

추가하기


### BGP Next Hop 해결 (AS 2개 부터)
<br/>

```yaml
BGP로 받은 경로의 next-hop 주소가 라우팅 테이블에서 확인되지 않으면 라우팅 테이블에 올릴 수 없다. (BGP 테이블 # show ip bgp 에는 보임)
    즉, 받은 BGP 네트워크 주소를 이웃 BGP neighbor에게 보낼 수 없음.
```

<br/>
- 해결 01. DMZ를 IGP에 포함시키기 <br/>
- 해결 02. next-hop-self 옵션 사용 (자신의 router-id 가 넥스트 홉) <br/><br/>

- 목적지 네트워크로 가기 위한 다음 라우터를 넥스트 홉(next hop) 라우터 라고 한다. <br/>
- BGP는 광고 받은 네트워크의 넥스트 홉 주소가 라우팅 가능한 것이어야만 해당 네트워크를 사용할 수 있다. ★ <br/><br/><br/>



### 해결 02. next-hop-self 옵션 
<br/>
사진 첨부
<br/>
이어서
<br/>
R3, R4) eigrp 100 -> no 3점대
<br/>
사진 첨부
<br/><br/>


#### 방법.
<br/>

```yaml
R3)
    router bgp 100
    neighbor 1.1.12.1 next-hop-self
    neighbor 2.2.12.2 next-hop-self

R6)
    router bgp 200
    neighbor 4.4.12.5 next-hop-self
```

<br/><br/>

#### 결과.
<br/>
사진첨부
<br/>
다 들어가진 것을 확인할 수 있따.
<br/><br/>
사진첨부




### BGP 동기화 법칙 해결
<br/>

```yaml
iBGP로 수신한 정보는 다시 IGP(RIP/EIGRP/OSPF...)로 동일하게 해당 정보를 광고 받아야 사용할 수 있다.
중간에 BGP가 구성되어 있지 않은 라우터에서 BlackHole 현상 방지 목적임.
```

<br/>
- 해결 01. No sync (기본값) <br/>
- 해결 02. BGP를 IGP에게 재분배 (AS 경계 라우터에서 재분배) <br/>
- 해결 03. Confederation <br/><br/>


사진첨부
<br/><br/>

- 동기화 법칙은 기본적으로 비활성화 되어 있음<br/>
: 신경 쓸 것이 없음. = `No sync`<br/><br/>

- 다시 원상복귀 된 채로 시작.<br/><br/>

### Sync<br/>

-> 어떤 변화가 있는지 확인하려는 용도.<br/><br/>

R3)<br/>

```yaml
router bgp 100
sync

# clear ip bgp *
```

<br/><br/>

- 결과 X<br/>

사진첨부
<br/>
BGP AS 200 보임.
<br/><br/>

사진첨부
<br/>
BGP AS 200은 안 보임.<br/>
(이런 경우에는 라우팅 테이블에 올리지 않는다는 뜻)
<br/><br/>

- 결과 X<br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

#### 최종 결과 - R1, R2, R3 모두 Clear<br/>

- `#show ip bgp`는 전부 다 뜨는 게 맞는 것이고<br/>
라우팅 테이블은 30까지만 뜨는 게 맞음.<br/><br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

- `clear ip bgp *` 하고 처음만 되고 나머지는 바로 동기화 되는 듯 함.<br/><br/>

- Sync의 목적 : `BlackHole` 현상<br/><br/>


### Ex. 01<br/>

사진첨부
<br/><br/>

- 10.1에서 60.1까지 ping이 다 되어야 하고<br/>
TTL 값은 최소 3으로 주어야 함.<br/>
전체 라우터에서 ip route 명령어 최소화 해야 됨.<br/><br/>

#### 방법<br/>

R1)<br/>

```yaml
router bgp 100
bgp router-id 1.1.1.1
neighbor 1.1.12.2 remote-as 100
network 192.168.10.0 mask 255.255.255.0

ip route 4.4.12.0 255.255.255.0 1.1.12.2
```

<br/><br/>

R2)<br/>

```yaml
router bgp 100
bgp router-id 2.2.2.2
neighbor 1.1.12.1 remote-as 100
neighbor 4.4.12.5 remote-as 200
neighbor 4.4.12.5 ebgp-multihop 3

ip route 4.4.12.0 255.255.255.0 2.2.12.3
```

<br/><br/>

R3)<br/>

```yaml
ip route 192.168.10.0 255.255.255.0 2.2.12.2
ip route 192.168.60.0 255.255.255.0 3.3.12.4
```

<br/><br/>

R4)<br/>

```yaml
ip route 192.168.10.0 255.255.255.0 3.3.12.3
ip route 192.168.60.0 255.255.255.0 4.4.12.5
```

<br/><br/>

R5)<br/>

```yaml
router bgp 200
bgp router-id 5.5.5.5
neighbor 5.5.12.6 remote-as 200
neighbor 2.2.12.2 remote-as 100
neighbor 2.2.12.2 ebgp-multihop 3

ip route 2.2.12.0 255.255.255.0 4.4.12.4
```

<br/><br/>

R6)<br/>

```yaml
router bgp 200
bgp router-id 6.6.6.6
neighbor 5.5.12.5 remote-as 200
network 192.168.60.0 mask 255.255.255.0

ip route 2.2.12.0 255.255.255.0 5.5.12.5
```

<br/><br/>

#### 결과<br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

R5)<br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

#### 방법 02. next-hop-self 옵션<br/>

R2) -> rip에 2.2.12.0 지우고 시작<br/>

```yaml
router bgp 100
neighbor 1.1.12.1 next-hop-self
```

<br/><br/>

R5) -> ospf에 4.4.12.0 지우고 시작<br/>

```yaml
router bgp 200
neighbor 5.5.12.6 next-hop-self
```

<br/><br/>

#### 결과<br/>

R1)<br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

### Ex. 02.<br/>

사진첨부
<br/><br/>

- 라우팅 테이블에 있는 네트워크 주소를,<br/>
BGP는 network 명령어로 포함할 수 있음.<br/><br/>

#### 방법<br/>

R3)<br/>

```yaml
router bgp 100
neighbor 3.3.12.4 remote-as 200
network 192.168.10.0 mask 255.255.255.0
network 192.168.20.0 mask 255.255.255.0
network 192.168.30.0 mask 255.255.255.0

ip route 0.0.0.0 0.0.0.0 null 0

router eigrp 100
network 0.0.0.0
```

<br/><br/>

R4)<br/>

```yaml
router bgp 200
neighbor 3.3.12.3 remote-as 100
network 192.168.40.0 mask 255.255.255.0
network 192.168.50.0 mask 255.255.255.0
network 192.168.60.0 mask 255.255.255.0

router ospf 1
default-information originate always
```

<br/><br/>
