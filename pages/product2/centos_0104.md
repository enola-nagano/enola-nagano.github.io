---
title: 01 04
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_0104.html
folder: product2
---


### Ex.Build Up


![1](https://user-images.githubusercontent.com/117553252/211180918-fcc1c064-147a-4f0f-baeb-ab55dcae5d7d.png)
<br/><br/><br/>


#### 01.방법.
<br/><br/>

1. ip 주소 적기

1. network

1. DHCP Client, PAT 공인 주소
```yaml
R4)
    ip dhcp pool ciso 1
    network 223.255.255.0 255.255.255.0
    default-router 223.255.255.254
    ip dhcp excluded-address 223.255.255.254

    ip nat pool cisco 192.168.40.254 192.168.40.254 netmask 255.255.255.0
    access-list 10 permit 223.255.255.0 0.0.0.255
    ip nat inside source list 10 pool cisco overload

    interface e0/0
    ip nat inside
    interface s1/0
    ip nat outside
    interface s1/1
    ip nat outside
```

1. 축약<br/>

```yaml
R1)
    interface s1/0
    ip summary-address rip 193.168.10.0 255.255.255.0

R5)
    interface s1/0
    ip summary-address eigrp 100 194.168.50.0 255.255.254.0

R7)
    router ospf 1
    area 70 range 196.168.70.0 255.255.254.0

R8)
    router ospf 1
    area 80 range 195.168.80.0 255.255.254.0
```

<br/><br/><br/>


#### 02. 결과
<br/><br/>

![2](https://user-images.githubusercontent.com/117553252/211180920-174ed35a-8deb-44c7-9aa6-ea91d5c8f84e.png)
<br/><br/>

![3](https://user-images.githubusercontent.com/117553252/211180921-445d2f03-d8d7-4165-8595-411d862378fb.png)
<br/> R1 축약 확인 완.<br/><br/>


R4)<br/>
![4](https://user-images.githubusercontent.com/117553252/211180922-4d8bd2d8-9e15-4ae5-8c0b-9560001eff30.png)
<br/> 축약 확인 완.<br/><br/>


R5)<br/>
![5](https://user-images.githubusercontent.com/117553252/211180923-b96a7b47-6f6e-434d-8250-5c2e227667f9.png)<br/><br/>


![6](https://user-images.githubusercontent.com/117553252/211180925-1ca5d2df-f355-4205-8adc-43c388a2e100.png)<br/><br/>
![7](https://user-images.githubusercontent.com/117553252/211180926-809db304-21b9-4983-bb2d-847c310175bb.png)<br/><br/>
![8](https://user-images.githubusercontent.com/117553252/211180927-03a499c1-c39e-4c41-9d38-16a36ebaf230.png)<br/><br/><br/>



#### 03. 오류.
<br/>
- ping 안 감 -> 재분배 안 함.
<br/><br/><br/>


#### 04. 오류 해결.
<br/>
R5)<br/>
    interface s1/0<br/>
    ip summary-address eigrp 100 222.175.0.0 255.255.0.0<br/>
<br/>
R8)<br/>
    router ospf 1<br/>
    summary0address 211.175.0.0 255.255.0.0<br/>
<br/><br/><br/>



#### 04. 결과.
<br/>
![9](https://user-images.githubusercontent.com/117553252/211180929-56cf8fda-9513-4bad-9530-6507df2e0a89.png)<br/><br/>
![10](https://user-images.githubusercontent.com/117553252/211180931-009c8019-9f2f-4bdc-a8ab-062e48815542.png)<br/><br/>
![11](https://user-images.githubusercontent.com/117553252/211180932-0b89e12c-3558-4376-80c7-684695c7079a.png)<br/><br/>
![12](https://user-images.githubusercontent.com/117553252/211180933-9f8237f5-0054-45de-b6b6-1f08be8030c1.png)<br/><br/>
![13](https://user-images.githubusercontent.com/117553252/211180934-e5cbec9b-239c-439e-9e20-dec33db60ea0.png)<br/><br/>
![14](https://user-images.githubusercontent.com/117553252/211180935-cf29f723-9e4b-427b-b0cc-0f22f78c8102.png)<br/><br/>
![15](https://user-images.githubusercontent.com/117553252/211180936-ff149a40-403b-47ca-ad82-52e6b7958e08.png)<br/><br/>
![16](https://user-images.githubusercontent.com/117553252/211180938-d9ac2872-bb9c-4934-917d-978d0065dcc5.png)<br/><br/>
![17](https://user-images.githubusercontent.com/117553252/211180939-1ee9dbdb-ae2a-4367-b5ee-0463c3221b26.png)<br/><br/>
![18](https://user-images.githubusercontent.com/117553252/211180940-5ff04968-755e-47ab-bc6e-bf9808e04560.png)<br/><br/><br/>




#### 05. 오류.
<br/>
![19](https://user-images.githubusercontent.com/117553252/211180942-e0d80290-b629-4e9e-9850-91ea997d83cc.png)<br/><br/>

- 223.255.255.254는 eigrp 가 아님.
- DHCP Client에서는 밖으로 ping이 가야 됨. / 밖에서 회사 내부로는 ping이 되면 안 됨.
<br/><br/><br/>



#### 06. 결과.
<br/><br/>


![20](https://user-images.githubusercontent.com/117553252/211180944-4af10bd0-d359-49ec-902c-02fe63f78579.png)<br/><br/>
![21](https://user-images.githubusercontent.com/117553252/211180946-4c86f9f6-35a5-466f-a3d5-4b2fa7befbfa.png)<br/><br/>
![22](https://user-images.githubusercontent.com/117553252/211180947-c42cb28b-9412-45d1-a2e4-d356292e6d08.png)<br/><br/>
![23](https://user-images.githubusercontent.com/117553252/211180948-650a8c1d-1f7c-4ef9-b634-3755e3eb6065.png)<br/><br/>
![24](https://user-images.githubusercontent.com/117553252/211180949-44dfb90d-f75c-46e7-926d-c5a458bc2d09.png)<br/><br/>
![25](https://user-images.githubusercontent.com/117553252/211180950-a4636c11-0bc7-46b3-a7e4-082767b830e2.png)<br/><br/>
![26](https://user-images.githubusercontent.com/117553252/211180951-2215efbf-7ada-4752-b0cc-ad2399aa514e.png)<br/><br/>
![27](https://user-images.githubusercontent.com/117553252/211180952-e2300fd3-0afa-4146-aa60-7053d9aee7b4.png)<br/><br/>
![28](https://user-images.githubusercontent.com/117553252/211180953-55f0e5d9-bdd1-4702-b700-2e3da4257840.png)<br/><br/>
![29](https://user-images.githubusercontent.com/117553252/211180955-c0b3d5e8-df4d-4d31-b41d-e0549437928c.png)<br/><br/>
![30](https://user-images.githubusercontent.com/117553252/211180956-9ba7b61d-3b5c-485b-a032-f509fadfdda8.png)<br/><br/>








### Frame-Relay
<br/>

- Frame-Relay는 PPP, HDLD와 같이 WAN 구간에서 사용하는 encapsulation 방식이지만 동시에 여러 라우터가 접속하여 링크를 공유할 수 있다는 점에서 차이가 있다. 또한 broadcast를 통해 통신을 하는 일반 이더넷 방식과는 달리 정해진 길(Virtual Circuit)을 통해서만 통신이 가능하다는 점에서도 차이가 있다. 이러한 두가지 특징 때문에 frame-relay 의 통신 방식을 <span style="color.red">NBMA(Non Broadcast & Multi access)</span> 라고 부른다.<br/><br/>

- 일반적으로 ISP에서는 DCE로 연결되고 커스터머쪽은 DTE로 연결을 시킨다. 앞서 말했듯이 F/R에서는 통신을 위해 VC(virtual circuit)가 필요하다. 이 VC를 어떻게 만드냐에 따라 SVC(Sitched VC)와 PVC(Permanent VC)로 나눌 수 있다.<br/>
말 그대로 세션을 맺었다고 terminate 되고 다시 데이터 전달을 위해 커스터머 쪽에서 새로운 VC를 만드는 방법은 SVC이고 미리 정해진 VC만들 영구적으로 사용할 수 있는 방법은 PVC라고 한다.<br/><br/>

- 여러 정비가 위와 같이 VC를 사용하므로 F/R에서는 프레임 내부의 주소 부분에서 FECN, BECN, DE 비트를 활용하여 혼잡을 제어할 수 있다.<br/><br/>

- F/R은 통신을 위해 DLCI 라는 변호를 사용하여 클라이언트(DTE)와 DCE간의 연결을 가능케 해주며 통신시 local DLCI 만을 사용하게 되므로 하나의 물리 인터페이스에서는 항상 unique 한 DLCI 번호를 사용해야 한다.<br/><br/>

- local DLCI 번호화 목적지 IP 주소를 매핑하는 방법은 inverse-ARP를 이용하는 방법과 관리자가 직접 지정하는 정적인 방법으로 나뉠 수 있지만 여러가지 면에서 직접 지정하는 방식이 유용하다.<br/><br/>

- 구성이 끝난 뒤에는 DTE-DCE 간에 PVC의 상태와 주소, 멀팈스트에 관한 사항을 알 수 있는 신호를 주고 받는데 이를 LMI(Local Management Interface) 라고 하며 keepalive 의 역할도 병행하게 된다.<br/><br/>

- 기술 향상 : <span style="color.red">X.25</span> → <span style="color.red">프레임 릴레이</span> → <span style="color.red">ATM</span>
<br/><br/>

![31](https://user-images.githubusercontent.com/117553252/211182049-c08a3b28-a969-42af-a759-b9d936441c14.PNG)<br/><br/><br/>




### Point-to-Point Frame-Relay
<br/>
```yaml
- Subinterface는 하나의 전용선처럼 작동한다.
- 각각의 Point-to-Point connection은 각각의 Subnet을 필요로 한다.
```
<br/>
- Frame-Relay를 하는 이유 : 논리적인 연결을 하기 위해서.
- 라우터를 Frame-relay Switch로 만들 수 있음. 우리는 그렇게 할 것임.
<br/><br/><br/>


### 실습.
<br/><br/>

![32](https://user-images.githubusercontent.com/117553252/211182205-a1f1f43d-c166-42e4-9e6c-dc6b95cc0292.png)

<br/><br/>

1. 명령어<br/>

```yaml
FR) Frame-relay switch

    enable
    conf t
    #frame-relay switching

    interface s1/0
    encapsulation frame-relay
    frame-relay intf-type dce
    frame-relay route 102 interface serial 1/1 201
    frame-relay route 103 interface serial 1/2 301
    no shutdown

    interface s1/1
    encapsulation frame-relay
    frame-relay intf-type dce
    frame-relay route 201 interface serial 1/0 102
    no shutdown

    interface s1/2
    encapsulation frame-relay
    frame-relay intf-type dce
    frame-relay route 301 interface serial 1/0 103
    no shutdown
```
<br/>
```yaml
R1)
    interface s1/0
    encapsulation frame-relay
    no shutdown

    interface s1/0.12 point-to-point
    ip address 1.1.12.1 255.255.255.0
    frame-relay interface-dlci 102

    interface s1/0.13 point-to-point
    ip address 1.1.13.1 255.255.255.0
    frame-relay interface-dlci 103

R3)
    interface s1/0
    encapsulaiton frame-relay
    no shutdown

    interface s1/0.12 point-to-point
    ip address 1.1.12.2 255.255.255.0
    frame-relay interface-dlci 201

R4)
    interface s1/0
    encapsulation frame-relay
    no shutdown

    interface s1/0.13 point-to-point
    ip address 1.1.13.3 255.255.255.0
    frame-relay interface-dlci 301
```
<br/><br/><br/>


2. 결과 : Frame-Relay (연결된 라우터끼리 ping 되어야 함.)
<br/>
![33](https://user-images.githubusercontent.com/117553252/211182207-992b4022-9035-44ee-bd94-08353df94d73.png)<br/><br/>
![34](https://user-images.githubusercontent.com/117553252/211182208-d7ba4999-c416-445d-88ff-474dd402d183.png)<br/><br/>
![35](https://user-images.githubusercontent.com/117553252/211182209-5bd47404-61c4-4768-9f15-682f3e80578c.png)<br/>
PC끼리는 통신이 안 되는 게 맞음.<br/><br/><br/>


`# show frame-relay map` : R1, R3, R4
<br/><br/>

![36](https://user-images.githubusercontent.com/117553252/211182210-639c3b28-b29d-41a4-87da-724cf3c603e1.png)<br/><br/>
![37](https://user-images.githubusercontent.com/117553252/211182212-fdff2468-69e9-4f45-8d16-d5a289be529d.png)<br/><br/>
![38](https://user-images.githubusercontent.com/117553252/211182213-4ea92e4c-0888-437f-89f5-bd84ee50371f.png)<br/><br/>


`# show frame-relay route` : FR
<br/><br/>
![39](https://user-images.githubusercontent.com/117553252/211182214-e3997d8d-93e2-4c04-963c-611d1434b854.png)
<br/><br/><br/>



c
3. 더 해줘야 하는 명령어 : ip route
<br/>

```yaml
    
W5) R1
    ip route 192.168.20.0 255.255.255.0 1.1.12.2
    ip route 192.168.30.0 255.255.255.0 1.1.13.3

W6) R3
    ip route 192.168.10.0 255.255.255.0 1.1.12.1
    ip route 192.168.30.0 255.255.255.0 1.1.12.1

W7) R4
    ip route 192.168.10.0 255.255.255.0 1.1.13.1
    ip route 192.168.20.0 255.255.255.0 1.1.13.1
```

<br/><br/>


4. 결과 : W6에서 확인
<br/><br/>
![40](https://user-images.githubusercontent.com/117553252/211183113-a8dd4b7d-4c58-4870-8164-2bc87bbcf846.png)<br/><br/>
![41](https://user-images.githubusercontent.com/117553252/211183114-66890f89-9dea-47a8-bde9-159aa796d38d.png)<br/><br/><br/>


5. RIPv2로도 해보기.
<br/>

```yaml
R1)
    router rip
    version 2
    network 192.168.10.0
    network 1.1.12.0
    network 1.1.13.0
    no auto-summary

R3)
    router rip
    version 2
    network 192.168.20.0
    network 1.1.12.0
    no auto-summary

R4)
    router rip
    version 2
    network 192.168.30.0
    netowrk 1.1.13.0
    no auto-summary
```
