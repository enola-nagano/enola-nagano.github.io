---
title: Ubuntu Terminal 기본 구성
keywords: ubuntu client
summary: "Ubuntu Client Terminal"
sidebar: systemoperation_sidebar
permalink: ubuntu_02_10.html
folder: product1
---

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



