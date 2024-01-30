---
title: 1214
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_1214.html
folder: product2
---


### IP 주소 예약하기 <br/>


![1](https://user-images.githubusercontent.com/117553252/213336559-4dba92a9-8b3d-44e7-8382-2f43ae57be31.png)


R1)<br/>

#show ip dhcp binding<br/>
#clear ip dhcp binding * (임대한 정보 모두 삭제)<br/>
#show ip dhcp conflict (충돌난 주소가 있으면 충돌난 주소를 보여 줌 / 아무것도 안 나오는 것이 좋은 것.)<br/><br/>

이제는 예약을 하고 싶다. <br/>
특정 pc 에 특정 ip를 주고 싶음.<br/><br/>

L9) 192.168.10.100 을 예약하고 싶다.<br/>
W16) 223.255.255.100 을 예약하고 싶다.<br/><br/>


이때 쓰는 것,<br/>

```yaml
ip dhcp pool host1
host 192.168.10.100 255.255.255.0
client-identifier 01(pc host1의 mac 주소 / ipdonfig /all 또는 ifconfig)
```
<br/>

01을 안 붙여야 되는 경우도 있음.<br/>
일단 01을 붙이고 하고 안 되면 01을 제거하고 해 보자.<br/><br/>

조건) R1을 깔끔하게 지우고 시작하자.<br/>
release renew 하면 각각 100번대를 받아오면 성공.<br/><br/><br/>




#### 리눅스 오류 발생. <br/>

