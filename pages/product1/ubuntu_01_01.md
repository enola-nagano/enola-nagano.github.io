---
title: Ubuntu Client 설치
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: ubuntu_01_01.html
folder: product1
---

## Virtual Box

* Virtual Box 정보 : Version 7.0<br/><br/>

{% include inline_image.html file="01_virtualbox_ver.png" alt="virtualbox_ver" %}<br/><br/>


## Information Virtual Machine

### 1. 최소 용량

* Ubuntu Client RAM : 2G (2048MB)<br/>
* Ubuntu Server RAM : 1G (1024MB)<br/>
* Windows 10 Server / Client RAM : 4G (4096MB)<br/><br/><br/>


{% include inline_image.html file="01_task_manager.png" alt="task_manager" %}<br/>

→ 메모리 용량을 보고 더 늘려도 상관 없음.<br/><br/>


### 2. CPU 개수

* 2개<br/><br/><br/>

## Install Ubuntu Client

{% include inline_image.html file="01_ubuntudesk_install_1.png" alt="01_ubuntudesk_install_1" %}<br/><br/>

{% include inline_image.html file="01_ubuntudesk_install_2.png" alt="01_ubuntudesk_install_2" %}<br/><br/>

{% include inline_image.html file="01_ubuntudesk_install_3.png" alt="01_ubuntudesk_install_3" %}<br/>

→ ubuntu / ubuntu <br/>
호스트 이름과 도메인 이름은 동일하게<br/><br/>

{% include inline_image.html file="01_ubuntudesk_install_4.png" alt="01_ubuntudesk_install_4" %}<br/><br/>

{% include inline_image.html file="01_ubuntudesk_install_5.png" alt="01_ubuntudesk_install_5" %}<br/>

→ 미리 전체 크기 할당 - 실제로 물리적으로 그만큼 할당 됨.<br/>
체크하지 않을 경우 유동적으로 디스크 크기를 할당함.<br/><br/>

{% include inline_image.html file="01_ubuntudesk_install_6.png" alt="01_ubuntudesk_install_6" %}<br/><br/>

{% include inline_image.html file="01_ubuntudesk_install_7.png" alt="01_ubuntudesk_install_7" %}<br/><br/>

{% include inline_image.html file="01_ubuntudesk_install_8.png" alt="01_ubuntudesk_install_8" %}<br/>

→ 가상머신이 자동으로 설치된다.<br/><br/>

{% include inline_image.html file="01_ubuntudesk_install_9.png" alt="01_ubuntudesk_install_9" %}<br/>

→ vbox - 가상 머신<br/>
vdi - 가상 이미지<br/>
이 폴더가 하나의 가상 머신 환경을 구성함.<br/><br/>

{% include inline_image.html file="01_ubuntudesk_install_10.png" alt="01_ubuntudesk_install_10" %}<br/>

→ 설치 완료.<br/><br/>

{% include inline_image.html file="01_ubuntudesk_install_11.png" alt="01_ubuntudesk_install_11" %}<br/><br/>



## Ubuntu Terminal이 열리지 않을 때

사진1
<br/>
터미널이 열리지 않는다.<br/><br/>

* 다음과 같이 환경설정을 바꾸기.<br/>
사진2
<br/>
`Setting > Language > 한국어로 변경 > 재부팅` <br/><br/>

사진3
<br/><br/>

사진4
<br/>
* 정상적으로 Terminal이 열리는 것을 확인할 수 있다.<br/><br/><br/>


## SnapShot, 네트워크 1개 추가

사진5
<br/>
* `Power Off` 후 진행<br/><br/>

사진6
<br/>
* 기본으로 설정되어 있는 것.<br/><br/>

사진7
<br/>
* `활성화 > 호스트 전용 어댑터` <br/><br/>

사진8
<br/>
* 정상적으로 네트워크가 추가된 것을 확인 가능.<br/><br/>

사진9
<br/><br/>

사진10
<br/>
* 찍기 선택.<br/><br/>

사진11
<br/><br/>

사진12
<br/>
* 스냅샷은 종료 후 찍어야 함.<br/><br/>

사진13
<br/>
* 네트워크가 잘 추가된 것을 확인할 수 있음.<br/><br/>

