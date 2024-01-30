---
title: 1227
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_1227.html
folder: product2
---


### Ex. 01 (ft.show ip protocol)<br/>

![1](https://user-images.githubusercontent.com/117553252/216245156-76a84d0e-511c-4362-91ee-1b37f71a94d0.png)
<br/><br/>

#### 결과<br/>

![2](https://user-images.githubusercontent.com/117553252/216245158-5a789a32-c75f-4303-98fe-58c4a763528f.png)
<br/><br/>

![3](https://user-images.githubusercontent.com/117553252/216245160-da019e77-9bbb-4867-ac0a-8c161b73d940.png)
<br/><br/>

![4](https://user-images.githubusercontent.com/117553252/216245164-1aacfa5f-be5c-484e-b30a-7d69e04d1719.png)
<br/><br/>

R6)<br/>
![5](https://user-images.githubusercontent.com/117553252/216245166-28f1064b-da6b-4908-b93b-832b7d96a554.png)
<br/><br/>

![6](https://user-images.githubusercontent.com/117553252/216245169-5c69f6bc-a905-448a-a832-e2c84f4489ea.png)
<br/><br/>

![7](https://user-images.githubusercontent.com/117553252/216245171-abd47ab5-c2d6-4038-a491-bc4da2a7649b.png)
<br/><br/><br/>


### # show ip eigrp topology<br/>

기본구성)<br/>
![8](https://user-images.githubusercontent.com/117553252/216245174-6b86efca-1912-487f-8022-f7203cf60a18.png)
<br/><br/><br/>

- Unequal Cost Load Balancing 를 지원한다. ★<br/>
-> 꼭 best 경로가 아니더라도 라우팅 테이블에 올 수 있다.<br/><br/>

```yaml
# show ip eigrp topology
```
<br/>

![9](https://user-images.githubusercontent.com/117553252/216245177-4be1a711-f8f8-4522-a74c-9f52540ec7c0.png)
<br/><br/>

- `# show ip route` <- best 경로, 가장 빠른 경로<br/>
- `# show ip eigrp topology` <- 목적지까지 가는 all 경로<br/><br/>

테이블이 3개<br/>
1. neighbor<br/>
2. topology<br/>
3. route<br/><br/>

### metric 값 바꾸기<br/>

![10](https://user-images.githubusercontent.com/117553252/216245181-b91f63e5-9d6e-49ca-ae69-1fc8d098463f.png)
<br/><br/>

자꾸 에러 발생.<br/>
![11](https://user-images.githubusercontent.com/117553252/216245182-4fe3c947-c73e-45a0-bfab-79406240d5ec.png)
<br/><br/><br/>

### 결론 <br/>
neighbor랑 metric 값이랑 활성화 되어 있는 것이 같아야 함.<br/><br/><br/>


### # show ip eigrp topology all-link<br/>

- Only EIGRP<br/>
출발지 -> 목적지 까지의 메트릭 : FD<br/>
이웃하는 라우터에서 출발지 -> 목적지까지의 메트릭 : RD = AD<br/>
베스트 경로에 있는 이웃 라우터 : Succesor<br/>
feasible successor이 될 수 있는 조건 : 최적 조건의 FD보다 작은 AD값을 가지고 있는 것.<br/><br/>

```yaml
# show ip eigrp topology all-link
```
<br/>
![12](https://user-images.githubusercontent.com/117553252/216245185-5019b044-4d43-4943-afd9-7456366b813c.png)
<br/><br/>

![13](https://user-images.githubusercontent.com/117553252/216245186-74481f99-a55b-4133-9386-01e735a2769e.png)<br/><br/>

![14](https://user-images.githubusercontent.com/117553252/216245188-f1546e12-2a30-46c7-afc4-df9dc9b8f67e.png)
<br/><br/>

![15](https://user-images.githubusercontent.com/117553252/216245192-0233669b-035e-4bc8-8fe6-d05ef2543e24.png)
<br/><br/>

AD값이 똑같으면 unequl을 하지 못하기 때문에, 작게 만들어 줘야 함.<br/><br/><br/>

### 명령어<br/>

```yaml
conf t
ip access-list standard R4L0
permit 1.1.4.0 0.0.0.255

router eigrp 100
offset-list R4L0 in 1000 s1/0

clear ip eigrp *
```
<br/><br/><br/>

#### 결과<br/>

![16](https://user-images.githubusercontent.com/117553252/216245196-10ba7250-faaf-4bdd-bda2-1c90447ca10e.png)
<br/><br/>

### EIGRP variance (곱하기)<br/>
<br/><br/>

### 명령어 <br/>

```yaml
router eigrp 100
variance 2 <- 2 곱하기
```
<br/><br/>

#### 결과<br/>

- show ip route<br/><br/>
![17](https://user-images.githubusercontent.com/117553252/216245198-55769710-0084-42eb-94f7-9c38604db28e.png)
<br/><br/><br/><br/>

### Unequal Load Balancing<br/>

![18](https://user-images.githubusercontent.com/117553252/216245199-f5e736b7-f656-4cc2-a872-b84ed0dd174f.png)
<br/><br/>

- Feasible Successor는 라우팅 테이블에 있을까 없을까?<br/>
-> 기본적으로 라우팅 테이블에는 없다.<br/><br/>

- Successor는 topology 테이블에 있? 없?<br/>
-> 테이블에 있.<br/><br/>

- Feasible Successor는 topology 테이블에 있? 없?<br/>
-> 테이블에 있.<br/><br/>

- EIGRP 의 가장 큰 특징.<br/>
-> metric 더 큰 값도 올릴 수 있다.<br/><br/>

- null 0 : 휴지통으로 버려라<br/><br/><br/>


### 명령어 - 방법 01<br/>

R4) 유형 01<br/>
```yaml
ip route 0.0.0.0 0.0.0.0 null 0

router eigrp 100
redistribute static metric 1 1 1 1 1
```
<br/><br/>

#### 결과<br/>

![19](https://user-images.githubusercontent.com/117553252/216245201-4cada5fa-7025-4754-838b-8a514c5f6ddc.png)
<br/>
eigrp를 타고 자동으로 0.0.0.0이 생기는 것을 확인할 수 있다.<br/><br/>

![20](https://user-images.githubusercontent.com/117553252/216245204-4f1a334b-0e55-4f90-be93-3145901468bb.png)
<br/>
재분배 = D EX<br/><br/>

no redistribute static metric 1 1 1 1 1<br/>
R1)<br/>
![21](https://user-images.githubusercontent.com/117553252/216245205-dcb46725-fbec-4669-87df-6fe82c28ddea.png)
<br/><br/><br/>


### 명령어 - 방법 02<br/>

R4)<br/>
```yaml
ip route 0.0.0.0 0.0.0.0 null 0

router eigrp 100
network 0.0.0.0
```
<br/><br/><br/>

#### 결과<br/>

![22](https://user-images.githubusercontent.com/117553252/216245211-25134496-24d7-43b7-9118-68e312f76fa0.png)
<br/><br/>

![23](https://user-images.githubusercontent.com/117553252/216245213-b28a63b3-b78c-4ac3-825d-03694bc73d90.png)
<br/><br/>

### 명령어 - 방법 03<br/>

R4)<br/>
```yaml
interface (int) s1/2
ip summary-address eigrp 100 0.0.0.0 0.0.0.0
```
<br/><br/>

#### 결과<br/>


![24](https://user-images.githubusercontent.com/117553252/216245215-fa47c4bd-74dc-4661-b442-3cfb4f0fb04d.png)
<br/><br/>

![25](https://user-images.githubusercontent.com/117553252/216245217-f879fd16-804a-43a0-aaee-2819ddf070de.png)
<br/><br/>

![26](https://user-images.githubusercontent.com/117553252/216245220-08b5ef75-6b61-4f3c-99df-aa399a5933ca.png)
<br/><br/>
R4 - Null0 : 루핑 방지<br/><br/><br/>


### 정리<br/>

![27](https://user-images.githubusercontent.com/117553252/216245221-b56f9da4-6222-4707-8687-508ee27cf43b.png)
<br/><br/><br/><br/>


### ip summary-address eigrp ~<br/>

![28](https://user-images.githubusercontent.com/117553252/216245223-a51ea669-d805-42ca-b5f6-c35008e36637.png)
<br/><br/>

통신X<br/><br/>

이유 :<br/>

![29](https://user-images.githubusercontent.com/117553252/216245224-f8857ec7-0d71-4b2a-b941-858d3f8f81d4.png)
<br/><br/>

![30](https://user-images.githubusercontent.com/117553252/216245228-131d3a08-6c17-4411-98b3-fc17a054de1f.png)
<br/><br/>

![31](https://user-images.githubusercontent.com/117553252/216245229-8947651e-3698-4ca6-88c5-1e0e27fc849b.png)
<br/><br/>

- 돌아올 때 막힘.<br/>
: R3에서 1.1.1.1이 없으니깐.<br/><br/>

- 통신이 되게 하고 싶다면?<br/>
: R2에서 R3으로 보냈을 때 AD = 90<br/>
R3의 AD 기본 값 = 5 -> 90보다 더 크게 주면 됨.<br/><br/><br/>


#### 방법<br/>

![32](https://user-images.githubusercontent.com/117553252/216245230-84821e69-5086-408c-802b-64b34b6097d5.png)
<br/><br/><br/>

#### 결과<br/>

![33](https://user-images.githubusercontent.com/117553252/216245232-12ca415b-d6b4-4baf-808d-1feb34c74ab8.png)
<br/><br/>

R4)<br/>

![34](https://user-images.githubusercontent.com/117553252/216245235-ef65e395-5da5-4625-aeab-c04612187e2d.png)
<br/><br/>
ping이 가는 것을 확인할 수 있음.<br/><br/><br/><br/>
