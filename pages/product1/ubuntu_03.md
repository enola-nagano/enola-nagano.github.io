---
title: Day 03
keywords: ubuntu client
summary: "Ubuntu Client Terminal"
sidebar: systemoperation_sidebar
permalink: ubuntu_03.html
folder: product1
---

# md

# 복붙

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled.png)

- 양방향으로 설정

# 화면 자동 확장

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%201.png)

- 게스트 확장 CD 이미지 삽입

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%202.png)

- [autorun.sh](http://autorun.sh) 실행시키기

## 해제

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%203.png)

- CD > Unmount

# 명령어 패턴

- 패턴 : find ./ -name *.html
    - * : 어떠한 문자열과도 매치가 됨을 의미
    - ? : 어떠한 문자와도 매치가 됨을 의미
    - [ ] : 대괄호 안의 문자와 매치가 됨을 의미
        - ex. [a-z] : a부터 z까지

# 정규 표현식

- 기본 정규 표현식
    - grep 에서 주로 사용
    - * : 선행 문자가 0번 이상 매치 됨을 의미
        - ex. a* → a, aa, aaa, etc.
    - . : 어느 문자와도 매치가 됨을 의미
    - ^ : 후행 문자로 반드시 시작되어야 매치 됨을 의미
        - ex. ^a → a, ab, a123, etc.
    - $ : 선행 문자로 반드시 종료되어야 매치 됨을 의미
        - ex. a$ → sefa, 123a, 가나다a, etc.
    - [ ] : 대괄호 안의 문자와 매치가 됨을 의미
        - ex. [abc] → a, b, c
    - [^ ] : 대괄호 안의 문자들을 제외한 문자와 매치 됨을 의미
        - ex. [^abc] → d, e, f, etc.
    - {n} : 선행 문자의 반복 회수를 의미
        - ex. a{3} → aaa
    - {n,m} : 선행 문자의 반복 회수를 의미 (범위 설정)
        - \를 써야함
        - ex. a{2,4} → aa, aaa, aaaa
- 확장 정규 표현식
    - egrep 에서 주로 사용 (grep -E 옵션을 붙여서 사용 가능)
    - + : 선행 문자가 1번 이상 매치 됨을 의미
        - ex. a+ → aa, aaa, aaaa
    - ? : 선행 문자가 0번 또는 1번 매치 됨을 의미
    - | : 논리적으로 OR를 나타내기 위해 사용
    - ( ) : 패턴에 대한 그룹화를 하여 하위 패턴을 생성하기 위해 사용

- email, phone, date, time, etc 패턴에 사용
    - 전화번호 패턴 정규 표현식 (국내 핸드폰 번호 기준)
    - 이메일 주소 패턴 정규 표현식
    - 날짜 패턴 정규 표현식 (날짜 구분 문지로 . / - 이 사용 될 수 있습니다.)
    - 시간 패턴 정규 표현식 (시간은 천분의 1초 단위도 사용 될 수 있습니다.)
- log file 분석 시 내가 원하는 것만 찾아서 분석 가능

## 정규 표현식 확인 방법

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%204.png)

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%205.png)

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%206.png)

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%207.png)

# Q. ip addr에서 ip 주소만 출력이 가능하게 정규 표현식 만들기

### 기본 정규 표현식

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%208.png)

### 확장 정규 표현식

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%209.png)

- 왜 안 맞는지 확인 필요
- ? : 있거나 없거나

## *, + 차이

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2010.png)

# regexr.com

