---
title: Day 06
keywords: ubuntu client
summary: "Ubuntu Client Terminal"
sidebar: systemoperation_sidebar
permalink: ubuntu_06.html
folder: product1
---

# md

## 5일차 계속

- GW는 꼭 확인해 볼 것. (NAT 때문에)
    - sudo iptables -t nat -i
    - 위가 없을 경우 sudo iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE

# DNS Server - 192.168.56.53

- 기본적으로 hosts 파일을 사용
    - Linux) /etc/hosts

![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled.png)

- Domain Address = EndPoint Address

## W

- hosts파일에 추가한 192.168.x.x [home-page.com](http://home-page.com) 삭제
    
    → ip 주소만으로 접속 가능
    
    ![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled%201.png)
    

## UbuntuGW - UbuntuServer에 DNS 설정

- sudo apt-get install bind9
    - bind9 패키지
    - 서비스 명칭 : named
- sudo systemctl status named
    - active
    
    ![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled%202.png)
    
- sudo vi /etc/bind/named.conf.local
    - 제일 마지막 줄에 추가)
    
    ```powershell
    zone "home-page.com" {
    			type primary;
    			file "/etc/bind/zones/db.home-page.com";
    };
    ```
    
    - 실제로 /etc/bind에 zones가 없음 → 만들면 됨
    - sudo mkdir /etc/bind/zones
    - ls -la /etc/bind
    
    ![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled%203.png)
    
    - zones가 존재해야 함
    
    ![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled%204.png)
    
- sudo cp /etc/bind/db.local /etc/bind/zones/db.home-page.com
    - db.local이라는 sample file
- sudo vi /etc/bind/zones/db.home-page.com

```powershell
	@    IN    SOA   [DNS.]    [admin@mail address.]->@는 .으로 인식
               home-page.com.  admin.home-page.com.

------------------------------------------------------------------------
	@    IN    NS    [localhost.] -> 레코드 정보
       [NameServer]   ns1.home-page.com.  -> NS 도메인은 ns1.home-page.com.이다 라는 것을 알려줌
ns1.home-page.com.   IN   A   192.168.56.53  -> A : Address 레코드 / NS로 질의를 하면 ip 주소를 알려줌

---------------------------------------------------------
ns1 이라고 작성해도 됨

@   IN   A   192.168.56.10   -> @ = 자기자신
www   IN   A 192.168.56.10
```

- Serial : 시리얼
- Refresh : 초단위
- Retry : 갱신
- Expire : 만료

![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled%205.png)

![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled%206.png)

- sudo vi /etc/bind/named.conf.options

```powershell
dnssec-validation auto:
listen-on { any; };
recursion yes;
forwarders {
				8.8.8.8;
				8.8.4.4;
};
listen-on-v6 { any; };
};
```

- listen-on : DNS Server에게 질의 요청할 때 청취 상태를 만들어 줌
    - DNS Server Port = 53
