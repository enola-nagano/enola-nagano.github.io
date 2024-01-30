---
title: 1230
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_1230.html
folder: product2
---


### OSPF Stub / 완전 Stub <br/>

- `Stub` : 항상 끝에 있는 라우터를 스텁 라우터라고 함.<br/>
스텁 = 끝에 있는 것.<br/><br/>

![Untitled](https://user-images.githubusercontent.com/117553252/229319213-20d41706-6fe8-42a3-b075-d23bf1e21e44.png)
<br/><br/>

#### Stub 명령어<br/>

R1)<br/>
```yaml
router ospf 1
area 1 stub
```
<br/><br/>

R2)<br/>
```yaml
router ospf 1
area 1 stub
```
<br/><br/>

#### 결과<br/>

외부경로는 모두 다 차단하고 대신 R2가 자동으로 0.0.0.0만 준다. (0.0.0.0이 자동으로 생성됨)<br/><br/>

R1)<br/>

![Untitled (1)](https://user-images.githubusercontent.com/117553252/229319177-6b17f45d-795e-46e7-a0b5-4f1baf00c4db.png)
<br/>
0.0.0.0이 자동으로 생성된 것을 확인할 수 있다.<br/>
여기서 외부 경로 = 4.4.4.4<br/>
ping은 역시 다 감.<br/><br/>

### 완전 Stub<br/>

: 모두 다 차단 한다는 뜻.<br/>
ping은 역시 다 감.<br/><br/>

#### 명령어<br/>

R1)<br/>
```yaml
router ospf 1
area 1 stub
```
<br/><br/>

R2)<br/>
```yaml
router ospf 1
area 1 stub no-summary
```
<br/><br/>

#### 결과<br/>

R1)<br/>

![Untitled (2)](https://user-images.githubusercontent.com/117553252/229319180-a09e72b1-73e9-43bd-a624-c5c982cbc635.png)
<br/><br/>

![Untitled (3)](https://user-images.githubusercontent.com/117553252/229319181-566eaaa7-4e10-4312-acb0-4a2d3f98bf26.png)
<br/><br/><br/>

### NSSA / Totally NSSA<br/>

ASBR이 있는 경우 Stub으로 만들고 싶을 때 루핑에 문제가 생겨서 만들어 놓은 것이 NSSA 그리고 Totally NSSA.<br/><br/>

#### 명령어<br/>

R2)<br/>
```yaml
lo 0
ip add 2.2.2.2 255.255.255.0
router ospf 1
redistribute connected subnet
```
<br/><br/>

R3)<br/>
```yaml
lo 1
ip add 3.3.3.3 255.255.255.0
router ospf 1
redistribute connected subnet
```
<br/><br/>

R4)<br/>
```yaml
lo 1
ip add 4.4.4.4 255.255.255.0
router ospf 1
redistribute connected subnet
```
<br/><br/>

R3)<br/>
```yaml
router ospf 1
area 4 nssa default-information-originate
```
<br/><br/>

R4)<br/>
```yaml
router ospf 1
area 4 nssa
```
<br/><br/><br/>

#### 결과<br/>

![Untitled (4)](https://user-images.githubusercontent.com/117553252/229319182-3c01732b-6332-4a10-ba7a-5ff91a0f80a5.png)
<br/><br/>

![Untitled (5)](https://user-images.githubusercontent.com/117553252/229319183-a3ad3419-1db9-4c37-8b05-d10dd41e2c41.png)
<br/><br/>

### Totally NSSA<br/>

R2)<br/>

```yaml
lo 1
ip add 2.2.2.2 255.255.255.0
router ospf 1
redistribute connected subnet
```

<br/><br/>

R3)<br/>

```yaml
lo 1
ip add 3.3.3.3 255.255.255.0
router ospf 1
redistribute connected subnet
```

<br/><br/>

R4)<br/>

```yaml
lo 1
ip add 4.4.4.4 255.255.255.0
router ospf 1
redistribute connected subnet
```

<br/><br/>

R3)<br/>

```yaml
router ospf 1
area 4 nssa no-summary
```

<br/><br/>

R4)<br/>

```yaml
router ospf 1
area 4 nssa
```

<br/><br/>


- 결과<br/>

![Untitled (6)](https://user-images.githubusercontent.com/117553252/229319185-9dd273fb-e83d-4830-9149-ac28d2f24bc9.png)
<br/><br/>

![Untitled (7)](https://user-images.githubusercontent.com/117553252/229319186-4a70ba43-e9c7-47fc-bae5-fbad25067ae8.png)
<br/><br/>


### NSSA no-redistribution<br/>

R2)<br/>

```yaml
lo 1
ip add 2.2.2.2 255.255.255.0
router ospf 1
redistribute connected subnet
```

<br/><br/>

R3)<br/>

```yaml
lo 1
ip add 3.3.3.3 255.255.255.0
router ospf 1
redistribute connected subnet
```

<br/><br/>

R4)<br/>

```yaml
lo 1
ip add 4.4.4.4 255.255.255.0
router ospf 1
redistribute connected subnet
```
<br/><br/>

R3)<br/>

```yaml
router ospf 1
area 4 nssa no-summary
area 4 nssa no-redistribution
```

<br/><br/>

R4)<br/>

```yaml
router ospf 1
area 4 nssa
```

<br/><br/>

- 결과<br/>

