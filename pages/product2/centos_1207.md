---
title: 1207
keywords: ubuntu client
summary: "Ubuntu Client 설치 방법"
sidebar: systemoperation_sidebar
permalink: centos_1207.html
folder: product2
---


### Ex01. Router 4개, 리눅스 4개를 서로 연결해보자.


![1](https://user-images.githubusercontent.com/117553252/211179350-a901a0bb-794d-4af1-b361-b31ecde440d6.png)




1. EVE에서 라우터 4개랑 리눅스 4개를 만든다.
  - Router 설정은 Node > Cisco IOL > Number: 4, Serial: 1.
  - Linux 설정은 Node > Linux > Number: 4, Image: linux-5, RAM: 2048.

1. 장치들을 연결하기.
  - 모든 장치 드래그 > 오른쪽 마우스 > Start Selected

1. 라우터에 주소 부여하기.

ex. Router 1

  ```yaml

     > enable
     # conf t

     (config)# interface e0/0
     (config-if)# ip address 192.168.10.2 255.255.255.0
     (config-if)# no shutdown
     (config-if)# exit

     (config)# interface s1/0
     (config-if)# ip address 1.1.1.1 255.0.0.0
     (config-if)# no shutdown
     (config-if)# end

     # show ip int brief

   ```




1. 리눅스에 주소 부여하기.



ex. Linux5

  ```yaml

      # cd /etc/sysconfig/network-scripts
      # ls
      # vi ifcfg-eth0
      dhcp를 static으로 수정하고 아래 3줄은 추가하기.
      IPADDR=192.168.10.1
      NETMASK=255.255.255.0
      GATEWAY=192.168.10.2
      중립상태(ESC 누르기) -> 저장 후 vi 종료 (:wq 입력)

      # ifdown eth0
      # ifup eth0
      # ifconfig
      # netstat -rn

   ```




1. 라우터에 지도 만들어주기.


![2](https://user-images.githubusercontent.com/117553252/211179386-8710c3a8-1872-4413-b780-1c761ccb697f.png)



ex. Router1) 직접 물리적으로 연결 된 네트워크는 2개이기 때문에 5개의 지도를 만들어 줘야 함.

  ```yaml

    > enable
    # conf t
    (config)# ip route 2.0.0.0 255.0.0.0 1.1.1.2
    (config)# ip route 3.0.0.0 255.0.0.0 1.1.1.2
    (config)# ip route 192.168.20.0 255.255.255.0 1.1.1.2
    (config)# ip route 192.168.30.0 255.255.255.0 1.1.1.2
    (config)# ip route 192.168.40.0. 255.255.255.0 1.1.1.2

   ```




1. 연결이 잘 되었는지 ping으로 확인하기.


ex. Linux 5에서 ping으로 모든 IP 주소를 쳐 보자.
     모두가 연결이 된다면 모두가 잘 연결 되었다는 뜻임!


![3](https://user-images.githubusercontent.com/117553252/211179396-785fb406-a638-491d-a7ff-a452892be099.png)





### Ex02. Router 4개, 리눅스 4개를 서로 연결해보자.


![4](https://user-images.githubusercontent.com/117553252/211179403-eb98e28c-7dc5-4bb4-90e1-713ad47f00cb.png)



1. EVE에서 라우터 4개랑 리눅스 4개를 만든다.
 - Router 설정은 Node > Cisco IOL > Number: 4, Serial: 1.
 - Linux 설정은 Node > Linux > Number: 4, Image: linux-5, RAM: 2048.

1. 장치들을 연결하기.
 - 모든 장치 드래그 > 오른쪽 마우스 > Start Selected

1. 라우터에 주소 부여하기.

 ex. Router 1

  ```yaml

    > enable
    # conf t

    (config)# interface e0/0
    (config-if)# ip address 192.168.10.2 255.255.255.0
    (config-if)# no shutdown
    (config-if)# exit

    (config)# interface s1/0
    (config-if)# ip address 1.1.1.1 255.0.0.0
    (config-if)# no shutdown
    (config-if)# end

    # show ip int brief

   ```

1. 리눅스에 주소 부여하기.

  ex. Linux5

   ```yaml

     # cd /etc/sysconfig/network-scripts
     # ls
     # vi ifcfg-eth0
     dhcp를 static으로 수정하고 아래 3줄은 추가하기.
     IPADDR=192.168.10.1
     NETMASK=255.255.255.0
     GATEWAY=192.168.10.2
     중립상태(ESC 누르기) -> 저장 후 vi 종료 (:wq 입력)

     # ifdown eth0
     # ifup eth0
     # ifconfig
     # netstat -rn

   ```

1. 라우터에 지도 만들어주기.

  ex. Router1) 직접 물리적으로 연결 된 네트워크는 2개이기 때문에 5개의 지도를 만들어 줘야 함.

    ```yaml

     > enable
     # conf t
     (config)# ip route 126.0.0.0 255.0.0.0 1.1.1.2
     (config)# ip route 191.255.0.0 255.255.0.0 1.1.1.2
     (config)# ip route 2.0.0.0 255.0.0.0 1.1.1.2
     (config)# ip route 3.0.0.0 255.0.0.0 1.1.1.2
     (config)# ip route 223.255.255.0 255.255.255.0 1.1.1.2

     ```






1. 연결이 잘 되었는지 ping으로 확인하기.

ex. Linux 5에서 ping으로 모든 IP 주소를 쳐 보자.
    모두가 연결이 된다면 모두가 잘 연결 되었다는 뜻임!




### 라우터에서 ip route 확인하기.


```yaml

        > enable
        # show ip route

```

또는

```yaml

    > enable
    # conf t
    (config)# do show ip route

```



- C : 물리적으로 직접 붙어 있는 것.
- L : 자신의 라우터 주소를 확인할 대 쓰는 것.
- S : 수동으로 직접 지도 만들어 준 것.

ex.
![3](https://user-images.githubusercontent.com/117553252/211134885-1c52c464-b1be-4d91-a0d5-25c9c0a4bd7d.png)




### 참고.

PC, Router 둘 중에 어느 것을 먼저 주소를 부여하든지 상관 없음.
단, 주소가 정확하게 들어가야 함. (주소 주는 것이 제일 중요)
여기서 오타가 나게 되면 찾기가 힘들기 때문에 주의하자.

한쪽에만 ping으로 다 연결이 된다면 다 연결이 되었다는 뜻.






### no ip route - ip route 삭제하기


- ip route 삭제하는 방법
```yaml
ex.
  no ip route 2.0.0.0 255.0.0.0 1.1.1.2
  no ip route 3.0.0.0 255.0.0.0 1.1.1.2
  no ip route 126.0.0.0 255.0.0.0 1.1.1.2
  no ip route 191.255.0.0 255.255.0.0 1.1.1.2
  no ip route 223.255.255.0 255.255.255.0 1.1.1.2
```
![5](https://user-images.githubusercontent.com/117553252/211134993-2dca573c-457c-40bb-97f0-af9c5ee33175.png)




- Default Route : 경로를 찾지 못하는 모든 네트워크의 경로를 미리 정해놓는 것.


우선, 지정되어 있는 ip route를 지우고 default-gateway를 설정해주자.



### 01. 라우터 ip route 지우기/삭제.


```yaml

> enable
# show run

```

![6](https://user-images.githubusercontent.com/117553252/211135083-43facdfb-60ea-4e0b-94b8-9f1673aad5dd.png)




- show run에서
  Space Bar) 한 페이지씩 넘기
  Enter Key) 한 줄씩 넘기기

- 메모장 켜서 ip route 주소 복붙해서 맨 앞에 'no' 붙이기

```yaml

ex.
  no ip route 2.0.0.0 255.0.0.0 1.1.1.2
  no ip route 3.0.0.0 255.0.0.0 1.1.1.2
  no ip route 126.0.0.0 255.0.0.0 1.1.1.2
  no ip route 191.255.0.0 255.255.0.0 1.1.1.2
  no ip route 223.255.255.0 255.255.255.0 1.1.1.2

```


- 그리고 전체 복사해서 Router에 입력해주기.



```yaml

#conf t
(config-if)# 오른쪽 마우스 클릭 ! (자동으로 붙여넣기 됨)

```




### 02. 라우터 Default-Gateway 설정 - 방법 01.
  : 이웃의 주소를 쓰는 경우.

![7](https://user-images.githubusercontent.com/117553252/211135085-5f1a9278-c09d-41c2-a440-9970edfd8d5e.png)




![8](https://user-images.githubusercontent.com/117553252/211135090-47950d30-da7e-43f1-9af5-450637b78b2f.png)




위 사진을 예로 들면,

R1의 경우)
  ip route 0.0.0.0 0.0.0.0 1.1.1.2 찾는 목적지가 조건에 없으면 1.1.1.2로 (모르는 건 전부 오른쪽으로)

R3의 경우)
  ip route 0.0.0.0 0.0.0.0 2.1.1.1 찾는 목적지가 조건에 없으면 2.1.1.1로 (모르는 건 전부 왼쪽으로)

R4의 경우)
  ip route 0.0.0.0 0.0.0.0 3.1.1.1 찾는 목적지가 조건에 없으면 3.1.1.1로

R2) 이것만 주의해 주면 됨.




그렇다면 R2는 어떻게 해야 될까?

  -> R2는 1개만 Default-Gateway를 설정해주고, 나머지는 다 직접 지정해줘야 한다.



정리를 해 보자면,

틀린 예시
  ```yaml
  ip route 0.0.0.0 0.0.0.0 1.1.1.1
  ip route 0.0.0.0 0.0.0.0 2.1.1.2
  ip route 0.0.0.0 0.0.0.0 3.1.1.2
  ```

올바른 예시
  ```yaml
  ip route 0.0.0.0 0.0.0.0 1.1.1.1
  ip route 191.255.0.0 255.255.0.0 2.1.1.2
  ip route 223.255.255.0 255.255.255.0 3.1.1.2
  ```



### 03. 라우터 Default-Gateway 설정 - 방법 02.
  : 자신의 로컬 주소를 쓰는 경우.


![11](https://user-images.githubusercontent.com/117553252/211135252-bcc804e9-654a-4762-aecc-0d4580ef2c81.png)



- 가끔 라우터 주소가 멀리 떨어져 있는 경우 주소를 모를 때,
  이웃 주소 자리에 자신의 인터페이스를 지정할 수도 있다.

- R1) PC쪽만 잡아주자.
  ip route 126.0.0.0 255.0.0.0 s1/0(자기자신의 s임. 왼쪽 s1/0임)
  ip route 191.255.0.0 255.255.0.0 s1/0
  ip route 223.255.255.0 255.255.255.0 s1/0
  이런식으로 설정해 볼 수 있다.

- 즉 ip route 유형이 2가지라는 뜻.
- 하나는 이웃의 주소를 쓰는 경우 나머지 하나는 자신의 로컬 주소를 쓰는 경우.


![12](https://user-images.githubusercontent.com/117553252/211135251-549304df-3a66-46a9-a793-7e8b0046e0a4.png)





### 04. Looping (루핑)


- R2)
    no ip route 191.255.0.0 255.255.0.0 2.1.1.2
    no ip route 223.255.255.0 255.255.255.0 3.1.1.2

- 루핑 : 뺑글뺑글 도는 것.

- 루핑 만들어서 Linux5 ping한 번 해 보기 191.255.255.1
  그래서 ip 주소를 잘 못 준 것은 꼭 지워줘야 함.
  Linux5에서는 계속 주고 받고 하다가 죽어버림.


![9](https://user-images.githubusercontent.com/117553252/211135093-56b73d30-d30c-47b5-a31e-65edf0670255.png)




![10](https://user-images.githubusercontent.com/117553252/211135094-93810d3e-4246-4462-913f-350992288a80.png)


에러가 뜬 것을 확인할 수 있다.




### 05. 참고

```yaml
# show ip int brief
```

```yaml
# show run
```

```yaml
# show ip route
```


이 명령어들은 자주 사용하니 꼭 기억해두자.





### 06. 모든 것을 활용하여 예제 풀어보기.


- 아까 예제에서 ip route를 삭제하자.


![13](https://user-images.githubusercontent.com/117553252/211179638-c5f5750d-3176-4f44-9f17-7b647e02b7cd.png)




#### Ex.

![14](https://user-images.githubusercontent.com/117553252/211179639-bdb50176-8735-4a32-8fd3-942eda7ef836.png)


- hop : 라우터를 거치는 숫자
- R3에서 next-hop 주소는 2.1.1.1



#### R1.

![15](https://user-images.githubusercontent.com/117553252/211179640-b8da94da-0bfe-4a6b-9e28-f8592c87a78f.png)


- R1) ip route 명령어 1개, default network 사용하기
```yaml
>enable
#conf t
ip route 0.0.0.0 0.0.0.0 1.1.1.2
```



#### R2.

![16](https://user-images.githubusercontent.com/117553252/211179641-e3c77cee-a402-487a-9306-eb088b830fec.png)


- R2) ip route 명령어 3개만 사용하기,
  next-hop 주소 1개 / Local Interface 1개 / default network 1개(next-hop 사용)

```yaml
>enable
#conf t
ip route 192.168.10.0 255.255.255.0 1.1.1.1
ip route 223.255.255.0 255.255.255.0 s1/1
ip route 0.0.0.0 0.0.0.0 2.1.1.2
```



#### R3.

![17](https://user-images.githubusercontent.com/117553252/211179642-c6436aa6-3272-432b-8bfc-faeac0424e7f.png)


- R3) ip route 명령어 5개만, next-hop 주소 사용하기

```yaml
>enable
#conf t
ip route 192.168.10. 255.255.255.0 2.1.1.1
ip route 126.0.0.0 255.0.0.0 2.1.1.1
ip route 233.255.255.0 255.255.255.0 2.1.1.1
ip route 1.0.0.0 255.0.0.0 2.1.1.1
ip route 3.0.0.0 255.0.0.0 2.1.1.1
```



#### R4.

![18](https://user-images.githubusercontent.com/117553252/211179643-d0154e2c-bcc6-4b2c-b885-9f17d784ad17.png)


- R4) ip route 명령어 5개만, Local Interface 주소 사용하기

```yaml
>enable
#conf t
ip route 192.168.10.0 255.255.255.0 s1/0
ip route 126.0.0.0 255.0.0.0 s1/0
ip route 191.255.0.0 255.255.0.0 s1/0
ip route 1.0.0.0 255.0.0.0 s1/0
ip route 2.0.0.0 255.0.0.0 s1/0
```



#### 결과.


##### R1.

![19](https://user-images.githubusercontent.com/117553252/211179644-f98c91f8-a7d1-4891-a903-20bcf47a4f80.png)

- `S*` 의 의미 : 지도 찾아보고 목적지가 없으면 여기로 보내는 경우, 디폴트 네트워크
- `via` : ~로 보내라.
- S = 1개



##### R2.

![20](https://user-images.githubusercontent.com/117553252/211179645-c6bc8eca-916b-46fe-991e-223aae8fc126.png)

- S = 3개 , via 2개 / directly connected 1개



##### R3.

![21](https://user-images.githubusercontent.com/117553252/211179646-c17eaaab-ae6c-44f9-8d70-640fba9bef21.png)

- S = 5개, 모두 via가 붙어있는 것을 확인할 수 있다.



##### R4.

![22](https://user-images.githubusercontent.com/117553252/211179647-9809b3e7-2765-4711-ad4e-d6a978621b71.png)

- S = 5개, directly connected로 된 것을 확인할 수 있다.




### 07. Satic Route에 대하여.


- `스텁` : 끝 단위에 있는 것.


- Static Routing 구성
    
      - 네트워크 관리자가 수동으로 직접 목적지 별로 지정해 주는 경로를 의미
    
      - Static Routing Protocol은 외부 네트워크와 연결되는 경로가 하나뿐인 스텁(Stub) 네트워크에서 많이 사용한다. 
    
    (참고 : Stub = 끝 단위에 있는 것.)
    


- Static Routing 장점
    
      - 운영자가 경로를 직접 입력하기 때문에 라우터는 머리를 쓰지 않아 CPU상에 부담이 없다.
    
      - 라우팅 테이블 교환 및 업데이트를 안 하기 때문에 라우터들 간에 대역폭을 낭비하는 일이 없다.
    
      - 보안성이 있다.
    


- Static Routing 단점
    
      - 라우터가 어떻게 연결되어 있는지를 알아야 한다.
    
      - 한 네트워크에 회선이 추가될 경우, 추가된 경로를 설정해야 한다.
    
      - 동적 라우팅과는 달리 회선에 문제가 생겨도 다른 길을 동적으로 찾지 못하고 계속 불능이 된다.
    

    
- Static Route 설정하기.
    
     Router#(config) ip route [destination_network] [subnet_mask] [next_hop_address] [distance]
    
          - destination_network : 목적지 네트워크 지정 (주의: 네트워크 주소임) - 가고 싶은 곳의 주소
    
     - subnet_mask : 목적지 네트워크의 서브넷 마스크 지정 - 서브넷 마스크의 주소
    
     - next_hop_address : 목적지 네트워크로 가기 위한 넥스트 홉 어드레스 지정 - 통해 갈 곳의 주소 (넥스트 홉의 경우, 1홉을 건너뛴 홉, 즉 자신의 건너편 라우터라고 생각하면 된다. 인터페이스를 쓸 경우, 자신의 인터페이스를 적는다.)
    
     - distance : 라우팅 정보의 가치로서 커지면 커질수록 가치가 떨어지게 되며, 명시하지 않아도 된다. (디폴트 값은 1임)

