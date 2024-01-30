---
title: 1223
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_1223.html
folder: product2
---


- 중복하기 싫을 때 나온 것이 재분배.<br/>
- rip : 소규모<br/>
- eigrp : 중소규모<br/>
- ospf : 대기업<br/>
- BGP : 전세계<br/><br/>

- rip으로 배운 네트워크를 eigrp 로 재분배.<br/>
- rip으로 배운 네트워크를 ospf 로 재분배.<br/>
- eigrp 를 rip으로 재분배.<br/>
- eigrp 를 ospf 로 재분배.<br/><br/>

- `Connected` = 직접 붙어 있는 것.<br/><br/>

router rip<br/>
redistribute ~ A ~ : A를 rip으로 재분배 <br/><br/><br/>


### Ex. 재분배 01<br/>

![1](https://user-images.githubusercontent.com/117553252/215958090-c0bf890d-5e60-435c-9f35-8fb9cf668712.png)
<br/><br/>

#### 방법<br/>

R2)<br/>
```yaml
router rip
version 2
network 1.0.0.0
network 126.0.0.0
no auto-summary
redistribute eigrp 100 metric 1
redistribute ospf 1 metric 1

router eigrp 100
network 2.0.0.0
no auto-summary

router ospf 1
network 3.3.34.0 0.0.0.255 area 0
```
<br/><br/>

#### 결과<br/>

![2](https://user-images.githubusercontent.com/117553252/215958097-a59fa58f-8874-4c1e-bd8d-249e4ce80d48.png)
<br/><br/>

### Ex. 재분배 02<br/>

![3](https://user-images.githubusercontent.com/117553252/215958099-a8665c0e-b0b9-49e4-a32c-43cefa4f2d65.png)
<br/><br/>

#### 방법.<br/>

R2) - X<br/>
```yaml
router ospf 1
redistribute rip subnets metric 20
redistribute eigrp 100 subnets metric 20
```
<br/><br/>

R2) - O<br/>
```yaml
router eigrp 100
redistribute rip metric 1 1 1 1 1
redistribute ospf 1 metric 1 1 1 1 1
```
<br/><br/>

#### 결과<br/>

![4](https://user-images.githubusercontent.com/117553252/215958103-e06ee1dc-ad72-4221-b1dc-2710d02b12dd.png)
<br/><br/>

### 정리<br/>

![5](https://user-images.githubusercontent.com/117553252/215958108-f7fa5b1f-4148-4e68-a658-86452dd88788.png)
<br/><br/>

R1)<br/>
R 5개<br/>
R2)<br/>
R 1개<br/>
D 1개<br/>
O 1개<br/>
R3)<br/>
D EX 5개<br/>
R4)<br/>
E2 5개<br/><br/>

