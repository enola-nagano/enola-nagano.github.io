---
title: Day 04
keywords: ubuntu client
summary: "Ubuntu Client Terminal"
sidebar: systemoperation_sidebar
permalink: ubuntu_04.html
folder: product1
---

# md

# SSH = Secure-Shell

- 네트워크 상에 연결된 다른 컴퓨터에 로그인을 하거나 원격 시스템에 명령을 실행하기 위해 사용하는 응용프로그램 또는 프로토콜
- 기존 Telnet을 대체하기 위해 설계되었으며, 강력한 인증을 통해 네트워크 상에서도 안전하게 통신을 할 수 있는 기능을 제공
- TCP 22번 포트를 사용하며, 암호화된 문자로 통신을 하기 때문에 보안이 더 강화됨.

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled.png)

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%201.png)

- 윈도우에 이렇게 뜬다면 ssh가 뜬 것.
- 윈도우를 ssh client로 사용할 것임.
- cmd OR PowerShell OR putty OR XShell

# 실습

## Ubuntu

- sudo apt-get install openssh-server

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%202.png)

# 오류

could not get lock /var/lib/dpkg/lock-frontend. It is held by process 2938 (unattended-upgr)

[해결 된 "잠금 / var / lib / dpkg / lock-frontend (unattended-upgr)를 가져올 수 없습니다." | 사이버ITHub (cyberithub.com)](https://www.cyberithub.com/solved-could-not-get-lock-var-lib-dpkg-lock-frontend-unattended/)

- `sudo apt update && sudo apt upgrade`
    
    → 정상적으로 업그레이드 됨
    

- systemctl status sshd

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%203.png)

# 오류

warning some journal files were not opened due to insufficient permissions

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%204.png)

- ss -tunas | grep ‘:22’

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%205.png)

→ LISTEN 상태로 제대로 되어있는지 확인.

- sudo systemctl stop sshd
- sudo systemctl status sshd

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%206.png)

- sudo systemctl start sshd
- sudo systemctl restart sshd
- sudo systemctl enable ssh.service
- sudo systemctl disable sshd

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%207.png)

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%208.png)

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%209.png)

# 오류

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2010.png)

[라즈베리파이 우분투 openssh-server Failed with result 'exit-code' :: 고마워서 만든 블로그 by 맛소금 (msalt.net)](https://blog.msalt.net/326)

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2011.png)

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2012.png)

## Windows

- ssh ubuntu@192.168.56.10
- ssh 계정명@Ubuntu서버주소

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2013.png)

# Ubuntu Network 설정 변경

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2014.png)

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2015.png)

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2016.png)

# Birdge

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2017.png)

# Port 번호

- /etc/ssh/sshd_config
    - #PubkeyAuthentication yes → PubkeyAuthentication yes : 주석해제
        - 만들어진 키로 접속이 가능하게 하려고 함. 만들어진 키는 암호화 되어 있음.
    - #AuthorizedKeysFile → 주석해제
    - #PasswordAuthentication yes → PasswordAuthentication yes : 주석해제

```powershell
- #PubkeyAuthentication yes → PubkeyAuthentication yes : 주석해제
    - 만들어진 키로 접속이 가능하게 하려고 함. 만들어진 키는 암호화 되어 있음.
- #AuthorizedKeysFile → 주석해제
- #PasswordAuthentication yes → PasswordAuthentication yes : 주석해제
```

- sudo systemctl restart sshd
- systemctl status sshd

# KeyPair

- SSH에서 사용할 키(키페어)를 만들어야 한다면
- ssh-keygen
    - ssh-keygen : 키를 만드는 명령어
    - public/private
        - public key : 공개 키
            - 공유가 되어야 하는 키
            - 서버에 등록하면 됨.
        - private key : 개인 키
            - 공유하면 안 되는 키
            - 개인이 보관 후 서버에 접속 시 사용.
    - /home/ubuntu/.ssh/id_rsa
        - .ssh : 숨김 폴더
        - id_rsa : 개인 키
        - id_rsa.pub : 공개 키

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2018.png)

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2019.png)

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2020.png)

- 복사를 할 경우 BEGIN / END 까지 복사해야 함.

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2021.png)

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2022.png)

- 호스트 전용 어댑터로 실습

# SSH

- scp [source] [target]
    - source(원격지 정보)에 있는 것을 target에 붙여넣기

### W VM CMD에서 실행

- scp ubuntu@192.168.56.101:/home/ubuntu/.ssh/id_rsa ./
- scp ubuntu@192.168.56.101:~/.ssh/id_rsa ./

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2023.png)

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2024.png)

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2025.png)

- 윈도우) C > 사용자 > user에 id_rsa 파일이 생성 되어야 함

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2026.png)

- vi /etc/ssh/sshd_config
    - AuthorizedKeysFiles      ./ssh/authorized_keys ./ssh~

