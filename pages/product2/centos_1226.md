---
title: 1226
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_1226.html
folder: product2
---


### Ex. Redistribute<br/>

![1](https://user-images.githubusercontent.com/117553252/215973321-2f3167f0-4740-43ec-aeab-400ada07dbf8.png)
<br/><br/>

조건)<br/>
loopback은 전부 재분배로 넘기기<br/>
R1, R2, R5 전부 재분배<br/>
R3, R4는 전부 넘겨주기<br/><br/>

#### 방법<br/>

1. ip 주소 주기<br/>
2. EIGRP / OSPF / RIPv2 주기<br/>
3. 재분배<br/>
R1)<br/>
```yaml
router eigrp 100
redistribute ospf 1 metric 1 1 1 1 1 - X
redistribute connected metric 1 1 1 1 1
```
<br/><br/>

R2)<br/>
```yaml
router eigrp 100
redistribute ospf 1 metric 1 1 1 1 1 - X
redistribute connected metric 1 1 1 1 1
```
<br/><br/>

R3)<br/>
```yaml
router ospf 1
redistribute eigrp 100 subnets metric 20
redistribute connected subnets metric 20
```
<br/><br/>

R4)<br/>
```yaml
router ospf 1
redistribute rip subnets metric 20
redistribute connected subnets metric 20
```
<br/><br/>

R5)<br/>
```yaml
router rip
redistribute ospf 1 metric 1 - X
redistribute connected metric 1
```
<br/><br/><br/>

#### 오류 발생 - 변경<br/>

R1)<br/>
```yaml
router eigrp 100
redistribute connected metric 1 1 1 1 1
```
<br/><br/>

R2)<br/>
```yaml
router eigrp 100
redistribute connected metric 1 1 1 1 1
```
<br/><br/>

R3)<br/>
```yaml
router ospf 1
redistribute eigrp 100 subnets metric 20
redistribute connected subnets metric 20

+) router eigrp 100
redistribute ospf 1 metric 1 1 1 1 1
redistribute connected metric 1 1 1 1 1
```
<br/><br/>

R4)<br/>
```yaml
router ospf 1
redistribute rip subnets metric 20
redistribute connected subnets metric 20

+) router rip
redistribute ospf 1 metric 1
redistribute connected metric 1
```
<br/><br/>

R5)<br/>
```yaml
router rip
redistribute ospf 1 metric 1 - X
redistribute connected metric 1
```
<br/><br/><br/>

### service network restart<br/>

