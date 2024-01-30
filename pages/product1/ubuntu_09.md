---
title: Day 09
keywords: ubuntu client
summary: "Ubuntu Client Terminal"
sidebar: systemoperation_sidebar
permalink: ubuntu_09.html
folder: product1
---

# md

# WinServer

- 네트워크 설정 변경

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled.png)

# Windows Server

# AD - Active Directory Service

- 네트워크 상에 존재하는 사용자, 그룹, 컴퓨터 및 다른 개체들에 대한 정보를 저장하고 이러한 정보를 사용자와 컴퓨터가 사용할 수 있도록 하기 위한 기능
    
    ## DS - Domain Service
    
    - 합쳐서 ADDS라고 하기도 함.
- (AD는) DS까지 사용하여 네트워크와 사용자 컴퓨터가 그들에게 허용된 네트워크 자원에 접근할 수 있도록 접근 제한을 하기 위해 사용.

- AWS의 IAM이라는 기능은 Windows Server의 AD Service와 관련이 있음.
- AWS 공부할 때 참고할 것.

- W Server IP : 172.16.20.250/24
- W Gateway : 172.16.20.254/24
- W Server DNS : X

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%201.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%202.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%203.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%204.png)

- Add Roles and Features에 설치 Wizzard가 있음.

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%205.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%206.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%207.png)

- 설정한 IP로 떠야 함

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%208.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%209.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2010.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2011.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2012.png)

- 역할은 추가 했으나 구성이 안 되었기 때문에 노란 느낌표가 뜸.

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2013.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2014.png)

- Root Domain name → 해당 도메인이 루트 도메인 (중심)이 될 것임.

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2015.png)

- Password : P@ssw0rd

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2016.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2017.png)

- NetBIOS domain name : 자동으로 생성됨.
    - AD로 묶여져있는 도메인을 식별하기 위한 또 다른 이름.

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2018.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2019.png)

- 의존성 문제일 경우,
    
    ![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2020.png)
    
    - 여기에 빨간색이 뜸. 그럼 그것을 먼저 설치 후 해당 구성을 설치 진행하면 됨.

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2021.png)

- Active Directory 가 활성화가 되면 이런 창이 뜬다.

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2022.png)

- 추가된 것을 확인할 수 있다.

- Ex. DNS 를 사용하고 싶을 때
    
    ![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2023.png)
    

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2024.png)

- 추가 가능한.

# 현재 상황

![허지원 (1).png](md%20d9a75227f41e4a41a7fbe55d1e976eb1/%25ED%2597%2588%25EC%25A7%2580%25EC%259B%2590_(1).png)

- 시스템 ON

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2025.png)

- 어댑터 #2 잘 되어있는지 확인 후 실행 시키기.

- W Server에 AD를 설치했지만 W Client가 가입이 되어야지만 접근이 가능함.
- Domain에 가입할 때는 Domain Address 형태로 가입을 하기 때문에 DNS Service 자체가 필수적으로 설치가 되어 있어야 함.
- AD DNS를 통해서 다양한 접근을 제어할 수 있게 함.
- DHCP가 있다면 자동으로 setting 하게 할 수는 있겠지만 우리가 이때까지 한 것은 setting이 맞지는 않음.
- AD의 DNS를 사용하게 해 보자.

# Windows Client

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2026.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2027.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2028.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2029.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2030.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2031.png)

## → AD에 Join 됨.

# 오류

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2032.png)

- 설치 시 무인 설치 건너뛰기 해야 함.
- Home 말고 Pro로 되어야 함.

# Windows Client 설치

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2033.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2034.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2035.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2036.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2037.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2038.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2039.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2040.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2041.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2042.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2027.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2043.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2029.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2044.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2045.png)

- Administrator / P@ssw0rd

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2046.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2047.png)

- 기타 사용자로 로그인 할 것.

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2048.png)

- 사용자 이름에 Administrator 입력 시,
    
    ![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2049.png)
    
    - WINCLIENT 도메인이 뜸.
    - 우리는 이게 없기 때문에 추가로 더 적어 주어야 함.

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2050.png)

- Administrator@domain.co.kr / P@ssw0rd
- domain\Administrator / P@ssw0rd

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2051.png)

- 로그인 완.

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2052.png)

# W Server - 확인

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2053.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2054.png)

- 잘 들어온 것을 확인할 수 있다.

# W Client

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2055.png)

- 한 번 로그인하면 자동으로 저장이 된다.

# 01. 계정 생성

# W Server

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2056.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2057.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2058.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2059.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2060.png)

- 추가 완.

# 확인) W Client

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2061.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2062.png)

- 잘 로그인이 되었다.

# 02. 그룹 정책 (GPO, OU)

# W Server - Group Policy Management

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2063.png)

- 그룹 정책

## GPO - Group Policy Objects

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2064.png)

- 새 정책 생성

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2065.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2066.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2067.png)

- Computer, User 각각 정책을 내릴 수 있음.

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2068.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2069.png)

### OU

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2070.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2071.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2072.png)

- 정책 연결

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2073.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2074.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2075.png)

- 생성된 것을 확인할 수 있다.