- recursion : 재귀 기능 활성화
    - ex. [www.naver.co.kr](http://www.naver.co.kr) 이 DNS를 어떻게 찾는가?
    - root DNS Server / kr DNS Server / [co.kr](http://co.kr) DNS Server / [naver.co.kr](http://naver.co.kr) DNS Server
    - 재귀 활성화 ON → kr 뒤에 .이 있음 . = root
    - . → root DNS → kr DNS → [co.kr](http://co.kr) → co.kr DNS → [naver.co.kr](http://naver.co.kr) → naver.co.kr DNS → [www.naver.co.kr](http://www.naver.co.kr)
        - 이런식으로 자꾸 호출시켜 줌
        - 이 기능을 활성화 하겠다는 뜻
    - 재귀에서 처리할 수 있는 범위를 벗어났다 / 질의를 하는 구분에 있어서 실패를 했다
        
        → 외부 DNS에게 요청할 수 있게 만들어 줄 수 있음.
        
    - 내부 사설망 크게 만들 때 재귀 기능 활성화 시킬 수 있음.
- forwarders : 공개된 망에 DNS 요청
- listen-on-v6 : version 6
    - 지금은 지워도 됨
    - 앞에 // 추가 하면 주석 처리 됨

![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled%207.png)

- sudo vi /etc/netplan/~
    
    ```powershell
    enp0s8:
      dhcp4: true
      addresses: [192.168.56.53/24]
    ```
    

![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled%208.png)

- sudo netplan apply
    - ip addr → 53번 추가 확인

![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled%209.png)

**~~→ dhcp4: true일 경우 ip가 자동으로 풀리는지 확인 해 보기~~**

- bind 설정 정상적으로 추가 됐는지 확인
    - named-checkconf
    - error → sudo vi /etc/bind/named.conf.options 에서 다시 확인
    - 아무것도 안 뜸 → 정상

- sudo systemctl restart named
- sudo systemctl status named
    - active (running)인지 확인
    
    ![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled%2010.png)
    

- nslookup [home-page.com](http://home-page.com) 192.168.56.53
    - test 하는 것임
    - 결과가 나와야 함.
    - 잘 응답하는 지 확인.
- nslookup [DNS] [DNS주소]
- nslookup [www.home-page.com](http://www.home-page.com) 192.168.56.53
    - 구성 후 자체 테스트

![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled%2011.png)

![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled%2012.png)

# Q. UbuntuDesk, Windows 10에 오늘 구성한 사설 DNS를 사용할 수 있도록 등록 후 전날 구성한 웹 서버의 정적 페이지가 서비스 될 수 있도록 하기

- Default GW를 192.168.56.53으로 설정해야 함

![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled%2013.png)

# Q. UbuntuGW에 DHCP 서버 기능을 추가 설치 및 구성하여 일반 클라이언트 PC에 자동으로 IP 주소, GW 주소, DNS 주소 정보가 설정 될 수 있도록 하기

# (VirtualBOX의 DHCP는 비활성화 하여 진행)

![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled%2014.png)

# Q. 현재 웹 서버로 동작하고 있는 UbuntuDesk에 [main.domain.com](http://main.domain.com) 형식의 도메인 주소 요청에 대해 html&css-training 폴더에 있는 정적 페이지가 서비스 될 수 있도록 하여, 추가로 [shop.domain.com](http://sho.domain.com) 형식의 도메인 주소 요청에 대해서는 room-homepage-master 폴더에 있는 정적 페이지가 서비스 될 수 있도록 하기

# (필요한 경우 서버를 1개 더 추가하여 구성해도 됨)

# 참고

![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled%2015.png)

# 브라우저에 도메인 주소를 입력하여 특정 웹 사이트에 접근을 하는 모든 과정(흐름)을 도식화 후 설명하기

![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled%2016.png)

![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled%2017.png)

1. 사용자가 브라우저에 도메인을 입력
2. 로컬 DNS 캐시에 도메인에 대한 주소를 확인
3. DNS 캐시에 없을 경우, 설정된 DNS 서버에 질의
4. DNS 서버가 도메인의 IP 주소를 찾아 응답
5. 사용자의 컴퓨터는 응답받은 IP주소를 사용하여 웹 서버에 접속 (3-way handshake)
6. 클라이언트가 웹 서버로 HTTP 요청 메시지 전송
7. 웹 서버가 요청 처리 후 웹 브라우저로 HTTP 응답 메시지 전송
8. 웹 브라우저가 전송 받은 콘텐츠 렌더링

![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled%2018.png)

DNS 서버는 Root, 최상위 레벨 도메인(Top Level Domain), 제 2레벨 도메인(Second Level Domain),

제 3레벨 또는 서브 도메인(Third Level/Sub Domain)등으로 구성되어있습니다.

DNS cache에는 방금 받아온 도메인의 IP 주소를 다시 재 방문할 때, 복잡한 과정을 재 반복하는 것은

비효율적이고, 이 Cache 안에는 자주 사용하는 Domain Name 주소를 저장해 놓습니다.

![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled%2019.png)

## 도식화

![Untitled](md%2021415fd03d2f4fd88d31fa2157ab964e/Untitled%2020.png)
