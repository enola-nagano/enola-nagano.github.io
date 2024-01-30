---
title: Day 10
keywords: ubuntu client
summary: "Ubuntu Client Terminal"
sidebar: systemoperation_sidebar
permalink: ubuntu_10.html
folder: product1
---

# md

- 기본 설정
    - W Server가 외부망이 연결이 되어 있어야 함.
    - W Server의 시각이 서울로 되어 있어야 함.

# UbuntuGW

- 메모리 줄이기

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled.png)

- NAT 정보 넣기
    - sudo iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE

# Ubuntu Client

- Static IP로 바꾸기
    
    ```powershell
    enp0s8:
      dhcp4: no
      addresses: [172.16.10.10/24]
      routes:
        - to: default
          via: 172.16.10.254
      nameservers:
        addresses: [172.16.30.30]
    ```
    

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%201.png)

- ping google.com

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%202.png)

# W Server

- AD 인증서 발급받아 HTTP로 변환되는지만 일단 확인.

- 새로운 서비스 추가.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%203.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%204.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%205.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%206.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%207.png)

- 자동으로 IIS가 추가된 것을 확인할 수 있다.

- 설치 후 추가 구성.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%208.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%209.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2010.png)

- 인증 기반 형식으로 할 것임

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2011.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2012.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2013.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2014.png)

- CA 인증기관에 대한 구성.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2015.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2016.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2017.png)

- 나머지 추가 구성.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2018.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2019.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2020.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2021.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2022.png)

# CA 구성 완.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2023.png)

- Certification Authority 메뉴가 추가된 것을 확인할 수 있음.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2024.png)

- 인증서 관련된 정보를 확인 가능.

# IIS Manager 설정.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2025.png)

- IIS = Windows 에서 제공해주는 웹 서비스.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2026.png)

- CertSrv 메뉴가 존재해야 함.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2027.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2028.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2029.png)

- Common name : 도메인 네임 - 도메인 이름이 매치가 되어야 함.
- Organization : 조직 이름
- Organizational unit : 단위

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2030.png)

- Commom name이 제일 중요함.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2031.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2032.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2033.png)

# 요청서 확인.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2034.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2035.png)

- 암호화가 되어 있음.

- 요청서 copy 후, 웹에 https://AD/certsrv 로 이동

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2036.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2037.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2038.png)

- 정상 접속이 된다.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2039.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2040.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2041.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2042.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2043.png)

- 인증서 파일이 잘 다운 받아짐.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2044.png)

- 인증서에 대한 상세 정보를 확인할 수 있음.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2045.png)

- 약자는 알아두도록 하자.

# C-Coretex git code Download

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2046.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2047.png)

- 압축까지 풀어주자.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2048.png)

- Copy

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2049.png)

- IIS의 기본 경로에 Paste

## Sites > Web Site > Add Application - 웹을 먼저 생성하자.

- 아래 사항은 잘못된 방법임. (경로 오류)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2050.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2051.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2052.png)

- roommaster가 생성된 것을 확인할 수 있다.

- 바인딩 옵션 필요.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2053.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2054.png)

### → roommaster 삭제 후 진행.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2055.png)

### 01. 웹사이트 추가.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2056.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2057.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2058.png)

- room site 웹 사이트가 생성된 것을 확인할 수 있다.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2059.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2060.png)

- SSL 등록

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2061.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2062.png)

- domain.com 이 추가된 것을 확인할 수 있다.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2063.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2064.png)

- SSL 요청 적용하기.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2065.png)

- 바인딩 하기.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2066.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2067.png)

- Forward Lookup Zones 에서 New Zone 생성.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2068.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2069.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2070.png)

- domain.com Zone이 잘 생성된 것을 확인할 수 있다.

- New Host 추가.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2071.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2072.png)

- IP Address에 AD Server Host Address를 적기.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2073.png)

- 172.16.20.250 Host가 잘 추가된 것을 확인할 수 있다.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2074.png)

- nslookup 으로 확인 해 보니 잘 된다.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2075.png)

- 웹 서버도 잘 접속 되는 것을 확인할 수 있다.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2076.png)

- 내가 등록한 인증서 등록 확인.

# 임시 방편 - 실제로 사용하면 안 됨.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2077.png)

- 쓰지 않는 Internet Explorer를 활용.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2078.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2079.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2080.png)

- 인증서를 신뢰하게 강제 등록하자.
- 컴퓨터에 강제 등록하기.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2081.png)

- Certificates 들어가면

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2082.png)

- 신뢰할 수 있는 인증서를 볼 수 있음.

- AD에 가입만 되어 있으면 AD에 들어가 있는 인증서로 바로 신뢰할 수 있는 인증서로 적용이 가능함.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2083.png)

# W Client 접속 확인.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2084.png)

- 접속이 잘 된다.

## W Client) Internet Explorer 가능하게 설정하기.

- Administrator@domain.co.kr로 로그인

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2085.png)

- 레지스트리 편집기 열기

- 컴퓨터\Hkey_local_machine\Software\Microsoft\Windows\CurrentVersion\Policies\Ext\Clsid

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2086.png)

- 데이터 값을 1 → 0

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2087.png)

- Internet Explorer가 잘 접속 된다.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2088.png)

- 인증서가 잘 등록 되어 있는지 확인.