사진14
<br/>
* Wired Settings 에 들어가보면<br/><br/>

사진15
<br/>
* 첫 번째 톱니바퀴<br/><br/>

사진16
<br/>
* 기본 네트워크 구성 정보 확인 가능<br/>
- Hardware Address : MAC Address<br/>
- Default Route : Gateway<br/><br/>

사진17
<br/>
* `enp0s8`을 앞으로 다룰 것임.<br/><br/><br/>


## Gateway

* 격리된 내부 네트웤 영역에서 외부 네트워크 영역과의 통신을 하기 위해 사용되는 출입문 역할<br/>
사진18
<br/><br/>

* enp0s3은 외부와 연결이 되어 있기 때문에 Gateway에 대한 정보가 있음.<br/>
* enp0s8은 내부망이기 때문에 Gateway에 대한 정보가 없음.<br/><br/>

사진19
<br/>
* 현재 상황을 도식화 했을 때 이렇게 그릴 수 있음.<br/><br/><br/>


## 기본 명령어

### ip addr

사진
<br/>
ip addr = ip address<br/><br/>

* 10.0.2.15/24 중 /24에 대하여<br/>

```
1. = prefix = CIDR
```
<br/><br/>


- 10.0.2.15/24 중 /24에 대하여
    - = prefix = CIDR
    - 0 - 32 까지 사용 가능.
        - IPv4 = 32bit
- 10.0.2.15/24 중 10.0.2.15에 대하여
    - 10 = 8bit
    - 32bit를 4개씩 나눈 것임
    - 8bit를 10진수로 변환 = 10.0.2.15
        - 주소 체계 : 0 - 255 까지 사용 가능.
- Prefix = CIDR
    - 네트워크 주소에서 변하면 안 되는 bit의 범위를 설정하여 네트워크 영역의 크기를 자유롭게 조절하기 위해 사용.
    - 현재의 경우 10.0.2 까지는 변하면 안 됨.
        - 같은 네트워크 = 내부 : 10.0.2.1 , 10.0.2.2 , 10.0.2.128
        - 다른 네트워크 = 외부 : 10.0.3.1 , 10.0.4.129
        - 내부와 외부를 연결시키기 위해 Gateway를 만들면 서로 통신이 가능함.

### ip route

![Untitled](AM%2010%20-%20Terminal%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%20%E1%84%80%E1%85%AE%E1%84%89%E1%85%A5%E1%86%BC%20358edf6ec31c440fbdf14d2ed861ece3/Untitled%2020.png)

- default = 기본적으로 사용되는 출입문 = 10.0.2.2

![Untitled](AM%2010%20-%20Terminal%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%20%E1%84%80%E1%85%AE%E1%84%89%E1%85%A5%E1%86%BC%20358edf6ec31c440fbdf14d2ed861ece3/Untitled%2021.png)

- ip 구성 수정할 경우

![Untitled](AM%2010%20-%20Terminal%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%20%E1%84%80%E1%85%AE%E1%84%89%E1%85%A5%E1%86%BC%20358edf6ec31c440fbdf14d2ed861ece3/Untitled%2022.png)

- ls : 파일 목록
- cat : 특정 파일 내용 확인
- vi : 파일 수정 (bin, nano etc)
    - nano, vi는 기본적으로 설치 돼 있음.
    - 기본적으로 vi를 사용할 줄 알면 됨.
 

# md (AM 11 - )

## ls --help

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled.png)

## man ls

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%201.png)

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%202.png)

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%203.png)

# /home/ubuntu/ = 사용자 디렉터리

- /home/계정명/
- 윈도우) C:\Users\mzc\
    - C:\Users\계정명\

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%204.png)

## cd = change directory

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%205.png)

- ~에서 /로 바뀜
- ~ = home directory = 자신의 홈 디렉터리를 의미

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%206.png)

- /home/계정명 → ~로 바뀜

## cd .. = 상위 디렉터리로 이동

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%207.png)

## pwd = 현재 나의 위치

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%208.png)

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%209.png)

## ls -l

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2010.png)

- 첫 번째 문자로 파일, 디렉터리 등 구분
    - - : 파일
    - d : 디렉터리
    - l : 링크 (바로가기)
    - c : 입출력 장치 파일
    - b : 저장 장치 디바이스 파일

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2011.png)

