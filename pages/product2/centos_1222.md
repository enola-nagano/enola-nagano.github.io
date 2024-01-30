---
title: 1222
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_1222.html
folder: product2
---


### 동적 라우팅 기초 명령어<br/>

#### RIPv2 protocol <-암기★ <br/>
- Administrative Distance : 120<br/>
- Metric : 15 Hop (RIP은 메트릭으로 홉을 사용, 메트릭 값은 낮은 것이 좋음), 16 Hop(목적지 도달X)<br/>
- Code : R (cf. ip route = S)<br/>
- Update : 30초 (간격) Ex. clear ip route * <- 새로고침<br/><br/>


![1](https://user-images.githubusercontent.com/117553252/215936369-ca6dd2b4-6e04-45ee-aeb5-b66f6cde3549.png)
<br/><br/>
ex. R1을 211.175.185.1 으로 전달하고 싶으면 3hop이 됨.<br/><br/><br/>



#### EIGRP AS [100]<br/>

AS 번호가 반드시 같아야 함.<br/>
다르면 서로 라우팅 불가.<br/><br/><br/>

#### 명령어<br/>

router eigrp ? <- 쓸 수 있는 번호가 나옴.<br/><br/>

R1)<br/>
```yaml
router eigrp 100
network 1.0.0.0
network 10.0.0.0
no auto-summary
```
<br/><br/>

R2)<br/>
```yaml
router eigrp 100
network 1.0.0.0
no auto-summary
```
<br/><br/>

R3)<br/>
```yaml
router eigrp 100
network 1.0.0.0
network 223.255.255.0
no auto-summary
```
<br/><br/>

R4)<br/>
```yaml
router eigrp 100
network 1.0.0.0
network 211.175.185.0
no auto-summary
```
<br/><br/>

eigrp는 금방 교환이 됨.<br/>
중소 규모에서 많이 씀.<br/><br/>

#### EIGRP protocol<br/>

- Administrative Distance : 90(D), 170(D EX)<br/>
- Metric : B, D, R, L, M <- 최적 경로를 계산할 때 `최대 5개를 쓴다`는 것만 기억하자.<br/>
- Code : D (같은 AS), D EX(재분배)<br/>
- Update : 순간<br/><br/><br/>


### Ex.EIGRP(혼합)<br/>

![2](https://user-images.githubusercontent.com/117553252/215936387-b63aef96-e7fa-4c82-9afb-135f0a1eca13.png)
<br/><br/>
요구 조건)<br/>
1.1.45.1은 넘기지 X<br/><br/>

방법) 이때는 OSPF처럼 와일드 카드를 만들면 됨.<br/><br/>

EIGRP는 구성하는 방법이 2개가 있음.<br/><br/>

`EIGRP protocol (혼합) = RIP + OSPF ; 조금 더 디테일하게.`<br/><br/>

R1)<br/>
```yaml
router eigrp 100
network 1.1.12.0 0.0.0.255
network 10.0.0.0 0.255.255.255
no auto-summary
```
<br/><br/>

왜 와일드 카드를 쓰는지의 차이점을 알아두면 됨.<br/><br/>

R2)<br/>
```yaml
router eigrp 100
network 1.1.12.0 0.0.0.255
network 1.1.23.0 0.0.0.255
no auto-summary
```
<br/><br/>

R3)<br/>
```yaml
router eigrp 100
network 1.1.23.0 0.0.0.255
network 1.1.34.0 0.0.0.255
network 223.255.255.0 0.0.0.255
no auto-summary
```
<br/><br/>

R4)<br/>
```yaml
router eigrp 100
network 1.1.34.0 0.0.0.255
network 211.175.185.0 0.0.0.255
no auto-summary
```
<br/><br/>

`와일드 카드 쉽게 = 서브넷마스크 -> 와일드카드`<br/>
255.0.0.0 -> 0.255.255.255<br/>
255.255.0.0 -> 0.0.255.255<br/>
255.255.255.0 -> 0.0.0.255<br/>
255.255.255.192 -> 0.0.0.00111111=63<br/><br/><br/>

### no ip domain-lookup<br/>
: 문자를 잘못 쳤을 때 dns에 질의를 하지 않는 명령어.<br/><br/>

### OSPF protocol<br/>
OSPF는 `AREA`로 구성됨.<br/><br/>

area가 여러 개 있을 경우 반드시 area 0이 필요함.<br/>
area 0 = 중심 area, 나머지 area 랑 물리적으로 전부 연결 되어 있어야 함.<br/><br/>

