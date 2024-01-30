---
title: Day 08
keywords: ubuntu client
summary: "Ubuntu Client Terminal"
sidebar: systemoperation_sidebar
permalink: ubuntu_08.html
folder: product1
---

# md

# 강사님 예제 - 내가 만든 구성도를 예제로 할 경우

- Network Adapter가 하나 더 필요함
    - 도구 > 어댑터 3개까지 더 필요함
- 참고 : GW가 172.16.10.254/24라고 하더라도 도구의 어댑터 IPv4 주소를 굳이 맞춰줄 필요는 없음. 크게 신경써도 되지 않음.
    - 그럼 언제 맞춰야 하나?
        
        → Host : PC / Guest : VM
        
        - Host와 Guest의 연결이 필요할 때 사용.
        - 만약 도구 > 어댑터의 IPv4 IP가 192.168.56.1/24라면 Host의 IP를 192.168.56.1/24로 준다는 것.
        
        ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled.png)
        
    - 현재 우리는 Guest VM과 Guest VM을 서로 연결하는 것이기 때문에 맞춰 줄 필요는 없음.

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%201.png)

# 실습

## 01. UbuntuGW

- 설정 > 네트워크 > 어댑터4 추가.
    - Host Only - Adapter #2
- 총 어댑터는 4개가 있어야 함.
- 어댑터 1개당 하나의 네트워크 영역에 소속 됨.
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%202.png)
    

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%203.png)

- Forwarding 기능 활성화 → 라우트 기능 활성화
    - sudo vi /etc/sysctl.conf
    - #net.ipv4.ip_forward=1 → 주석 해제 (net.ipv4.ip_forward=1)
- sudo sysctl -p
    - net.ipv4.ip_forward=1 가 출력되는 것을 확인.

- 현재 상황
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%204.png)
    
- 사설 영역 안에 있는 주소가 공공망에서 다시 사설로 돌아올 수 있게 NAT가 필요함.
- 즉, 공인 IP 주소가 필요.
- 현재 Router에 공인 IP를 10.0.2.15라고 생각하자.

→ VM에서 10.0.2.15로 IP를 바꾸게 되면 외부망과 연결이 가능함.

- IP 주소 변환 시켜 주려고 하는 명령어
    - sudo iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE
        - -o : out
            - enp0s3(10.0.2.15)으로 빠져나갈 때
        - MASQUERADE : 출력 된 주소로 자동 변환

- ex. Web2에서 ping 8.8.8.8을 할 때,
    - IPv4 헤더 정보
        - src (출발지) : 172.16.10.20
        - dst (목적지) : 8.8.8.8
        - NAT 적용 : src는 10.0.2.15 로 변환 됨.
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%205.png)
    
    - 다음과 같은 경로를 통해 나갔다가 돌아옴.
        1. src : 172.16.10.10 → 10.0.2.15 
        2. dst : 8.8.8.8
        3. src : 8.8.8.8
        4. dst : 10.0.2.15 → 172.16.10.10
    
- 이때 꼭 확인해 줄 것.
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%206.png)
    
    - ex.
        
        ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%207.png)
        
    - 다음과 같은 경우, 통신이 가능함.

- ip addr
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%208.png)
    
    - enp0s10이 추가된 것을 확인.

- sudo vi /etc/netplan/00-installer-config.yaml
    - dhcp4 최대한 비활성화.
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%209.png)
    
- sudo netplan apply
- ifconfig
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2010.png)
    
    - 주소가 잘 들어간 것을 확인할 수 있다.
    - 항상 확인하는 습관을 들일 것.

- AM 09 에 이어서.

# UbuntuServer - 새 기기 만들기 (DHCP 서버를 위한)
→ 기본 UbuntuGW의 DHCP는 비활성화 시키기.

- sudo systemctl stop isc-dhcp-server
- sudo systemctl disable isc-dhcp-server

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2011.png)

- 켜져있는 VM 모두 종료 후 생성할 것.

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2012.png)

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2013.png)

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2014.png)

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2015.png)

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2016.png)

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2017.png)

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2018.png)

- SSH server는 꼭 설치 해 주어야 함.

- 구성 > 네트워크 > NAT (X) / 호스트 전용 (O) > Adapter
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2019.png)
    

