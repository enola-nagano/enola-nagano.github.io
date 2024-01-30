---
title: 1215
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_1215.html
folder: product2
---


1번 DHCP Discover <br/>
2번 DHCP Offer <br/>
3번 DHCP Request <br/>
4번 DHCP ACK <br/><br/>


![1](https://user-images.githubusercontent.com/117553252/213615765-638af311-2707-43b0-bc2d-b184bf4222af.png)
<br/><br/>
클라이언트와 서버의 4단계.
<br/><br/>
![2](https://user-images.githubusercontent.com/117553252/213615769-fbd15108-58c5-420a-8106-d7ec3824a239.png)
<br/><br/>
주의) 어느 과정이 Unicast로 이루어지는지. <br/>
어느 과정이 Broadcast로 이루어지는지 주의하기.<br/>
Unicast (1:1)로 이루어짐. <br/><br/>

Ex. PC가 각 한 대씩 있다고 가정해보자.<br/>
PC간 통신이 되려면 각각의 ip주소가 필요하다.<br/>
각각의 ip가 1.1.1.1 / 1.1.1.2 / 1.1.1.3 이라고 가정하자.<br/><br/>


### Port <br/>

- 방(room)의 개념. <br/>
한 쪽은 TCP, 나머지 한 쪽은 UDP 방으로 이루어져있음. (유형이 그냥 그렇다는 말임)<br/>
TCP, UDP 방은 각각 수도 없이 많다.<br/>
왼쪽으로 TCP, 오른쪽으로 UDP 방으로 생각해보자. <br/><br/>

- 방에는 방 번호가 존재함.<br/>
TCP, UDP 방 번호를 Port 번호라고 함.<br/>
80번의 방이 있다고 가정했을 때, 이 방이 존재하려면 웹 서버를 깔아야 한다.<br/>
DNS 서버를 깔면 dns 방에서 대기하고 있음.<br/><br/>

- 클라이언트가 http로 1.1.1.1에서 1.1.1.2로 접근하려고 한다.<br/>
항상 ip주소를 찾아서 가야한다. 뭐든.<br/>
http로 시작하면 방 번호는 기본적으로 80번으로 시작한다.<br/>
이건 기본이기 때문에 눈에 보이지 않는다.<br/>
이렇게 접근을 하는데 포트 번호가 1.1.1.1에서 랜덤으로 배정됨.<br/>
1.1.1.2로 접근하고 싶은데 랜덤으로 포트 번호가 지정된다.<br/>
80번과 100번 방이 접근이 되어진다.<br/>
연결은 포트(방)으로 함. 그렇게 왔다 갔다 통신이 되어지는 것임.<br/>
http://1.1.1.2:80 으로 하면 200번 포트랑 연결.<br/><br/>

- 연결할 때, 포트 단위로 연결되어 진다는 것.<br/>
항상 IP 주소랑 포트 번호랑 같이 간다는 것.<br/>
전송 프로토콜이기 때문에 TCP, UDP.<br/>
TCP, UDP 에는 포트 번호가 있다는 것을 기억하자.<br/><br/>

- 서버는 기본적으로 UDP 포트번호를 사용함. 이 경우 67을 사용함.<br/>
클라이언트는 UDP 포트 번호 68번을 기본적으로 사용함.<br/>
대화할 때는 기본적으로 포트번호를 사용함.<br/><br/><br/>


![3](https://user-images.githubusercontent.com/117553252/213615772-b1949e02-8f32-47f6-8a33-55538d70f0ee.png)
<br/><br/>

1. 주소를 요청할 때, ip 주소는 아직 없기 때문에 source 포트는 68번 포트, 목적지는 67번 포트.<br/>
클라이언트에는 68번의 포트 번호가 대기하고 있다는 것.<br/>
2. 67에서 받아서 출발지로 offer<br/>
보낼 때는 67번, 목적지는 68번.<br/>
3. request 출발지 68, 목적지 67<br/>
4. ack 출발지 67, 목적지 68<br/><br/>

AGET의 경우,<br/>
68 -> 67 (여기에는 에이전트가 존재) -> 1:1로 보냄(추발지 67, 목적지 67) -> 반대로 1:1 (출발지 67, 목적지 67) -> <br/><br/>

즉, 서버들은 포트가 정해져 있다는 것.<br/>
UDP 방에 68번 방에 가면 DHCP client가 대기하고 있다는 것.<br/>
요청이 오면 바로 응답함.<br/><br/>

![4](https://user-images.githubusercontent.com/117553252/213615773-0c519688-a598-4aa1-8274-72469952c6bd.png)
<br/><br/><br/>



### ARP Protocol / RARP <br/>

- 마찬가지고 pc가 두 대 있다고 가정하자.<br/>
PC간 케이블이 연결되어 있고 랜카드가 연결되어 있다.<br/>
주소줄 때 랜카드에 준 것임.(PC든 Router든)<br/><br/>

- 컴퓨터 간에 통신을 하기 위해서는 ip주소가 필요함.<br/>
1.1.1.2 / 1.1.1.3 으로 가정해보자.<br/>
주소 간 통신하기 위해서는 ip 주소는 건물의 3층에 있다.<br/>
2층에는 MAC 주소가 있다.<br/>
주소가 2개임 (IP / MAC)<br/>
MAC 주소 = 랜 카드 칩에 들어가 있는 장비 식별을 위한 전 세계에서 유일한 주소.<br/>
항상 기본적으로 이렇게 구성되어 있음.<br/>
MAC 주소는 우리가 따로 주는 것이 아닌 제품을 만들 때 들어가 있는 것이고, ip 주소는 우리가 주는 것임.<br/><br/>

- 1.1.1.2랑 1.1.1.3이 통신하기 위해서는 제일 먼저 1.1.1.3의 2층 MAC 주소를 알아야 함.<br/>
왜냐하면 그래야 3층으로 올라갈 수 있기 때문임.<br/>
알아내는 것을 `ARP과정`이라고 함.<br/>
목적지 ip주소에 대한 MAC 주소를 알아내는 것 = ARP 프로토콜 (즉, ip 주소를 MAC 주소로 맵핑 시켜주는 프로토콜) <br/><br/>

- ARP 과정은 자동으로 이루어짐.<br/>
그래서 컴마다 명령프롬토콤에서 `#art -a`하면 볼 수 있음.<br/>
ex.<br/>
#art -a<br/>
1.1.1.3 MAC3 (주소가 들어가 있음)<br/>
arp가 자동으로 알아옴.<br/><br/>

- 알아두기.<br/>
ARP : IP -> MAC<br/>
RARP : MAC -> IP<br/><br/>
![5](https://user-images.githubusercontent.com/117553252/213615775-0f8cc5a1-31fa-40af-9767-a7f3b103cfaf.png)
<br/><br/>

- ping 1.1.1.3을 하면<br/>
arp 브로드캐스트를 1.1.1.3에 던짐.<br/>
던질 때, 출발지 : 1.1.1.2/MAC2, 목적지 : 1.1.1.3/FFFF.FFFF.FFFF(모르니까 브로드캐스트를 던짐, 즉 모두 받아라)<br/>
응답을 할 때는 MAC2를 알고 있으니까 그대로 보냄.<br/>
그럼 자동 등록 되어짐.<br/><br/>

이 상태에서 다시 ping을 해 보자.<br/>
그럼 출발지 : 1.1.1.2/MAC2, 목적지 : 1.1.1.3/MAC3이고 나머지는 같음.<br/><br/>

이게 ARP의 기본과정.<br/>
arp는 요청과 응답 2단계로 이루어져 있음.
<br/><br/><br/>



### ARP 2단계 정리 <br/>

```yaml
>ping 1.1.1.3 (첫 번째 ping)
s : 1.1.1.2/MAC2
D : 1.1.1.3/FFFF.FFFF.FFFF
>ping 1.1.1.3 (두 번째 ping)
s : 1.1.1.2/MAC2
D : 1.1.1.3/MAC3
```
<br/><br/>

![6](https://user-images.githubusercontent.com/117553252/213615776-1399ce15-2c81-4c2d-a07c-217b78def010.png)
<br/><br/>
여기서 틀린 것을 찾아보자.<br/>
Client : 스텝 01, 03<br/>
Server : 스텝 02, 04<br/><br/>

정답 : 스텝 02의 목적지가 MAC1로 바뀌어야 함.<br/>
이유 : 주소는 모르지만 arp의 단계를 자동으로 거쳤으니깐.<br/>
그리고 스텝 03의 목적지가 MAC2로 바뀌어야 함. (아직 ip주소는 모름)<br/>
또한 스텝 04의 목적지가 MAC1로 바뀌어야 함.<br/><br/>

스텝 02에서 제안하는 주소가 스텝 02에서 들어가 있음.<br/>
이것을 클라이언트 주소인 것처럼 목적지로 셋팅함.<br/>
따라서 어제할 때 Wireshark에서 주소가 나온 것임(서버는)<br/><br/>

근데 클라이언트는 전부 아직 주소가 없기 때문에 255.255.255.255임<br/><br/>

나중에 wireshark로 확인을 해 보자.<br/>
![7](https://user-images.githubusercontent.com/117553252/213615777-8e65428f-b6f1-4d50-9376-d9ca18cc49d3.png)
<br/><br/>

실제로 바뀌는 것이 아니라 wireshark에서만 바뀐다는 것.<br/><br/><br/><br/>



### Wireshark <br/>

![8](https://user-images.githubusercontent.com/117553252/213615780-5493af1a-f645-43e9-9ec2-c18cc6297c1e.png)
<br/><br/>

```yaml
No : 숫자
Time : 시간
Source : 출발지
Destination : 목적지
Protocol : 어떤 프로토콜이 지나가는지
Length : 크기
Info : 정보
Transactiong : 작업 단위
ex. 홍길동 : A은행 -10만원 -> B은행 +10만원
```
<br/><br/>

4단계를 확인할 수 있음.<br/>
트랜잭션 아이디가 또 다름.(renew를 했기 때문)<br/>
Discover부터 ACK까지가 하나의 작업 단위임.<br/><br/><br/>


![9](https://user-images.githubusercontent.com/117553252/213615783-db5622c8-8cbb-4cab-8b5f-329933bf63e5.png)
<br/><br/>
하나의 작업 단위만 보자 <br/>
클라이언트에서 보내는 것은 전부 브로드캐스트임<br/><br/>

![10](https://user-images.githubusercontent.com/117553252/213615784-39aa39f3-22da-4d0d-9dbd-cd0ccb24150c.png)
<br/><br/>
2계층 주소, MAC 주소를 의미함.<br/><br/>

![11](https://user-images.githubusercontent.com/117553252/213615787-48f17320-2672-4290-906c-1ece238d3a8d.png)
<br/><br/>


![12](https://user-images.githubusercontent.com/117553252/213615789-37edbae4-fd09-4548-bb72-3929dd83801e.png)
<br/><br/>
클라이언트는 지금 주소가 없는 상태임.<br/>
그냥 제안한 주소임.<br/><br/>

![13](https://user-images.githubusercontent.com/117553252/213615791-6a8ca41b-91fd-4779-b05b-83bebcd454ec.png)
<br/><br/>
우리가 DHCP 구성했던 DNS, GATEWAY, SUBNET MAKS, IP 주소 가 모두 들어가 있는 것을 확인할 수 있다.<br/><br/>


이걸 보고 대략 무슨 말인지 이해하면 됨.<br/>
![14](https://user-images.githubusercontent.com/117553252/213615793-1cb3bec2-b3d4-4dda-a6b2-cfe0bb2467bd.png)
<br/><br/><br/>


4단계를 지나면 이제 갱신하는 것임.<br/>
이 것이 request임. 바로 request를 함.<br/>
그 request를 보면 클라이언트 아이피 주소가 제대로 정확하게 주어진 것을 확인할 수 있음.<br/>
![15](https://user-images.githubusercontent.com/117553252/213615796-7c7e7ad3-7765-4df2-851c-309cb8719db4.png)
<br/><br/><br/><br/>



### WireShark 저장하는 방법 <br/>

![16](https://user-images.githubusercontent.com/117553252/213615798-8f5b19f1-5742-4e79-b269-0e65d55ee78a.png)
<br/><br/>
빨간 네모를 누른다<br/>
그리고 Save As.













### ACL <br/>

ACL (Router 장비)<br/>
라우터가 받으면 무조건 in, 나갈때는 out.<br/><br/>

- 하나의 인터페이스에서 나갈 때 ACL을, 들어올 때 ACL을 걸러낼 수 있음.<br/>
라우터를 꼭 방화벽처럼 쓰고 있는 것.<br/>
즉, 외부에서 학원에 특정 네트워크가 들어올 수 있게/없게 설정할 수 있다는 것.<br/><br/>

- 데이터가 들어올 때/나갈 때 자동으로 목록(List)을 체크함. (IN / OUT)<br/>
이 리스트는 우리가 설정하는 것임.<br/><br/>

- 방화벽 개념은 다 비슷함.<br/>
말 그대로 필터링 한다는 의미<br/><br/>

- ACL을 라우터 인터페이스에 설정을 함. 들어올 때/나갈 때 ACL을 걸 수 있다.<br/>
ACL은 문형이 2개임 <br/>
숫자에 따른 분류에서 숫자를 조심하자.<br/>
숫자를 이름형식으로도 할 수 있다.<br/><br/><br/>


### 명령어 <br/>

`Standard ACL : 1-99, Only 출발지 IP (혹은 네트워크 주소)`<br/><br/>

```yaml
conf t
access-list [1-99] [permit/deny] [출발지IP/네트워크주소] [Wildcard Mask] -> 조건 만들기
interface s1/0 -> 만들고 나서 적용하는 과정
ip access-group [1-99] [in/out]
```
<br/><br/>


- Subnet Mask가 하는 역할? <br/>
-> IP 주소가 있으면 어디까지가 네트워크 주소이고, 호스트 주소인지를 구분해주는 것.<br/>
IP 주소 = Network 주소 자리(비트 1로 표기) + Host 주소 자리(0)<br/>
: Network (1) + Host (0)<br/>
ex.<br/>
192.168.10.1 255.255.255.0 -> 192.168.10 + 1 <br/>
192.168.10.1 255.255.0.0 -> 192.168 + 10.1 <br/>
192.168.10.1 255.0.0.0 -> 192 + 168.10.1 <br/>
192.168.10.1 0.255.255.255 (이런 서브넷 마스크는 존재 X)<br/>
192.168.10.1 255.255.0.255 (존재 X)<br/><br/>


```yaml
Wildcard Mask : 조건을 주기 위해서 사용하는 것.
```
<br/>
비트로 했을 때, 1은 조건 검사를 하지 않겠다는 것.<br/>
비트로 했을 때, 0은 조건 검사를 하겠다는 것.<br/>
ex.<br/>
192.168.10.1 0.0.0.255 -> 192.168.10.X (0에서 255까지 모두 포함)<br/>
192.168.10.1 0.0.255.255 -> 192.168.X.X (3,4번째 자리는 뭐가 오든 관계X)<br/>
192.168.10.1 0.255.255.255 -> 192.X.X.X<br/>
192.168.10.1 0.255.0.255 -> 192.X.10.X<br/>
192.168.10.1 0.0.0.0 -> 192.169.10.1<br/><br/>

와일드 카드가 0.0.0.0인 경우 정확하게 ip 주소가 옴.(즉, 특정ip 주소를 의미)<br/>
이걸 우리는 의미하는 키워드가 있는데 host 192.168.10.1 -> 192.168.10.1 이라고 나타내기도 한다.<br/><br/>

192.168.10.1 255.255.255.255 -> X.X.X.X (그럼 앞에 오는 것이 의미가 없음)<br/>
이거랑 같은 건 0.0.0.0 255.255.255.255 -> X.X.X.X (보통 모든 주소를 의미할 때는 ip 주소가 아니라 0.0.0.0을 줌)<br/>
이걸 간단한 키워드로 나타내기도 하는데 any -> X.X.X.X<br/><br/><br/>


### 정리 <br/>

`특정 주소 표현` <br/>
192.168.10.1 0.0.0.0<br/>
host 192.168.10.1<br/><br/>

`모든 주소 표현` <br/>
192.168.10.1 255.255.255.255<br/>
0.0.0.0 255.255.255.255<br/>
any<br/><br/>


### Ex. <br/>

```yaml
access-list 10 permit 192.168.10.1 0.0.0.0
access-list 10 deny 192.168.10.0 0.0.0.255
access-liat 10 permit host 192.168.10.2

interface s1/0
ip access-group 10 out
```
<br/><br/>

s1/0에서 10번이 나갈 때 검사할 때,<br/>
위에서 밑으로 조건을 하나하나 따짐.<br/>
조건에 맞으면 더 이상 밑에 조건을 체크하지 않음.<br/><br/>

ex01. 출발지 = 192.168.10.1 (목적지는 신경 쓸 필요X) -> 첫 번째 조건 맞음. permit이기 때문에 나감.<br/>
ex02. 출발지 = 192.168.10.2 -> 두 번째 조건 맞음. deny기 때문에 못 나감.<br/>
그럼 마지막은 의미가 없어짐. 두 번째에서 걸리기 때문에.<br/>
그래서 ACL은 위아래 순서가 중요함!<br/><br/><br/>


그렇다면 순서를 바꿔보자.<br/>
```yaml
access-list 10 permit 192.168.10.1 0.0.0.0
access-list 10 permit host 192.168.10.2
access-list 10 deny 192.168.10.0 0.0.0.255
```
<br/>
그럼 위로 갈 수록 좁아지고, 밑으로 갈 수록 넓어지는 것이 좋다.<br/><br/>


ex03. 출발지 = 192.168.20.1 -> 나가려고 하는데 ACL이 걸려있음.<br/>
첫 번째, 두 번째, 세 번째 조건 모두 안 맞음.<br/>
조건에 맞는 게 하나도 없으면 전부 다 거름.<br/><br/>

항상 ACL 제일 끝에는 눈에는 보이지 않지만<br/>
```yaml
[access-list 10 deny 0.0.0.0 255.255.255.255]
```
<br/>

이 조건이 항상 들어가 있음. 우리는 이것을 `묵시적인 거부` 라고 부름.<br/>
즉 조건에 안 맞으면 무조건 거부한다는 뜻.<br/><br/>

조건에 안 맞는 것은 다 허용하고 싶으면 마지막 줄에 다 허용해주면 됨.<br/>
```yaml
access-list 10 permit 0.0.0.0 255.255.255.255
```
<br/><br/><br/>


#### Ex01. <br/>


![17](https://user-images.githubusercontent.com/117553252/213615801-84250bb7-433f-4fe7-b2c1-25264bf66a8a.png)
<br/><br/>

조건)<br/>
우선 1.1.1.1이 2.1.1.1이 ping이 되면<br/>
1.1.1.1이 2.1.1.1으로 못 나가게 out 시켜라.<br/>
나머지는 다 넘어가게 시켜라.<br/><br/>

방법)<br/>

```yaml
access-list 10 deny host 1.1.1.1
access-list 10 permit any

interface e0/1
ip access-goup 1o out
```
<br/>

ping 하면 1.1.1.1은 drop.<br/>
but 2.1.1.1에서 ping은 in.(in은 아무 관계 없음. 왜냐면 우리는 out을 했음.)<br/>
되돌아 갈 때 막히는 것.<br/><br/>

Standard ACL은 한 쪽에서 ping이 안 되면 양쪽에서 다 안 되는 것.<br/>
3.1.1.1은 통신이 잘 되도록 하기.<br/>
1.1.1.1이랑 3.1.1.1은 ping잘 되도록.<br/><br/>


결과)<br/><br/>

![18](https://user-images.githubusercontent.com/117553252/213615802-e7f122d8-01f9-4c7b-b50f-e66a33e91289.png)
<br/><br/>

![19](https://user-images.githubusercontent.com/117553252/213615803-c3e8c32e-aa3c-4462-8f58-93a8f831c247.png)
<br/><br/>


그럼 router에서 2.1.1.1으로 ping을 하면 ping이 감.<br/>
router에서 출발하는 건 2.1.1.2에서 출발하는 것이기 때문임<br/><br/>

```yaml
주의) 라우터를 경유하는 경우에만 ACL이 적용 되어 짐.
```
<br/>

![20](https://user-images.githubusercontent.com/117553252/213615805-da1a6e13-dfa0-4017-ace0-957840dd3247.png)
<br/><br/>

만약 access-list 10 deny host 2.1.1.1이라면<br/>
L3에서 L2를 갈 때는 가지만 out할 때만 안 됨.<br/><br/><br/><br/>






#### Ex02. Standard ACL <br/>

![21](https://user-images.githubusercontent.com/117553252/213615807-bef90071-da64-4318-a6cb-9213306eb56d.png)
<br/><br/>

모든 PC가 다 ping이 되도록.
<br/><br/>

방법)<br/>
```yaml
access-list 10 deny host 192.168.10.1
access-list 10 deny 192.168.20.0 0.0.0.255
access-list 10 deny 1.1.23.0 0.0.0.255
access-list 10 permit 0.0.0.0 255.255.255.255

interface e0/0
ip access-group 10 out
```
<br/>
<br/><br/>



결과)<br/>

![22](https://user-images.githubusercontent.com/117553252/213615812-1f307d66-49fd-407b-8a58-fa086aade384.png)
<br/><br/>
느김표 = 통신이 잘 되어진다는 뜻.<br/><br/>

![23](https://user-images.githubusercontent.com/117553252/213615813-f40ea0a5-b12a-48a1-82ac-bbb8f1e7479a.png)
<br/><br/> 통신이 안 되는 것.<br/><br/>


![24](https://user-images.githubusercontent.com/117553252/213615815-1042eb3c-f0b9-4230-8110-e70261cc0334.png)
<br/><br/>

![25](https://user-images.githubusercontent.com/117553252/213615817-970781db-5b10-43e0-b07b-0dfe7532bdc9.png)
<br/><br/>막혀있다는 뜻.<br/><br/>

![26](https://user-images.githubusercontent.com/117553252/213615819-3d62fad4-ee12-41a6-b029-baab606404d1.png)
<br/><br/>

![27](https://user-images.githubusercontent.com/117553252/213615821-5e64bedf-e774-49f0-9782-fc0a52ca5f7b.png)

<br/><br/>






### 참고. <br/>

```yaml
# show run
# show access-list
# show ip access-list
```
<br/>
중에 아무거나 확인해도 됨.
<br/><br/>


`# show ip interface e0/0` <br/>
in/out 중 어디에 걸려 있는지 확인.<br/><br/>


유형)<br/>
유형1 - 거부 목록 -> 나머지 전부 허용<br/>
유형2 - 허용 목록 -> 나머지 전부 거부<br/><br/><br/><br/>



### Ex. Standard ACL <br/>

![28](https://user-images.githubusercontent.com/117553252/214204362-78a0bec3-cf30-4dae-9fcc-27cb2aabdd93.png)

<br/>

풀이<br/>
```yaml
access-list 30 deny host 192.168.10.1
access-list 30 deny 1.1.12.0 0.0.0.255
access-list 30 permit 0.0.0.0 255.255.255.255

interface e0/1
ip access-group 30 out


access-list 20 permit host 192.168.10.1
access-list 20 permit 1.1.12.0 0.0.0.255

interface e0/0
ip access-group 20 out
```
<br/>


궁금한 점) ACL 번호가 다른 데 ping은 어떻게 확인하는 지. <br/>
-> 해결 : e0/0랑 e0/1이기 때문에 관계 X
<br/><br/>
