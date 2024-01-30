---
title: 0106
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_0106.html
folder: product2
---


### Ex. Build Up (ft.Frame-Relay)<br/>

![Untitled](https://user-images.githubusercontent.com/117553252/229319578-c7211b6f-e8cc-4c75-9177-190c045c6aa1.png)

<br/><br/>

#### 방법<br/>

1. ip 주소<br/>
2. DNS 서버, DHCP client, Apache Web Server<br/>

![Untitled (1)](https://user-images.githubusercontent.com/117553252/229319559-75a5ff22-548c-4435-95ac-a417678b546d.png)
<br/><br/>

L)<br/>

```yaml
service httpd restart
```

<br/><br/>

R8)<br/>

```yaml
ip dhcp pool cisco 1
network 192.168.10.0 255.255.255.0
default-router 192.168.10.254
dns-server 223.255.255.1
ip dhcp excluded-address 192.168.10.254
```

<br/><br/>

W12, 13)<br/>

```yaml
ipconfig /release
ipconfig /renew
```

<br/><br/>

3. FR구성<br/>

FR1)<br/>

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
no shutdown
```

<br/><br/>

FR2)<br/>

```yaml
frame-relay switching

interface s1/0
encapsulation frame-relay
frame-relay intf-type dce
frame-relay route 506 interface serial 1/1 605
no shutdown

interface s1/1
encapsulation frame-relay
frame-relay intf-type dce
frame-relay route 605 interface serial 1/0 506
no shutdown
```

<br/><br/>

4. 라우터 구성 (ft.FR)<br/>

R1)<br/>

```yaml
encapsulation frame-relay
no shutdown

interface s1/0.123 point-to-point
ip address 1.1.123.1 255.255.255.0
frame-relay interface-dlci 102
```

<br/><br/>

R2)<br/>

```yaml
interface s1/0
encapsulation frame-relay
no shutdown

interface s1/0.123 multipoint
no frame-relay inverse-arp
ip address 1.1.123.2 255.255.255.0
frame-relay map ip 1.1.123.1 201 broadcast
frame-relay map ip 1.1.123.3 203 broadcast
```

<br/><br/>

R3)<br/>

```yaml
interface s1/0
encapsulation frame-relay
no shutdown

interface s1/0.123 multipoint
no frame-relay inverse-arp
ip address 1.1.123.3 255.255.255.0
frame-relay map ip 1.1.123.2 302 broadcast
```

<br/><br/>

R5)<br/>

```yaml
encapsulation frame-relay
no shutdown

interface s1/0.56 point-to-point
ip address 1.1.56.1 255.255.255.0
frame-relay interface-dlci 506
```

<br/><br/>

R7)<br/>

```yaml
interface s1/0
encapsulation frame-relay
no shutdown

interface s1/0.56 point-to-point
ip address 1.1.56.2 255.255.255.0
frame-relay interface-dlci 605
```

<br/><br/>

5. OSPF, EIGRP 구성<br/>

R1)<br/>

```yaml
router ospf 1
network 211.175.185.0 0.0.0.255 area 0
network 1.1.123.0 0.0.0.255 area 0
```

<br/><br/>

R2)<br/>

```yaml
router ospf 1
network 1.0.0.0 0.255.255.255 area 0
```

<br/><br/>

R3)<br/>

```yaml
router ospf 1
network 1.0.0.0 0.255.255.255 area 0
```

<br/><br/>

R5)<br/>

```yaml
router eigrp 100
network 1.0.0.0
no auto-summary

router ospf 1
network 1.0.0.0 0.255.255.255 area 0
```

<br/><br/>

R7)<br/>

```yaml
router eigrp 100
network 1.0.0.0
no auto-summary
```

<br/><br/>

R8)<br/>

```yaml
router eigrp 100
network 1.0.0.0
network 192.168.10.0
no auto-summary

router ospf 1
network 1.0.0.0 0.255.255.255 area 0

router ospf 1
network 1.0.0.0 0.255.255.255 area 0
```

<br/><br/>


6. NAT 서버 구성<br/>

R8)<br/>

```yaml
ip nat pool cisco 77.1.1.1 77.1.1.1 netmask 255.255.255.0
access-list 10 permit 192.168.10.0 0.0.0.255
ip nat inside source list 10 pool cisco overload

interface e0/0
ip nat inside

