---
title: 1216
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_1216.html
folder: product2
---


### Named ACL <br/>

- ACL은 인터페이스별, 방향별(IN/OUT), 프로토콜별(ip, ipx 등)로 설정 가능함.<br/>
- ACL은 위에서 아래로 조건이 맞을 때까지 검사함<br/>
 -> 조건에 맞으면 더 이상 아래로 조건 검사 X<br/>
- 라우터를 경류하는 패킷만 ACL로 필터링 할 수 있음.<br/>
(바로 나가는 경우는 ACL 적용 X)<br/><br/>

- interface에서 삭제할 경우는 명령어 앞에 no만 붙이면 됨.<br/>
- 숫자는 하나만 지우면 다 지우면 되고, interface 안에 들어가서 꼭 `no ip access-group 30 out`을 해 줘야 함.<br/>
- 그렇지 않으면 묵시적 거부가 있기 때문에 다 `거부`됨.<br/><br/>

- 숫자 형태로 만든 것을 `Numbered ACL`이라고 한다.<br/>
- 이것을 우리는 `Named ACL`로 바꿔 보려고 한다.<br/><br/><br/>



### Named ACL 명령어 <br/>

```yaml
conf t
ip access-list standard cisco1
deny host 192.168.10.1
deny 1.1.12.0 0.0.0.255
permit 0.0.0.0 255.255.255.255

interface e0/1
ip access-group cisco1 out
```
<br/>

```yaml
conf t
ip access-list standard cisco2
permit host 192.168.10.1
permit 1.1.12.0 0.0.0.255

interface e0/0
ip access-group cisco2 out
```
<br/><br/>

### # show access-list <br/>

