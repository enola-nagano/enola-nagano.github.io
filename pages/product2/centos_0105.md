---
title: 0105
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_0105.html
folder: product2
---


### Ex. Build Up (ft.Frame-Relay)<br/>


![Untitled](https://user-images.githubusercontent.com/117553252/229319460-3bb63337-9c5e-4e6b-baf2-e69013cf264d.png)
<br/><br/>

DLCI 102의 의미
R1에서 FR까지의 길을 의미함. (논리적인 길)
<br/><br/>

#### 방법<br/>

1.ip주소<br/>

2.FR<br/>


R1)<br/>

```yaml
interface s1/0
encapsulation frame-relay
no shutdown

interface s1/0.12 point-to-point
ip address 1.1.12.1 255.255.255.0
frame-relay interface-dlci 102
```
<br/><br/>

R2)<br/>

```yaml
interface s1/0
encapsulation frame-relay
no shutdown

interface s1/0.12 point-to-point
ip address 1.1.12.2 255.255.255.0
frame-relay interface-dlci 201

interface s1/0.23 point-to-point
ip address 1.1.23.2 255.255.255.0
frame-relay interface-dlci 203
```

<br/><br/>


R3)<br/>

```yaml
interface s1/0
encapsulation frame-relay
no shutdown

interface s1/0.23 point-to-point
ip address 1.1.23.3 255.255.255.0
frame-relay interface-dlci 302

interface s1/0.34 point-to-point
ip address 1.1.34.3 255.255.255.0
frame-relay interface-dlci 304
```

<br/><br/>


R4)<br/>

```yaml
interface s1/0
encapsulation frame-relay
no shutdown

interface s1/0.34 point-to-point
ip address 1.1.34.4 255.255.255.0
frame-relay interface-dlci 403
```

<br/><br/>

FR)<br/>

```yaml
frame-relay switching

interface s1/1
encapsulation frame-relay
frame-relay intf-type dce
frame-relay route 102 interface serial 1/0 201
no shutdown

interface s1/0
encapsulation frame-relay
frame-relay intf-type dce
frame-relay route 201 interface serial 1/1 102
frame-relay route 203 interface serial 1/2 302
no shutdown

interface s1/2
encapsulation frame-relay
frame-relay intf-type dce
frame-relay route 302 interface serial 1/0 203
frame-relay route 304 interface serial 1/3 403
no shutdown

interface s1/3
encapsulation frame-relay
frame-relay intf-type dce
frame-relay route 403 interface serial 1/2 304
no shutdown
```

<br/><br/>

3.ip route
<br/><br/>


#### 결과<br/>

![Untitled (1)](https://user-images.githubusercontent.com/117553252/229319439-4593f6a8-b2b9-4770-a0a7-3701ad4b26c2.png)
<br/><br/>


### Multipoint Frame-Relay<br/>

- Multipoint의 경우 여러 개가 연결 될 수 있다.<br/>
- Multipoint는 동적 라우팅에 신경을 써 줘야 한다.<br/><br/>

R2)<br/>

```yaml
no interface s1/0.12
no interface s1/0.23
```
<br/><br/>

### R2를 Multipoint로 하기<br/>

![Untitled (2)](https://user-images.githubusercontent.com/117553252/229319441-5c7ac126-300e-40db-b9b1-c1fc72c55fa7.png)
<br/><br/>

#### 방법<br/>

R2)<br/>

```yaml
interface s1/0.123 multipoint
no frame-relay inverse-arp
ip address 1.1.12.2 255.255.255.0
frame-relay map ip 1.1.12.1 201 broadcast
frame-relay map ip 1.1.12.3 203 broadcast
```

<br/><br/>

#### 결과<br/>

R1에서 1.1.12.2랑 1.1.12.3에 ping이 가야 됨.<br/><br/>

![Untitled (3)](https://user-images.githubusercontent.com/117553252/229319442-13982b2b-2817-4486-a604-dd995ac2caf2.png)
<br/><br/>

이거 확인 했으면 PC끼리 통신 되는지 확인.<br/>
(순서 꼭 지킬 것)<br/><br/>

#### 추가<br/>

R2)<br/>

```yaml
no ip route 192.168.30.0 255.255.255.0 1.1.23.3
no ip route 192.168.40.0 255.255.255.0 1.1.23.3
ip route 192.168.30.0 255.255.255.0 1.1.12.3
ip route 192.168.40.0 255.255.255.0 1.1.12.3
```

<br/><br/>

R3)<br/>

```yaml
no ip route 192.168.10.0 255.255.255.0 1.1.23.2
no ip route 192.168.20.0 255.255.255.0 1.1.23.2
ip route 192.168.10.0 255.255.255.0 1.1.12.2
ip route 192.168.20.0 255.255.255.0 1.1.12.2
```

<br/><br/>

#### ip route 후 결과<br/>

