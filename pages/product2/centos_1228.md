---
title: 1228
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_1228.html
folder: product2
---


### # Ex 01. EIGRP 축약 <br/>

![Untitled](https://user-images.githubusercontent.com/117553252/217123336-a44c03f0-8c59-4bbe-87c5-7b364a1117e5.png)
<br/><br/>

조건)<br/>
R5에서 ping을 223.255.255.1로 보내기<br/>
-> AD 값을 조정해야 됨<br/><br/>

#### 방법)<br/>
R2)<br/>
int s1/1<br/>
ip summary-address eigrp 100 0.0.0.0 0.0.0.0 120<br/><br/>

R3)<br/>
int s1/1<br/>
ip summary-address eigrp 100 0.0.0.0 0.0.0.0 110<br/><br/>

R4)<br/>
int s1/1<br/>
ip summary-address eigrp 100 0.0.0.0 0.0.0.0 100<br/><br/>

#### 결과 - 오류 발생)<br/>

![Untitled (1)](https://user-images.githubusercontent.com/117553252/217123267-a21a33c1-ff00-4acd-a9a5-41235edb91ef.png)
<br/><br/>

192.168.10.1까지는 가는데 223.255.255.1까지는 안 감<br/><br/>

#### 해결)<br/>
R1에서 loopback 1 재분배 해야 됨.<br/><br/>

#### 결과 - R5)<br/>
![Untitled (2)](https://user-images.githubusercontent.com/117553252/217123272-73b1a7d7-6d83-40b8-98bf-a7f6ccfd8d23.png)
<br/><br/>

#### 과정의 오류) <br/>

100은 R2에는 줄 필요가 없고,<br/>
R3이랑 R4에서 줄 필요가 있다.<br/>
왜냐하면 R3이랑 R4에서 0.0.0.0이 2개니까. 그런데 R2는 0.0.0.0이 1개라서 굳이 줄 필요 없고 90만 넘으면 어떤 숫자를 주든 상관 없다.<br/><br/>

#### 새로 고친 방법<br/>

R2)<br/>
int s1/1<br/>
ip summary-address eigrp 100 0.0.0.0 0.0.0.0<br/><br/>

R3)<br/>
int s1/1<br/>
ip summary-address eigrp 100 0.0.0.0 0.0.0.0 100<br/><br/>

R4)<br/>
int s1/1<br/>
ip summary-address eigrp 100 0.0.0.0 0.0.0.0 100<br/><br/>

#### 결과<br/>

![Untitled (3)](https://user-images.githubusercontent.com/117553252/217123274-9d72aa0d-271b-4075-8d43-7e518eb59bfd.png)
<br/><br/>

#### 결과 해설<br/>

R2)<br/>
![Untitled (4)](https://user-images.githubusercontent.com/117553252/217123275-5ebf3f03-8818-4c2e-8973-cc8fe0b00e19.png)
<br/><br/>

![Untitled (5)](https://user-images.githubusercontent.com/117553252/217123276-73933ae0-ea4c-4420-993d-cc4b64338e94.png)
<br/>
왼쪽 네트워크가 모두 안 보이는 것을 알 수 있음.<br/><br/>

R4)<br/>
![Untitled (6)](https://user-images.githubusercontent.com/117553252/217123277-cd7af2e1-b8cb-41e9-9008-ae5d46cc1e7a.png)
<br/><br/>

R5)<br/>
![Untitled (7)](https://user-images.githubusercontent.com/117553252/217123279-0dd0608f-2b20-41c4-ac37-6debccbb7c91.png)
<br/>
축약의 목적 : 라우팅 테이블이 확 비는 것을 볼 수 있다.<br/>
이것이 축약의 목적임.<br/><br/>

R2에서는 굳이 수를 더해 줄 필요가 없다.<br/>
어차피 왼쪽에 네트워크를 다 가지고 있기 때문임.<br/><br/><br/>



### Ex 02. EIGRP 축약<br/>

![Untitled (8)](https://user-images.githubusercontent.com/117553252/217123281-350b26e9-c7b2-4ced-9817-8369ff6c3748.png)
<br/><br/>

#### 방법<br/>

R1)<br/>
```yaml
int s1/0
ip summary-address eigrp 100 172.16.128.0 255.255.192.0
172.16.10/100100.0
172.16.10/000000.0
->172.16.10/000000=128.0
Sub:255.255.192.0
```
<br/><br/>

R2)<br/>
```yaml
int s1/0
ip summary-address eigrp 100 172.16.32.0 255.255.255.0
172.16.32./01000001
172.16.32./10000001
->172.16.32.0
Sub:255.255.255.0
```
<br/><br/>

R3)<br/>
```yaml
int s1/0
ip summary-address eigrp 100 172.16.64.0 255.255.192.0
172.16.01/100000.0
172.16.01/000000.0
->172.16.64.0
Sub:255.255.192.0
```
<br/><br/>