- OU1 조직에 사용자 새로 생성.
    
    ![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2076.png)
    
    - 사용자가 이 조직에 포함 됨.
    
    ![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2077.png)
    

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2078.png)

- 생성 완.

# 확인) W Client - user2

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2079.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2080.png)

- 제어판 접근이 금지된 것을 확인할 수 있다.

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2081.png)

- OU1에 포함시키지 않은 user1은 정상적으로 제어판에 접근이 가능하다.

# 01. 제어판 - 가시성

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2082.png)

- 컴퓨터에 대한 설정이라 컴퓨터가 조직에 등록이 되어야 함.

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2083.png)

- 어떤 걸 보이게 하고 어떤 것을 숨길 것인지 설정 값을 넣어주어야 함.

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2084.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2085.png)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2086.png)

# 01 - 확인) W Client

- W Client Reboot
    - 정책을 빨리 확인할 수 있는 방법 = Reboot

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2087.png)

- user1에서 장치만 표시가 되었다.

- user2는 아까 설정해 둔 user 제어판 금지 정책으로 인해 아예 설정이 들어가지지가 않음.
- user2를 OU1 → Users 로 이동했을 경우,
    - 제어판 접근 가능.
    - 설정에 접근이 가능하고 해당 정책이 적용이 된 것을 확인할 수 있음.
    
    ![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2088.png)
    

# GPO

- 적용 범위
    - Computer
    - User

- 적용을 하기 위해서는 OU에 해당 기기/사용자 가 등록이 되어야 함.

# 01. 사용자의 로그온 가능 시간 설정 정책
      ex. AM 09 - PM 18 : 접속 가능
02. 하나의 계정에 대해 컴퓨터에 로그온 못 하도록 설정 정책

# 방법.

## 01. Create New GPO

### W Server)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2089.png)

## 02. Setting Policy Enabled

### 02 - 1. 사용자의 로그온 가능 시간 설정 정책 (09 - 18 Access 허용)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2090.png)

- Active Directory Users and Computers > domain.co.kr > Users > 해당 유저 Properties > Account > Logon Hours
- 로그인 시간은 User만 가능함. Computer는 불가능함.
    
    ![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2091.png)
    
    - Computer Properties에 들어가면 Account 메뉴가 없기 때문.

### 02 - 2. 하나의 계정에 대해 컴퓨터에 로그온 못 하도록 설정 정책

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2092.png)

- Group Policy Management Editor > Group Policy Management Editor > Computer Configuration > Policies > Windows Settings > Security Settings > User Rights Assignment > Deny log on locally

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2093.png)

- 대상 : user2 로그인 금지 정책.
- 로그인 금지 대상을 그룹으로도 등록 가능.

## 02. Link an Existing GPO

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2094.png)

# 결과 - W Client.

## User1)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2095.png)

- 9to6는 로그인 가능.

- User1의 시간을 변경시.

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2096.png)

→ 결과)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2097.png)

- 로그인 접근 금지 정책에 포함되어 있지 않기 때문에 user1은 로그인 불가능한 시간이라도 로그인이 가능하다.

- User1을 OU1로 이동했을 때.

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2098.png)

→ 결과)

- 로그인 금지 정책에 User1 대상 추가

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%2099.png)

→ 결과)

- OU1에 해당 컴퓨터 추가 시.

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%20100.png)

→ 결과)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%20101.png)

- 적용이 잘 되었다.

- 그럼 정책 대상에서 해제 후, 시간을 변경하고 로그인 가능 시간대로 지역을 바꾼 후 다시 로그인 시도 해 보자.

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%20102.png)

→ 결과)

로그인 안 됨.

- 정책 대상 user1 삭제

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%20103.png)

→ 결과)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%20104.png)

- 해당 시간 접근 가능.

- 흠 뭐가 문젠지 몰겠음;; 추가 TC 필요.

## User2)

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%20105.png)

- 로그인 금지 정책이 잘 되었다.

# 설정 방법.

- 시간 → 계정만 가능.
- 로그인 금지 → 별도의 정책 생성.

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%20106.png)

# Windows Client

- gpupdate /force

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%20107.png)

- 정책을 바로 업데이트 할 수 있다.

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%20108.png)

# New Object - Group

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%20109.png)

- 도메인이 1개일 경우 Group scope를 구분해 줄 필요는 없음. 그냥 기본으로 Global 하면 됨.
- Group scope
    - Domain local :
    - Global :
    - Universal :

# 외부망 연결.

# UbuntuGW

- MAS~
- ping 8.8.8.8

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%20110.png)

# W Server

- ping 8.8.8.8
- ping google.com

![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%20111.png)

- 해결)
    1. Network 설정에서 DNS 추가
        1. 8.8.8.8
    2. Forwarder 설정
        
        ![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%20112.png)
        
        ![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%20113.png)
        
        ![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%20114.png)
        
        ![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%20115.png)
        
        ![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%20116.png)
        
    
    # SnapShot 하기
    
    ![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%20117.png)
    
    ![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%20118.png)
    
    ![Untitled](md%20d9a75227f41e4a41a7fbe55d1e976eb1/Untitled%20119.png)