![Untitled (4)](https://user-images.githubusercontent.com/117553252/229319443-735f35aa-1116-4b20-af5e-8aeba0f13cb2.png)
<br/><br/>

ping이 다 가는 것을 확인할 수 있다.<br/><br/>


### Point & Multi 혼합<br/>

![Untitled (5)](https://user-images.githubusercontent.com/117553252/229319444-360236ab-053f-4c82-add1-9267d1a53b2f.png)
<br/><br/>

#### 방법<br/>

R1)<br/>

```yaml
interface s1/0
encapsulation frame-relay
no shutdown

interface s1/0.123 point-to-point
ip address 1.1.123.1 255.255.255.0
frame-relay interface-dlci 102
```

<br/><br/>

R3)<br/>

```yaml
interface s1/0
encapsulation frame-relay
no shutdown

interface s1/0.34 point-to-point
ip address 1.1.34.3 255.255.255.0
frame-relay interface-dlci 304
```

<br/><br/>

R4)<br/>

```yaml
interface s1/0
encapsulation frame-relay
no shutdown

interface s1/0.34 point-to-point
ip address 1.1.34.4 255.255.255.0
frame-relay interface-dlci 403
```

<br/><br/>

R2)<br/>

```yaml
interfae s1/0
encapsulation frame-relay
no shutdown

interface s1/0.123 multipoint
no frame-relay inverse-arp
ip address 1.1.123.2 255.255.255.0
frame-relay map ip 1.1.123.3 203 broadcast
frame-relay map ip 1.1.123.1 201 broadcast
```

<br/><br/>


R3)<br/>

```yaml
interface s1/0.123 multipoint
no frame-relay inverse-arp
ip address 1.1.123.3 255.255.255.0
frame-relay map ip 1.1.123.2 302 broadcast
```

<br/><br/>


FR)<br/>

```yaml
frame-relay switching

interface s1/0
encapsulation frame-relay
frame-relay intf-type dce
frame-relay route 102 interface serial 1/1 201
no shutdown

interface s1/1
encapsulation frame-relay
frame-relay intf-type dce
frame-relay route 201 interface serial 1/0 102
frame-relay route 203 interface serial 1/2 302
no shutdown

interface s1/2
encapsulation frame-relay
frame-relay intf-type dce
frame-relay route 302 interface serial 1/1 203
frame-relay route 304 interface serial 1/3 403
no shutdown

interface s1/3
encapsulation frame-relay
frame-relay intf-type dce
frame-relay route 403 interface serial 1/2 304
no shutdown
```

<br/><br/>


### no ip split-horizion<br/>

- 명령어 : `ip split-horizon`<br/><br/>

router에 rip해서 show ip route 해 보기<br/>
Router)<br/>

```yaml
router rip
version 2
network 1.0.0.0
no auto-summary
```

<br/><br/>


![Untitled (6)](https://user-images.githubusercontent.com/117553252/229319445-1bd3ccfc-f0bd-43a4-a733-c4f2e7edb9dd.png)
<br/><br/>

![Untitled (7)](https://user-images.githubusercontent.com/117553252/229319446-0ec06c86-eb76-43d9-bb99-32ac9d5e59d2.png)
<br/>
R3, R4가 안 나오는 것을 확인할 수 있음.<br/><br/>

R2)<br/>

```yaml
interface s1/0.123 multipoint
no ip split-horizon
```

<br/><br/>

![Untitled (8)](https://user-images.githubusercontent.com/117553252/229319447-9f28eebc-f644-4b4e-9d1e-dca8b4e360b2.png)
<br/>
모두 들어온 것을 확인할 수 있다.<br/><br/>

### 부분 메시 구조 = hub and spoke 구조<br/>

![Untitled (9)](https://user-images.githubusercontent.com/117553252/229319448-b0ae5e42-ac83-44af-b3e7-ddb35d501274.png)
<br/><br/>

이런 구조를 `부분메시 구조`라고 한다.<br/>
하나의 인터페이스에 여러 개가 연결되어 있는 것 : 부분 메시 구조, hub and spoke 구조<br/>
그래서 rip으로 할 때는 `in split-horizon`을 비활성화해야 무조건 교환이 된다.<br/><br/>

### Eigrp로 해 보기<br/>

Router)<br/>

```yaml
router eigrp 100
network 1.0.0.0
no auto-summary
```

<br/><br/>


R2)<br/>

```yaml
interface s1/0.123 multipoint
no ip split-horizon eigrp 100
```

<br/><br/>

#### R2 명령어 전<br/>

![Untitled (10)](https://user-images.githubusercontent.com/117553252/229319449-1a787094-63db-47ca-bb85-c82b672cdc0f.png)
<br/><br/>

#### R2 명령어 후<br/>

![Untitled (11)](https://user-images.githubusercontent.com/117553252/229319450-68bd4c98-e9b6-4c62-b7c7-aab33533eea1.png)
<br/>

추가 된 것을 확인할 수 있다.<br/><br/>


-> 전부 지우기 (eigrp 100, split-horizon)<br/><br/>


- DR 선출 = Broadcast, NonBroadcast<br/>
NonBroadcast는 자동으로 neighbor가 안 됨.<br/><br/>

### NonBroadcast Type<br/>

- 네트워크 타입 확인<br/><br/>

R1)<br/>

```yaml
router ospf 1
network 1.1.1.1 0.0.0.0 area 0
network 1.1.123.1 0.0.0.0 area 0
```

<br/><br/>

R2)<br/>

```yaml
router ospf 1
network 1.1.123.2 0.0.0.0 area 0
network 1.1.2.2 0.0.0.0 area 0
```
<br/><br/>

R3)<br/>

