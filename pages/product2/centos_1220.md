---
title: 1220
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_1220.html
folder: product2
---


### ipconfig /flushdns <br/>
윈도우에서 Cache 비울 때 쓰는 명령어.<br/><br/>

![1](https://user-images.githubusercontent.com/117553252/215650793-15c34d5e-2211-4210-8ddd-818b0dae0c28.png)
<br/><br/>

### netstat -apn tcp <br/>
외부 주소 확인<br/><br/>

![2](https://user-images.githubusercontent.com/117553252/215650800-7a4e2a71-64f3-4c63-8e69-7df1191e148b.png)
<br/><br/><br/>


### Ex. <br/>
 
![3](https://user-images.githubusercontent.com/117553252/215650802-948c7f47-d5d3-42d0-b64e-f54da802d41b.png)
<br/><br/>
사설 주소는 ip route 안 잡음. 주소 제대로 잡기<br/>
R1) ip route 4개<br/>
R2) ip route 2개 <br/>
R3) ip route 3개<br/>
R4) ip route 3개<br/><br/>

#### 방법.<br/>

inside에서 outside로 나갈 때 항상 변환이 되어 짐.<br/><br/>

W7, W8 - DHCP Client<br/>
```yaml
ip dhcp pool cisco1
network 192.168.10.0 255.255.255.0
default-router 192.168.10.254
ip dhcp excluded-address 192.168.10.254
```
<br/><br/>

W9, W10 - DHCP Client<br/>
```yaml
ip dhcp pool cisco2
network 192.168.30.0 255.255.255.0
default-router 192.168.30.254
ip dhcp excluded-address 192.168.30.254
```
<br/><br/>

R1 - PAT용 공인주소<br/>
```yaml
ip nat pool cisco1 1.1.12.10 1.1.12.10 netmask 255.255.255.0
access-list 10 permit 192.168.10.0 0.0.0.255
ip nat inside source list 10 pool cisco1 overload

interface e0/0
ip nat inside

interface s1/0
ip nat outside
```
<br/><br/>

R2 - PAT용 공인주소<br/>
```yaml
ip nat pool cisco2 1.1.34.10 1.1.34.10 netmask 255.255.255.0
access-list 10 permit 192.168.30.0 0.0.0.255
ip nat inside source list 10 pool cisco2 overload

interface s1/1
ip nat inside

interface e0/0
ip nat inside

interface s1/0
ip nat outside
```
<br/><br/>

#### 결과.<br/>