![6](https://user-images.githubusercontent.com/117553252/215958110-8a476136-6e11-4aed-be4a-0294e5c65a20.png)
<br/><br/>

![7](https://user-images.githubusercontent.com/117553252/215958111-ed5342c3-4124-4335-8d18-cfee7e89ec76.png)
<br/><br/>

![8](https://user-images.githubusercontent.com/117553252/215958113-686dc2a9-087e-4f86-bb55-e6014ca17fb7.png)
<br/><br/>

![9](https://user-images.githubusercontent.com/117553252/215958114-92505bbe-6351-427a-84d9-a719e12569f8.png)
<br/><br/>

ping은 정상적으로 다 잘 가야 됨.<br/><br/><br/>



### Ex. 재분배 03<br/>

재분배는 항상 중간에서.<br/>

![10](https://user-images.githubusercontent.com/117553252/215958115-56604cc3-06b3-43f3-9297-f6b4e5c56151.png)
<br/><br/>

#### 방법.<br/>

R3)<br/>
```yaml
router eigrp 100
redistribute rip metric 1 1 1 1 1
redistribute ospf 1 metric 1 1 1 1 1
redistribute eigrp 200 metric 1 1 1 1 1
```
<br/><br/>

```yaml
router eigrp 200
redistribute rip metric 1 1 1 1 1
redistribute ospf 1 metric 1 1 1 1 1
redistribute eigrp 100 metric 1 1 1 1 1
```
<br/><br/>

```yaml
router rip
version 2
redistribute eigrp 100 metric 1
redistribute eigrp 200 metric 1
redistribute ospf 1 metric 1
```
<br/><br/>

```yaml
router ospf 1
redistribute rip subnets metric 20
redistribute eigrp 100 subnets metric 20
redistribute eigrp 200 subnet metric 20
```
<br/><br/>

#### 결과.<br/>

ping 전부 감.<br/><br/>

![11](https://user-images.githubusercontent.com/117553252/215958116-b09f2880-5c6c-428e-a70e-90939975871e.png)
<br/><br/>

![12](https://user-images.githubusercontent.com/117553252/215958118-739af9da-b22a-49ee-87d8-7cf8c3e70d85.png)
<br/><br/>

![13](https://user-images.githubusercontent.com/117553252/215958120-c671e5cc-8ccd-492b-b0ff-d6e3dac03273.png)
<br/><br/>

![14](https://user-images.githubusercontent.com/117553252/215958122-96371604-0f92-4251-9106-56c182ba4376.png)
<br/><br/>

![15](https://user-images.githubusercontent.com/117553252/215958126-960afdb4-f363-43a0-8c2f-0fdfc39a1a83.png)
<br/><br/><br/>


### Ex. Loopback 추가<br/>

![16](https://user-images.githubusercontent.com/117553252/215958127-b77e51b6-95f1-49de-a6ef-2a7a9e086c3f.png)
<br/><br/>

Loopback 추가<br/><br/><br/>


#### 결과<br/>

![17](https://user-images.githubusercontent.com/117553252/215958128-e6de7981-a11c-42ff-8329-27c27759e38c.png)
<br/><br/>

![18](https://user-images.githubusercontent.com/117553252/215958131-9c7159a3-3ca2-41b0-837c-e2aefdd3e981.png)
<br/><br/>

![19](https://user-images.githubusercontent.com/117553252/215958134-794b7760-e357-411b-9984-4bdc597e754b.png)
<br/><br/>

![20](https://user-images.githubusercontent.com/117553252/215958136-8fe4e806-cb04-4dfa-9d86-7fe02d5e85fd.png)
<br/><br/>

![21](https://user-images.githubusercontent.com/117553252/215958137-f09ad778-9133-4843-93e4-9e65112089d8.png)
<br/><br/><br/><br/>


### Ex. redistribute connected ~<br/>

![22](https://user-images.githubusercontent.com/117553252/215958140-4fcf9491-b3d8-4468-a642-ff0ef7f79270.png)
<br/><br/>

#### 방법<br/>

R1)<br/>
```yaml
router eigrp 100
network 192.168.10.0
network 1.0.0.0
redistribute connected metric 1 1 1 1 1
```
<br/><br/>

R2)<br/>
```yaml
router eigrp 200
network 192.168.30.0
network 2.0.0.0
redistribute connected metric 1 1 1 1 1
```
<br/><br/>

R4)<br/>
```yaml
router rip
version 2
network 192.168.30.0
network 3.0.0.0
redistribute connected metric 1
```
<br/><br/>

R5)<br/>
```yaml
router ospf 1
network 192.168.40.0 0.0.0.255 area 0
network 4.4.12.0 0.0.0.255 area 0
redistribute connected subnets metric 20
```
<br/><br/>

#### 결과<br/>


![23](https://user-images.githubusercontent.com/117553252/215958141-141ba8b8-bfe9-42c1-8b8e-c2a391edc7e3.png)
<br/><br/><br/>

### Ex. Static Route를 재분배<br/>

![24](https://user-images.githubusercontent.com/117553252/215958143-0301cb64-b29b-42b0-8072-37f13db824a5.png)
<br/><br/>

#### 방법<br/>

R3)<br/>
```yaml
ip route 100.1.1.0 255.255.255.0 1.1.15.1

router eigrp 200
redistribute static metric 1 1 1 1 1

router rip
redistribute static metric 1

router ospf 1
redistribute static subnets metric 20
```
<br/><br/>


#### 결과<br/>

R3)<br/>
![25](https://user-images.githubusercontent.com/117553252/215958146-41466c8d-23e9-4153-b1aa-709b6a3213cc.png)
<br/><br/>

![26](https://user-images.githubusercontent.com/117553252/215958152-3e38688b-9f0c-4302-b6b6-8a592a5b88ba.png)
<br/><br/>

![27](https://user-images.githubusercontent.com/117553252/215958155-1ae38025-4728-427a-a0e9-9fe23434c735.png)
<br/><br/>


![28](https://user-images.githubusercontent.com/117553252/215958156-9231a1d2-d6ef-4e8c-801b-d90830431c8e.png)
<br/><br/>

![29](https://user-images.githubusercontent.com/117553252/215958160-bd2f6910-2b6d-4bb4-bf96-3c7d328c5a1a.png)
