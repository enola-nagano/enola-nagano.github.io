---
title: Day 07
keywords: ubuntu client
summary: "Ubuntu Client Terminal"
sidebar: systemoperation_sidebar
permalink: ubuntu_07.html
folder: product1
---

# md

# DHCP

- Host 장치에서 사용할 네트워크 정보(IP, Gateway, DNS etc)를 자동으로 설정하고 관리하기 위해 사용.
    - IP : 범위 할당 (pool)
        - ex. 192.168.1.0/24 일 경우, range는 최대 범위 : 192.168.1.1 - 192.168.1.254
        - 0, 255 - 사용 불가
            - 0 : Network 주소로 예약 되어 있음.
            - 255 : Broadcast 주소로 예약 되어 있음.
    - Gateway : 주소를 예약해 주어야 함. (IP, DNS 주소와 동일하면 X)
        - DHCP 범위에서 제외해 주면 됨.
    - DNS : 주소를 예약해 주어야 함. (IP, Gateway 주소와 동일하면 X)
- 기본적으로 동일한 네트워크 영역에 DHCP 서버가 있어야 함.
    - DHCP가 초기에는 브로드캐스트 통신을 사용하기 때문.
    
    ![Untitled](md%209c6f21e790844592a6450d9837621f45/Untitled.png)
    
    - 구성에 따라서는 다른 네트워크 대역에 있는 DHCP를 요청할 수도 있음.

# DNS

- 도메인 주소를 IP 주소로 변환하여 사용자들이  Host 장치에 연결하기 위한 IP 주소를 사용하지 않고 읽을 수 있는 형태의 문자로 연결할 수 있게 만들기 위해 사용.
- 공공 네트워크에서 사용할 때에는 도메인 주소를 별도로 구입을 해야 함.
- 사설 네트워크에서 사용할 때에는 임의의 도메인 주소를 사용할 수 있음.
- 도메인 주소와 IP 주소를 mapping한 정보를 A 레코드라고 하며, 이 외에도 CNAME, MX, PTR, TXT, AAAA, NS etc 의 레코드가 있음.
    - CNAME : 별칭.
- Port Number : 53.
    - TCP/UDP를 사용.
        - TCP : Server - Server 간.
        - UDP : Client - Server 간.
- 하나의 도메인 주소에 여러 개 웹 서버를 둘 수 있음.

![Untitled](md%209c6f21e790844592a6450d9837621f45/Untitled%201.png)

- DNS가 되지 않을 경우,
    1. DNS 주소 체크
    2. Port 번호 체크 (방화벽 체크)
    3. 사설 DNS에 레코드 정보가 올바르게 설정이 되어 있는지 체크
    
    +) zone 영역 확인, 설정 파일 확인 etc. 
    

# Q. 도식표 / Windows로 Test

## Google 공유 폴더에 만들어진 구성도 이미지를 넣어주세요. 
- 위치 : 활동하기/[20240102]사설네트워크 구성도
- 제목 : 이름.png 또는 이름.svg

