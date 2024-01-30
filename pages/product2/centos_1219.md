---
title: 1219
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_1219.html
folder: product2
---


- 서버 중 제일 많이 쓰는 게 DHCP 서버랑 NAT 서버임. <br/>
서버들은 자신들만의 고유한 기능이 있음.<br/><br/>

- DHCP 서버 : dhcp client 에게 ip 주소 제공<br/>
- DNS 서버 : 도메인 이름을 ip 주소로 변경해 줌<br/>
- NAT 서버 : ip 주소 변환<br/><br/>

- 사설 주소 / 공인 주소 <- 구분할 것!<br/>
- 사설 주소 : 회사 내부에서만 쓰는 것, 무료<br/>
- 공인 주소 : 전 세계에서 유일, 유료<br/><br/>

- NAT 하는 역할 : 보통 사설 주소를 공인 주소로 변환시켜 줌 ★<br/><br/>

- 공인 주소 -> 사설 주소<br/>
- 사설 -> 공인 : 임대료가 많이 듦, 내부에서 외부로 나갈 때 많이 씀.<br/>
- 공인 -> 사설 : 외부에서 내부로 들어올 때 많이 씀.<br/>
외부에서 내부로 나갈 때는 출발지를 바꿔주는 것.<br/><br/>

### 정리<br/>

- DHCP 서버 : IP 주소 제공<br/>
- DNS 서버 : 도메인 이름 -> IP 주소<br/>
- NAT 서버 : IP 주소 변환<br/>
- 사설 주소 : 돈 X<br/>
- 공인 주소 : 돈 O<br/>
- 사설 주소 -> 공인 주소 : 내부 -> 외부<br/>
- 공인 주소 -> 사설 주소 : 외부 -> 내부<br/>
- 사설 주소 -> 사설 주소 -> 사설 주소 -> 공인 주소 -> ......<br/>
- 공인 주소 -> 공인 주소 -> 사설 주소 -> .......<br/><br/>

![1](https://user-images.githubusercontent.com/117553252/215642977-18fc11e0-2bcc-4ee7-87eb-241e289c3269.png)<br/><br/><br/>


### 유형 01. PAT<br/>

![2](https://user-images.githubusercontent.com/117553252/215642984-bb54b568-12b1-4d0b-ac47-53447a0e2f8d.png)<br/>

항상 NAT 서버는 경계에 있음.<br/>
검은색 : 사설<br/>
빨간색 : 공인<br/>
but router는 어느 것이 사설인지 공인인지 모른다. 우리가 그것을 설정해 줘야 하는 것임.<br/><br/><br/>


### 명령어.<br/>

```yaml
interface e0/0
ip nat inside <- 사설 주소
```
<br/>
<br/>

```yaml
interface s1/0
ip nat outside <- 공인 주소
```
<br/><br/>

R1) NAT 서버 구성(나갈 때 공인 주소 하나로 나가기를 원하는 것)<br/>
빌린 공인 주소를 우리는 '1.1.12.10'이라 가정하자.<br/>
```yaml
conf t
ip nat pool cisco 1.1.12.10 1.1.12.10 netmask 255.255.255.0 (1개만 빌려서 시작과 끝이 같은 것임)
access-list 10 permit 192.168.10.0 0.0.0.255 (standard를 사용)
ip nat inside source list 10 pool cisco overload (cisco(=공인주소)랑 10번(=사설주소)은 묶어주기)

interface e0/0
ip nat inside

interface s1/0
ip nat outside
```
<br/><br/>


### 정리 - NAT 서버 구성<br/>

유형 1) PAT(Port Address Translation)<br/>
```yaml
conf t
ip nat pool cisco 1.1.12.10 1.1.12.10 netmask 255.255.255.0
access-list 10 permit 192.168.10.0 0.0.0.255
ip nat inside source list 10 pool cisco overload

interface e0/0
ip nat inside

interface s1/0
ip nat outside
```
<br/><br/>

