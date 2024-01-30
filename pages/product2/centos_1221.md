---
title: 1221
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_1221.html
folder: product2
---


### Router<br/>
1. 다른 Network 주소를 연결<br/>
2. 지도 관리<br/>
- Static Routing : ip route 명령어 (S, S*)<br/>
- Dynamic Routing : RIP(R), EIGRP(D, D EX), OSPF(O, OIA, E1/E2/N1/N2) -> BGP(B)<br/><br/>

![1](https://user-images.githubusercontent.com/117553252/215656566-0991bba8-5650-4e4b-893e-28eeba18749f.png)
<br/><br/><br/>

### 동적 라우팅 만들기<br/>
<br/>

R1)<br/>

```yaml
conf t
router rip
version 2
network 1.1.12.0
network 192.168.10.0
no auto-summary <- 축약 X / 원래 네트워크대로 넘기라는 의미.
```
<br/><br/>

R2)<br/>
```yaml
conf t
router rip
version 2
network 1.1.12.0
network 1.1.23.0
network 192.168.20.0
```
<br/><br/>

R3)<br/>
```yaml
conf t
router rip
version 2
network 1.1.23.0
network 1.1.34.0
netowrk 192.168.30.0
```
<br/><br/>

R4)<br/>
```yaml
conf t
router rip
version 2
network 1.1.34.0
network 192.168.40.0
```

<br/><br/><br/>

- version 1, 2는 각자 호환이 안 됨.<br/>
모두 통일 해야 됨.<br/>
말이 없으면 version 2로 하자.<br/>
rip은 라우팅 교환이 조금 느림.<br/>
전체적으로 그림은 알 필요가 없다.<br/>
이웃을 통해 전파하는 것이라 그렇다.<br/><br/>

- 명령어 다 주고 # show ip route 하면 모든 네트워크 주소가 들어와 있어야 함.<br/><br/>

- 동기화 됐는지 확인하기 위해서 R1은 R이 5개, C는 2개<br/>
R2) C 3개, R 4개<br/>
R3) C 3개, R 4개<br/>
R4) C 2개, R 5개<br/><br/>

- C = 자기 자신에게 붙어 있는 네트워크.<br/><br/>

10.1에서 모든 pc에 ping이 다 가야함.<br/><br/>

- Classful address : A(255.0.0.0), B(255.255.0.0), C(255.255.255.0) RIP은 클래스펄까지만 인식함.<br/><br/><br/>


### 주 클래스<br/>
<br/>

R1)<br/>
```yaml
conf t
router rip
version 2
network 1.0.0.0
network 192.168.10.0
no auto-summary
```
<br/><br/>

R2)<br/>
```yaml
conf t
router rip
version 2
network 1.0.0.0
network 192.168.20.0
no auto-summary
```
<br/><br/>

R3)<br/>
```yaml
conf t
router rip
version 2
network 1.0.0.0
network 192.168.30.0
no auto-summary
```
<br/><br/>

R4)<br/>
```yaml
conf t
router rip
version 2
network 1.0.0.0
network 192.168.40.0
no auto-summary
```
<br/><br/>

으로 쳐도 된다는 뜻. 즉 네트워크 주소를 줄 때 네트워크 주 주소로 줘도 된다는 것을 의미함.<br/><br/><br/>


### 정리.<br/>

```yaml
conf t
router rip
version 2
network [Classful Network 주소]
```

<br/><br/><br/>

### 지우고 싶을 때.<br/>
`no router rip` <- rip 전부 삭제<br/>
`no network 1.0.0.0` <- 이 부분만 삭제<br/><br/>

- RIP은 소규모 네트워크에서 쓰는 것을 권장함.<br/><br/><br/><br/>


### RIPv2 <br/>