interface s1/0
ip nat outside
```

<br/><br/>

7. 추가 구성<br/>

R1)<br/>

```yaml
interface s1/0.123 point-to-point
ip ospf network point-to-multipoint
```

<br/><br/>

R2, R3)<br/>

```yaml
interface s1/0.123 multipoint
ip ospf network point-to-multipoint
```

<br/><br/>

8. 재분배<br/>

R5)<br/>

```yaml
router eigrp 100
redistribute ospf 1 metric 1 1 1 1 1

router ospf 1
redistribute eigrp 100 subnets metric 20
```

<br/><br/>

9. Loopback<br/>

R8)<br/>

```yaml
interface loopback 0
ip address 77.1.1.1 255.255.255.0

router eigrp 100
network 77.1.1.0
no auto-summary
```

<br/><br/>

#### 결과 - R1<br/>

![Untitled (2)](https://user-images.githubusercontent.com/117553252/229319560-22a73168-ec45-4915-882b-b361e86692e9.png)
<br/><br/>

77.1.1.1 이 뜨는 것을 확인할 수 있음.<br/><br/>

![Untitled (3)](https://user-images.githubusercontent.com/117553252/229319561-98366f8a-253b-4901-b058-c0863983c12a.png)
<br/><br/>

#### 오류 해결 방법<br/>

R2)<br/>

```yaml
router ospf 1
network 223.255.255.0 0.0.0.255 area 0
```

<br/><br/>

#### 결과<br/>

W12)<br/>
tracert 211.175.185.1 - 총 거치는 것이 7개<br/><br/>

![Untitled (4)](https://user-images.githubusercontent.com/117553252/229319563-ae42900d-1808-456e-a2fb-c81a879e69eb.png)
<br/><br/>

![Untitled (5)](https://user-images.githubusercontent.com/117553252/229319564-d9089785-7b5c-45ec-b493-4e9bbfd5f3a4.png)
<br/><br/>

### Routing Filtering<br/>

- # show ip route : 지도를 보여줌<br/><br/>

- Filtering : 원하는 것만 넘겨주려고 함.<br/>
- 라우팅 테이블 교환할 때 필터링을 하고 싶다는 것임.<br/><br/>

- access-list : 조건을 주고 원하는 인터페이스에 적용을 함. 그러면 방화벽이 됨. 즉, 조건 -> 인터페이스에 적용<br/><br/>

- Prefix-list 명령어는 access-list랑 비슷하다.<br/>
- ge 24 <= x<br/><br/>

- Prefix-list : Only 조건<br/>
- Route-map : 조건 + 수정<br/><br/>

### Ex. 조건 : /32 -> /24<br/>

![Untitled (6)](https://user-images.githubusercontent.com/117553252/229319565-dcf54a50-1977-4a66-9429-a0587c85b613.png)
<br/><br/>

- int lo 0 -> `ip ospf network point-to-point`<br/>
- loopback 주소만 그런 것임. 주의하자.<br/><br/>

#### 결과 (기본 구성)<br/>

각각 via 3개가 나온 것을 확인할 수 있음.<br/><br/>

#### 설명<br/>

- RIP, EIGRP 등과 달리 OSPF 에서는 라우팅 정보를 인터페이스에서 송신할 때에는 차단할 수 있다.<br/><br/>

#### 방법 Ex. 01<br/>

R1) -> 2점대만 받게 하기 위해선.<br/>

```yaml
ip prefix-list D0 deny 3.3.3.0/24
ip prefix-list D0 deny 4.4.4.0/24
ip prefix-list D0 permit 0.0.0.0/0 le 32

router ospf 1
distribute-list prefix D0 in s1/0
```

<br/><br/>

#### 결과<br/>

![7](https://user-images.githubusercontent.com/117553252/229319881-666181bd-3822-42ed-8f57-fa3bb9303941.png)
<br/>

3점대랑 4점대는 1.1.12.2가 막혔음을 확인할 수 있다.<br/><br/>

#### 추가 명령어<br/>

R1)<br/>

```yaml
ip prefix-list D1 deny 2.2.2.0/24
ip prefix-list D1 deny 4.4.4.0/24
ip prefix-list D1 permit 0.0.0.0/0 le 32

router ospf 1
distribute-list prefix D1 in s1/1

ip prefix-list D2 deny 2.2.2.0/24
ip prefix-list D2 deny 3.3.3.0/24
ip prefix-list D2 permit 0.0.0.0/0 le 32