![3](https://user-images.githubusercontent.com/117553252/215642989-592aab4c-d595-444d-8b80-b81ccd9e6ada.png)
<br/><br/>


#### 결과<br/>
웹을 켜 놓고 이렇게 입력하면 1.1.12.10 이 뜨는 것을 확인할 수 있다.<br/><br/>

![4](https://user-images.githubusercontent.com/117553252/215642992-b2e682fc-aeab-4480-b43c-7c944415c165.png)
<br/><br/>

여러 번 새로 고침하게 되면<br/>
![5](https://user-images.githubusercontent.com/117553252/215642997-d83c9bec-b196-46d0-a747-8d697adea572.png)
<br/><br/><br/>



### #show ip nat translation<br/>

ping을 한 번 해 주고 하면<br/>
![6](https://user-images.githubusercontent.com/117553252/215643000-35551438-8233-4bef-bb11-7d3f826ad368.png)
<br/><br/>
이렇게 정보를 확인할 수 있다.<br/><br/>

10.1에서 웹 서버를 접근했다면 출발지는 자기 자신임.<br/>
목적지 223.~<br/>
포트 번호는 랜덤으로 지정됨.<br/><br/>

`NAT 테이블에 정보가 남는다`는 것이 중요함.<br/><br/>

#### NAT Table<br/>

192.168.10.1:1001 -> 1.1.12.10:1001 -> 223.255.255.1:80<br/>
192.168.10.2:1002 -> 1.1.12.10:1002 -> 223.255.255.1:80<br/><br/>

갔다가 다시 돌아올 때는 NAT 테이블을 참조하여 서버 내부로 돌아오는 것임.<br/>
1.1.12.10으로 같기 때문에 포트 번호 1001, 1002로 pc를 구분한다.<br/>
그 포트 번호는 랜덤으로 지정됨.<br/><br/>

현재는 인터넷 공간에서 회사 내부로 ping(192.168.10.1)을 하면 ping이 가지 않음.<br/>
![7](https://user-images.githubusercontent.com/117553252/215643001-7e17361b-7c02-47e9-9c8a-bf790cc584f8.png)
<br/>
왜? 라우터 자체에 지도가 없으니깐.<br/>
그래서 인터넷 공간에서는 내부로 못 들어감.<br/>
그러나 내부에서 인터넷 공간으로는 넘어갈 수 있음.<br/><br/><br/>


### 유형 02. Static NAT<br/>


![8](https://user-images.githubusercontent.com/117553252/215643004-8d94a4be-5755-43d6-bd25-5d383536750b.png)
<br/><br/>

### 명령어.<br/>

```yaml
conf t
ip nat inside source static 192.168.10.1 1.1.12.20

interface e0/0
ip nat inside

interface s1/0
ip nat outside
```
<br/><br/>

10.1이 나갈 때 1.1.12.20으로 나가라는 의미.<br/>
그리고 #show ip nat translation 으로 확인.<br/>
Static은 1:1개념임.<br/><br/>

`#clear ip nat translation`<br/><br/><br/>


#### 결과.<br/>


![9](https://user-images.githubusercontent.com/117553252/215643009-d1c23278-f86e-4d56-88f9-c8e0e74f7427.png)
<br/><br/>

![10](https://user-images.githubusercontent.com/117553252/215643011-d3f9a181-f664-40eb-a61e-c5019bd7caea.png)
<br/><br/>


![11](https://user-images.githubusercontent.com/117553252/215643012-ec972237-1868-4f57-8f34-fb8c75a6192f.png)
<br/><br/>
![12](https://user-images.githubusercontent.com/117553252/215643015-73a0f483-fe53-4899-8318-8008709b5cbb.png)
<br/><br/><br/><br/>


### Ex.<br/>

![13](https://user-images.githubusercontent.com/117553252/215643019-50b74bc7-55c6-48a2-ba6c-cd56c5d077e6.png)
<br/><br/>

#### 방법.<br/>

S6)<br/>
```yaml
ip nat pool cisco 1.1.12.100 1.1.12.100 netmask 255.255.255.0
access-list 10 permit 192.168.10.0 0.0.0.255
ip nat inside source list 10 pool cisco overload

interface e0/1
ip nat inside

interface s1/0
ip nat outside
```
<br/><br/>

W9)<br/>
```yaml
ip nat inside source static 192.168.10.1 1.1.12.10

interface e0/1
ip nat inside

interface s1/0
ip nat outside
```
<br/><br/>

S5)<br/>
```yaml
ip nat pool cisco2 1.1.12.200 1.1.12.200 netmask 255.255.255.0
access-list 20 permit 192.168.20.0 0.0.0.255
ip nat inside source list 20 pool cisco2 overload

interface e0/0
ip nat inside

interface s1/0
ip nat outside
```
<br/><br/>


W7)<br/>
```yaml
ip nat inside source static 192.168.20.1 1.1.12.20

interface e0/0
ip nat inside

interface s1/0
ip nat outside
```
<br/><br/>


#### 오류 발생.<br/>
W12에 기본 DNS223.255.255.1로 줘야 함.<br/><br/><br/>