![1](https://user-images.githubusercontent.com/117553252/214485658-83dce742-9c6e-4ccc-aa76-8467f0f4bd6c.png)
<br/>
앞 번호는 자동으로 찍히는 것임.<br/><br/>
숫자 형태는 제일 마지막에 추가되어 지는 것임.<br/>
중간에 ACL 명령어 삽입 안 됨. <br/>
그러나 Named 같은 경우에는 원하는 곳에 추가가 가능함.<br/><br/>

10번과 20번 사이에 넣고 싶을 때,<br/>
ip access-list standard cisco1 에 들어가서<br/>
15 deny 1.1.23.0 0.0.0.255 이렇게 하면<br/>
순서적으로 봤을 때 15번은 10번과 20번 사이에 들어간다.<br/>
다시 # show access-list 하면 순서가 바뀌어져 있을 것임.<br/><br/>

![2](https://user-images.githubusercontent.com/117553252/214485664-c4c59418-b947-401f-a237-5cbe07c02bef.png)
<br/>
<br/>

마찬가지로 제거하고 싶을 때,<br/>
ip access-list standard cisco1 에 들어가서 하면 됨.<br/>
ex. no 20 하면 20번이 사라짐.<br/>
확인할 때는 마찬가지로 # show access-list 로 확인하면 됨.<br/><br/>

![3](https://user-images.githubusercontent.com/117553252/214485666-e7dc8457-fffc-4143-8431-07a9422a8e0e.png)<br/><br/><br/>




### Extended ACL <br/>

```yaml
Extended ACL : 100-199, 출발지IP/목적지IP/프로토콜/포트번호
```
<br/><br/>

```yaml
access-list [100-199] [permit/deny] [protocol] [출발지IP] [Wildcard Mask] [목적지IP] [Wildcard] eq [Port 번호]

interface s1/0
ip access-group [100-199] [in/out]
```
<br/>

eq = equal<br/>
[protocol]....[목적지 IP] [Wildcard] eq [Port 번호] 가 추가 됨.<br/><br/>



### 참고 <br/>

- Protocol : IP, ICMP, TCP, UDP ....... 多<br/>
- Port 번호<br/>
- TCP : HTTP(80), TELNET(23), SSH(22), FTP(21/20), DNS(53), SMTP(25), POP3(110) ........多<br/>
- UDP : TFTP(69), DNS(53), SNMP(161,162) ........多<br/><br/>

프로토콜은 하나의 규약, 하나의 규칙, 즉 특정 역할을 하는 프로그램이라 생각하자.<br/>
프로토콜들은 자신만의 고유한 기능을 함.<br/><br/>

ping하기 위해선 ICMP 프로그램이 각각 다 들어가 있어야 함.<br/>
[protocol] 자리에 TCP, UDP가 있는 경우 eq [Port 번호]가 들어감.<br/>
ex. [TCP] ..... eq [80] - TCP의 80번 방.<br/><br/>

[protocol] 자리에 IP가 들어가면 eq [Port 번호]가 들어가지 않음.<br/>
방 하나하나가 포트번호임.<br/>
TCP는 웹 서버를 이용함. 웹 서버는 TCP 80번임.<br/>
이렇게 서버들은 대부분 포트 번호가 기본 값으로 정해져 있다.<br/><br/>

DNS(53) - UDP : 이름 질의<br/>
DNS(53) - TCP : 서버 간 데이터 복제할 때<br/><br/>


#### Ex. <br/>

```yaml
access-list 100 permit ip host 1.1.1.1 host 2.2.2.2
access-list 100 permit tcp 192.168.10.0 0.0.0.255 host 3.3.3.3 eq 80
access-list 100 permit tcp host 2.2.2.2 192.168.30.0 0.0.0.255 eq 23
access-list 100 permit icmp host 3.3.3.3 host 4.4.4.4
access-list 100 permit udp host 1.1.1.1 host 2.2.2.2 eq 53
[access-list 100 deny op any any]

interface e0/1
ip acess-group 100 out
```
<br/><br/>

- 출발지 : 1.1.1.1 / 목적지 : 2.2.2.2 -> 첫 번째 조건 -> 2.2.2.2 PC에 있는 모든 방을 허용<br/>
ip를 허용 -> 다 허용<br/><br/>

- 두 번째 조건 : 3.3.3.3에 있는 80번 방에만 접근이 가능한.<br/>
- 세 번째 조건 : 2.2.2.2는 30점대로 시작하는 23번 방 가능한.<br/>
icmp = ping을 말함.<br/>
- 네 번째 조건 : 3.3.3.3에서 4.4.4.4로 가는 ping을 허용한다는 의미.<br/>
4.4.4.4에 있는 모든 것 들어갈 수 X. 그냥 ping만 가능한 것. 즉, 갔다가 오는 것만 되는 것이지 방에 들어갈 수 있다는 것을 의미하지는 않음.<br/>
- 다섯번째 조건 : 2.2.2.2의 포트 53을 의미함. 만약 eq가 1.1.1.1 바로 뒤에 붙었다면 1.1.1.1의 포트 53을 의미함. 즉, 2.2.2.2의 53을 허용한다는 의미.<br/><br/>

`항상 눈에 안 보이는 '묵시적 거부'가 있음을 기억할 것.`
<br/><br/><br/>


#### Ex.01 <br/>

![4](https://user-images.githubusercontent.com/117553252/214485667-1a45b927-7fe8-4428-8d8b-a89fc54e53ed.png)
<br/>

방법)<br/>
```yaml
access-list 100 permit tcp host 192.168.10.1 host 192.168.20.1 eq 80
[access-list 100 deny icmp host 192.168.10.1 host 192.168.20.1]

interface e0/1
ip access-group 100 out
```
<br/>
<br/>

결과)<br/>

![5](https://user-images.githubusercontent.com/117553252/214485670-b45e3d88-4027-4ca9-8a7f-54cc3d8c1901.png)
<br/><br/>

오류 발생)<br/>
![6](https://user-images.githubusercontent.com/117553252/214485674-6a326dc0-3968-454e-a5d6-5ab0f7745098.png)
<br/>httpd로 해야 하는데 http로 해 버림.<br/><br/>

오류 해결)<br/>
![7](https://user-images.githubusercontent.com/117553252/214485677-3623ee19-4f75-458e-a66a-ded91246c628.png)
<br/><br/><br/>


### Ex. 명령어 3개만 사용 <br/>

![8](https://user-images.githubusercontent.com/117553252/214485678-2958e53b-b8b4-4183-9534-afd796a9943b.png)
<br/>
마지막은 access-list 100 permit ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255 이걸로 하기<br/>
223은 그러면 접근이 안 됨.<br/><br/>


방법 01 - Standard <br/>
```yaml
access-list 100 permit tcp host 192.168.20.1 host 192.168.10.1 eq 80
access-list 100 deny tcp 192.168.20.0 0.0.0.255 host 192.168.10.1 eq 80
access-list 100 permit ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255

interface e0/0
ip access-group 100 out


cf. 다른 방법 01-01
access-list 100 deny tcp any host 192.168.10.1 eq 80
```
<br/>
<br/>

방법 02 - Named <br/>
```yaml
ip access-list extended cisco1
permit tcp host 192.168.20.1 host 192.168.10.1 eq 80
deny tcp 192.168.20.0 0.0.0.255 host 192.168.10.1 eq 80
permit ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255

interface e0/0
ip access-group cisco1 out
```
<br/>
<br/><br/>


### Ex. <br/>

![9](https://user-images.githubusercontent.com/117553252/214485680-ab60ef14-d578-466f-8dcb-7f32fb298411.png)
<br/>DNS 질의는 되는데 ping은 불가능<br/><br/>

![10](https://user-images.githubusercontent.com/117553252/214485681-723b9830-8b30-47ff-b697-e56c97cf3ad3.png)
<br/>
- W10만 도메인 이름을 사용하여 웹 서버에 접근 되기를 원함. (DNS 이름 질의 -> 3대 다 접근)<br/>
- W11 / W12 단지 IP 주소로는 접근 되기를 원함. (DNS 이름 질의 X, IP 주소로만 접근 가능)<br/>
- 나머지는 전부 거부.<br/><br/>

조건)<br/>
1. 192.168.20.1 만 도메인 이동을 이용해 웹 서버에 접근 가능<br/>
2. 192.168.20.2 / 192.168.20.3은 IP 주소를 이용해 웹 서버에 접근 가능<br/>
3. 나머지는 전부 거부<br/><br/>

방법)<br/>
```yaml
R1)
access-list 100 permit ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255

interface e0/0
ip access-group 100 out
```
<br/>
<br/>

```yaml
R3)
access-list 100 permit udp host 192.168.20.1 host 223.255.255.1 eq 53

interface e0/0
ip access-group 100 out
```

<br/><br/><br/>


### Ex. Build Up<br/>

![11](https://user-images.githubusercontent.com/117553252/215295764-566a0d00-09e5-4091-9266-cd7f4ccc2da4.png)
<br/>
ICMP = ping<br/><br/>


#### 순서.<br/>
1. 모두 ping이 가도록<br/>
2. HTTP 서버 / DNS 서버 설정<br/>
3. DHCP Client 예약하기 (주소 반납/받기/제외 주소)<br/>
4. ACL OUT 설정하기<br/>
방법)<br/>
R2)<br/>
오른쪽 - 오른쪽 domain으로 가능<br/>

```yaml
access-list 100 permit udp host 192.168.40.1 host 223.255.255.1 eq 53
access-list 100 permit udp host 192.168.40.2 host 223.255.255.1 eq 53

interface e0/0
ip access-group 100 out
```

<br/>
<br/>

오른쪽 ip으로 불가능<br/>
```yaml
access-list 100 deny ip host 192.168.40.1 host 223.255.255.1
access-list 100 deny ip host 192.168.40.2 host 223.255.255.1

interface e0/0
ip access-group 100 out
```
<br/><br/>

오른쪽 ping 가능 / 나머지는 전부 불가능<br/>
```yaml
access-list 100 permit icmp host 192.168.40.1 host 223.255.255.1
access-list 100 permit icmp host 192.168.40.2 host 223.255.255.1
access-list 100 deny icmp 192.168.40.0 0.0.0.255 host 223.255.255.1

interface e0/0
ip access-group 100 out
```
<br/><br/>

왼쪽 - 왼쪽 domain으로 불가능)<br/>
```yaml
access-list 100 deny udp host 192.168.10.10 host 223.255.255.1 eq 53
access-list 100 deny udp host 192.168.10.20 host 223.255.255.1 eq 53
access-list 100 permit any

interface e0/0
ip access-group 100 out
```
<br/><br/>

왼쪽 ip을 가능<br/>
```yaml
access-list 100 permit ip host 192.168.10.10 host 223.255.255.1
access-list 100 permit ip host 192.168.10.20 host 223.255.255.1

interace e0/0
ip access-group 100 out
```
<br/><br/>

왼쪽 ping 불가능/ 나머지는 전부 허용<br/>
```yaml
access-list 100 deny icmp host 192.168.10.10 host 223.255.255.1
access-list 100 deny icmp host 192.168.10.20 host 223.255.255.1
access-list 100 permit icmp 192.168.10.0 0.0.0.255 host 223.255.255.1

interface e0/0
ip access-group 100 out
```

<br/><br/><br/>


#### 나와야 하는 결과.<br/>

왼쪽)<br/>
http://223.255.255.1 - O<br/>
http://www.x.com - X<br/>
ping 223.255.255.1 - X<br/>
ping www.x.com - X<br/><br/>

오른쪽)<br/>
http://223.255.255.1 - O<br/>
http://www.x.com - O<br/>
ping 223.255.255.1 - O<br/>
ping www.x.com - O<br/><br/><br/>