![Untitled (8)](https://user-images.githubusercontent.com/117553252/229319187-206c284a-898e-464e-86dd-e25e2cee6294.png)
<br/><br/>

![Untitled (9)](https://user-images.githubusercontent.com/117553252/229319190-1ab64270-7a7d-4108-b1e3-c5a4414e5266.png)
<br/><br/>

![Untitled (10)](https://user-images.githubusercontent.com/117553252/229319191-6e20813e-90d6-4596-ae39-b0b2b100f192.png)
<br/><br/>

R1)<br/>

![Untitled (11)](https://user-images.githubusercontent.com/117553252/229319192-743976f4-eabe-4093-ab1c-40069f9c5979.png)
<br/><br/>

- aut0 = 인증 설정 X<br/>
- aut1 = 텍스트 인증 O<br/>
- aut2 = MD5 인증 O<br/><br/>


### Virtual-link<br/>

- 모든 Area는 반드시 Area 0에 연결되어 있어야 한다.<br/><br/>

![Untitled (12)](https://user-images.githubusercontent.com/117553252/229319193-41639851-12cb-4730-a784-d5b18eadf59c.png)
<br/><br/>

#### 명령어<br/>

R2)<br/>

```yaml
router ospf 1
router-id 1.1.2.2
area 23 virtual-link 1.1.3.3
```

<br/><br/>

R3)<br/>

```yaml
router ospf 1
router-id 1.1.3.3
area 23 virtual-link 1.1.2.2
```

<br/><br/>

R2)<br/>

```yaml
# show ip ospf neighbor -> virtual-link가 보일 것임
```

<br/><br/>

R3)<br/>

```yaml
# show ip osp neighbor -> virtual-link가 보일 것임
```

<br/><br/>

논리적으로 R2, R3을 붙게 해 준 것.<br/><br/>


#### 결과<br/>

![Untitled (13)](https://user-images.githubusercontent.com/117553252/229319194-60e6a8e7-921e-4442-b3cf-f17b55860eae.png)
<br/><br/>

![Untitled (14)](https://user-images.githubusercontent.com/117553252/229319195-6ebb1783-6892-494c-9584-839ecf93e9fd.png)
<br/><br/>

### Ex. Virtual-link<br/>

- 모든 loopback간의 통신이 다 되어야 함.<br/>

![Untitled (15)](https://user-images.githubusercontent.com/117553252/229319197-ddbb2a68-303a-452f-9a1b-6bfda6b59488.png)
<br/><br/>

#### 방법 - Virtual-link<br/>

R2)<br/>

```yaml
router opsf 1
area 20 virtual-link 4.4.4.4
```

<br/><br/>

R4)<br/>

```yaml
router ospf 1
area 30 virtual-link 2.2.2.2
```

<br/><br/>

#### 오류<br/>

해결)<br/>

R4 - `area 20 virtual-link 2.2.2.2`<br/><br/>

#### 결과<br/>

R2)<br/>

![Untitled (16)](https://user-images.githubusercontent.com/117553252/229319198-c1d4db69-7bdb-497a-980f-127b72649b9e.png)
<br/><br/>


![Untitled (17)](https://user-images.githubusercontent.com/117553252/229319199-7ee3fc07-9cfc-4e4c-8bf7-f96328979f75.png)
<br/><br/>

![Untitled (18)](https://user-images.githubusercontent.com/117553252/229319200-6f3f12ba-9c15-47d9-83ef-8bd4762fe164.png)
<br/><br/>

### /32 -> /24<br/>

![Untitled (19)](https://user-images.githubusercontent.com/117553252/229319201-e2791b7d-143b-4a52-a9f3-531501d9e1d1.png)
<br/><br/>

#### 명령어<br/>

```yaml
ip ospf network point-to-point
```

<br/><br/>

#### 결과<br/>

![Untitled (20)](https://user-images.githubusercontent.com/117553252/229319202-7baf08df-413d-40c0-ae8c-a14925c1d036.png)
<br/><br/>

![Untitled (21)](https://user-images.githubusercontent.com/117553252/229319204-e0e99f83-2aff-400e-9ceb-1435ace0ed29.png)
<br/><br/>

![Untitled (22)](https://user-images.githubusercontent.com/117553252/229319205-d93be73a-1422-46a8-a2ed-08754af6690a.png)
<br/><br/>

![Untitled (23)](https://user-images.githubusercontent.com/117553252/229319206-b7536de0-8af5-41fa-aec8-ec1f73e707b1.png)
<br/><br/>

![Untitled (24)](https://user-images.githubusercontent.com/117553252/229319207-9c5aa20a-e1f5-4449-8d8e-beb69d7fe670.png)
<br/><br/>

![Untitled (25)](https://user-images.githubusercontent.com/117553252/229319208-82079eb4-0ed3-43c6-b389-c86a941ea420.png)
<br/><br/>


### # show ip ospf database<br/>

![Untitled (26)](https://user-images.githubusercontent.com/117553252/229319210-e55422f3-bd8a-4066-896d-bb636ffce73a.png)
<br/><br/>

- Link ID = 라우터 ID<br/>
- ADV Router = 광고 라우터<br/><br/>

- 데이터베이스는 계층적으로 되어있다.<br/>
- 데이터베이스를 Area 별로 관리한다.<br/><br/>

R2)<br/>

![Untitled (27)](https://user-images.githubusercontent.com/117553252/229319211-948b7800-c80d-40bc-b13d-75bd95e5888b.png)
<br/><br/>

R4, R5는 똑같은 AREA 속해 있어서 똑같은 속성을 가지고 있다.<br/>
