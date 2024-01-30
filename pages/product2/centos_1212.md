---
title: 1212
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_1212.html
folder: product2
---


### Ex.01 B Class / Host 기준 / Subnetting
<br/><br/><br/>


![1](https://user-images.githubusercontent.com/117553252/211529864-e9081b84-844e-4957-bd55-f18b60fe9132.png)
<br/><br/>

```yaml

조건) Host 기준으로 사진을 참고하여 Subnetting하여 PC끼리 통신되도록.

```
<br/><br/><br/>

191.255.0.0/16<br/>
255.255.11/000000.0<br/>
Subnet Mask=255.255.192.0<br/>
<br/><br/><br/>

1. 1020개<br/>
2^10=1024<br/>
255.255.111111/00.0<br/>
Subnet Mask = 255.255.252.0<br/>
191.255.0.0 - 191.255.3.255<br/>
1111 11/00 . 0000 0000<br/>
0000 00/00 . 0000 0000<br/>
        01<br/>
        10<br/>
        11 . 1111 1111<br/>
<br/><br/>

1. 510개<br/>
2^9=512<br/>
255.255.1111111/0.0<br/>
Subnet Mask = 255.255.254.0<br/>
191.255.4.0 - 191.255.5.255<br/>
<br/><br/>

1. 30개<br/>
2^5=32<br/>
255.255.255.111/00000<br/>
Subnet Mask = 255.255.255.224<br/>
191.255.6.0 - 191.255.6.31<br/>
<br/><br/>

1. 10개<br/>
2^4=16<br/>
255.255.255.1111/0000<br/>
Subnet Mask = 255.255.255.240<br/>
191.255.6.32 - 191.255.6.47<br/>
<br/><br/>


결론.
<br/>
![2](https://user-images.githubusercontent.com/117553252/211529867-949428a6-5201-464f-8c0e-88ce2333e8f0.png)

<br/><br/><br/>

### Ex 02. Subnetting을 Subnetting 하기

<br/><br/>
![3](https://user-images.githubusercontent.com/117553252/211529868-335c047e-e7e2-48f3-8247-0bf2a0d8563c.png)
<br/><br/><br/>


192.168.10.0/24<br/>
255.255.255.11/000000<br/>
Subnet Mask=255.255.255.192<br/>
<br/><br/>

1. 60개<br/>
2^6=64<br/>
255.255.255.11/000000<br/>
Subnet Mask = 255.255.255.192<br/>
192.168.10.0 - 192.168.10.63<br/>
<br/><br/>

1. 30개<br/>
2^5=32<br/>
255.255.255.111/00000<br/>
Subnet Mask = 255.255.255.224<br/>
192.168.10.64 - 192.168.10.95<br/>
<br/><br/>

1. 6개<br/>
2^3=8<br/>
255.255.255.11111/000<br/>
Subnet Mask = 255.255.255.248<br/>
192.168.10.96 - 192.168.10.103<br/>
<br/><br/>

1. 2개<br/>
2^2=4<br/>
255.255.255.111111/00<br/>
Subnet Mask = 255.255.255.252<br/>
192.168.10.104 - 192.168.10.107<br/>
<br/><br/><br/>
================================
<br/>
1-1. 60개를 4구역으로 나누기.
<br/>
2^2=4<br/>
255.255.255.11/11/0000<br/>
Subnet Mask = 255.255.255.240<br/>
192.168.10.0 - 192.168.10.63<br/>
<br/>
<br/>
[ 1 Subnet ]<br/>
192.168.10.0 - 192.168.10.15<br/>
<br/>
<br/>
[ 2 Subnet ]<br/>
192.168.10.16 - 192.168.10.31<br/>
<br/>
<br/>
[ 3 Subnet ]<br/>
192.168.10.32 - 192.168.10.47<br/>
<br/>
<br/>
[ 4 Subnet ]<br/>
192.168.10.48 - 192.168.10.63<br/>
<br/>

- 결과
![4](https://user-images.githubusercontent.com/117553252/211529860-32264c82-6ce8-49b5-8695-f35689e30a31.png)
<br/><br/><br/>





### Supernetting
<br/>

```yaml
192.168.10.1/24 → 255.255.255.0
192.168.10.1/16 → 192.168.0.0
192.168.10.1/8 →192.0.0.0
```
<br/><br/><br/>

- Supernetting Ex.
<br/><br/>

```yaml
다음을 Supernetting 해 보자.

192.168.1(00000 001).0
192.168.2(00000 010).0
192.168.3(00000 011).0
192.168.4(00000 100).0
192.168.5(00000 101).0
==========================
255.255.11111 000.0 (축약한 네트워크 주소)


축약된 Subnet MAsk = 255.255.248.0
축약된 Network 주소 = 192.168.0.0
축약된 Broadcast 주소 = 192.168.7.255
축약된 것 중 할당 가능한 주소 영역 = 192.168.0.1 - 192.168.7.254
축약된 주소 개수 = 2^11 - 2
```
축약의 결론 : 같으면 1 , 다르면 0

<br/><br/><br/>


### 연습 1
<br/><br/>
- interface fa0/0<br/>
ip address 211.175.185.1 255.255.255.0<br/>
ip address 211.175.186.1 255.255.255.0 secondary (두 번째부터는 secondary 필수)<br/>
ip address 211.175.186.1 255.255.255.0 secondary<br/><br/>

이렇게 하면 주소가 3개가 됨.<br/>
라우터 인터페이스에 주소를 여러 개 줄 때, 두 번째부터는 꼭 `secondary`를 써야 함. 안 그러면 앞의 주소를 덮어 쓰게 됨.<br/>
지금 이 경우는 축약을 하지 않은 경우임.<br/><br/><br/>


### 연습 2
<br/><br/>
- 같은 네트워크로 맞추면 라우터가 없어도 ping이 다 감.<br/>
게이트웨이 주소도 다 같음.<br/>
단지 서브넷 마스크만 다른 것임.<br/>
<br/><br/><br/>


### Ex.
<br/><br/>
![5](https://user-images.githubusercontent.com/117553252/211530355-7356f4ee-c8fb-43a0-83d7-3b58d3e99580.png)
<br/><br/>
![6](https://user-images.githubusercontent.com/117553252/211530357-0931d66a-7597-4959-89eb-9be68abfc59c.png)
<br/><br/>
![7](https://user-images.githubusercontent.com/117553252/211530353-6eb987fe-7f97-4886-bbd5-5816694f984d.png)
<br/><br/>

R1) ip route 명령어 2개<br/>
R2) ip route 명령어 3개<br/>
R3) ip route 명령어 2개<br/>
R4) ip route 명령어 2개<br/><br/>

Serial 에서 255.255.255.0 으로 안 주면 자꾸 overlap이 걸림.<br/>