- ip addr
- sudo vi /etc/netplan/00-installer-config.yaml
    
    ```powershell
    enp0s3:
      dhcp4: false
      addresses : [172.16.10.253/24]
    ```
    
    - 지금 상태면 현재 DHCP가 외부망과 설치가 안 되어 있는 것임.
    
    ```powershell
    addresses: ~
    routes:
      - to: default
        via: 172.16.10.254 //gateway_address
    ```
    
    - 아직 내부에 (사설) DNS를 구성하지 않았으니 DHCP에서는 우선 nameservers 설정은 해야 함.
    
    ```powershell
    routes:
    nameservers:
      addresses: [8.8.8.8]
    ```
    
- sudo netplan apply
- sudo cat /etc/netplan/00-installer-config.yaml

- `man` 명령어를 잘 활용하자.

- 확인할 것.
    - IP Address (명령어 : ip add / ip address show dev enp0s3)
    - Routing Information (명령어 : ip route)
    - DNS (명령어 : resolvectl)

- `default via 172.16.10.254 dev enp0s3 proto static`  이 있어야 Routing이 가능함.
- 172.16.10.0/24 dev epn0s3 proto kernel scope link src 172.16.10.253
    - 172.16.10.0/24 = Destination
    - dev enp0s3 = 경유지(송신 인터페이스)
    - proto = protocol
- 목적지는 CIDR까지만 보면 됨.
    - 이때 목적지는 ping 172.16.20.10 했을 때의 경우임.
    - 매치가 안 되면 default를 씀. (Default를 차후로 쓰겠다는 의미)
- 목적지의 GW가 Routing 시켜주는 것임.
- Gateway는 출발지를 바꿔서 보내지 않음.

- NAT 구성
    
    ```powershell
    sudo iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE
    ```
    
    - 이것 때문에 ip가 변환되는 것.
    - DHCP Server도 NAT 구성 해 주어야 ping 8.8.8.8부터 나감.

- resolvectl
    - 사설 DNS일 경우, 사설 DNS가 잡혀져 있는지를 확인하면 됨.

- 실제로도 되는지 확인하고 싶을 땐 ? (AWS에서는 적합하지 않음.)
    
    → TroubleShooting
    
    - 온프레미스일 경우)
    - ping 172.16.10.254 > ping 8.8.8.8 > ping google.com
    - ping `Gateway`  > ping `내부 인터넷`  > ping `DNS`

- AM 10에 이어서.

- IP 중복 문제가 발생할 경우 확인할 것.
    1. 물리적 연결.
    2. 논리 연결 (IP 주소).
    3. 방화벽. (나중에 확인해도 상관 X)
        1. AWS - Security Group
    4. 서비스 동작 유무.
        1. 웬만해서는 8.8.8.8로 동작이 되기 때문에 굳이 할 필요가 없음.
        2. 사용하고 있는 사설에 관한 서비스 확인.
    5. 개방 Port.
        1. LISTEN 상태로 포트가 있는 건지.
            1. ss -tuna | grep “:80” 
            2. LISTEN 상태인지 아닌지 확인 가능.
            
            ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2020.png)
            
            iii. ss -tua | grep “:80” 
            
            → HTTP로 출력됨.
            
            iiii. ss -tunap | 
            
            → p 옵션은 관리자 권한이 필요함.
            
            iv. Windows) netstat
            
    
    b. Web이라고 꼭 80번 포트 번호가 아닐 수도 있음.
    
    1. 3-tier 중 WAS는 80번이 아닐 수 있음.

# UbuntuDHCP

- DHCP 설치
    
    ```powershell
    sudo apt-get install isc-dhcp-server -y
    ```
    

- sudo vi /etc/default/isc-dhcp-server
    
    ```powershell
    INTERFACESv4="enp0s3"
    # INTERFACESv6=""
    ```
    
    - 수신할 Interface가 enp0s3이라는 뜻.
    - 여러 개를 쓸 경우 공백을 활용.
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2021.png)
    
- sudo vi /etc/dhcp/dhcpd.conf
    
    ```powershell
    option domain-name-servers 172.16.30.30;
    
    - 주석해제 후 입력 - 
    subnet 172.16.10.0 netmask 255.255.255.0 {
    		range 172.16.10.100 172.16.10.243;
    		option routers 172.16.10.254;
    }
    ```
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2022.png)
    

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2023.png)

- 여러 subnet 생성 시
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2024.png)
    

- sudo systemctl restart isc-dhcp-server
- sudo systemctl status isc-dhcp-server
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2025.png)
    

- status 했을 때 ERROR 떴을 때 상태 확인이 안 될 때
    
    ```powershell
    sudo tail -n 20 /var/log/syslog
    ```
    
    - /var/log/syslog 를 확인하자.

