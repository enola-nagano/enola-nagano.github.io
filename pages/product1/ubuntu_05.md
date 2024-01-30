---
title: Day 03
keywords: ubuntu client
summary: "Ubuntu Client Terminal"
sidebar: systemoperation_sidebar
permalink: ubuntu_03.html
folder: product1
---

# md

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled.png)

# Ubuntu Server 설치

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%201.png)

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%202.png)

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%203.png)

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%204.png)

# 기본 구성

- NAT 비활성화
    
    ![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%205.png)
    

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%206.png)

- 네트워크 설정

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%207.png)

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%208.png)

- VirtualBox 네트워크 추가 시 로컬 cmd에서 확인해 보면 뜸.
    - ipconfig
    - ipconfig /all

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%209.png)

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2010.png)

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2011.png)

- 나머지는 기본 설정

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2012.png)

- ubuntu / ubuntu

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2013.png)

- sudo shutdown -h now

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2014.png)

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2015.png)

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2016.png)

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2017.png)

- GW 설정

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2018.png)

Loopback address : 127.0.0.1 - 255.255.255.255

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2019.png)

- 로컬 cmd ) ssh ubuntu@127.0.0.1

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2020.png)

- 접속 완

# Ubuntu Server 실습

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2021.png)

## DHCP IP 설정 추가

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2022.png)

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2023.png)

- ip가 정상적으로 잘 받아졌다.

# Ubuntu Server는 Gateway 역할을 하기 위해 라우팅 역할을 해야 함.

- sudo vi /etc/sysctl.conf

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2024.png)

- #net.ipv4.ip_forward=1 → 주석해제

- sudo sysctl -p → 적용하는 명령어

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2025.png)

- 0 = 비활성화 / 1 = 활성화

## Windows IP 설정 변경

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2026.png)

- Default GW = Ubuntu Server dhcp ip로 설정

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2027.png)

- 통신이 잘 되는 것을 확인할 수 있다.

## Ubuntu Client IP 설정 변경

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2028.png)

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2029.png)

- 통신이 잘 되는 것을 확인할 수 있다.

## 최종 확인

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2030.png)

- Windows에서 Ubuntu Client까지 통신이 되어야 한다.

# HTTP

- HyperText Transfer Protocol
- 하이퍼텍스트는 컴퓨팅과 관련된 개념으로, 텍스트 조작을 서로 연결할 수 있는 시스템을 말하며, 이를 통해 사용자는 순차적인 아닌 관련 항목을 통해 정보에 액세스 할 수 있다.
- HTML 문서와 같은 리소스들을 가져올 수 있도록 해 주는 프로토콜
- 웹에서 이루어지는 모든 데이터 교환의 기초이며, 클라이언트-서버 프로토콜이기도 함
- 하나의 완전한 문서는 텍스트, 레이아웃 설명, 이미지, 비디오, 스크립트 등 불로온(fetched) 하위 문서들로 재구성됨.

- 클라이언트와 서버들은 (데이터스트림과 대조적으로) 개별적인 메시지 교환에 의해 통신함
- 보통 브라우저인 클라이언트에 의해 전송되는 메시지를 요청(requests)이라고 부르며, 그에 대해 서버에서 응답으로 전송되는 메시지를 응답(responses)이라고 부름
    - 응답(responses) : HTML, Image, Media

# HTTP 요청

- Method(메서드) : 보통 클라이언트가 수행하고자 하는 동작을 정의한 GET, POST 같은 동사나 OPTIONS, HEAD와 같은 명사임
    - GET : 조회 요청
    - POST : DB 추가/수정/삭제와 같은 요청
- Path : 가져오려는 리소스의 경로, 예를 들면 프로토콜(http://), 도메인, 또는 TCP 포트인 요소들을 제거한 리소스의 URL
    - 서버에 저장되어 있는 파일의 위치를 알려주는 정보
    - ex. 리눅스) ls `/home/ubuntu/.ssh`
- Version of the protocol : HTTP 포로토콜의 버전
    - 기본 : 1.1ver
- Headers : 서버에 대한 추가 정보를 전달하는 선택적 헤더들
    - Host / Accept-Language 가 있음.

# HTTP 응답

- Version of the protocol : HTTP 프로토콜의 버전
- Status code : 요청의 성공 여부와, 그 이유를 나타내는 상태코드
    - 서버에 요청을 했을 때 정상적으로 응답이 가능한지, 오류가 있는지
    - 200번대 코드 : 정상 응답
    - 400번대 코드 : 오류 응답
        - ex. 404 Page Not Found
        - 개발자가 주로 Path 정보 잘못 입력, 관리자가 서버 설치 경로를 다르게 구성했을 경우 etc.
        - 문서에 대한 위치를 제대로 지정하지 않았을 경우 발생
    - 500번대 코드 : 오류 응답
        - 주로 서버에 대한 문제가 있을 때
        - 3-Tier 구성을 할 때, 주로 발생함
        
        ![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2031.png)
        