R4)<br/>
```yaml
int s1/3
ip summary-address eigrp 100 172.16.0.0 255.255.0.0
172.16./100000000.0
172.16./001000000.0
172.16./010000000.0
->172.16.0.0
Sub:255.255.0.0

int s1/3
ip summary-address eigrp 100 1.1.0.0 255.255.192.0
1.1.00/001110.0
1.1.00/011000.0
1.1.00/100010.0
->1.1.0.0
Sub:255.255.192.0
```
<br/><br/>

#### 결과<br/>

![Untitled (9)](https://user-images.githubusercontent.com/117553252/217123284-7c07f122-5cb3-4fbc-a376-fbc1de37ade4.png)
<br/><br/>

![Untitled (10)](https://user-images.githubusercontent.com/117553252/217123285-52341855-2a35-4672-a9f8-9ace31529052.png)
<br/><br/>

R3)<br/>
![Untitled (11)](https://user-images.githubusercontent.com/117553252/217123288-2933a9f0-17c9-43a0-9733-5effd69d192d.png)
<br/><br/>

![Untitled (12)](https://user-images.githubusercontent.com/117553252/217123291-95b62d77-9c2c-4788-8dd6-da46d0d1c91e.png)
<br/><br/>

R5)<br/>
![Untitled (13)](https://user-images.githubusercontent.com/117553252/217123293-7b2cbf20-a38c-493b-8f30-24ec0be3da7e.png)
<br/>
ex. R4에서 1.1.1.16? 같은 것들이 R5에서 보며 1.1.0.0으로 가는 것을 확인할 수 있음.<br/><br/>

![Untitled (14)](https://user-images.githubusercontent.com/117553252/217123296-68ce10fa-3002-47a2-971e-2840f6b5a599.png)
<br/>ping도 전부 감.<br/><br/><br/>

### MD5 NEIGHBOR 인증<br/>

![Untitled (15)](https://user-images.githubusercontent.com/117553252/217123300-e992f5d5-c353-4baa-b22e-ba77c6eb9453.png)
<br/><br/>

#### 결과<br/>

R1)<br/>

![Untitled (16)](https://user-images.githubusercontent.com/117553252/217123302-73e3cc9f-c03f-4a70-a9f0-0d1d8c1a4067.png)
<br/><br/><br/>


### EIGRP의 SIA 문제점 / Stub<br/>

![Untitled (17)](https://user-images.githubusercontent.com/117553252/217123304-43a9755c-587d-45ba-8147-841bfc45df3a.png)
<br/><br/>

![Untitled (18)](https://user-images.githubusercontent.com/117553252/217123306-847934ef-4b00-46ea-83ea-54196e0359f0.png)
<br/><br/>

기본구성)<br/>
![Untitled (19)](https://user-images.githubusercontent.com/117553252/217123309-d4e23c51-f3b1-4d30-828e-6c921270f8f6.png)
<br/><br/><br/><br/>

### DR / BDR / DROther<br/>

![Untitled (20)](https://user-images.githubusercontent.com/117553252/217123312-ebecca86-b0ec-4289-8402-b4292a7442cc.png)
<br/>
#show ip ospf neighbor 각각 라우터에서 네이버가 3개씩 나와야 함.<br/><br/>

#### # show ip ospf neighbor 결과<br/>

![Untitled (21)](https://user-images.githubusercontent.com/117553252/217123316-002754a1-b67d-4922-96d2-65b359c204fa.png)
<br/><br/>

![Untitled (22)](https://user-images.githubusercontent.com/117553252/217123320-6058504d-cd23-4a1b-92ef-ec8baec563d5.png)
<br/><br/>

![Untitled (23)](https://user-images.githubusercontent.com/117553252/217123323-be799875-7ccf-47b0-985c-1c3618cec678.png)
<br/><br/>

![Untitled (24)](https://user-images.githubusercontent.com/117553252/217123326-bc571136-1c2a-4330-8c6a-af270ee56698.png)
<br/><br/>

### 라우팅을 효율적으로 만들기 위해<br/>

- DR : 반장<br/>
- BDR : 부반장<br/>
- DROther : 나머지<br/><br/><br/>

### DR, BDR, DROther이 선출되는 경우<br/>

- Broadcast network : eth0/0, fa0/0<br/>
- NonBroadcast network : frame-relay 환경에서 multipoing 인터페이스<br/><br/>

### 중요한 것<br/>

- DROther 간에는 Routing 교환을 하지 않음.<br/>
- DROther 간에는 2WAY<br/><br/><br/>



![Untitled (25)](https://user-images.githubusercontent.com/117553252/217123329-6d227029-7bb0-4657-96be-9fe7791d69cb.png)
<br/><br/>

### # clear ip ospf process -> yes<br/><br/>

### 결과<br/>

![Untitled (26)](https://user-images.githubusercontent.com/117553252/217123331-f43ffc35-955a-49e8-836d-18ee3b4226d2.png)
<br/><br/><br/><br/>