### 오류 발생<br/>

ip 가 제일 크기 때문에 마지막으로 ACL을 해줘야 함.<br/>
`domain >= http >= icmp >= ip` <br/>

바꾼 방법) domain >= http >= icmp >= ip <br/><br/>

오른쪽 - 오른쪽 domain으로 가능)<br/>
```yaml
access-list 100 permit udp 192.168.40.0 0.0.0.255 host 223.255.255.1 eq 53

interface e0/0
ip access-group 100 out
```
<br/><br/>

오른쪽 http 가능<br/>
```yaml
access-list 100 permit tcp 192.168.40.0 0.0.0.255 host 223.255.255.1 eq 80

interface e0/0
ip access-group 100 out
```
<br/><br/>

오른쪽 ping 가능 / 나머지는 전부 불가능<br/>
```yaml
access-list 100 permit icmp 192.168.40.0 0.0.0.255 host 223.255.255.1

interface e0/0
ip access-group 100 out
```
<br/><br/>

오른쪽 ip으로 불가능<br/>
```yaml
access-list 100 deny ip 192.168.40.0 0.0.0.255 host 223.255.255.1

interface e0/0
ip access-group 100 out
```
<br/><br/>

왼쪽 - 왼쪽 domain으로 불가능)<br/>
```yaml
access-list 100 deny udp 192.168.10.0 0.0.0.255 host 223.255.255.1 eq 53

interface e0/0
ip access-group 100 out
```
<br/><br/>

왼쪽 http 가능<br/>
```yaml
access-list 100 permit tcp 192.168.10.0 0.0.0.255 host 223.255.255.1 eq 80

interface e0/0
ip access-group 100 out
```
<br/><br/>

왼쪽 ping 불가능 / 나머지는 전부 허용<br/>
```yaml
access-list 100 deny icmp 192.168.10.0 0.0.0.255 host 223.255.255.1

interface e0/0
ip access-group 100 out
```
<br/><br/>

왼쪽 ip으로 가능<br/>
```yaml
access-list 100 permit ip 192.168.10.0 0.0.0.255 host 223.255.255.1

interface e0/0
ip access-group 100 out
```