router ospf 1
distribute-list prefix D2 in s1/2
```

<br/><br/>

#### 총 결과<br/>

![8](https://user-images.githubusercontent.com/117553252/229319883-1d0ef4c4-a669-43a5-9695-88220896a6e1.png)
<br/>

각각 1개씩 뜬 것을 확인 할 수 있다.<br/><br/>

### Access-list<br/>

R1)<br/>

```yaml
access-list 10 deny 3.3.3.0 0.0.0.255
access-list 10 deny 4.4.4.0 0.0.0.255
access-list 10 permit 0.0.0.0 255.255.255.255

access-list 20 deny 2.2.2.0 0.0.0.255
access-list 20 deny 4.4.4.0 0.0.0.255
access-list 20 permit 0.0.0.0 255.255.255.255

access-list 30 deny 2.2.2.0 0.0.0.255
access-list 30 deny 3.3.3.0 0.0.0.255
access-list 30 permit 0.0.0.0 255.255.255.255

router ospf 1
distribute-list 10 in s1/0
distribute-list 20 in s1/1
distribute-list 30 in s1/2
```

<br/><br/>


#### 결과<br/>

![9](https://user-images.githubusercontent.com/117553252/229319884-bc37aa75-7015-477d-8d4b-3a178242eaa6.png)
<br/>

pre랑 했을 때랑 똑같은 것을 확인할 수 있음.<br/><br/>

- 필터링 명령어 : distribute ~<br/><br/>


### Named ACL<br/>

R1)<br/>

```yaml
ip access-list standard cisco1
deny 3.3.3.0 0.0.0.255
deny 4.4.4.0 0.0.0.255
permit any

ip access-list standard cisco2
deny 2.2.2.0 0.0.0.255
deny 4.4.4.0 0.0.0.255
permit any

ip access-list standard cisco3
deny 2.2.2.0 0.0.0.255
deny 3.3.3.0 0.0.0.255
permit any

router ospf 1
distribute-list cisco1 in s1/0
distribute-list cisco2 in s1/1
distribute-list cisco3 in s1/2
```

<br/><br/>


#### 결과<br/>

![10](https://user-images.githubusercontent.com/117553252/229319885-73a778df-b9cb-41fc-8626-4512e1eabbf0.png)
<br/><br/>

### route-map<br/>

R1)<br/>

```yaml
ip prefix-list D0 deny 3.3.3.0/24
ip prefix-list D0 deny 4.4.4.0/24
ip prefix-list D0 permit 0.0.0.0/0 le 32

ip prefix-list D1 deny 2.2.2.0/24
ip prefix-list D1 deny 4.4.4.0/24
ip prefix-list D1 permit 0.0.0.0/0 le 32

ip prefix-list D2 deny 2.2.2.0/24
ip prefix-list D2 deny 3.3.3.0/24
ip prefix-list D2 permit 0.0.0.0/0 le 32

router ospf 1
distribute-list route-map D0 in
distribute-list route-map D1 in
distribute-list route-map D2 in → 이것만 남음. 아마 덮어씌어진 것을 추정.
```

<br/><br/>


#### 결과 - R1<br/>

![11](https://user-images.githubusercontent.com/117553252/229319886-9e017717-c9d8-4c05-8aa4-a9838ee22cf9.png)
<br/><br/>

### route-map<br/>

- 요구 조건 : 2.2.2.2 3.3.3.3만 허용<br/><br/>

- 1) Prefix-list<br/>
R1)<br/>

```yaml
ip prefix-list D3 permit 2.2.2.0/24
ip prefix-list D3 perimt 3.3.3.0/24

route-map cisco1
match ip address prefix-list D3

router ospf 1
distribute-list route-map cisco1 in
```

<br/><br/>

- 결과)<br/>

![12](https://user-images.githubusercontent.com/117553252/229319887-cc385f08-fb9e-4e8f-85e5-248719a7bca3.png)
<br/><br/>

- 2) Numbered ACL<br/>

R1)<br/>

```yaml
access-list 10 permit 2.2.2.0 0.0.0.255
access-list 10 permit 3.3.3.0 0.0.0.255

route-map cisco2 permit
match ip address 10

router ospf 1
distribute-list route-map cisco2 in
```

<br/><br/>


- 3) Named ACL<br/>

R1)<br/>

```yaml
ip access-list standard acl1
permit 2.2.2.0 0.0.0.255
permit 3.3.3.0 0.0.0.255

route-map cisco3
match ip address acl1

router ospf 1
distribute-list route-map cisco3 in
```

<br/><br/>

- 결과)<br/>


![13](https://user-images.githubusercontent.com/117553252/229319888-88db507e-8afa-4692-b5b6-165e0dc96afa.png)
<br/><br/>