![3](https://user-images.githubusercontent.com/117553252/215936389-af897336-11b2-4a26-8b05-cb9168e9c6d5.png)
<br/><br/>

잘못된 예시)<br/>
![4](https://user-images.githubusercontent.com/117553252/215936391-a2ce6347-3848-4a92-bd45-2adf7db9ff43.png)
<br/><br/>
나머지 area는 area0을 거쳐서 전달이 가능함.<br/><br/>

- Administrative Distance : 110<br/>
- Metric : Cost<br/>
- Code : O (같은 Area), OIA (다른 Area), E1/E2/N1/N2 (재분배)<br/>
- Update : 순간<br/><br/><br/>





### OSPF 명령어 (무조건 WildCard)<br/>

R1)<br/>
```yaml
router ospf 1
network 10.0.0.0 0.255.255.255 area 10
network 1.1.45.0 0.0.0.255 area 10
network 1.1.12.0 0.0.0.255 area 0
(no auto-summary 없음!)
```
<br/><br/>

R2)<br/>
```yaml
router osfp 1
network 1.1.12.0 0.0.0.255 area 0
network 1.1.23.0 0.0.0.255 area 0
```
<br/><br/>

R3)<br/>
```yaml
router osfp 1
network 1.1.23.0 0.0.0.255 area 0
network 1.1.34.0 0.0.0.255 area 20
network 223.255.255.0 0.0.0.255 area 20
```
<br/><br/>

R4)<br/>
```yaml
router ospf 1 
network 1.1.34.0 0.0.0.255 area 20
network 211.175.185.0 0.0.0.255 area 20
```
<br/><br/><br/><br/>


### Ex. OSPF<br/>

![5](https://user-images.githubusercontent.com/117553252/215936392-45f7aa61-89d2-421d-bb59-161345a7bb67.png)
<br/><br/>

방법<br/>
![6](https://user-images.githubusercontent.com/117553252/215936394-24b3a467-6dda-4638-a0fc-3320f5c21e38.png)
<br/><br/><br/><br/>



### AD 값<br/>
AD 값 (Administrative Distance)<br/>
- RIPv2 = 120 < OSPF = 110 < EIGRP = 90<br/>
- AD 값은 `낮은 값`이 우선.<br/><br/>
![7](https://user-images.githubusercontent.com/117553252/215936396-4c1af268-bde3-41b4-b30b-93e723eeacef.png)
<br/><br/>

결과)<br/>
OSPF : 1.1.1.1/32로 넘어감<br/>
RIP : 1.1.1.0/24로 넘어감<br/>
-> OSPF가 이겨서 32가 되는 것임.<br/>

![8](https://user-images.githubusercontent.com/117553252/215936398-265e7580-ae81-4620-85eb-b1928c23225f.png)
<br/><br/><br/>


### ip ospf network point-to-point<br/>
: 그런데 우리는 원래 네트워크 대역대로 넘기고 싶을 때 쓰는 명령어.<br/><br/>

```yaml
interface lo 0
ip ospf network point-to-point
```
<br/>
를 모든 Router에 입력해주면 됨.<br/>
그러면 /24로 넘어감.<br/><br/>


#### 결과<br/>

![9](https://user-images.githubusercontent.com/117553252/215936399-5218fd3f-3d40-4c77-93ee-fa1296072b5b.png)
<br/>
/24로 넘어온 것을 확인할 수 있다.<br/><br/>

![10](https://user-images.githubusercontent.com/117553252/215936401-22c69d33-d5ad-40c7-b892-46e25cff497c.png)
<br/><br/>

결과 - R1<br/>
![11](https://user-images.githubusercontent.com/117553252/215936403-f088b524-3877-45d8-9ad7-e426c7b0cb67.png)
<br/><br/>

RIPv2 < OSPF < EIGRP < Static Route <br/>
![12](https://user-images.githubusercontent.com/117553252/215936405-a21b04c1-3fec-4c0b-9f62-0560496becb0.png)
<br/><br/>

결과<br/>
![13](https://user-images.githubusercontent.com/117553252/215936407-47fd6227-51eb-4f8c-9a44-2f8bd366525b.png)
<br/><br/>
S가 생긴 것을 발견할 수 있다.
<br/><br/><br/>

### 우선순위 순서<br/>

RIPv2 -> OSPF AREA 0 -> EIGRP 100 -> Static Route (1) -> Connected (0)<br/><br/><br/><br/>



### Ex. Routing 마무리<br/>
![14](https://user-images.githubusercontent.com/117553252/215936409-15df3d66-bab0-4402-b0b7-3c5cb84376ac.png)
<br/><br/>

방법<br/>
R1) ip route 2개<br/>
R2) ip route X<br/>
R3) ip route 2개<br/>
R4) ip route 2개<br/><br/><br/>

결과<br/>


![15](https://user-images.githubusercontent.com/117553252/215936411-1cee53d6-dab7-463c-9795-4eef99bf7590.png)
<br/><br/>

![16](https://user-images.githubusercontent.com/117553252/215936414-a8f68470-f35c-4c72-b32e-d7427c1481f7.png)
<br/><br/>

![17](https://user-images.githubusercontent.com/117553252/215936416-5656dcd6-f09f-4446-a787-50b1cf909964.png)
<br/><br/>


![18](https://user-images.githubusercontent.com/117553252/215936419-da5b066b-bf0a-4fe2-9646-442d6e46de6b.png)

<br/><br/>