- 공유 파일 : 누구나 들어가기 가능함

## Directory

- root directory
    - /
    - 리눅스의 모든 디렉터리의 시작점
- bin directory
    - /bin
    - 리눅스의 기본 명령어가 저장되어 있는 디렉터리
    - ex. cp, rm, mv, ls etc.
- boot directory
    - /boot
    - 리눅스의 부트로더가 들어 있는 디렉터리
    - 부팅 프로세스가 진행되는 과정에 필요한 파일만 있음
- dev directory
    - /dev
    - 시스템 장치 파일이 들어 있는 디렉터리
    - 하드디스크 장치 파일 및 마우스 키보드 등의 파일
- etc directory
    - /etc
    - 리눅스 시스템의 설치된 프로그램의 구성 정보 파일이 저장되는 디렉터리
    - 추가적으로 /usr/etc 디렉터리를 사용하기도 함
    - ex. 네트워크 매니저, sshd, apache etc.
- home directory
    - /home
    - 리눅스 사용자 디렉터리
    - 리눅스 사용자를 생성하면 기본적으로 생성되어 사용되는 디렉터리
- lib directory
    - /lib
    - 커널이 필요로 하는 커널 모듈 파일과 프로그램(C, Python etc)에 필요한 라이브러리 파일
- mnt directory
    - /mnt
    - CD 및 DVD 또는 USB 장치들의 마운트 포인트로 사용되는 디렉터리
- opt directory
    - /opt
    - 우분투에서 제공되지 않는 프로그램을 추가로 설치할 경우에 사용되는 디렉터리
- proc directory
    - /proc
    - 메모리에 존재하는 모든 작업들이 파일 형태로 존재할 때 위치하는 디렉터리
- usr directory
    - /usr
    - 리눅스를 실행하는 다양한 시스템에서 마운트 할 수 있도록 공유 가능한 읽기 전용 데이터를 보유하는 디렉터리
- var directory
    - /var
    - 로그 파일과 같이 크기가 변경될 수 있는 파일이 포함되어 있는 디렉터리
    - ex. /var/log 해당 디렉터리에 로그 파일이 저장되어 있음
- run directory
    - /run
    - Run-time variable data를 관리하는 디렉터리
    - 시스템 정보를 관리하는 디렉터리로 시스템 정보를 찾아 볼 수 있음
- tmp directory
    - /tmp
    - 공유 디렉터리로 모든 사용자가 사용할 수 있는 임시 디렉터리
    - 재부팅 시 디렉터리 내용이 삭제되며, 정기적으로 10일 정도 기간을 두고 디렉터리 내용을 삭제함

## ls -la

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2012.png)

- 숨김파일까지 같이 보여짐

## 상대경로, 절대경로

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2013.png)

- cd home : 상대경로
    - 현재 위치
- cd /home : 절대경로
- ./ : 현재 위치
- ../ : 상위 디렉터리

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2014.png)

- cd /etc/netplan : 절대경로
- cd ../etc/netplan : 상대경로

## mkdir

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2015.png)

- 디렉터리 생성

## rmdir / rm

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2016.png)

- rmdir : 디렉터리 삭제
- rm : 파일 삭제

## touch

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2017.png)

- 파일 생성

## Q. f1 디렉터리 생성 후 f1 디렉터리에 file 생성하기

- 방법 01.

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2018.png)

- 방법 02.

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2019.png)

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2020.png)

- rmdir은 디렉터리 안에 파일이 지워져 있어야 함
    - 방법) rm -rf
        
        ![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2021.png)
        

## echo

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2022.png)

- > : redirection

## mv

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2023.png)

- 이름 변경

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2024.png)

- 파일 이동

## cp

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2025.png)

- 복붙

## Q. 다음 웹의 디렉토리/파일 경로 구조를 mkdir, touch, echo 등의 명령어를 활용하여 만들어보세요. [root] 는 사용자의 홈 디렉토리에 `html` 디렉토리를 만들어서 하세요.

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2026.png)

- root를 /home/ubuntu/html 에 둘 것

## ls -R

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2027.png)

## apt-get install tree

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2028.png)

## tree

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2029.png)

## cat /var/log/gpu-manager.log

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2030.png)