![4](https://user-images.githubusercontent.com/117553252/215650806-7144b1bf-130f-45de-967d-5ef6d8f01100.png)
<br/><br/>
![5](https://user-images.githubusercontent.com/117553252/215650807-ce0d7e28-7c30-4d0c-9533-d2c76f09e482.png)
<br/><br/>
![6](https://user-images.githubusercontent.com/117553252/215650810-daab06aa-ab40-40e1-a630-e6237e7bb8ba.png)
<br/><br/>

1.1.12.10으로 오는 것을 확인할 수 있음.<br/><br/>

![7](https://user-images.githubusercontent.com/117553252/215650814-b27222d8-3b7d-4c14-ac6e-2aaf829f31e4.png)
<br/><br/>
![8](https://user-images.githubusercontent.com/117553252/215650817-90e4382f-d9eb-455e-bb30-3d668ad54f2f.png)
<br/><br/>
![9](https://user-images.githubusercontent.com/117553252/215650820-50dcfe45-3d47-46ac-8962-bf5f54d8f019.png)
<br/><br/>
1.1.34.10이 나온 것을 확인할 수 있다.<br/>
주소가 한 번 더 변환되는 것이기 때문에 1.1.12.10이 나오면 틀린 것임.<br/><br/><br/>


#### 오류 발생<br/>

![10](https://user-images.githubusercontent.com/117553252/215650821-9c34a5fd-f88a-48f6-973d-bf408db7d22c.png)
<br/><br/>
R3에 access-list 10 permit host 1.1.12.10을 추가해야 함.<br/><br/><br/><br/>



### 가상 인터페이스 (Loopback)<br/>

- PAT 주솔르 1.1.12.10 -> 10.1.1.1 로 바꾸고 싶음.<br/>
그럼 나갈 때 10.1.1.1 임.<br/>
그런데 라우팅 테이블에 10점대가 없기 때문에 바로 error가 남.<br/>
근데 그림에 10점대는 없기 때문에 이럴 때 쓰는 것이 `가상 인터페이스`임.<br/><br/><br/>


### 명령어<br/>

```yaml
#conf t
interface loopback ?
```
<br/><br/>

![11](https://user-images.githubusercontent.com/117553252/215650824-317b2f7b-c11d-4ca5-bcdf-f171f7b61104.png)
<br/><br/>

```yaml
interface loopback 0 <- 자동생성<br/>
do show ip interface loopback <br/>
no interface loopback 0 <- 삭제<br/><br/>
```
<br/><br/>

```yaml
interface loopback 0
ip address 10.1.1.1 255.255.255.0
no shutdown 안 해도 됨.(가상의 인터페이스이기 때문에 자동으로 문이 열림.)
```
<br/><br/><br/>

![12](https://user-images.githubusercontent.com/117553252/215650826-f32dcfd8-a6b6-4292-8018-6b87daa9b8e0.png)
<br/><br/>

그럼 interface 0에 10.1.1.1/24가 생기는 것임!<br/>
즉, R1은 interface를 3개 가지고 있게 된다.<br/>
그리고 R1에서 ACL을 수정해주면 된다. NAT를 삭제(CLEAR)<br/>

```yaml
#clear ip nat translation *
no ip nat inside source list 10 pool cisco1 overload
no ip nat pool cisco1 1.1.34.10 1.1.34.10 netmask 255.255.255.0
no access-list 10 permit 1.1.12.10 0.0.0.0
no access-list 10 permit 192.168.30.0 0.0.0.255
```
<br/><br/><br/><br/>


### Ex. 01<br/>

![13](https://user-images.githubusercontent.com/117553252/215650828-687c497c-7469-41e4-934f-ea495a39da4c.png)
<br/><br/>

### 방법<br/>

```yaml
ip nat pool cisco1 10.1.1.1 10.1.1.1 netmask 255.255.255.0
access-list 10 permit 192.168.10.0 0.0.0.255
ip nat inside source list 10 pool cisco1 overload

interface s1/0
ip nat outside

interface e0/0
ip nat inside

interface loopback 0
ip nat inside
```
<br/><br/>

중요) R2에 ip route 10.1.1.0 255.255.255.0 1.1.12.1을 추가해 줘야함.<br/>
길이 하나 더 생긴 것이기 때문임.<br/>
만들어 주지 않으면 갔다가 다시 돌아오지 않는다.<br/><br/>

#### 결과<br/>


![14](https://user-images.githubusercontent.com/117553252/215650829-1e4d408b-b7b6-4fc7-9d4c-e556a3dcc73b.png)
<br/><br/>

![15](https://user-images.githubusercontent.com/117553252/215650831-d63f315c-818f-4601-bfe8-7dea37f5ed0a.png)
<br/><br/>

10.1.1.1로 뜨는 것을 확인할 수 있음.
<br/><br/><br/><br/>


### Ex. 02<br/>

![16](https://user-images.githubusercontent.com/117553252/215651450-6ee036f8-0b91-4758-b971-07076461bf77.png)
<br/><br/>
R1) loopback 3개 - 10점대, 20점대, 30점대<br/>
전송 다 되는지 확인하고 ACL을 마지막에 주기.<br/><br/><br/>

#### 방법.<br/>

1. 기본 ip 주소 static으로 주기<br/>
2. DNS, HTTP, DHCP Client(예약) 하기<br/>
    R1)<br/>
    ```yaml
    ip dhcp pool cisco1
    network 192.168.0.0 255.255.255.0
    default-router 192.168.10.254
    dns-server 223.255.255.1

    ip dhcp excluded-address 192.168.10.1 192.168.10.9
    ip dhcp excluded-address 192.168.10.21 192.168.10.254
    ```
    <br/>
3. Loopback 해주기 + ip route 추가<br/>
    R1)<br/>
    ```yaml
    interface loopback 0
    ip address 10.1.1.1 255.255.255.0

    interface loopback 1
    ip address 20.1.1.1 255.255.255.0

    interface loopback 2
    ip address 30.1.1.1 255.255.255.0

    + ip route 를 R2, R3, R4에 추가해주기
    ```
    <br/>
4. Static NAT / PAT / NAT 서버<br/>
R1)<br/>
    W6)<br/>

    ```yaml
    ip nat inside source static 192.168.10.1 10.1.1.1
    
    interface e0/0
    ip nat inside

    interface loopback 0
    ip nat inside

    interface s1/0
    ip nat outside
    ```
<br/>
<br/>

    W7)<br/>

    ```yaml
    ip nat pool cisco1 30.1.1.1 30.1.1.1 netmask 255.255.255.0
    access-list 10 permit 192.168.0.0 0.0.255.255
    ip nat inside source list 10 pool cisco1 overload

    interface e0/0
    ip nat inside

    interface loopback 2
    ip nat inside

    interface s1/0
    ip nat outside
    ```
<br/><br/>


    W8)<br/>

    ```yaml
    ip nat inside source static 192.168.10.10 20.1.1.1

    interface e0/0
    ip nat inside

    interface loopback 1
    ip nat inside

    interface s1/0
    ip nat outside
    ```
    <br/><br/>


5. ACL<br/>
R3)<br/>
```yaml
access-list 100 permit udp 192.168.10.0 0.0.0.255 host 223.255.255.1 eq 53
access-list 100 permit udp 128.255.0.0 0.0.255.255 host 223.255.255.1 eq 53

interface e0/0
ip access-group 100 out
```
<br/><br/>

R4)<br/>
```yaml
access-list 100 permit tcp 192.168.10.0 0.0.0.255 host 125.255.255.1 eq 80
access-list 100 permit tcp 128.255.0.0 0.0.255.255 host 125.255.255.1 eq 80

interface e0/0
ip access-group 100 out
```
<br/><br/><br/>

#### 결과 - 01 <br/>

![17](https://user-images.githubusercontent.com/117553252/215651460-66762025-6763-4ed4-8f41-56f31b23b0c7.png)
<br/>
<br/>
<br/>

![18](https://user-images.githubusercontent.com/117553252/215651462-ac6e340b-1ea4-4bdd-ba4c-f4d59147a48f.png)
<br/>
<br/>


#### 오류 발생<br/>

![19](https://user-images.githubusercontent.com/117553252/215651465-ad6e2200-36b7-4405-951e-5507aa6e602c.png)
<br/><br/>
해결) S8은 static이 아니라 자동으로 DNS를 받아 와야하고<br/>
W11은 새 호스트 추가할 때 www2는 192.168.10.1이 아니라 10.1.1.1로 줘야함.<br/><br/><br/>


#### 오류 발생 02 - ACL<br/>

R3)<br/>
```yaml
access-list 100 permit udp host 10.1.1.1 host 223.255.255.1 eq 53
access-list 100 permit udp host 20.1.1.1 host 223.255.255.1 eq 53
access-list 100 permit udp host 30.1.1.1 host 223.255.255.1 eq 53
access-list 100 permit udp 128.255.0.0 0.0.255.255. host 223.255.255.1 eq 53

interface e0/0
ip access-group 100 out
```
<br/><br/><br/>

R4)<br/>
```yaml
access-list 100 permit tcp host 10.1.1.1 host 125.255.255.1 eq 80
access-list 100 permit tcp host 20.1.1.1 host 125.255.255.1 eq 80
access-list 100 permit tcp host 30.1.1.1 host 125.255.255.1 eq 80
access-list 100 permit tcp 128.255.0.0 0.0.255.255 host 125.255.255.1 eq 80

interface e0/0
ip access-group 100 out
```
<br/><br/><br/>

#### 결과 <br/>
![20](https://user-images.githubusercontent.com/117553252/215651447-f0d07a78-0b8d-45d3-a1a6-1ca46924c310.png)