# Ubuntu

- cd .ssh
- touch authorized_keys
- cat id_rsa.pub >> authorized_keys
    - >> : 마지막 줄에 추가
- cat authorized_keys

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2027.png)

# 윈도우

- ssh -i id_rsa ubuntu@192.168.56.101

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2028.png)

# 우분투 - Ownership 변경

- cd /home
- sudo chmod 750 ubuntu
    - 755 → 750
- ls -la

---

- sudo tail -n 8 /var/log/auth.log 로 확인
    - sudo cat /var/log/auth.log
        - sshd라는 log 메시지가 뜸. bad ownership으로 판별한 것.
    
    ![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2029.png)
    

- cd ~/ubuntu/.ssh 에서 rsa에 ownership에 문제가 생길 수 있음.
    - 400이나 644로 바꾸면 됨.

---

# 윈도우

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2030.png)

# 우분투

- PasswordAuthentications yes 밑에 작성
    - AuthenticationMethods publickey,password → 키인증 + 패스워드인증 : 이중 보안

## 윈도우

- ssh -i id_rsa ubuntu@192.168.56.101

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2031.png)

- ssh ubuntu@192.168.56.101

# 윈도우

## cmd - 키 생성

- ssh-keygen
    - ubuntu_rsa
    - ubuntu
    - ubuntu
        
        ![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2032.png)
        
    
    ## 서버에 등록
    
    - scp -i id_rsa ubuntu_rsa.pub ubuntu@192.168.56.101:~/.ssh/
        - -i : 인증키 (는 id_rsa)
    
    ![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2033.png)
    
    ![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2034.png)
    
    ## 키 등록
    
    - ssh -i id_rsa ubuntu@192.168.56.101
    - $cd .ssh
    - $ls -la
        - ubuntu_rsa.pub이 있어야 함.
        
        ![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2035.png)
        
    - $cat ubuntu_rsa.pub >> authorized_keys
    - $cat authorized_keys
    
    ![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2036.png)
    
    - $logout
    - ssh -i ubuntu_rsa ubuntu@192.168.56.101
    
    ![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2037.png)
    
    ## 서버에 키 폐지
    
    - $vi ~/.ssh/authorized_keys
        - dd로 삭제
    
    ![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2038.png)
    
    ![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2039.png)
    
    - $logout
    - ssh -i id_rsa ubuntu@192.168.56.101
    
    ![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2040.png)
    
    - ssh -i ubuntu_rsa ubuntu@192.168.56.101

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2041.png)

# 키 새로 생성해서 다시 인증 가능한지

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2042.png)

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2043.png)

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2044.png)

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2045.png)

# Q. ssh config 파일을 사용하여 연결하는 방법에 대해 조사 : 윈도우 → 우분투

## 윈도우

- C:\Users\user\.ssh 위치에 config 확장자 없는 파일 만들기
    
    ```powershell
    Host my_server
        HostName 192.168.56.101
        Port 22
        User ubuntu
        IdentityFile "C:\Users\user\id_rsa" 또는 ~/id_rsa
    ```
    

- ssh my_server

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2046.png)

# 기타

- ssh my_server “명령어”

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2047.png)

# 참고사항

- cmd에서 `ssh -i` 할 때, Directory 위치가 C > Users > user 안에 있기 때문에 그냥 바로 가능
- 없으면 `ssh -i c:\경로지정\id_rsa`
- scp -i c:\Users\user\id_rsa [Source] [Target]
    - 로컬 → 원격지
        - ex. ./file ubuntu@192.168.56.102:~/
    - 원격지 → 로컬
        - ex. ubuntu@192.168.56.102:~/file ./
    - 에 따라 [Source]와 [Target]의 위치가 서로 바뀜.
    - 원본(파일의 위치)만 잘 파악하기

# Ubuntu - NAT Interface

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2048.png)

- 포트 포워딩

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2049.png)

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2050.png)

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2051.png)

- 호스트 IP = 로컬 IP
- 게스트 IP = VirtualMachine의 NAT IP
- 게스트 포트 = SSH = 22

## 01. 확인 - 로컬 PC cmd에서 할 것

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2052.png)

- 키만 만들어주면 확인이 됨
- Ubuntu 클립보드 공유 양방향으로 설정 되어 있어야 함
- CD 이미지 삽입도 되어 있어야 함

- ssh-keygen

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2053.png)

- id_rsa.pub 을 ubuntu로 복붙
    - ubuntu) vi .ssh/authorized_keys
    
    ![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2054.png)
    
- sudo systemctl restart sshd

![Untitled](md%206b6509def84348f29400cd7d8e1821a7/Untitled%2055.png)

- 다시 접속하니 정상적으로 접속이 가능하다.
- ssh ubuntu@호스트IP