![2](https://user-images.githubusercontent.com/117553252/213336565-dbd55165-95ac-4ba0-97a2-f2d6b6a841f7.png)
<br/>
![3](https://user-images.githubusercontent.com/117553252/213336567-7827ab2a-5106-45ec-9b4e-32d2e73cb2dc.png)
<br/>

100을 줬는데 왜 102가 나왔을까 ?<br/><br/>




#### 오류 해결. <br/>

리눅스를 hardware-address 01MAC주소 라고 입력해보자.<br/>


![4](https://user-images.githubusercontent.com/117553252/213336571-b71e3a86-d8c9-4c8f-998b-585995f365af.png)
<br/>

그런데도 자꾸 에러가 발생한다.<br/>

대체 왤까 ?<br/>

 hardware-address 로 앞으로 eve에서 하자.<br/>

01을 붙이지 않아도 잘 먹기 때문에 01도 붙이지 말자.<br/><br/>




#### 찐 해결. <br/>

```yaml
윈도우) client-identifier 01MAC주소
리눅스) hardware-address MAC주소
```
<br/>


![5](https://user-images.githubusercontent.com/117553252/213336574-6c773692-da20-4c8c-a394-da94ff937c55.png)
<br/>
![6](https://user-images.githubusercontent.com/117553252/213336575-63feafbc-3340-4119-85e0-39411b4da3e5.png)
<br/><br/>

![7](https://user-images.githubusercontent.com/117553252/213336578-7e8b8d58-3cd1-4b7a-b8ff-5c42499ebd9d.png)
<br/>
Automatic은 clear로 해결<br/>
Infinite는 no로 해결<br/>
![8](https://user-images.githubusercontent.com/117553252/213336580-b51d5464-c4f6-4d1a-af88-640e4d44534e.png)
<br/><br/><br/>





### Ex. <br/>

![31](https://user-images.githubusercontent.com/117553252/213599678-2f8c0e2b-63de-4c53-a18c-7d9f7920bf57.png)
<br/>

조건<br/>
```yaml
http://www.naver.com
http://naver.com
둘 다 가능하게.
똑같이 만들 되 www을 입력 안 하면 ‘부모와 같음’이 뜸.
```
<br/><br/>

![32](https://user-images.githubusercontent.com/117553252/213599685-ab3f23f4-41e5-4cde-be3a-aaf8ea4bc070.png)
<br/>

#### 실습 <br/>

![33](https://user-images.githubusercontent.com/117553252/213599687-227bb6ed-a8a5-48bc-8f3c-4368380fd00a.png)
<br/><br/>
![34](https://user-images.githubusercontent.com/117553252/213599688-2c42670b-1fdd-469f-b9c5-c93e6a119d4e.png)
<br/><br/>
host2로 하니 자꾸 예약 주소가 안 돼서 host3으로 함 <br/>
-> 해결 방법 <br/>
conf t > no ip dhcp pool host2 > 해당 컴으로 가서 ip 반납 해야 됨.<br/>
이때까지 주소를 반납하지 않아서 자꾸 exist로 떴다.<br/><br/>

![35](https://user-images.githubusercontent.com/117553252/213599690-75e82fb3-4240-45a6-8ceb-05b950b59cff.png)
<br/><br/>


#### 오류 해결 <br/>

![36](https://user-images.githubusercontent.com/117553252/213599691-84d3fa50-d2ed-441e-8e89-30e3b5756c6e.png)
<br/><br/>
하 이거 설정 순서를 잘못해서 자꾸 생기나?<br/>
뭔가 짜구 분명 지웠는데 자꾸 생겼다.<br/><br/>

#### 오류 해결 - 결과 <br/>

![37](https://user-images.githubusercontent.com/117553252/213599692-74820899-20b5-4760-8a7b-165cf5a94dc9.png)
<br/><br/>
![38](https://user-images.githubusercontent.com/117553252/213599695-58c08da2-a09a-403a-bbe3-786f6c3f5084.png)
<br/><br/>
![39](https://user-images.githubusercontent.com/117553252/213599696-39f6feea-1637-4394-81e5-0370d975574d.png)
<br/><br/>
![40](https://user-images.githubusercontent.com/117553252/213599698-363683ec-9911-402a-acd4-b6a55a529a7f.png)
<br/><br/>


#### 결과 <br/>

![41](https://user-images.githubusercontent.com/117553252/213599700-096fd0fa-690d-4f91-9791-6eaafc76cefa.png)
<br/><br/>
![42](https://user-images.githubusercontent.com/117553252/213599704-edcfd655-93e4-4bae-b2d5-387497e2caa2.png)
<br/><br/>










### 4단계 (Broadcast) <br/>



DHCP 서버와 Client가 어떻게 주소를 주고 받는지를 이해하자. <br/>

DHCP 서버는 환경에 따라 구성하는 방법만 다르고 나머지는 다 똑같고 매번 나오는 아이임.<br/>
DHCP Client를 킴 (집 컴을 킴) <br/>
키면 DHCP 를 찾는다. → DHCP Discover<br/>
통신하면 전부 브로드캐스트로 전송함.<br/>
DHCP 서버가 돌아가고 있기 때문에 받고, 브로드캐스트해서 또 보낸다.<br/>
여기까지가 컴을 키면 되는 것들.<br/><br/>

DHCP Offer → 서버가 브로드캐스트로 어떤 주소를 쓸래 ? 라고 제안하는 것.<br/>
DHCP Request → offer 한 그 주소를 내가 쓰겠다고 요청하는 것.<br/>
그럼 마지막에 DHCP가 ACK을 함.<br/>
확인 메시지임.<br/>
DHCP ACK<br/><br/>


순서) 항상 이 4단계를 거침.<br/>

```yaml
1. DHCP Discover<br/>
2. DHCP Offer<br/>
3. DHCP Request<br/>
4. DHCP ACK<br/>
```

이 4단계는 전부 브로드 캐스트를 사용함.<br/><br/>


L2 2계층 브로드캐스트 주소 : FFFF.FFFF.FFFF (모든 MAC 주소를 의미함)<br/>
스마트폰 제조 번호랑 비슷함. 전 세계에서 유일함.<br/>

L3 3계층 브로드캐스트 주소 : 255.255.255.255<br/>
2층을 거쳐서 3층으로 올라감. L2 → L3<br/><br/>


#### 1. DHCP Discover <br/>
1. DHCP Discover → 컴 켰을 때<br/>
출발지 : MAC1, 0.0.0.0, 목적지 : FFFF.FFFF.FFFF, 255.255.255.255<br/><br/>

#### 2. DHCP Offer <br/>
2. DHCP Offer → 서버가 이제 오퍼한다.<br/>
출발지 : MAC2, 192.168.10.254, 목적지 : FFFF.FFFF.FFFF(브로드캐스트 사용), 255.255.255.255(아직 할당 안 함 그렇기 때문에 다 받아야 함) / 여기서는 단지 dhcp 서버만 알고 있음., Payload(192.168.10.10/클라이언트에서 제안할 ~ 것들 / 실제 데이터임)<br/><br/>

#### 3. DHCP Request <br/>
3. DHCP Request → 이 정보를 쓰겠다는 것.<br/>
출발지 : MAC1, 0.0.0.0, 목적지 : FFFF.FFFF.FFFF, 255.255.255.255, Payload : 위와 같음.(192.168.10.10)<br/><br/>

#### 4. DHCP ACK <br/>
4. DHCP ACK → 서버에서 ack함.<br/>
출발지 : MAC2, 192.168.10.254, 목적<br/><br/>

payload : 실제 보내려고 하는 정보.<br/><br/>

이 4단계는 모두 브로드캐스트를 이용한다.<br/>
목적지 부분이 모두 브로드캐스트를 사용하는 것을 발견할 수 있다.<br/>
(FFFF.FFFF.FFFF / 255.255.255.255)<br/><br/>

이 4단계를 거쳐야 IP주소가 생기는 것임.<br/><br/>

즉, 4단계를 지나기 전에는 ip주소가 없는 것임.<br/>
4단계를 거쳐야 ip 주소가 생김.<br/><br/>

여기서 중요한 것,<br/>
클라이언트와 서버가 통신을 할 때는 모두 `브로드캐스트`를 사용한다는 것.<br/><br/>


![14](https://user-images.githubusercontent.com/117553252/213336593-c89c9690-50ca-483f-9164-97f26bfadc43.png)
<br/>

이제 이것을 찾아 보는 것임.<br/>
각각의 출발지와 목적지 페이로드까지 찾아보자.<br/><br/>


L10)<br/>
주소 반납, 그리고 오른쪽 마우스 capture<br/>

![15](https://user-images.githubusercontent.com/117553252/213336594-2fbeea12-5b5f-415d-9a79-c9b8c815b940.png)
<br/>

누가 지나갔는지는 첫 번째 칸에서 확인함.<br/>
클릭하면 두 번째 칸에서 안에 세부적인 정보를 제공 함.<br/>
또 두 번째 칸에서 선택하면 이것을 바이트로 바꿔주는 것을 세 번째 칸에서 확인할 수 있음.<br/><br/>


이 상태에서 주소요청을 한다. 그러면 그 4단계가 이루어질텐데 그것을 찾고 discover을 클릭하면 세부 내용을 확인할 수 있다.<br/><br/><br/><br/>



### 실습 과정.<br/>

#### 1. 주소 요청<br/>

![16](https://user-images.githubusercontent.com/117553252/213336596-b9fe6028-868e-4987-86be-99484ec26f0b.png)
<br/>

#### 2. DHCP Discover <br/>

![17](https://user-images.githubusercontent.com/117553252/213336599-1c27b6ae-f936-478b-ad78-dea3775416c8.png)
<br/>
![18](https://user-images.githubusercontent.com/117553252/213336603-a434ba4d-c3d0-406c-a10d-e67a8c71850e.png)
<br/> 출발지 목적지까지 확인 완.<br/>

#### 3. DHCP Offer <br/>

![19](https://user-images.githubusercontent.com/117553252/213336605-49f9c436-bfbd-4363-a4d7-30fd6e50cc58.png)
<br/>

![20](https://user-images.githubusercontent.com/117553252/213336608-e7ae77bd-f27d-4f3d-93cd-56844be84135.png)
<br/> 출발지 목적지까지 확인 완.<br/>
페이로드는 어디에 ? <br/>

![21](https://user-images.githubusercontent.com/117553252/213336610-3786185c-4d3b-498b-8895-695f369e25ea.png)
<br/>
이건가 아무튼 확인 완.<br/>

#### 4. DHCP Request <br/>

![22](https://user-images.githubusercontent.com/117553252/213336612-2b5725ea-2013-481d-b1bb-261b55485e62.png)
<br/>
![23](https://user-images.githubusercontent.com/117553252/213336615-10b71516-df7c-41f5-be6a-011ae6d207d5.png)
<br/>
출발지 목적지까지 확인 완.<br/>
그런데 request의 payload의 값이 offer와 다름.<br/>

왜 그런 걸까 ?
<br/>

#### 5. DHCP ACK <br/>

![24](https://user-images.githubusercontent.com/117553252/213336618-e40d4a9f-7d07-4fe2-8086-3ccd119510b6.png)
<br/>
![25](https://user-images.githubusercontent.com/117553252/213336619-e3413c65-5e81-4304-920c-1ecb7a7863e4.png)
<br/>
![26](https://user-images.githubusercontent.com/117553252/213336620-7dd47be9-6345-4f77-a9b5-5678a541f019.png)
<br/>
출발지 목적지 payload까지 확인 완.<br/>

<br/><br/>

### bootp.dhcp <br/>

![27](https://user-images.githubusercontent.com/117553252/213336622-be0d1484-1360-4b1d-b200-96f89e7d1e48.png)
<br/>

![28](https://user-images.githubusercontent.com/117553252/213336623-2dea4c23-4f1d-4f7d-b0eb-83d69fa9ccde.png)
<br/>
e0/1에서 clear 하고 L10에서 주소 반납 / 주소 받고 S6 샤크에서 확인. <br/>

![29](https://user-images.githubusercontent.com/117553252/213336625-104d1c51-6e1d-497e-ad70-5bb31d41cbe7.png)
<br/>

![30](https://user-images.githubusercontent.com/117553252/213336627-18f7ea12-bca4-463b-b5ff-e2db3714687f.png)

원래는 Destination 에 255.255.255.255가 떠야 하는 데 가상화 환경이라 그런지 주소가 오히려 잡혀버림.<br/><br/><br/>









- 모든 것은 다 기본 값으로 하면 된다. <br/>

- 압축풀기 > 압축 푼 파일에 들어가서 기존 것을 다 복사. <br/>

- C > Progra mFiles x86 >SupperPUtty<br/>
SUperputty `관리자 권한`으로 실행<br/>

![9](https://user-images.githubusercontent.com/117553252/213336581-01fc56f6-011e-4814-b8cd-921efa123508.png)
<br/>
![10](https://user-images.githubusercontent.com/117553252/213336583-27e5d32d-0a47-4a21-a6ce-3b2edb8f0cfe.png)
<br/> 이것만 체크하고 나버지는 체크 해제하기 <br/><br/>
![11](https://user-images.githubusercontent.com/117553252/213336585-2f00d899-82ed-43ad-a480-b3676158dc5f.png)
<br/>
![12](https://user-images.githubusercontent.com/117553252/213336586-b739134c-344b-4b3b-8069-c062660d1ea1.png)
<br/>
![13](https://user-images.githubusercontent.com/117553252/213336590-a2ed11f3-7a00-406a-92ff-fd5966a2d711.png)
<br/><br/>