[draw.io (diagrams.net)](https://app.diagrams.net/)

- 조건

```powershell
Network     172.16.0.0/16

SubNetwork1 172.16.10.0/24
Gateway     172.16.10.254
DHCP        172.16.10.253

SubNetwork2 172.16.20.0/24
Gateway     172.16.20.254
DHCP        172.16.20.253

WebServer   172.16.10.10 domain.com www.domain.com
            172.16.10.20 shop.domain.com

DNS         172.16.xxx.xxx
```

- Test : Windows로 할 것.
1. DHCP, DNS, WebServer 의 주소는 식별 가능해야 함.
2. 네트워크 영역은 식별 가능해야 함.

## 참고

컴퓨터가 외부로 나갈 때 일반적인 순서는 다음과 같습니다:

1. **DNS 조회:** 컴퓨터는 웹 주소(도메인 이름)를 IP 주소로 변환하기 위해 DNS 서버에 조회를 합니다. DNS 서버는 도메인 이름에 해당하는 IP 주소를 찾아 컴퓨터에게 반환합니다.
2. **게이트웨이 통과:** 컴퓨터는 DNS 조회 결과를 기반으로 목적지의 IP 주소로 데이터를 전송하기 위해 로컬 네트워크의 게이트웨이를 통과합니다.
3. **외부 네트워크 진입:** 게이트웨이는 목적지 IP 주소를 확인하고 외부 네트워크(인터넷)으로 패킷을 전송합니다.
4. **웹 서버 도착:** 패킷은 인터넷을 통해 목적지 IP 주소에 도달하며, 해당 IP 주소는 웹 서버를 나타냅니다.
5. **웹 서버 응답:** 웹 서버는 요청된 페이지나 리소스에 대한 응답을 생성하고, 이 응답은 다시 컴퓨터를 향해 돌아갑니다.

요약하면, 컴퓨터가 외부로 나갈 때 먼저 DNS 서버에 도착하여 도메인 이름을 IP 주소로 해석하고, 그 다음에 게이트웨이를 통해 외부 네트워크로 나가게 됩니다. 최종적으로는 해당 IP 주소를 가진 웹 서버에 도착하여 웹 서버로부터 응답을 받습니다.

## 초안

![Untitled](md%209c6f21e790844592a6450d9837621f45/Untitled%202.png)

## 피드백 후

![Untitled](md%209c6f21e790844592a6450d9837621f45/Untitled%203.png)

- 기기끼리 연결되어있다는 개념보다는 같은 대역에 있기 때문에 물리적, 논리적으로 같을 경우 통신이 다 된다고 전제함.
- 영역별로 연결되어지는 개념이라고 보면 됨.
- 스위치는 굳이 넣을 필요는 없지만 넣게 된다면 같은 대역끼리는 통신이 가능함.
- 항상 네트워크 영역을 나눌 때 큰 영역을 나누어서 하기 (ex. VPC/16 → /24)
    - ex. 172.16.0.0/16을 나눌 때 172.16.0.1/16으로 굳이 또 나누지 말 것.
    - 나누고 싶다면 Subnetting을 통해 나눌 것.

## 참고

![Untitled](md%209c6f21e790844592a6450d9837621f45/Untitled%204.png)

# VM 구성

## UbuntuGW - 일단 DNS : 172.168.10.3/24

![Untitled](md%209c6f21e790844592a6450d9837621f45/Untitled%205.png)

![Untitled](md%209c6f21e790844592a6450d9837621f45/Untitled%206.png)

- 첫 대역대는 수동으로 해준 후 다시 자동으로 설정해야 먹힘.
    
    → 기본적으로 동일한 네트워크 영역에 DHCP 서버가 있어야 함
    (DHCP가 초기에는 브로드캐스트 통신1~254을 사용하기 때문)
    

# 강사님 추가

- sudo vi /etc/bind/named.conf.options
    
    ```powershell
    liste-on {172.16.10.252; };
    ```
    
    - 내가 특정 하는 ip에 mapping 하도록 할 것이라는 의미.
    - 물론 이 주소(172.16.10.252)가 interface에 실제로 사용이 되어야 함.

- ss -tuna | grep “:53”
    - 172.16.20.254:53
    - 172.16.10.254:53
    - 모든 DNS가 53으로 활성화 되어 있는 상태

- sudo systemctl restart named
- ss -tuna | grep “:53”
    - 172.16.10.252:53
    - 주소를 지정하면 이렇게 172.16.10.252만 사용하게 설정할 수 있음.

# #1. 구성도에 맞추어 IP 주소 변경이 완료 되었으면.
- Windows Server 2022 `Desktop 버전 (GUI)` 으로 설치 해 주세요.
- Windows Server의 IP 주소는 `172.16.20.250` 으로 사용하고 DNS, GW는 기존에 구성한 DNS서버와 GW로 설정 해 주세요.
- Memory는 `4G (4096MB)` 를 사용해야 합니다.
- (모든 가상 머신이 동작하는 경우 부족할 수 있으니 `Ubuntu Desk는 종료` 후 진행하세요.
- 무인 설치 건너뛰기.
- Custom 으로 설치.
- Standard Desktop 2번째 것 버전으로.

![Untitled](md%209c6f21e790844592a6450d9837621f45/Untitled%207.png)

![Untitled](md%209c6f21e790844592a6450d9837621f45/Untitled%208.png)

![Untitled](md%209c6f21e790844592a6450d9837621f45/Untitled%209.png)

![Untitled](md%209c6f21e790844592a6450d9837621f45/Untitled%2010.png)

![Untitled](md%209c6f21e790844592a6450d9837621f45/Untitled%2011.png)

![Untitled](md%209c6f21e790844592a6450d9837621f45/Untitled%2012.png)

- Custom 선택

![Untitled](md%209c6f21e790844592a6450d9837621f45/Untitled%2013.png)

![Untitled](md%209c6f21e790844592a6450d9837621f45/Untitled%2014.png)

- Administrator / P@ssw0rd

![Untitled](md%209c6f21e790844592a6450d9837621f45/Untitled%2015.png)

- 설치 완.

![Untitled](md%209c6f21e790844592a6450d9837621f45/Untitled%2016.png)

- 입력 > 키보드 > Ctrl + Alt + Del

![Untitled](md%209c6f21e790844592a6450d9837621f45/Untitled%2017.png)

- CD 이미지도 삽입해줄 것.