- cat : 전체 내용 보기

- head -n 5 /var/log/gpu-manager.log
    - 처음 5줄 내용 보기
- tail -n 5 /var/log/gpu-manager.log
    - 마지막 5줄 내용 보기
- more /var/log/gpu-manger.log
    - 
    
    ![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2031.png)
    

## find

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2032.png)

- ./ : 현재 위치에서 찾기

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2033.png)

- 해당 확장자 찾기

## df - 디스크 정보 확인

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2034.png)

## du

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2035.png)

## ls -lh

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2036.png)

## useradd

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2037.png)

- 두 개의 차이
- -m : 사용자 홈 디렉터리
- -s : 셸

- 차이 결과
    
    ![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2038.png)
    

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2039.png)

- /bin/bash 셸 = /root
    
    ![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2040.png)
    

## passwd 계정명

- user1 / user1
- user2 / user2

## userdel

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2041.png)

## /etc/passwd : 사용자 정보가 저장된 파일, 시스템 계정 정보

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2042.png)

- user1 : 유저계정
- 1001 : uid = user id = 사용자 아이디
- 1001 : gid = group id = 그룹 아이디
- 보통 uid = gid
- /home/user1 : home directory
- /bin/bash : 사용하고 있는 bash shell

## /etc/shadow : 암호가 저장 되어 있는 파일

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2043.png)

- 사용자계정:암호:암호생성일자:변경기간:유효기간:만료경고일:만료후경과일:만료일
- 암호는 암호화 되어 있음
- 1970.01.01부터 19712일이 지난 후 생성 되었고 99999일 후 만료 경고일임

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2044.png)

## /etc/group : 그룹 정보

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2045.png)

- user1:x:1001:사용자
- 유저명:암호:GID:

## /etc/gshadow : 그룹 패스워드

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2046.png)

## 정리

- /etc/passwd : 사용자 정보 파일
- /etc/shadow : 사용자 암호 정보 파일
- /etc/group : 그룹 정보 파일
- /etc/gshadow : 그룹 암호 정보 파일

## Permission denied - 권한 거부

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2047.png)

## 권한 정보 확인

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2048.png)

- rwxr-xr-x
    - rwx : user
    - r-x : group
    - r-x : otehr
- r : read
- w : write
- x : execute

## chmod : 권한 부여

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2049.png)

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2050.png)

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2051.png)

- o+rw 만 할 경우 mkdir할 경우 허가 거부 됨.

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2052.png)

- 정상적으로 생성 됨

# 정리

- 파일
    - r : 파일 읽기 권한 (cat, head, tail, etc)
    - w : 파일 쓰기 권한 (touch, echo message > filename, etc)
    - x : 파일 실행 권한 (.sh, .py, etc 스크립트 실행 파일)
        - sh = shell

- 디렉터리
    - r : 디렉터리 내부의 파일 및 디렉터리 정보를 읽기 위한 권한 (ls)
    - w : 디렉터리 내부의 파일 생성 및 삭제를 위한 권한 (mkdir, touch, rm, mv, etc)
    - x : 디렉터리 접근과 관련된 권한

- chmod 숫자모드
    - 10진수 = 2진수
        - 2진수 = rwx
    - 777 = 111 111 111
    - 666 = 110 110 110
    - 555 = 101 101 101
    - 444 = 100 100 100
    - 000

## sudo = su - root, su -

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2053.png)

- sudoers file에 등록해야 sudo 가능함.

- 원래 cat /var/log/syslog → Permission denied
- 구글링 후 임시권한 만들어서 sudo cat /var/log/syslog → 가능

## sudoers file 수정 방법

- root) visudo → nano editor
    - vi /etc/sudoers

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2054.png)

- ubuntu ALL=(ALL) ALL
- Cmnd_Alias APTGET=/usr/bin/apt-get
- user1 ALL=APTGET
- 이후 Ctrl + X

## 방법 02

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2055.png)

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2056.png)

- 결과)

![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2057.png)

- sudo 그룹에 가입을 시켜준 것
    
    ![Untitled](md%20(AM%2011%20-%20)%200329ca00e61d43edba71a7d04dc0cc7d/Untitled%2058.png)
    
    - 이것을 활용하는 방법

## su와 sudo의 차이
