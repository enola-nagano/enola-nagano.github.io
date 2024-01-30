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