![2](https://user-images.githubusercontent.com/117553252/215973328-9cc15a1e-a8ef-4c4c-a30f-59faa0fa1994.png)
<br/><br/><br/><br/>


### 결과<br/>


![3](https://user-images.githubusercontent.com/117553252/215973332-ca55a732-8afc-4db2-9a77-c4a7d0920668.png)
<br/><br/>

![4](https://user-images.githubusercontent.com/117553252/215973335-993897c5-0ae1-4da3-8fff-b90bc43054e3.png)
<br/><br/>

R3)<br/>
![5](https://user-images.githubusercontent.com/117553252/215973336-e9ca0209-66d6-46eb-83be-41ec99b9d0c8.png)
<br/><br/>

![6](https://user-images.githubusercontent.com/117553252/215973338-8fce0fb4-389f-4177-80d7-1a6c5733f0fb.png)
<br/><br/>

R5)<br/>
![7](https://user-images.githubusercontent.com/117553252/215973341-3471eb64-fefc-4eef-a5b0-f4fde601e032.png)
<br/><br/><br/>


### 추가 R1)<br/>

R1) `no redistribute connected metric 1 1 1 1 1`<br/><br/>

![8](https://user-images.githubusercontent.com/117553252/215973342-bba8aa66-631b-47c8-8761-6fbbb3d4ddfe.png)
<br/><br/>
11점대가 없는 것을 확인할 수 있다.
<br/><br/><br/>

R3) ip route 11.1.1.0 255.255.255.0 1.1.12.1 <- Static Router 잡아주기, 이것을 재분배하기 <br/>

```yaml
R3)
router eigrp 100
redistribute static metric 1 1 1 1 1
```
<br/><br/>

결과)<br/>
![9](https://user-images.githubusercontent.com/117553252/215973343-1447f6c6-c4bd-405c-9580-72a4b9445ebe.png)
<br/><br/>

![10](https://user-images.githubusercontent.com/117553252/215973344-6b81a412-a046-458a-88aa-2d0e8de47906.png)
<br/><br/>

![11](https://user-images.githubusercontent.com/117553252/215973347-cb2ed334-0fab-4b60-979b-6c152c975b09.png)
<br/><br/>


![12](https://user-images.githubusercontent.com/117553252/215973349-14d041ba-a370-4c5a-bdd2-266d3b06e784.png)
<br/><br/>

오류해결)<br/>
R3)<br/>
router ospf 1<br/>
redistribute static subnets metric 20<br/><br/>

결과)<br/>
![13](https://user-images.githubusercontent.com/117553252/215973352-dabc89c9-10fd-4358-abda-d73dfb09e050.png)
<br/><br/>

![14](https://user-images.githubusercontent.com/117553252/215973355-adb0b82b-50e6-43d5-bfb6-c87769615edb.png)
<br/><br/><br/><br/>



### AD(관리자 거리값) 조정.<br/>

- 재분배 주의점.<br/>
: 하나의 네트워크를 재분배받아, `동일 라우터에서` 다시 다른 라우팅 프로토콜로는 재분배되지 않는다.<br/>
: 라우팅 설정모드에서 `network 명령어`가 redistribute 명령어`보다 우선한다.★<br/>
: 모든 라우팅 프로토콜에서 재분배시 지정하는 초기 메트릭값을 어떻게 지정하는 것이 좋을까? -> 정답은 `문제가 발생하지 않게 적당히` 지정하는 것이다.<br/><br/><br/>


### Ex. AD 조정<br/>

![15](https://user-images.githubusercontent.com/117553252/215973357-4b368e68-2ff9-49d2-ab8b-3e5b13736555.png)
<br/><br/>

#### 기본구성<br/>
: 재분배(는 항상 경계에서)<br/><br/>

R2)<br/>
```yaml
router eigrp 100
redistribute ospf 1 metric 1 1 1 1 1

router ospf 1
redistribute eigrp 100 subnets metric 10
```
<br/><br/>

R3)<br/>
```yaml
router eigrp 100
redistribute ospf 1 metric 1 1 1 1 1

router ospf 1
redistribute eigrp 100 subnets metric 10
```

<br/><br/>

결과)<br/>
![16](https://user-images.githubusercontent.com/117553252/215973359-f999227c-20ab-4522-9d6f-8def29911fa5.png)
<br/><br/>


![17](https://user-images.githubusercontent.com/117553252/215973360-0f43f311-19cf-457a-8ffc-a8953bc3201e.png)
<br/><br/>

오류 해결) 숫자 잘못 입력함<br/>
![18](https://user-images.githubusercontent.com/117553252/215973362-1431774e-0fa9-47de-a824-d76303f4e4bf.png)
<br/><br/>
R2에서 via 192.168.3.4 확인 완.<br/>
AREA 0이 목표 점임.<br/><br/>


![19](https://user-images.githubusercontent.com/117553252/215973364-9988e4ce-c65a-4c68-acce-49d72b9f1566.png)
<br/><br/><br/>

- AD 기본 값 : 낮은 게 우선.<br/>
RIP : 120<br/>
OSPF : 110<br/>
EIGRP : 90 / 170(재분배) / 5<br/><br/>

- 따라서 next hop이 via 192.168.3.2로 잡히는 것임.<br/>
여러 경로에서 배운 경우, 가장 빠른 경로만 올라옴.<br/><br/><br/>


### 정리.<br/>


![20](https://user-images.githubusercontent.com/117553252/215973367-0e9d387a-62a7-474b-9a20-d3273d1fe416.png)
<br/><br/><br/><br/>


### AD값 조정(Only 본인 라우터)<br/>

명령어)<br/>
```yaml
conf t
router rip
distance 220 <- 본인의 값에서만 바꿔라는 뜻.(해당 라우터에서만 변경 됨) / AD값은 옆의 라우터로 전송되지 않음.
```
<br/><br/>

```yaml
router eigrp 100
distance 80 120 <- 재분배 : 120
```
<br/><br/>

```yaml
router ospf
distance 150
```
<br/><br/><br/>

![21](https://user-images.githubusercontent.com/117553252/215973368-976de8b6-2f19-428c-9a7f-b89bd3290a4f.png)
<br/><br/>
AD 값을 110보다 작게 주기만 하면 됨.<br/><br/><br/>


#### 방법<br/>

R2)<br/>
```yaml
router eigrp 100
distance eigrp 90 105
```
<br/><br/>
![22](https://user-images.githubusercontent.com/117553252/215973372-6af21348-617b-4ba0-ab97-411f7eefeb00.png)
<br/><br/>

#### 결과<br/>

![23](https://user-images.githubusercontent.com/117553252/215973374-ecdecc21-1bc2-4820-8194-30f1a30e0575.png)
<br/><br/>
via 192.168.2.3으로 바뀐 것을 확인할 수 있음.<br/><br/>

`cf.잘 안 되는 경우, R2) # clear ip eigrp neighbor *`<br/><br/><br/>



### RIP protocol<br/>

기본구성)<br/>
![24](https://user-images.githubusercontent.com/117553252/215973377-6457a55a-b18c-4317-bec7-78b817cf5d7b.png)
<br/><br/>
router rip<br/>
version 2<br/><br/>

### Ex.<br/>

![25](https://user-images.githubusercontent.com/117553252/215973380-7f439515-a81a-4299-bfc8-9fc0be5f578e.png)
<br/><br/>

![26](https://user-images.githubusercontent.com/117553252/215973383-73880795-4741-4afb-bd5e-ee34376c8cef.png)
<br/><br/>

### # show ip protocol<br/>

![27](https://user-images.githubusercontent.com/117553252/215973387-a7376252-3364-4500-846b-be86f331021c.png)
<br/><br/>

![28](https://user-images.githubusercontent.com/117553252/215973390-249076ea-22c6-4d30-bceb-d18820892f23.png)
<br/>rip version 2로 보내고 2로 받는다는 뜻.<br/><br/>

![29](https://user-images.githubusercontent.com/117553252/215973392-9af1e813-7aa0-4ac0-8e68-2c4df572573d.png)
<br/>기본 값.<br/><br/>

ip Split Horizon은 기본으로 다 설정되어 있음.<br/><br/>

R1)<br/>
![30](https://user-images.githubusercontent.com/117553252/215973394-a277aed5-acdb-4e71-97f6-b3bbf94cacab.png)
<br/><br/>
# show ip protocol<br/>
version 1<br/><br/>


### version을 안 썼을 경우,<br/>


![31](https://user-images.githubusercontent.com/117553252/215973399-2f5bdf6a-6ffa-48e7-b084-b7440d1cd8bc.png)
<br/><br/>

sedn = 1<br/>
receive = 1, 2<br/><br/><br/>
