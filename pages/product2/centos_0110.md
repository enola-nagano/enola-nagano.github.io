---
title: 0110
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_0110.html
folder: product2
---


### BGP
<br/><br/>

![1](https://user-images.githubusercontent.com/117553252/211525924-a6490ecd-254e-4921-9109-503d4108b8da.png)
<br/><br/>
BGP AS 100 = ex. SKT<br/>
BGP AS 200 = ex. LG U+<br/>
서로 간 협약(계약)을 하게 만들고 싶다.<br/><br/><br/>


#### 방법 - 명령어
<br/><br/>
```yaml
R1)
    router bgp 100
    bgr router-id 1.1.1.1
    neighbor 1.1.12.2 remote-as 100
    network 192.168.10.0 mask 255.255.255.0 <- 자신의 라우터에 해당된 PC의 정보만 넣어주면 된다는 것을 기억하자.(보통 회사 쪽에 있는 네트워크만 넣어줌.)

R2)
    router bgp 100
    bgp router-id 2.2.2.2
    neighbor 1.1.12.1 remote-as 100
    network 192.168.20.0 mask 255.255.255.0

R3)
    router bgp 200
    bgp router-id 3.3.3.3
    neighbor 3.3.12.2 remote-as 200
    network 192.168.30.0 mask 255.255.255.0

R4)
    router bgp 200
    bgp router-id 4.4.4.4
    neighbor 3.3.12.1 remote-as 200
    network 192.168.40.0 mask 255.255.255.0
```
<br/><br/><br/>

#### # show ip bgp
<br/><br/>
![2](https://user-images.githubusercontent.com/117553252/211525933-7c8a6c2a-4766-4d86-95e2-d270f4899f70.png)
<br/><br/>
![3](https://user-images.githubusercontent.com/117553252/211525938-075aabf0-0691-4372-a930-2dcb993522b5.png)
<br/><br/>
![4](https://user-images.githubusercontent.com/117553252/211525945-bec363d9-c513-4e3a-a2b1-7030aa613579.png)
<br/><br/>
![5](https://user-images.githubusercontent.com/117553252/211525947-4391e91c-7658-4329-88af-034f7461ac63.png)
<br/><br/>

#### iBGP
<br/>
: 같은 BGP 내에서 교환하는 것을 의미함.
<br/><br/>

#### eBGP
<br/>
: 다른 AS 간 교환.
<br/><br/>

#### 명령어 - 방법 1.
<br/><br/>
```yaml
R2)
    router bgp 100
    neighbor 2.2.12.2 remote-as 200

R3)
    router bgp 200
    neighbor 2.2.12.1 remote-as 100
```
<br/><br/><br/>


#### # show ip bgp
<br/><br/><br/>
![6](https://user-images.githubusercontent.com/117553252/211525951-f5a9c46c-54b2-4894-8a16-5412b1b49ab5.png)
<br/><br/>
![7](https://user-images.githubusercontent.com/117553252/211525956-76b3712f-00f8-4598-93f7-176514086072.png)
<br/><br/>
![8](https://user-images.githubusercontent.com/117553252/211525959-d507a3eb-584a-4d9c-92a9-b033141715f2.png)
<br/><br/>
![9](https://user-images.githubusercontent.com/117553252/211525963-6b1f9752-ac45-49fe-9a25-ec3c60432665.png)
<br/><br/>
![10](https://user-images.githubusercontent.com/117553252/211525967-728279c3-4083-4a3e-aded-3b8300db3d02.png)
<br/>B라고 뜬 것을 확인할 수 있음.<br/>이것을 우리는 BGP로 교환한 것을 알 수 있음.<br/><br/>
![11](https://user-images.githubusercontent.com/117553252/211525970-2d21bc8e-20c5-400d-b53c-90d4650b4f92.png)
<br/>역시 R3에서도 B라고 뜸.<br/><br/>
![12](https://user-images.githubusercontent.com/117553252/211525972-09ce15fd-fecc-4951-9ce3-9b37c9e006a1.png)
<br/>192.168.10.1 에서 아까는 안 되던 30.1과 40.1로의 ping이 됨.<br/><br/><br/><br/>



### DMZ 구간 - 2.2.12.0
<br/><br/>
![13](https://user-images.githubusercontent.com/117553252/211525973-4bcd2908-49e6-4038-8d80-02b44b6306aa.png)
<br/><br/>
2.2.12.0은 누구의 소유도 아니다.
<br/><br/><br/>

#### # show ip bgp summary
<br/>
: neighbor 확인
<br/><br/>
![14](https://user-images.githubusercontent.com/117553252/211525976-be81f970-e67a-40df-84b2-06c78923f29d.png)
<br/><br/>
![15](https://user-images.githubusercontent.com/117553252/211525980-0176ef83-c895-4032-8ba3-a655a6c0afa4.png)
<br/><br/>
![16](https://user-images.githubusercontent.com/117553252/211525984-d87b3d72-2cad-48d7-b5c5-aed0f1a7dc67.png)
<br/><br/>
![17](https://user-images.githubusercontent.com/117553252/211525985-cbd8953d-85a4-4a71-99f2-3f4521b5b3a8.png)
<br/><br/><br/><br/><br/>




### 방법 02. - 명령어 (ft. 상대 원칙)
<br/><br/><br/>
![18](https://user-images.githubusercontent.com/117553252/211525987-3c057412-85b9-4386-913a-b39ac18641c6.png)
<br/><br/>

```yaml
R1)
    ip route 3.3.3.0 255.255.255.0 1.1.12.2 <- 상대 원칙

    router bgp 100
    bgp router-id 1.1.1.1
    neighbor 2.2.2.2 remote-as 100
    neighbor 2.2.2.2 update-source lo 0
    network 192.168.10.0 mask 255.255.255.0

R2)
    ip route 3.3.3.0 255.255.255.0 2.2.12.2

    router bgp 100
    bgp router-id 2.2.2.2
    neighbor 1.1.1.1 remote-as 100
    neighbor 1.1.1.1 update-source lo 0
    neighbor 3.3.3.3 remote-as 200
    neighbor 3.3.3.3 update-source lo 0
    neighbor 3.3.3.3 ebgp-multihop 2
    network 192.168.20.0 mask 255.255.255.0

R3)
    ip route 2.2.2.0 255.255.255.0 2.2.12.1

    router bgp 200
    bgp router-id 3.3.3.3
    neighbor 4.4.4.4 remote-as 200
    neighbor 4.4.4.4 update-source lo 0
    neighbor 2.2.2.2 remote-as 100
    neighbor 2.2.2.2 update-source lo 0
    neighbor 2.2.2.2 ebgp-multihop 2
    network 192.168.30.0 mask 255.255.255.0

R4)
    ip route 2.2.2.0 255.255.255.0 3.3.12.1 <- 상대 원칙
    
    router bgp 200
    bgp router-id 4.4.4.4
    neighbor 3.3.3.3 remote-as 200
    neighbor 3.3.3.3 update-source lo 0
    network 192.168.40.0 mask 255.255.255.0
```
<br/><br/><br/>


#### 결과.
<br/><br/>
![19](https://user-images.githubusercontent.com/117553252/211525989-fe4e2f03-b2ef-4615-9b90-6a265bc97b81.png)
<br/><br/>
![20](https://user-images.githubusercontent.com/117553252/211525991-cd25cc3a-4b44-4f5e-b847-377860347279.png)
<br/><br/>
![21](https://user-images.githubusercontent.com/117553252/211525993-65a931bf-df24-4e09-831d-47b9063f937f.png)
<br/><br/>
![22](https://user-images.githubusercontent.com/117553252/211525995-ed98896e-19d7-43d9-b659-4c23fa69e3c2.png)
<br/><br/>
![23](https://user-images.githubusercontent.com/117553252/211525999-f8d38095-d794-48bb-88a2-3b6959858b87.png)
<br/><br/><br/><br/><br/>



### Ex. 유형 01
<br/><br/>
![24](https://user-images.githubusercontent.com/117553252/211526000-87e80e55-f7bb-48d5-827d-c5f12122450f.png)
<br/><br/>
물리적으로 직접 붙어있는 것으로 neighbor을 주기.
<br/><br/><br/>

#### 방법.
<br/><br/>

```yaml
R1)
    router bgp 100
    bgp router-id 1.1.1.1
    neighbor 1.1.12.2 remote-as 100
    network 192.168.10.0 mask 255.255.255.0

R2)
    router bgp 100
    bgp router-id 2.2.2.2
    neighbor 1.1.12.1 remote-as 100
    network 192.168.20.0 mask 255.255.255.0
    neighbor 2.2.12.3 remote-as 200

R3)
    router bgp 200
    bgp router-id 3.3.3.3
    neighbor 3.3.12.4 remote-as 200
    network 192.168.30.0 mask 255.255.255.0
    neighbor 2.2.12.2 remote-as 100

R4)
    router bgp 200
    bgp router-id 4.4.4.4
    neighbor 3.3.12.3 remote-as 200
    network 192.168.40.0 mask 255.255.255.0
    neighbor 4.4.12.5 remote-as 300

R5)
    router bgp 300
    bgp router-id 5.5.5.5
    neighbor 5.5.12.6 remote-as 300
    network 192.168.50.0 mask 255.255.255.0
    neighbor 4.4.12.4 remote-as 200

R6)
    router bgp 300
    bgp router-id 6.6.6.6
    neighbor 5.5.12.5 remote-as 300
    network 192.168.60.0 mask 255.255.255.0
    neighbor 6.6.12.7 remote-as 400

R7)
    router bgp 400
    bgp router-id 7.7.7.7
    neighbor 7.7.12.8 remote-as 400
    network 192.168.70.0 mask 255.255.255.0
    neighbor 6.6.12.6 remote-as 300

R8)
    router bgp 400
    bgp router-id 8.8.8.8
    neighbor 7.7.12.7 remote-as 400
    network 192.168.80.0 mask 255.255.255.0
```
<br/><br/><br/><br/>

#### 결과.

<br/><br/>

![25](https://user-images.githubusercontent.com/117553252/211526007-cba8f281-451e-4dd0-ad9a-704a1249af5d.png)
<br/>ping도 다 가면 됨.<br/><br/>
![26](https://user-images.githubusercontent.com/117553252/211526013-73274da6-704c-4709-8352-2dbe88f93fb6.png)
<br/><br/>
![27](https://user-images.githubusercontent.com/117553252/211526016-fe70b51d-5802-4c88-9706-35df85becfc5.png)
<br/><br/><br/><br/><br/>




### Ex. 유형 2(같은 구역) + 유형 1(다른 구역)
<br/><br/><br/>
![28](https://user-images.githubusercontent.com/117553252/211526020-2794ac35-7964-4715-b643-13e0109c1fdf.png)
<br/><br/><br/>


#### 방법.
<br/><br/>

```yaml
R1)
    router bgp 100
    bgp router-id 1.1.1.1
    neighbor 2.2.2.2 remote-as 100
    neighbor 2.2.2.2 update-source lo 0
    network 192.168.10.0 mask 255.255.255.0

R2)
    router bgp 100
    bgp router-id 2.2.2.2
    neighbor 1.1.1.1 remote-as 100
    neighbor 1.1.1.1 update-source lo 0
    neighbor 2.2.12.3 remote-as 200
    network 192.168.20.0 mask 255.255.255.0

R3)
    router bgp 200
    bgp router-id 3.3.3.3
    neighbor 4.4.4.4 remote-as 200
    neighbor 4.4.4.4 update-source lo 0
    neighbor 2.2.12.2 remote-as 100
    network 192.168.30.0 mask 255.255.255.0

R4)
    router bgp 200
    bgp router-id 4.4.4.4
    neighbor 4.4.12.5 remote-as 300
    neighbor 3.3.3.3 remote-as 200
    neighbor 3.3.3.3 update-source lo 0
    network 192.168.40.0 mask 255.255.255.0

R5)
    router bgp 300
    bgp router-id 5.5.5.5
    neighbor 4.4.12.4 remote-as 200
    neighbor 6.6.6.6 remote-as 300
    neighbor 6.6.6.6 update-source lo 0
    network 192.168.50.0 mask 255.255.255.0

R6)
    router bgp 300
    bgp router-id 6.6.6.6
    neighbor 6.6.12.7 remote-as 400
    neighbor 5.5.5.5 remote-as 300
    neighbor 5.5.5.5 update-source lo 0
    network 192.168.60.0 mask 255.255.255.0

R7)
    router bgp 400
    bgp router-id 7.7.7.7
    neighbor 6.6.12.6 remote-as 300
    neighbor 8.8.8.8 remote-as 400
    neighbor 8.8.8.8 update-source lo 0
    network 192.168.70.0 mask 255.255.255.0

R8)
    router bgp 400
    bgp router-id 8.8.8.8
    neighbor 7.7.7.7 remote-as 400
    neighbor 7.7.7.7 update-source lo 0
    network 192.168.80.0 mask 255.255.255.0
```

<br/><br/><br/><br/>

#### 결과.
<br/><br/>
![29](https://user-images.githubusercontent.com/117553252/211526024-598eb02e-f873-4630-934f-50d42e33c4d3.png)
<br/><br/>
![30](https://user-images.githubusercontent.com/117553252/211526028-4d0182b5-e975-4265-9a2c-8bad803b473f.png)
<br/><br/>
![31](https://user-images.githubusercontent.com/117553252/211526034-452d64ef-5c43-4422-aaf2-5a29a0edefe2.png)
<br/><br/><br/><br/>




### Ex.
<br/>
![32](https://user-images.githubusercontent.com/117553252/211774137-01c43477-5375-47da-8d85-b3a529881823.png)
<br/>

- loopback 간 neighbor를 맺으려고 하는 것임.<br/>
-> ip route 전부 2개씩. <br/>
- BGP에는 loopback만 들어가도록 할 것.
<br/><br/>

#### 방법.
<br/>

```yaml
R1)
    ip route 192.168.20.0 255.255.255.0 1.1.12.2
    ip route 192.168.30.0 255.255.255.0 1.1.12.2

    router bgp 100
    bgp router-id 1.1.1.1
    neighbor 2.2.2.2 remote-as 100
    neighbor 2.2.2.2 update-source lo 0
    network 192.168.10.0 mask 255.255.255.0

R2)
    ip route 192.168.10.0 255.255.255.0 1.1.12.1
    ip route 192.168.30.0 255.255.255.0 2.2.12.3

    router bgp 100
    bgp router-id 2.2.2.2
    neighbor 1.1.1.1 remote-as 100
    neighbor 3.3.3.3 remote-as 200
    neighbor 1.1.1.1 update-source lo 0
    neighbor 3.3.3.3 update-source lo 0
    neighbor 3.3.3.3 ebgp-multihop 2
    network 192.168.20.0 mask 255.255.255.0

R3)
    ip rotue 192.168.20.0 255.255.255.0 2.2.12.2
    ip route 192.168.40.0 255.255.255.0 3.3.12.4

    router bgp 200
    bgp router-id 3.3.3.3
    neighbor 4.4.4.4 remote-as 200
    neighbor 2.2.2.2 remote-as 100
    neighbor 4.4.4.4 update-source lo 0
    neighbor 2.2.2.2 update-source lo 0
    neighbor 2.2.2.2 ebgp-multihop 2
    network 192.168.30.0 mask 255.255.255.0

R4)
    ip route 192.168.30.0 255.255.255.0 3.3.12.3
    ip route 192.168.20.0 255.255.255.0 3.3.12.3

    router bgp 200
    bgp router-id 200
    bgp router-id 4.4.4.4
    neighbor 3.3.3.3 remote-as 200
    neighbor 3.3.3.3 update-source lo o
    network 192.168.40.0 mask 255.255.255.0

※ 참고) neighbor [neighbor를 맺으려는 루프백 주소]
```

<br/><br/><br/>


#### 결과.
<br/><br/>
![33](https://user-images.githubusercontent.com/117553252/211774148-957d6976-7653-42da-a5a6-c673102226b9.png)
<br/><br/>
![34](https://user-images.githubusercontent.com/117553252/211774149-14db5383-d878-46f0-a6c0-1ffec1a5e5f8.png)
<br/><br/>
![35](https://user-images.githubusercontent.com/117553252/211774152-609cb515-414e-4ffd-89b4-8f62d5f7cdf0.png)
