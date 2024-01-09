---
title: Ubuntu Client 설치
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: product1_sidebar
permalink: ubuntu_01_01.html
folder: product1
---

## Virtual Box

Virtual Box 정보 : Version 7.0
{% include inline_image.html file="01_virtualbox_ver.png" alt="virtualbox_ver" %}

## Information Virtual Machine

1. 최소 용량
- Ubuntu Client RAM : 2G (2048MB)
- Ubuntu Server RAM : 1G (1024MB)
- Windows 10 Server / Client RAM : 4G (4096MB)

{% include inline_image.html file="01_task_manager.png" alt="task_manager" %}
→ 메모리 용량을 보고 더 늘려도 상관 없음.


2. CPU 개수
- 2개

## Install Ubuntu Client

{% include inline_image.html file="01_ubuntudesk_install_1.png" alt="01_ubuntudesk_install_1" %}

{% include inline_image.html file="01_ubuntudesk_install_2.png" alt="01_ubuntudesk_install_2" %}

{% include inline_image.html file="01_ubuntudesk_install_3.png" alt="01_ubuntudesk_install_3" %}

→ ubuntu / ubuntu
호스트 이름과 도메인 이름은 동일하게

{% include inline_image.html file="01_ubuntudesk_install_4.png" alt="01_ubuntudesk_install_4" %}

{% include inline_image.html file="01_ubuntudesk_install_5.png" alt="01_ubuntudesk_install_5" %}

→ 미리 전체 크기 할당 - 실제로 물리적으로 그만큼 할당 됨.
체크하지 않을 경우 유동적으로 디스크 크기를 할당함.

{% include inline_image.html file="01_ubuntudesk_install_6.png" alt="01_ubuntudesk_install_6" %}

{% include inline_image.html file="01_ubuntudesk_install_7.png" alt="01_ubuntudesk_install_7" %}

{% include inline_image.html file="01_ubuntudesk_install_8.png" alt="01_ubuntudesk_install_8" %}

→ 가상머신이 자동으로 설치된다.

{% include inline_image.html file="01_ubuntudesk_install_9.png" alt="01_ubuntudesk_install_9" %}

→ vbox - 가상 머신
vdi - 가상 이미지
이 폴더가 하나의 가상 머신 환경을 구성함.

{% include inline_image.html file="01_ubuntudesk_install_10.png" alt="01_ubuntudesk_install_10" %}

→ 설치 완료.

{% include inline_image.html file="01_ubuntudesk_install_11.png" alt="01_ubuntudesk_install_11" %}