- Status message : 아무런 영향력이 없는, 상태 코드의 짧은 설명을 나타내는 상태메시지
- Headers : 요청 헤더와 비슷한, HTTP 헤더들
    - Content-Length : 문서 길이
    - Content-Type : 문서 타입
    - 보통 Content는 HTML

# HTTP 기능

- 심플함
    - 사람이 읽을 수 있으며 간단하게 고안 되었음. 심지어 HTTP/2가 다소 복잡해졌지만 여전히 HTTP 메시지를 프레임 별로 캡출화하여 간결함을 유지하였음.
- 확장 가능함
    - HTTP/1.0에서 소개된, HTTP 헤더는 HTTP를 확장하고 실험하기 쉽게 만들어주었음.
- 상태가 없지만, 세션은 있음
    - 상태를 저장하지 않음. (Stateless). 동일한 연결 상에서 연속하여 전달된 두 개의 요청 사이에는 연결고리가 없음.
    - 쿠키는 상태가 있는 세션을 만들도록 해줌. (장바구니, 로그인 상태 유지 등)
    - ex. 세션 확인 → 이전 상태 정보 저장되었는지 확인
        - +) 쿠키 vs 세션
- HTTP와 연결
    - 메시지 손실 없이 신뢰할 수 있는 연결을 요구할 뿐임
    - 연결이 필수는 아니지만 연결 기반인 TCP 표준에 의존함
    - HTTP/3은 UDP 기반 QUIC 프로토콜을 사용함
        - +) TCP vs UDP

# 3계층 아키텍쳐 (3 Tier)

- 애플리케이션을 3개의 논리적 및 물리적 컴퓨팅 계층으로 분리하는 3계층 아키텍쳐는 기존의 클라이언트 서버 애플리케이션을 위한 주요 소프트웨어 아키텍쳐
- 3계층
    - 프레젠테이션
        - 화면 표현 및 전환 처리 = Front
    - 비지니스로직
        - 비즈니스 개념, 규칙, 흐름 제어 = Back
    - 데이터액세트
        - 데이터 처리 = DB

- 과거)
    
    → 하나의 서버에 모두 배치
    

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2032.png)

## 장점

- 논리적 및 물리적 분리
- pdf 참고…….

# 웹사이트 호출 방식

- 남은 공간에 DB Server 추가할 것 - 그림 그리기
    - Backend →질의 DB
    - B ←응답 DB

- CSR vs SSR
- 하나의 페이지로 제공 : SPA
- 여러 페이지로 제공 : MPA
- 주로 프론트 페이지 만들 때 참고함

# Apache Server

- 꼭 아파치를 쓸 필요는 없음.

- Web Server 종류
    - Apache
    - NGINX
    - IIS

# Web Server

- HTTP 요청을 받아들이는 서버

# Web Server vs. WAS

- Web Server
    - 정적 컨텐츠(HTML, Image, File, CSS, JS, Media etc)을 제공하는 서버
- WAS
    - Web Application Server
    - 동적 컨텐츠(Servlet , JSP, Spring, Python(Django, Flask) etc) 제공
    - ex. Tomcat, WSGI
    
- 웹에 저장된 파일을 그대로 사용 → 정적 컨텐츠
- 동적 컨텐츠를 사용하여 페이지를 생성 or 데이터 생성 → 동적 컨텐츠

# 서버 구성 방식

- 1 Tier = Web + WAS + DB
- 2 Tier = Web Server / WAS + DB
- 3 Tier = Web Server / WAS / DB
- DB 추가하기

# Ubuntu Client

- 현재는 Apache, NGINX (Static Page)만 가능함
- 확인 방법
    - L Client - http://Server_Ip주소
    - W - http://Server_ip주소
- HTML 문서 - 메모장) 확장자 : index.html
    - <h1>Hello</h1>

# Q. Apache, Nginx 서비스를 동작 시켜서 Ubuntu Desktop 및 Windows 10의 브라우저에 Hello 메시지가 나올 수 있도록 하기.

- HTTP의 Port 번호 체크
    - 80번
    - ss -tuna | grep ‘:80’
- HTML 문서의 위치 확인
    - /var/www/html
        - Apache의 경우 : DocumentRoot /var/www/html
- 참고
    - http://ServerIP/
        - Root를 의미
    - ex. http://ServerIP/auth/user.html 의 경우, /var/www/html/auth/user.html에 있다는 의미.

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2033.png)

- 설치를 위해 외부망으로 되어야 함.
    - Ubuntu Server) sudo iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2034.png)

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2035.png)

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2036.png)

## 방법 - Nginx

```powershell
sudo apt install nginx
sudo service nginx start
sudo systemctl status nginx
```