```yaml
router ospf 1
network 1.1.123.3 0.0.0.0 area 0
network 1.1.3.3 0.0.0.0 area 0
network 1.1.34.3 0.0.0.0 area 0
```

<br/><br/>

R4)<br/>

```yaml
router ospf 1
network 1.1.34.4 0.0.0.0 area 0
network 1.1.4.4 0.0.0.0 area 0
```

<br/><br/>


R2)<br/>

```yaml
# show ip ospf interface s1/0.123
```

<br/><br/>

#### 결과 - NON_BROADCAST TYPE<br/>

![Untitled (12)](https://user-images.githubusercontent.com/117553252/229319451-74a5bd4f-f558-4b97-a7c9-0f2fa863b0e0.png)
<br/><br/>

![Untitled (13)](https://user-images.githubusercontent.com/117553252/229319452-f6a25951-d1e8-47f6-888b-e8c98149e2b0.png)
<br/>
R1에서 routing table이 제대로 안 들어오는 것을 확인 할 수 있음.<br/><br/>

#### 해결<br/>

- R1에서 네트워크 타입을 논브로드로 바꿔주면 됨.<br/>

R1)<br/>
```yaml
interface s1/0.123 point-to-point
ip ospf network non-broadcast
```

<br/><br/>

그리고 R2가 반드시 DR이 되어야 함.<br/><br/>

R1)<br/>

```yaml
(계속 이어서)
ip ospf priority 0
```

<br/><br/>

R3)<br/>

```yaml
interface s1/0.123 multipoint
ip ospf priority 0
```
<br/><br/>

그리고 나서 이제 수동으로 적용해줘야 함.<br/><br/>

R2)<br/>

```yaml
router ospf 1
neighbor 1.1.123.1
neighbor 1.1.123.3
```
<br/><br/>

#### 결과<br/>

![Untitled (14)](https://user-images.githubusercontent.com/117553252/229319453-4c49d77c-62a8-43de-9240-e058f9fb10ad.png)
<br/><br/>

정상적으로 loopback들이 다 들어온 것을 확인할 수 있음.<br/><br/>


### Broadcast Network Type<br/>

- Broadcast 환경에서 주의<br/><br/>

일단 타입 변경,<br/>

R1)<br/>

```yaml
interface s1/0.123 point-to-point
ip ospf network broadcast
```

<br/><br/>

R2, R3)<br/>

```yaml
interface s1/0.123 multipoint
ip ospf network broadcast

# show ip ospf interface s1/0.123
```

<br/><br/>

![Untitled (15)](https://user-images.githubusercontent.com/117553252/229319454-ab34ec91-b75f-4b0b-9c1a-21223fac5e35.png)
<br/><br/>

![Untitled (16)](https://user-images.githubusercontent.com/117553252/229319455-47c65b90-682c-4d92-adb3-03aa963fa276.png)
<br/><br/>

![Untitled (17)](https://user-images.githubusercontent.com/117553252/229319456-1ab2bc8f-d2d7-4de0-a627-546f10729dbd.png)
<br/>

브로드캐스트로 바뀐 것을 확인할 수 있음.<br/>

브로드 캐스트 환경에서는 neighbor가 자동으로 형성되어 지기 때문에 있을 필요는 없다.<br/><br/>

R2)<br/>

```yaml
router ospf 1
no neighbor 1.1.123.1
no neighbor 1.1.123.3
```

<br/><br/>

### Point-to-Multipoint 환경에서 주의<br/>

- 모양 자체가 point-to-multipoint<br/>
네트워크 타입만 바꿔주면 됨.<br/><br/>

R1)<br/>

```yaml
interface s1/0.123 point-to-ponit
ip ospf network point-to-multipoint
```

<br/><br/>

R2, R3)<br/>

```yaml
interface s1/0.123 multipoint
ip ospf network point-to-multipoint
```

<br/><br/>

![Untitled (18)](https://user-images.githubusercontent.com/117553252/229319457-d8eab022-072c-4af7-836c-c95aaa6b4830.png)
<br/><br/>

![Untitled (19)](https://user-images.githubusercontent.com/117553252/229319458-ef01acac-e261-4c57-917c-48e31dac0176.png)
<br/><br/>

![Untitled (20)](https://user-images.githubusercontent.com/117553252/229319459-9648c3a1-10bd-4de3-a149-1b22a8befa7c.png)
<br/><br/>