[RegExr: Learn, Build, & Test RegEx](https://regexr.com/)

# 전화번호 / 이메일 / 날짜 / 시간

- 전화번호
    - [0-9]{3}-[0-9]{4}-[0-9]{4}
    - [0-9]{3}-([0-9]{4}-?){2}
- 이메일
    - 
- 날짜
    - [0-9]{4}[./-][0-9]{2}[./-][0-9]{2}
    - [0-9]{4}/([0-9]{2}/?){2}
- 시간
    - [0-9]{2}[:][0-9]{2}[:][0-9]{2}[:][0-9]{2} → 00:00:00 시간은 불가능
    - ([0-9]{2}:?){3,4}
    
1. **전화번호 패턴 (국내 핸드폰번호 기준)**
    1. [0-9]{3}-[0-9]{4}-[0-9]{4}
    2. [0-9]{3}-([0-9]{4}-?){2}
    3. ([0-1]{1,3}\-?){1}([0-9]{3,4}\-?){1}([0-9]{3,4})
    4. [0-9]{3}(-[0-9]{4}){2}
2. **이메일 주소 패턴**
    1. ^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+.[A-Za-z]+$
    2. [a-zA-Z0-9]+@[a-zA-Z-0-9]+\.[a-zA-Z]+
    3. ([a-z,0-9]{1,12})@([a-z]{3,10}).([a-z]{2,5})
    4. [a-zA-Z0-9]+@[a-zA-Z-0-9]+\.[a-zA-Z]+
3. **날짜 패턴 (구분 문자 : . / - )**
    1. ^[0-9]{4}[./-][0-9]{2}[./-][0-9]{2}$
    2. [0-9]{4}/([0-9]{2}/?){2}
    3. [0-9]+/[0-1][0-9]/[0-3][0-9]
    4. ([0-9]{1,4}.?\-?\-?)([0-9]{1,2}.?\-?\-?)([0-9]{1,2})
4. **시간 패턴 (천분의 1초 단위 사용 가능)**
5. ([0-9]{2}:?){3,4}
6. [0-1][0-9]:[0-5][0-9]:[0-9]{3}
7. ([01]?[0-9]|2[0-3]):([0-5][0-9])(\.[0-9]{1,3})?
8. ^([01][0-9]|2[0-3]):[0-5][0-9]:[0-5][0-9](.[0-9]{1,3})?$

# vi

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2011.png)

- :w [File ReName]! = F2
- :set nu
- :set nocompatible → 입력 모드에서 방향 키 잘 안 먹힐 때
- gg = 제일 처음으로 이동
- :!ip addr → vi에서 명령어 실행도 가능
    
    ![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2012.png)
    
    - vi [test.py](http://test.py) 에서 결과 볼 때 :!python3 test.py

- / = 검색 (정규 표현식으로도 검색 가능)

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2013.png)

- n = 다음 검색 결과로 이동
- Shift + n = 이전 검색 결과로 이동

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2014.png)

- 10시 시간대 정규 표현식으로 검색

## 모드

- 명령 모드
    - vi를 실행하면 가장 먼저 동작하는 모드
    - vi에서 제공하는 다양한 명령을 처리
    - 명령 모드에서 편집 모드, 실행 모드로 전환 가능
    - dd
    - x = 커서 지점의 문자 한 개 삭제
- 편집 모드
    - 텍스트를 편집하기 위한 모드
    - 명령 모드에서 a, i, o 키를 사용하여 전환
        - a = 커서 다음부터 입력
        - i = 커서 지점부터 입력
        - o = 다음 줄로 이동
    - Esc 키를 누르면 다시 명령 모드로 전환
- 실행 모드
    - 문서 저장, 종료, 명령어 실행, 내용 검색 등의 기능을 위한 모드
    - 명령 모드에서 : 또는 / 를 사용하여 전환
    - Esc 키를 누르면 다시 명령 모드로 전환
    - :w = 저장
    - :q = 종료
    - :wq = 저장 후 종료
    - :q! = 강제 종료
    - :! = 리눅스 명령 사용이 필요한 경우

- sudo cp /var/log/syslog mylog

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2015.png)

- sudo vi mylog

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2016.png)

- 출력을 파일로 저장

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2017.png)

# /etc/netplan

- 은 기억해두기
- sudo vi /etc/netplan/01-network-manager-all.yaml

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2018.png)

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2019.png)

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2020.png)

## 확장자

- yaml = yml

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2021.png)

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2022.png)

- 상위 / 하위

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2023.png)

