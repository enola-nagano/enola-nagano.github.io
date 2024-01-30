---
title: 1206
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_1206.html
folder: product2
---


### EVE를 이용하여 윈도우 PC 2개 연결하기

![1](https://user-images.githubusercontent.com/117553252/211124754-4bd68412-3daa-41f0-9cb6-9b3fdef97853.png)


- 라우트 인터페이스 하나가 하나의 네트워크 주소를 의미함을 주의하자.

    ```yaml
    Router의 인터페이스(e0/0, e0/1) = 1개의 Network 주소
    ```

A의 PC가 B의 PC, 즉 다른 네트워크와 통신하기 위해서 피씨는 디폴트 게이트웨이가 꼭 필요함.



### 01. EVE에서 윈도우 서버 2개랑 라우터 1개를 만든다.
![1](https://user-images.githubusercontent.com/117553252/211124754-4bd68412-3daa-41f0-9cb6-9b3fdef97853.png)

### 02. 장치들을 연결하기.

### 03. 라우터에 주소 부여하기.

### 04. 윈도우 로컬 영역 연결하기.
![6](https://user-images.githubusercontent.com/117553252/211127783-44579bf1-8184-4671-bf52-b1d4d750fb13.png)



![7](https://user-images.githubusercontent.com/117553252/211127795-a38a21d6-0480-49e8-b413-7516f2986183.png)



![8](https://user-images.githubusercontent.com/117553252/211127802-4b30b1f9-278a-4b38-9077-ee52ebc97b67.png)



![9](https://user-images.githubusercontent.com/117553252/211127813-7cecf439-9aa4-4c2d-9f52-10fa4a3e835f.png)



### 05. 연결이 잘 되었는지 ping으로 cmd에서 확인하기.
![11](https://user-images.githubusercontent.com/117553252/211127872-bfaba0d7-e5fb-4f66-92e2-2b8ba20e34c3.png)



![12](https://user-images.githubusercontent.com/117553252/211127881-b024de95-9576-4abb-9912-d0d61e1e8db5.png)




### 참고. 로컬 영역 연결을 '사용 중지' 눌렀을 경우.
![13](https://user-images.githubusercontent.com/117553252/211127830-d70388a6-7a98-4a1f-ae9c-6fa3483a7430.png)



![14](https://user-images.githubusercontent.com/117553252/211127840-1bdc9005-5a10-434a-ae6e-74f91facfeda.png)




### EVE를 이용하여 리눅스 PC 2개 연결하기

![15](https://user-images.githubusercontent.com/117553252/211128270-9009f95f-40a6-4f87-9616-15596928daa9.png)

- PC가 다른 네트워크랑 통신이 필요하면 디폴트 게이트웨이가 꼭 필요함.
(인터넷을 하기 위해서는 게이트웨이가 꼭 필요함.)

- 리눅스의 최고 관리자 : root(유닉스 계열)

- 윈도우의 최고 관리자 : administrator



### 01. EVE에서 리눅스 2개랑 라우터 1개를 만든다.
![15](https://user-images.githubusercontent.com/117553252/211128270-9009f95f-40a6-4f87-9616-15596928daa9.png)



### 02. 장치들을 연결하기.

### 03. 라우터에 주소 부여하기.

### 04. 리눅스에 주소 부여하기.

- vi 에서 bootproto=dhcp를 `수동`으로 주고 싶어서 바꾸고 싶은 것임.
    : bootproto=static

- vi 에서 `a 또는 i`를 누르면 insert로 바뀜.

- Linux는 `대소문자를 구분`함.

![16](https://user-images.githubusercontent.com/117553252/211128378-1a5fe6e5-f790-4bd9-97e9-6108d59d89d6.png)



- 위 사진과 같이 다 입력했으면 `esc`

- 저장은 `:wq`

- w : 저장, q : 끝내다


- 주소 한 번 반납하고 수동으로 준 주소를 다시 받아오자.
```yaml
ifdown eth0
ifup eth0
```


- 주소가 잘 들어갔는지 확인 `ifconfig`
- 게이트 웨이 주소 확인 `netstat -rn`

![17](https://user-images.githubusercontent.com/117553252/211128429-e58d737e-594f-43a0-ab37-810ba6e88dc4.png)







### 05. 연결이 잘 되었는지 ping으로 확인하기.

![19](https://user-images.githubusercontent.com/117553252/211128445-555bb35d-7e74-41f9-bdb2-a7427838c1c3.png)




![20](https://user-images.githubusercontent.com/117553252/211128448-b810c6d7-ffe9-4399-a2ad-b0848bbf981c.png)






### 참고. 라우터에서는 두 개 다 해줘야 연결이 됨. 근데 하나만 연결 해 줘서 오류가 발생했었음.


### 최종 정리.

![18](https://user-images.githubusercontent.com/117553252/211128470-ae938d38-cbd9-43a4-8ab8-73091a8d4f2c.png)





### EVE를 이용하여 윈도우 PC 2개, 리눅스 2개 연결하기

![21](https://user-images.githubusercontent.com/117553252/211128519-fdd9430b-e3af-4762-806f-841f22d321f5.png)



### 01. EVE에서 윈도우 서버 2개랑 라우터 1개, 리눅스 2개를 만든다.

### 02. 장치들을 연결하기.

### 03. 라우터에 주소 부여하기.

### 04. 윈도우 로컬 영역 연결하기.

### 05. 리눅스에 주소 부여하기.

### 06. 연결이 잘 되었는지 ping으로 확인하기.



### EVE를 이용하여 리눅스 5개, Switch 2개 연결하기

![22](https://user-images.githubusercontent.com/117553252/211131172-1a627c0d-156f-485c-acfb-9ea9389c5e7d.png)


- 라우터 : L3

- 스위치 : L2



### 01. EVE에서 리눅스 5개랑 스위치 2개, 라우터 1개를 만든다.

### 02. 장치들을 연결하기.

### 03. 라우터에 주소 부여하기.

### 04. 스위치는 'Enter'만 여러번 치기.

### 05. 리눅스 주소 부여하기.

### 06. 연결이 잘 되었는지 ping으로 확인하기.

### 정리.

![23](https://user-images.githubusercontent.com/117553252/211131197-31b9d090-b1a2-4f09-b724-2ecd9dbf0fb1.png)



### 참고. ip 주소랑 모두가 다 잘 작성이 되었는데도 ping이 안 되는경우

- 스위치의 문제일 수 있다. 스위치를 한 번 껐다가 키면 다시 잘 ping이 된다.




### 01. Network 기초

#### 1) Classful Address

|Class|첫번째 옥텟의 범위|Network ID의 범위|사용가능한 Network ID의 개수|사용가능한 Host ID의 개수|
|:------:|:---:|:---:|:---:|:---:|
|A Class|1 ~ 126|1.0.0.0 ~ 126.0.0.0|2^(8-1) -2 = 126|2^24 - 2 = 16,777,214|
|B Class|128 ~ 191|128.0.0.0 ~ 191.255.0.0|2^(16-2) = 16,384|2^16 - 2 = 65,534|
|C Class|192 ~ 223|192.0.0.0 ~ 223.255.255.0|2^(24-3) = 2,097,152|2^8 - 2 = 254|


- D Class : 예약된 `멀티캐스트` 주소 = 224 ~ 239

- E Class : 예약된 `연구용` 주소 = 240 ~ 255


- RFC 1918은 사설 주소 (사설 네트워크 내에서의 식별용 주소)로 사용하기 위한 세 개의 IP 주소 블럭을 설정해두었다.
    - 10.0.0.0 ~ 10.255.255.255 (10/8 prefix)
    - 172.16.0.0 ~ 172.31.255.255 (172.16/12 prefix)
    - 192.168.0.0 ~ 192.168.255.255 (192.168/16 prefix)






#### 2) Subnet Mask

- 상대방 컴퓨터가 로컬인지 원격지인지를 구별.

![24](https://user-images.githubusercontent.com/117553252/211131644-58ce43f8-f5d3-4e2d-b418-6310b4fe9e09.PNG)




#### 3) Default Gateway

- 호스트가 TCP/IP 통신을 할 때 가장 먼저 목적지 호스트가 자신과 같은 로컬에 있는지 원격지에 있는지를 판단한다고 했다. 이때 원격지에 있는 결과가 나오면 컴퓨터는 Default Gateway를 이용해서 통신을 하게 된다.





### 02. 공인 주소와 사설 주소

- 공인주소 (Public Address) : 유료
- 사설주소 (Private Address) : 무료

- 회사 내에서는 모두 사설 주소를 사용.
- 인터넷 망에는 다 공인 주소를 사용.




### 03. # show ip route 

- 라우터에서 `# show ip route` 는 해당 라우터의 지도를 보여준다. 즉, 어디로 배달할 수 있는지를 보여줌.

![25](https://user-images.githubusercontent.com/117553252/211131745-607a7d72-113f-4fc7-a33c-6b7d9ab6cd4a.png)



- 네트워크 주소가 다를 경우 자신의 Default Gateway로 무조건 보낸다.
- 라우터가 받으면 항상 지도를 참고함.
- 만약, 같이 이동하는데 자기가 찾는 목적지가 아니면 버림.




### 04. Rouing

- 지도를 관리하는 방법에는 2가지가 존재함.

1) Static Routing :  ip route 명령어
    일일이 수정하고 만드는 것, 명령어는 `ip route`명령어를 사용함.

2) Dynamic Routing : RIP, EIGRP, ISPF, BGP 
    명령어만 주면 자동으로 생성됨, 4가지 방법이 있음.

3) Router라는 장비가 하는 역할
    다른 Network 주소를 연결(★) + 지도 관리(★)



