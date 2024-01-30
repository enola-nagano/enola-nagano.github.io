---
title: 0109
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_0109.html
folder: product2
---


### Ex01. Route-map
<br/><br/>
![1](https://user-images.githubusercontent.com/117553252/211533911-8c6850ef-67b5-44c3-844d-a5e43eb1348b.png)
<br/><br/>

#### 방법.
<br/><br/>
```yaml
R1)
    ip prefix-list D0 permit 192.168.10.0/24

    route-map cisco deny 10
    match ip address prefix-list D0

    router ospf 1
    distribute-list route-map cisco in
```
<br/><br/><br/>


#### 결과.
<br/><br/>
![2](https://user-images.githubusercontent.com/117553252/211533917-58edde17-2baa-438e-ba4d-509d9421f09b.png)
<br/><br/>
![3](https://user-images.githubusercontent.com/117553252/211533919-fe2530dc-d1f1-4490-9e86-e74eee904451.png)
<br/><br/><br/><br/>



### Ex02. Route-map
<br/>![4](https://user-images.githubusercontent.com/117553252/211533924-56d4cb0c-c26a-4896-b840-5ade74ec9fd1.png)<br/>

#### 방법.
<br/><br/>
```yaml
R1)
    ip prefix-list D1 deny 192.168.10.0/24
    ip prefix-list D1 permit 0.0.0.0/0 le 32

    route-map cisco
    match ip address prefix-list D1

    router ospf 1
    distribute-list route-map cisco in
```
<br/><br/><br/>


#### 결과.
<br/><br/>
![5](https://user-images.githubusercontent.com/117553252/211533927-e5ff2c3f-e9b1-4832-b397-07005485a35d.png)
<br/><br/>
![6](https://user-images.githubusercontent.com/117553252/211533928-2206c407-3901-41ce-b0cd-4738a3127701.png)
<br/>
역시 10점대가 뜨는 것을 확인할 수 있다.
<br/><br/><br/><br/>




### 정리.
<br/><br/>
![7](https://user-images.githubusercontent.com/117553252/211533929-b377b50b-6a12-4bd1-8e35-69029a3ec428.jpg)
<br/><br/>
즉, 최종적인 필터를 `루트맵`에서 한다고 생각하자.







### Ex01. ip prefix-list block2
<br/><br/>
![8](https://user-images.githubusercontent.com/117553252/211533934-4c1ed9fa-a6b3-4e24-83a0-7eaa878e51a3.png)

<br/>
조건 : RIP 쪽에 있는 것을 EIGRP로 재분배하고 싶어 하는 것임.
<br/><br/>

#### 방법.
<br/><br/>
```yaml
R3)
    ip prefix-list block1 deny 192.168.10.0/24
    ip prefix-list block1 deny 192.168.20.0/24
    ip prefix-list block1 permit 0.0.0.0/0 le 32

    route-map map1 permit 10
    match ip address prefix-list block1

    router eigrp 100
    network 1.0.0.0
    no auto-summary
    redistribute rip route-map map1 metric 1 1 1 1 1
```
<br/><br/><br/>


#### 결과.
<br/><br/>
R2에서 192.168.10 점대랑 192.168.20점대랑 안 넘어오면 됨.
<br/><br/>
![9](https://user-images.githubusercontent.com/117553252/211533938-6fc01330-6261-4532-a163-df194f2b6f0e.png)
<br/>
30 점대만 넘어오는 것을 확인할 수 있음.
<br/><br/><br/><br/>



### Ex02. ip prefix-list block2
<br/><br/>
![10](https://user-images.githubusercontent.com/117553252/211533940-469a6942-22d5-42d7-8da7-f997fed568b0.png)
<br/>
조건 : R3에서 192.168.11.1 만 넘어가고 나머지는 전부 거부.
<br/><br/><br/>


#### 방법.
<br/><br/>
```yaml
R3)
    ip prefix-list block2 permit 192.168.11.0/24

    route-map map2 permit 10
    match ip addres prefix-list block2
    set metric 5 (속성 값을 수정할 때 metric을 5로 넘기겠다는 뜻.)

    route-map map2 permit 20

    router rip
    redistribute eigrp 100 metric 1 route-map map2
```
<br/><br/><br/>


#### 결과.
<br/><br/>
`set metric 5`, `route-map map2 permit 20` 하기 전 결과<br/><br/>
![11](https://user-images.githubusercontent.com/117553252/211533944-fac678d0-27cf-418a-8f60-1f82a99c9c10.png)
<br/><br/><br/>
첨부 후 <br/><br/>![12](https://user-images.githubusercontent.com/117553252/211533948-6e49e694-26df-4c53-b53b-c5e030c53ff3.png)
<br/><br/><br/>

<br/><br/>


### ADD_TAG
<br/><br/><br/>
![13](https://user-images.githubusercontent.com/117553252/211533952-bf27dbfc-c1df-44d1-9ec2-81adc3ba81bb.png)
<br/><br/>
TAG 번호를 사용하여 필터링하는 작업.
<br/><br/><br/>

#### 방법.
<br/><br/>
```yaml
R2)
    route-map ADD_TAG
    set tag 1

    route eigrp 100
    redistribute ospf 1 route-map ADD_TAG metric 1544 2000 255 1 1500
```
<br/><br/>
R3에서 `# show ip eigrp topology 11.11.11.0/24` 한 결과,<br/><br/>
![14](https://user-images.githubusercontent.com/117553252/211533954-a0700535-dc5d-412d-865a-2a81e4cf244d.png)
<br/><br/>

```yaml
R3)
    route-map TAG_ONLY deny 10
    match tag 1
    
    route-map TAG_ONLY permit 20

    router rip
    redistribute eigrp 100 metric 2 route-map TAG_ONLY
```
<br/><br/><br/>


#### 결과.
<br/><br/><br/>
R4) # show ip route <br/><br/>
![15](https://user-images.githubusercontent.com/117553252/211533957-91deb21a-0170-4456-92db-035854241058.png)
<br/><br/>
11.11.11.0/24 (X)<br/>
1.1.1.0/24 (X)<br/>
1.1.12.0/24 (X) <- Wild Card를 넣었을 경우 : EIGRP <br/> 
![16](https://user-images.githubusercontent.com/117553252/211533962-344d35e0-b037-4082-adf5-651645a78118.png)
<br/><br/><br/>



### prefix Block1 out
<br/><br/>
![17](https://user-images.githubusercontent.com/117553252/211533966-5a2623d2-2cf1-4671-9c38-5220d2b16daa.png)
<br/><br/><br/>

#### 방법.
<br/><br/>
```yaml
R3)
    ip prefix-list Block1 seq 5 deny 11.11.11.0/24
    ip prefix-list Block1 seq 10 permit 0.0.0.0/0 le 32

    router eigrp 100
    network 1.1.23.0 0.0.0.255
    no auto-summary

    router rip
    version 2
    network 1.0.0.0
    no auto-summary
    distribute-list prefix Block1 out eigrp 100
```
<br/><br/><br/>

#### 결과.
<br/><br/>
R4)<br/>
![18](https://user-images.githubusercontent.com/117553252/211533968-0f2046c6-d950-4f95-b355-af8a9abdb9eb.png)
<br/>
11.11.11.0/24 (X)
<br/><br/><br/>

#### 추가 명령어.
<br/><br/>
```yaml
R3)
    router rip
    redistribute eigrp 100 metric 1
```
<br/><br/><br/>


#### 결과.
<br/><br/>
![19](https://user-images.githubusercontent.com/117553252/211533972-dde4233c-355e-4324-93d8-fc8f10fdd13d.png)
<br/><br/><br/><br/><br/>








### Ex. PBR (ip next-hop)
<br/><br/>
![20](https://user-images.githubusercontent.com/117553252/211533976-df6d7e68-fb7c-477d-8a6b-63ff8924156b.png)
<br/><br/>

#### 결과.
<br/><br/>
![21](https://user-images.githubusercontent.com/117553252/211533977-c4d851cd-5ac1-4a3c-b986-8ae01e798422.png)
<br/><br/>
아무것도 설정을 하지 않으면 1.1.32.3으로 자동으로 간다.
<br/><br/>

#### 방법.
<br/>
![22](https://user-images.githubusercontent.com/117553252/211533983-43656bc5-0518-4b47-a419-711848919c95.png)
<br/>
다 밑으로 가는데 우리는 위로 (serial)을 통해 가도록 만들고 싶은 것임.
<br/><br/>
```yaml
R2)
    ip access-list extended R14
    permit ip host 1.1.12.1 host 1.1.4.4

    route-map PBR14
    match ip address R14
    set ip next-hop 1.1.23.3 (위의 조건에 맞는 경우에만 1.1.23.3으로 보내라.)
    exit

    int s1/0
    ip policy route-map PBR14
```
<br/><br/><br/>

#### 결과.
<br/>
라우팅 테이블에는 변화가 없다.
<br/><br/>
![23](https://user-images.githubusercontent.com/117553252/211533987-ad50e861-c468-4384-9843-2315d50ab1df.png)
<br/><br/>
1.1.23.3으로 간 것을 확인할 수 있다.
<br/><br/><br/><br/><br/>


### PBR(Policy Based Routing)
<br/><br/><br/>
- PBR 이란 루트맵을 이용하여 특정 조건에 해당되는 패킷을 라우팅 테이블과 상관없이 관리자가 원하는 곳으로 전송시키는 기능을 말한다.
<br/><br/>
- 루트맵에서 정의하는 범주에 속하지 않는 패킷은 라우팅 테이블에 따라 전송된다.
<br/><br/>
- `(config-route-map)# set ?`
    : default interface - 라우팅 테이블에 없는 패킷을 `전송할 인터페이스`를 지정함.
    : ip default next-hop - 라우팅 테이블에 없는 패킷을 `전송할 넥스트 홉 IP 주소`를 지정함.
    : interface - 목적지가 match 명령어에 의해서 지정된 패킷을 `전송할 출력 인터페이스`를 지정함.
    : ip next-hop - 목적지가 match 명령어에 의해서 지정된 패킷을 `전송할 넥스트 홉 IP 주소`를 지정함.
<br/><br/><br/>