- dhcp-lease-list
    - 임대가 잘 되고 있으면 임대 목록에 뜸.
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2026.png)
    
    - 더 상세한 정보를 확인하고 싶다면 `/usr/local/etc/oui.txt` 를 확인하면 됨.

- AM 11에 이어서.

# UbuntuDesk

- Ethernet Adapter #1 인지 확인.

- sudo vi /etc/netplan/01-network-manager-all.yaml

```powershell
dhcp4: yes
```

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2027.png)

- sudo netplan apply
- ifconfig
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2028.png)
    
    - 잘 받아진 것을 확인할 수 있다.
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2029.png)
    

- ip route
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2030.png)
    
- resolvectl
    - DNS가 구성이 되었느냐 안 되었느냐는 별개의 문제임.
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2031.png)
    

- 통신 확인
    - ping 172.16.10.254
    - ping 8.8.8.8
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2032.png)
    

- PM 13에 이어서.

# UbuntGW

- GW에 DNS역할까지 부여하려고 함.

- sudo vi /etc/netplan/00-installer-config.yaml
    
    ```powershell
    enp0s10:
      dhcp4: false
      addresses: [172.16.30.254/24, 172.16.30.30/24]
    ```
    
    - 30.30은 Secondary 주소.
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2033.png)
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2034.png)
    

- sudo netplan apply
- ip address show dev enp0s10
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2035.png)
    

- DNS 설치
    - 기존에 되어 있음.
- sudo vi /etc/bind/named.conf.options
    
    ```powershell
    liste-on { 172.16.30.30; };
    ```
    
    - listen-on { any; }; 일 경우, 모든 인터페이스를 LISTEN 상태로 만들겠다는 의미.
    - 현재 172.16.30.30 만 DNS로 동작하게 하는 것임.
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2036.png)
    

- sudo systemctl restart named
- sudo systemctl status named
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2037.png)
    

- sudo vi /etc/bind/zones/db.domain.com
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2038.png)
    

- sudo systemctl restart naamed
- sudo systemctl status named

# UbuntuDesk

- DNS 확인 - 이전에는 되지 않았는데 이제는 되어야 함.
    
    ```powershell
    nslookup domain.com
    nslookup shop.domain.com
    nslookup google.com
    ```
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2039.png)
    

- DNS가 IP로 잘 변환 되는지.
    
    ```powershell
    nslookup domian.com
    ```
    

- 만약 변환이 잘 안 된다면 ?
    1. 연결이 잘 된 상태인지 확인.
    2. 레코드 잘못 등록했는지.
    3. 서비스 재시작 했는지.
        1. 서비스 재시작을 굳이 하지 않아도 일정 기간(약 5분)이상 지난 후 자동 설정 되긴 함.

# 정리

## 01. PC가 부팅 되면서 네트워크에 연결되는 과정
      (전제 : DHCP 사용)

1. PC : 부팅 → 네트워크 설정 시도
2. `DISCOVER` 
    
    : 동일 네트워크에 DHCP Server가 존재하는지 Broadcast 통신(255.255.255.255)을 사용하여 찾음.
    
3. `OFFER`
    
    : DHCP Server가 응답을 함.
    
    - 응답을 할 때에도 Broadcast로 응답.
    - OFFER에는 Client PC가 사용할 수 있는 IP 설정 정보를 담아서 응답.
4. Client PC는 OFFER 정보를 확인 후 해당 정보를 사용할 경우 그 정보를 그대로 REQUEST 보냄. (Default 값)
    1. 이전 정보가 있을 경우, REQUEST에 정보가 남아 있을 수도 있음.
5. DHCP Server에서 임대를 할 수 있는 경우, REQUEST에 대한 ACK로 응답.
    1. ACK로 응답 = 임대를 허용하겠다는 의미.
6. ACK를 받은 Client PC는 IP Address, Gateway Address, DNS Address가 구성이 되어 네트워크 연결이 이루어짐.

ex. 만약 임대 과정에 문제가 발생하는 경우, Client는 기존에 사용하던 IP Address를 그대로 사용하거나 **169.254.xxx.xxx** 주소가 사용 됨.

- 임대 실패하는 경우 뜨는 IP Address : 169.254.xxx.xxx

## 02. 사용자들이 브라우저를 통해 웹 사이트를 탐색하는 과정.