→ 웹 접속 시 인증서 오류 말고 인증서가 잘 신뢰되어야 함.

# 나 혼자서 해 보는 추가 확인.

# W Server) 신뢰할 수 있는 인증서에도 제대로 등록되어 있지만 인증서를 신뢰할 수 없다고 뜨는 경우.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2089.png)

- 웹 주소와 인증서의 Subnet의 CN이 달라서 신뢰할 수 없는 인증이라고 뜨는 듯 함.

- 그러나 내가 발급 받았던 인증서를 보면

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2090.png)

- 웹에서 등록된 인증서의 CN과 발급받은 인증서의 CN이 다르다는 것을 확인할 수 있음.
    - 웹 CN : domain-WIN-CA
    - 발급 CN : domain.com

- 이걸 해결하기 위해서는

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2091.png)

- SSL certificate를 domain.com으로 바꿈.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2092.png)

- 그러니 정상적으로 인증되었다.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2093.png)

- 웹의 인증서의 Subject CN이 정상적으로 domain.com 으로 변경이 되었다.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2094.png)

- 신뢰할 수 있는 루트 인증 기관을 등록하기 위해 AD를 한 것.

# W Server (W Client Pro에서도 가능)

- Internet Explorer에서 신뢰할 수 있는 인증서라 뜨더라도 최근 Browser에는 추가적인 인증이 필요함.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2095.png)

- 즉 요즘에는 `주체 대체 이름`이 추가적으로 필요함.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2096.png)

- 인증서 발급 시 속성 설정을 통해 여기에 추가 작성을 해야 함.

# UbuntuDesk - 현재 상황

- https 되는 지 확인 → 인증서 등록하여 https 되게 만들기
- 주체 대체 이름을 어떻게 하면 인증해서 등록이 되는지 어떤 값이 주체 대체 이름이 되어야 하는지

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2097.png)

# Q 01. IIS에 인증서를 등록하여 https가 동작하게 만들었던 것처럼 UbuntuDesk에 설치한 Apache2 또는 Nginx에도 인증서를 등록하여 https가 동작하게 만들기. (셀프사인)

# 방법.

# UbuntuDesk

- sudo apt update
- sudo apt upgrade
- sudo apt-get install openssl
- openssl genrsa -des3 -out server.key 2048
    - P@ssw0rd
    - P@ssw0rd

- openssl req -new -key server.key -out server.csr

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2098.png)

- sudo openssl x509 -req -days 7300 -in server.csr -signkey server.key -out server.crt

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%2099.png)

- ls -l

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%20100.png)

[Apache SSL 인증서 생성 방법 (tistory.com)](https://soir1984.tistory.com/64)

- sudo cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/000-default-ssl.conf
- sudo cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/001-domain.com-ssl.conf
- sudo cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/002-domain.com-ssl.conf

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%20101.png)

- cd /etc/apache2/sites-available
- sudo vi 001-domain.com-ssl.conf
- sudo vi 002-shop.domain.com-ssl.conf

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%20102.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%20103.png)

```powershell
SSLEngine on
SSLCertificateFile /home/ubuntu/key/server.crt
SSLCertificateKeyFile /home/ubuntu/key/server.key
```

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%20104.png)

- sudo a2ensite 001-domain.com-ssl.conf
- sudo a2ensite 002-shop.comain-ssl.conf
- sudo a2dissite default-ssl.conf
- sudo a2dissite 000-default-ssl.conf

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%20105.png)

- netstat -naop | grep 443

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%20106.png)

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%20107.png)

[Apache SSL 적용 방법 #Feat. ubuntu 22.04 (tistory.com)](https://soir1984.tistory.com/65)

# 에러 발생.

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%20108.png)

- 오류 찾음.

/home/ubuntu/key 는 없음.

- 변경

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%20109.png)

- 아파치 재시작

- 에러 발생

- 에러 발생

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%20110.png)

# Q 02. UbuntuDesk에서 [https://domain.co.kr/certsrv](https://domain.co.kr/certsrv)에 접속하여 인증서 발급 웹 페이지가 나오게 만들고 UbuntuDesk에서 인증서 요청서 (Certificate Request)를 만들어 요청 후 인증서를 발급 받아보기.

# Q 03. Windows Server CA에 주체 대체 이름이 인증서에 포함되어 만들어 질 수 있도록 하여 Edge 및 Chrome 에서도 올바른 인증서라는 것으로 식별할 수 있게 만들기.

# W Server

- cmd / powershell
- certutil -setreg policy\EditFlags +EDITF_ATTRIBUTESUBJECTALTNAME2

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%20111.png)

- 추가된 것 확인

![Untitled](md%202f175608f7b5410f944186de0e42f1c8/Untitled%20112.png)

- CA 중지 후 재시작

- 주체 ~
    - 

- 다운 후 인증서 비교해서 보기
    - SubjectAlternative가 추가되면 됨
    - 우리는 이게 필요한 것

- 등록하면 Edge에서 신뢰할 수 있다고 뜸.

- New 인증서를 IIS에 새 등록
    - 기존에 있던 것은 지우기
- 새로 등록한 것을 room site에 다시 바인딩하기
- room site 서버 재시작

- 서버 접속 후 인증서 주체 대체 이름 확인 해볼 것