[Nginx 기본 명령어 (velog.io)](https://velog.io/@iamsupercool/Nginx-%EA%B8%B0%EB%B3%B8-%EB%AA%85%EB%A0%B9%EC%96%B4)

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2037.png)

## 방법 - Apache

```powershell
sudo apt-get install apche2
sudo apt install net-tools 
service apache2 status
```

[[linux] 리눅스 우분투(ubuntu) 아파치(apache) 웹서버 설치 구축(ufw 방화벽 설정) (tistory.com)](https://askforyou.tistory.com/120)

[https://nyangnyangworld.tistory.com/4](https://nyangnyangworld.tistory.com/4)

## Port 번호

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2038.png)

## HTML 문서 위치

- /var/www/html

# Ubuntu Client

- sudo apt install git -y

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2039.png)

## 오류

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2040.png)

[오류 해결 - Waiting for cache lock: Could not get lock /var/lib/dpkg/lock-frontend. (tistory.com)](https://mingyucloud.tistory.com/entry/%EC%98%A4%EB%A5%98-%ED%95%B4%EA%B2%B0-Waiting-for-cache-lock-Could-not-get-lock-varlibdpkglock-frontend)

## 오류 2

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2041.png)

- sudo apt install —reinstall liberror-perl?

- git

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2042.png)

- 제대로 설치된 것을 확인 완료.

- git clone https://github.com/C-Coretex/my-websites

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2043.png)

- ls -la my-websites

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2044.png)

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2045.png)

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2046.png)

# Q. 윈도우 VM에도 index.html 파일 올려서 열어보기

- 내가 한 방법
    - Linux clinet)
        - sudo vi /etc/ssh/sshd_config
        - #PubkeyAuthentication yes → 주석 처리
        - #AuthorizedKeysFile → 주석 처리
        - #PasswordAuthentication yes → 주석 처리
        - #AuthenticationMethods publickey,password → 주석 처리
        - sudo systemctl restart sshd
    - Windows Client cmd)
        - scp -r ubuntu@192.168.56.10:~/my-websites C:\Users\user
        
        ![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2047.png)
        
        [windows에서 linux server로 파일 보내기, linux server에서 windows로 파일 내려 받기 (tistory.com)](https://jahong.tistory.com/entry/windows%EC%97%90%EC%84%9C-linux-server%EB%A1%9C-%ED%8C%8C%EC%9D%BC-%EB%B3%B4%EB%82%B4%EA%B8%B0-linux-server%EC%97%90%EC%84%9C-windows%EB%A1%9C-%ED%8C%8C%EC%9D%BC-%EB%82%B4%EB%A0%A4-%EB%B0%9B%EA%B8%B0)
        
        - 키 인증 후 다운 받기
        
        ![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2048.png)
        
        ![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2049.png)
        
        - 다운 받아지는 중
    
    ![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2050.png)
    
    ![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2051.png)
    
    ![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2052.png)
    
    ![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2053.png)
    
    - 잘 열린다

- 강사님 방법
    - L Client) Apache Server
        - sudo cp -r my-websites/room-hompage-master html
        - ls -la html
        - sudo chowm -R ubuntu:ubuntu html/room-hompage-master
        - mv html/room-hompage-master html/room
        - ls -la html
    

# 오류

- L Clinet에 /var/www/html/index.html이 없을 경우
    - sudo apt-get install -y php
    
    ![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2054.png)
    
- 

# Apache Server

- sudo cp -r my-websites/room-homepage-master/ /var/www/html
- cd /var/www/html
- ls
    - room-homepage-master가 있어야 함
- cd room-homepage-master
- ls
    - 모든 파일이 다 존재해야 함
- sudo cp -r css design hamburger.js images index.html slidershow.js ..
- apache2 active 확인
- firefox) 192.168.x.x → 홈페이지 나와야 함
- 나머지 하나도 똑같이 하면 됨

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2055.png)

# Nginx Server

- sudo cp -r ~/my-websites/html\&css-training /var/www/html
- cd ~/my-websites/html\&css-training
- cd /var/www/html
- ls
    - html\&css-training가 있어야 함.
- cd html\&css-training
- sudo cp -r design images index.html styles.css ..
- sudo rm index.html index.nginx-debian
- cd.html
    - index.html 에 nginx파일에 대한 정보가 다 들어있기 때문
    - nginx는 가짜(?) 파일
    - sudo mv index.html index.nginx-debian.html
- nginx active 확인
- 홈페이지 확인

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2056.png)

# DNS

- W VM

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2057.png)

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2058.png)

- hosts 파일을 복사하여 c에 붙여넣기

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2059.png)

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2060.png)

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2061.png)

- 할 경우 home-page.com으로 접속 가능

→ 이것을 간단하게 만들어 보자.

![Untitled](md%20186280c01c7d4f66ac61071aff65c756/Untitled%2062.png)