1. 브라우저 주소 입력 창에 도메인 주소를 입력.
2. 도메인 주소는 DNS Server를 이용하여 IP Address로 변환되는 과정을 가짐.
    1. DNS 질의 과정에 대한 부분은 생략 됨.
        1. DNS 질의 - forwarders / .(root) → .com. → google.com.
        2. 여튼 계속 질의를 한다는 것.
        3. 그렇게 해서 최종적으로 IP 주소를 얻어 오는 것임.

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2040.png)

- src : 출발지
- dst : 목적지
- port-src : 출발지 포트 번호
- port-dst : 목적지 포트 번호

1. 변환된 주소로 웹 서버에 연결.
    1. 연결되는 과정에서 라우팅에 대한 부분은 생략 됨.

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2041.png)

### ex. https://google.com

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2042.png)

- https의 port number : 443

# UbuntuDesk

## 01. Apache Web Server 활성화.

- sudo vi /etc/netplan/01-network-manager-all.yaml
    
    ```powershell
    enp0s8:
      dhcp4: yes
      addresses: [172.16.10.10/24, 172.16.10.20/24]
    ```
    
    - dhcp, ip : 활성화
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2043.png)
    
- sudo netplan apply

- sudo ss -tunap | grep “:80”
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2044.png)
    
    - apache2 사용 중.

- sudo vi /etc/apache2/sites-abailable/(Tab 버튼 * 2)
    - 000-default.conf   default-ssl.conf 가 나와야 함.
        
        ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2045.png)
        
- sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/001-domain.com.conf
- sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/002-shop.domain.com.conf

→ 001과 002를 사용할 것임.

- sudo a2ensite 001-domain.com.conf
    - 001 활성화
        
        ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2046.png)
        
- sudo a2ensite 002-shop.domain.com.conf
    - 002 활성화
- sudo a2dissite 000-default.conf
    - 000 비활성화
- Ubuntu Apache2의 경우 명령어임. 다른 버전의 경우 명령어가 다를 수 있음.
- Nginx의 경우 수동으로 다 해야함.

- sudo ls -la /etc/apache2/sites-enabled
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2047.png)
    
    - Apache는 심볼릭 링크가 바로 되는 것이고, Nginx는 ln -s 명령어를 사용해야 함.

- sudo vi /etc/apache2/ports.conf
    
    ```powershell
    Listen 80 -> 삭제
    Listen 172.16.10.10:80 //추가
    Listen 172.16.10.20:80 //추가
    ```
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2048.png)
    

- sudo systemctl restart apache2
- ss -tuna | grep “:80”
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2049.png)
    
    - 2가지가 떠야 함.
    
    → 주소로 분류하는 형식.
    

- sudo vi /etc/apache2/sites-available/001-domain.com.conf
    
    ```powershell
    <VirtualHost *:80> -> *을 변경
    <VirtualHost 172.16.10.10:80>
    
    DocumentRoot /var/www/html -> /var/www/html/first
    ```
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2050.png)
    

- sudo vi /etc/apache2/sites-available/002-shop.domain.com.conf
    
    ```powershell
    <VirtualHost *:80> -> *을 변경
    <VirtualHost 172.16.10.20:80>
    
    DocumentRoot /var/www/html -> /var/www/html/second
    ```
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2051.png)
    
- ls -la my-websites
    - 이전에 이런식으로 다 정적페이지를 만들었었던 것임.
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2052.png)
    
    - ‘html&css-trainin’ 과 room-homepage-master 가 존재하는 것을 확인.
    
- sudo cp -r my-websites/html\&css-training/ /var/www/html/
- sudo ls -l /var/www/html
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2053.png)
    
    - ‘html&css-training’이 존재해야 함.
- sudo mv /var/www/html/html\&css-training/ /var/www/html/first
- sudo ls -l /var/www/html
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2054.png)
    
    - first가 존재해야 함.
- cf. first = html&css-training , 10.10

- sudo cp -r my-websites/room-homepage-master/ /var/www/html
- sudo mv /var/www/html/room-homepage-master/ /var/www/html/second
- sudo ls -l /var/www/html
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2055.png)
    
    - second가 존재해야 함.
- cf. second = room-homepage-master , 10.20

- sudo systemctl restart apache2

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2056.png)

## 01 결과.

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2057.png)

## 02. Nginx Web Server 활성화. (혼자서 하는 중)

### Nginx가 자꾸 install이 안 되는 Error가 발생.

- 삭제 후 재설치.
- 완벽한 삭제) sudo apt-get remove --purge nginx nginx-full nginx-common

### 재설치 후 Failed.

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2058.png)