- static ip address

- :!sudo netplan try → 검증하기

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2024.png)

- 주의 표기 : man netplan으로 형식 보기

- :!show ip addr show dev enp0s8

# IP Address 확인

- ip addr show dev enp0s8

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2025.png)

# Gateway 확인

- ip route

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2026.png)

# DNS 확인

- resolvectl

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2027.png)

# yaml 파일 형식

network:

  version: 2

  renderer: NetworkManger

  ethernets:

    enp0s8:

      dhcp4: no

      addresses: [192.168.100.100/24,192.168.100.110/24]

- 데이터 타입 : 숫자, 문자열, 사전, 리스트
    - key: value
        - ethernets: enp0s8, dhcp4, addresses
        
- CloudFormation, Terraform, Ansible, spring boot (properties, etc), etc yaml을 사용
- JSON ↔ YAML
    - 상호 변환 가능
    

# convert-yaml-to-json

[Transform YAML into JSON - Online YAML Tools](https://onlineyamltools.com/convert-yaml-to-json)

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2028.png)

# Windows Clinet

# Windows Client 설치

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2029.png)

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2030.png)

- user / user

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2031.png)

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2032.png)

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2033.png)

- Ubuntu와 같은 Adapter를 사용하여야 함

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2034.png)

- 윈도우의 ip 확인

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2035.png)

- 한글 추가하기

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2036.png)

- ipconfig /all

- DHCP
    - Host 장치에서 사용할 IP 주소, GW 정보, DNS 정보 등을 자동으로 설정 할 수 있도록 도와주는 프로토콜 (서버)
    - 제약사항 : 모든 클라이언트 및 서버는 동일한 네트워크에 존재해야 함

# Windows Static IP 설정

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2037.png)

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2038.png)

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2039.png)

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2040.png)

- 현재 dhcp로 받아진 이유 : Virtual Box가 그 역할을 하고 있음

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2041.png)

- DHCP 서버 > 사용함 으로 되어 있음

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2042.png)

- Virtual Box는 101번 주소부터 254번 주소까지 자동으로 할당을 해 주겠다는 의미

- 할당 된 주소 = 주소 임대 (임시 사용)
    - 임대 기간이 있음
    - 임대 기간이 만료되면 다른 새로운 주소로 재임대함.

- W, L 둘 다 어댑터가 호스트 전용 어댑터 (같은 이름)일 경우
    
    → 같은 네트워크에 있다고 말함 = 물리적으로 동일함
    
    - 물리적 말고 논리적으로도 동일해야 하는데 논리적이라는 말은 IP Address를 뜻함.
        - IP Address = Network Address / Host Address
        - Network Address가 같아야 함.
    - Network Address = CIDR, Prefix, SubnetMask에서 고정적인 부분
    - Host Address = CIDR, Prefix, SubnetMask에서 유동적인 부분

# 물리는 같으나 논리가 다른 경우, ping (X)

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2043.png)

- ping = ICMP Protocol을 사용
- W : 192.168.56.102/24
- L : 192.168.100.100/24

# 해결 방법

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2044.png)

- :!sudo netplan apply

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2045.png)

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2046.png)

# Q. Windows : 192.168.250.10/24 , Ubuntu : 192.168.250.20/24 로 IP 바꾸기

- 조건 : GW는 둘 다 입력하지 말 것.

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2047.png)

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2048.png)

- GW는 #주석처리 할 것. 그래야 apply 가능함

## 결과

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2049.png)

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2050.png)

- 이것과는 상관없이 ping이 가능하다

### 참고

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2051.png)

- 리눅스만 GW 썼을 경우에도 ping이 똑같이 가능했음.

![Untitled](md%20aa1f0357667f4f7b91d20779f3249e98/Untitled%2052.png)

Ubuntu : 192.168.100.10/24

Windows : 192.168.101.20/24

Other : 192.168.100.30/24

Ubuntu&Windows : ping O

Ubuntu&Other : ping X

Windows&Other : ping X