![2](https://user-images.githubusercontent.com/117553252/215656569-aa9fd720-c57c-4a10-88bf-216adc6bb43b.png)
<br/><br/>

- 업데이트 : 30초 -> 조금 느림. (30초 간격으로 지도를 계속 주고 받음)<br/>
- R (코드)<br/>
- [120/3]<br/>
- 120 : 관리자 거리값 (Administrative Distance : AD값) <- 딱 정해져 있는 것<br/>
- 3 : 메트릭 값 (Metric 값) - Hop(라우터를 거치는 개수, 즉 거리) <- 값이 낮을 수록 좋은 것.<br/>
- Hop : 최대 15 Hop -> RIP은 소규모에서 사용<br/>
- 16 hop : 목적지에 도달 불가.<br/><br/>

1.1.12.0처럼 서로 공유하는 부분을 넣지 않을 경우 넘어가지 못하기 때문에, 서로 공유하는 부분은 꼭 network 로 넣어줘야 한다.<br/>
그러나 pc를 하고 싶지 않을 경우 network 를 넣어주지 않으면 된다.<br/><br/>

rip의 기본 값은 `auto-summary` : 축약 <- 입력하지 않을 경우.<br/><br/><br/>

![3](https://user-images.githubusercontent.com/117553252/215656570-2ff17a7f-dc48-4ce5-8cf8-6c3a3675f174.png)
<br/><br/><br/>

### auto-summary의 의미<br/>

이럴 경우 R1이 직접 알고 있는 주소는 4개.<br/>
C가 4개가 나와야 함.<br/>
그런데 network 7.1.1.0<br/>
network 7.2.2.0 이렇게 포함시키거나<br/>
network 7.0.0.0으로 할 경우<br/>
네트워크 3개를 s1/0으로 보낼 경우 이 경우의 클래스 펄이 1.0.0.0<br/>
그러나 루프백은 7.0.0.0<br/>
즉, 주소가 다른 경우(1점대랑 7점대) 주소가 `자동으로 축약`되어 넘어간다.<br/>
그러면 루프백은 넘어갈 때 `7.0.0.0/8`으로 넘어가게 된다.<br/><br/><br/>


### no auto-summary의 의미<br/>

![4](https://user-images.githubusercontent.com/117553252/215656572-c5c23aec-658c-4256-bb15-19a216f76d09.png)
<br/><br/>

왼쪽은 넘어갈 때 7.0.0.0/8로 넘어가고<br/>
오른쪽도 넘어갈 때 7.0.0.0/8로 넘어간다.<br/>
그럼 R3에서 7.1.1.1로 ping을 했을 경우 어디로 가야할 지를 모르는 상황이 발생.<br/>
따라서 R1에 no auto-summary <- 축약하지 말아달라. 를 하면 됨.<br/>
그러면 왼쪽도 7.1.1.0 / 7.2.2.0 로 각각 넘어가게 된다.<br/><br/><br/>

### auto-summary 에서 네트워크가 같을 경우<br/>


![5](https://user-images.githubusercontent.com/117553252/215656574-f29d20da-9002-4dd1-9c91-18ba16c84569.png)

<br/><br/>

주 네트워크가 1로 같을 경우, 자동축약이 되지 않는다.<br/>
1.1.45.0/24로 넘어간다는 뜻.<br/><br/><br/>

### 정리.<br/>
![6](https://user-images.githubusercontent.com/117553252/215656576-38ea51fa-63f3-4586-90b3-992abb076015.png)
<br/><br/><br/>

### # clear ip route *
<br/><br/>

### Ex. no auto-summary<br/>

![7](https://user-images.githubusercontent.com/117553252/215656577-c9c072ee-0cc3-4fd5-8bdc-b4b4e7f8fb8e.png)
<br/><br/>


### Ex. no auto-summary<br/>

![8](https://user-images.githubusercontent.com/117553252/215656580-cb60ec4a-c434-4f2a-9d33-b185474edb99.png)
<br/><br/><br/><br/>


### Ex.<br/>

![9](https://user-images.githubusercontent.com/117553252/215656879-ddce1605-8b4f-4514-a067-f97b120d3c20.png)
<br/><br/>

#### 방법<br/>

1. interface, static으로 pc에 ip 주소 주기<br/>
2. router rip (ip route 대신, 말이 없다면 no auto-summary를 넣을 것)<br/>
3. DNS 서버, HTTP 서버<br/>
4. PAT용, NAT 서버 설정<br/>
R1) C 2개, R 4개<br/>
```yaml
router rip
version 2
network 1.0.0.0
no auto-summary
```
<br/>

R2) C 2개, R 4개<br/>
```yaml
router rip
version 2
network 1.0.0.0
no auto-summary
```
<br/>

R3) C 3개, R 3개<br/>
```yaml
router rip
version 2
network 1.0.0.0
network 223.255.255.0
no auto-summary
```
<br/>

R4) C 2개, R 4개<br/>
```yaml
router rip
version 2
network 1.0.0.0
network 211.175.185.0
no auto-summary
```
<br/><br/><br/>

#### 결과<br/>

![10](https://user-images.githubusercontent.com/117553252/215656880-37514f3b-2bf1-4e1c-b669-188346186852.png)