- 해당 아래 주소를 참고 하니 갑자기 됐다.

[NGINX를 0.0.0.0에 바인딩하지 못했습니다 | 문제 해결 팁 (bobcares.com)](https://bobcares.com/blog/nginx-bind-to-0-0-0-0-failed/)

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2059.png)

## 02 - 1. 참고할 것.

### UbuntuDesk - NGINX Server

- Make new pages in sites-available.
    - sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/001-domain.com.conf
    - sudo cp /etc/nginx/sites-available/default /etc/ngixn/sites-available/002-shop.domain.com.conf
- Add symbolic links in sites-enabled
    - sudo ln -s /etc/nginx/sites-available/(Tab * 2)
    - sudo ln -s /etc/nginx/sites-available/001-domain.com.conf /etc/nginx/sites-enabled
    - ls /etc/ngins/sites-enabled/(Tab * 2)
        - default, 001-domain.com.conf
    - sudo ln -s /etc/nginx/sites-available/002-shop.domain.com.conf /etc/nginx.sites-enabled
    - ls /etc/nginx/sites-enabled/(Tab * 2)
        - default, 001-domain.com.conf, 002-shop.domain.com
- Delete default in sites-enabled
    - default를 dissite하라는 뜻 같음.

- **nginx server**
    
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2060.png)
    
    make new pages in sites-available
    
    add symbolic links in sites-enabled
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2061.png)
    
    delete default in sites-enabled
    

### Apache Server

- **apache2 server**
    
    site redirection has 2 ways
    
    use port or use server name
    
    port setting
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2062.png)
    
    listen 172.16.10.20:80
    
    when address 요청 is for 172.16.10.20 give index.html in root folder
    
    **or**
    
    use server_name to get page
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2063.png)
    

# UbuntuDesk

## 01 - 2. Apache에서 가상 호스트라는 기능을 최대한 활용해 본 것.

- sudo vi /etc/apache2/ports.conf
    
    ```powershell
    Listen 172.16.10.10:80 -> 하나만
    ```
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2064.png)
    
- sudo vi /etc/netplan/01-network-manager-all.yaml
    
    ```powershell
    enp0s8:
      dhcp4: yes
      addresses: [172.16.10.10/24] -> 20번 제외
    ```
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2065.png)
    
- sudo vi /etc/apache2/sites-available/001-domain.com.conf
    
    ```powershell
    <VirtualHost *:80> -> 주소 지정하지 않아도 됨.
    
    DocumentRoot /var/www/html/first
    ServerName domain.com -> 추가
    ```
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2066.png)
    
    - DNS 구성이 잘 되어 있지 않으면 ServerName이 잘 되지 않음.
    - DNS가 구성 되어 있지 않다면 직접 IP 주소를 써도 됨.

- sudo vi /etc/apache2/sites-available/002-shop.domain.conf
    
    ```powershell
    <VirtualHost *:80> -> 주소 지정하지 않아도 됨.
    
    DocumentRoot /var/www/html/first
    ServerName shop.domain.com -> 추가
    ```
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2067.png)
    

- sudo netplan apply
- sudo systemctl restart apache2
- sudo systemctl status apache2
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2068.png)
    
    - active가 되어야 함.

- ss -tuna | grep “:80”
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2069.png)
    
    - 포트가 잘 개방되어 있는지 확인.
    - 10.10 이 LISTEN으로 되어 있어야 함.

## UbuntuGW

- sudo vi /etc/bind/zones/db.domain.com
    
    ```powershell
    @  IN  A  172.16.10.10
    shop  IN  A  172.16.10.10
    ```
    
    ![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2070.png)
    
- sudo systemctl restart named
- nslookup shop.domain.com 172.16.30.30
    - 10.10으로 나와야 함.
- nslookup domain.com 172.16.30.30
    - 10.10으로 나와야 함.

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2071.png)

## UbuntuDesk - Client에서도 확인 해 볼 것.

- nslookup shop.domain.com 172.16.30.30
    - 10.10으로 나와야 함.
- nslookup domain.com 172.16.30.30
    - 10.10으로 나와야 함.

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2072.png)

- 구성 바꾼 후 늘 결과를 확인할 것.

## 01 - 2. 결과

- 되도력이면 캐시 문제 때문에 시크릿 모드를 사용할 것.
- Firefox로 Web 접속해보면 됨.

![Untitled](md%201361f362bee34fc5bcbf88eab7dae5b6/Untitled%2073.png)
