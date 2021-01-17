Let's Study Network 🔌📡
--------

#### Reference

[[youtube] 네트워크 기초(개정판)](https://youtube.com/playlist?list=PL0d8NnikouEWcF1jJueLdjRIC4HsUlULi)



### 캡슐화 형식

| **캡슐 이름** | 헤더     | 페이로드              | ~~푸터(잘 사용하지 않음)~~ |
| ------------- | -------- | --------------------- | -------------------------- |
| Frame         | ethernet | IPv4, TCP, http, data |                            |
| Packet        | IPv4     | TCP, http, data       |                            |
| Segment       | TCP      | http, data            |                            |

* 캡슐화를 할 때에는 높은 계층이 안쪽에, 낮은 계층이 (반드시) 바깥에 있어야 함

* 같은 계층이 연달아 오는 것은 가능

  예) 2계층 [3형식 [3형식 [4형식] ] ]

* 범용적으로는 걍 패킷이라고 말하고 좀더 명확히 말할 때는 캡슐 이름을 맞춰서 말함



### 네트워크 계층 구조

1. OSI 7 계층

2. TCP/IP 프로토콜 4계층

   ![다운로드](https://user-images.githubusercontent.com/41130448/104002032-d81ae780-51e3-11eb-938f-126a0641d342.png)



### 2계층 [Ethernet protocol]

* 같은 LAN 대역에서 존재하는 노드를 연결

  → **같은 네트워크 대역 간의 통신**에만 사용

  → 다른 네트워크와 통신하려면 3계층 프로토콜이 필요하다.

* 두 가지 목적을 가지고 있음

  1. 흐름 제어 : 데이터 전달

  2. 오류 제어

* 물리적 주소를 사용 : **MAC 주소** 

  - [ **OUI** (제조사 ID by IEEE) | **고유번호** (by 제조사) ]

    [0x000000 | 0x000000] ->총 16진수 12개, 6byte

  - 쉽게 바뀔 수 없으며, 고유한 주소

* 이더넷 프로토콜 구조 

  > | byte    | content               | content   |
  > | ------- | --------------------- | --------- |
  > | 0 byte  | dest addr             | dest addr |
  > | 4 byte  | dest addr             | src addr  |
  > | 8 byte  | src addr              | src addr  |
  > | 12 byte | protocol type (3계층) |           |

  - Protocol type은 다음 계층 타입을 미리 알려주는 것이다.

    IPv4인 경우 0x0800

    ARP인 경우 0x0806 

  - addr은 맥 주소이다.

  - 총 14 byte = 6 byte(dest addr) + 6 byte(src addr) + 2 byte(type)



### 3계층 프로토콜에 관한 건에 대하여

- 다른 LAN 대역에서 존재하는 노드를 연결

  → **다른 네트워크 대역 간의 통신**에 사용 (LAN과 LAN 연결, LAN 내에서는 스위치 사용)

  → 3계층 장비가 필요하다.

  1. IPv4 주소 : 현재 PC에 할당된 IP 주소

  2. 서브넷 마스크 : IP주소에 대한 네트워크의 대역을 규정하는 것

     네트워크 대역을 쪼개주며, IP 주소와 함께 쓴다.

  3. 게이트웨이 주소 : 외부와 통신할 때 사용하는 네트워크의 출입구

- 3계층 프로토콜

  1. ARP : IP 주소를 이용해 MAC 주소를 알아옴
  2. IPv4 : WAN에서 통신할 때 사용
  3. ICMP : 서로가 통신되는지 확인할 때 사용

- IP 주소의 분류

  1. Classful IP

     - 앞에서부터 잘라서 분류

     - 낭비가 심한 분류, class A~E로 나눔

     - 현재는 안씀

       > 0xxxx....
       >
       > → 0.0.0.0 ~ 127.255.255.255
       >
       > ​	127.~ : 네트워크 대역 (128개가 최대)
       >
       > ​	~.255.255.255 : pc의 개수 (개많음)
       >
       > 10xxx....
       >
       > 110xx....
       >
       > 1110x....
       >
       > 1111.....

  2. Classless IP

     IP주소에서 아무데나 기준으로 자르는 것

     - 서브넷 마스크

       - classful한 네트워크 대역을 나눠주는 데 사용

       - 어디까지가 네트워크 대역 구분이고 어디서부터 호스트 구분인지 지정

       - 2진수 표기 시 1로 시작, [11...11][00...00\]의 형식

         (예) 255.255.255.192

         192 = **11**<u>000000</u> 0과 1 사이를 기준으로 자른다.

     - 그래도 아이피가 부족해...

  3. 사설 IP와 공인 IP

     - 외부에서는 사설 IP 대역이 보이지 않는다. (NAT table을 통해서) 공유기가 처리해줌 
     - 내부에서 나간 적 없는 패킷에 대해 응답이 들어오면 (즉, NAT table에 기록되어 있지 않은 패킷이 들어오면) 공유기는 네트워크 내부로 뭔가 전달하지 않고 거기서 끝낸다.
     - 따라서 서버는 사설 IP를 쓰지 않는다. 왜냐면 외부에서 보이지 않기 때문이다.
     - 사설 IP가 외부에서 들어오는 뭔가를 받고 싶다면 포트 포워딩을 해줘야 한다. ("해당 공인 IP의 특정 포트로 들어오면 다 나에게 갖다줘")

<img src="https://user-images.githubusercontent.com/41130448/104004667-59c04480-51e7-11eb-92d9-f61b1448e0d2.png" alt="20210108_192551" style="zoom: 33%;" />

- 특수한 IP 주소

  1. wild card : 나머지 모든 IP

     0.0.0.0/0 

  2. 나 자신을 나타내는 주소

     127.0.0.1

  3. 게이트 웨이 주소 : 어딘가로 가려면 일단 여기로

     192.168.0.1

- 정리

  | 나를 구분                  | 외부 세상으로 가는 문 |
  | -------------------------- | --------------------- |
  | IP 주소<br />서브넷 마스크 | 게이트웨이            |



### 3계층 [ARP protocol]

- 같은 네트워크 대역에서 통신을 하기 위해 필요한 **MAC 주소**를, **IP 주소를 이용**해서 알아오는 프로토콜

- 같은 네트워크 대역 통신도 데이터 주고 받을 때 **캡슐화**를 통해 진행한다. 따라서 **IP 주소와 MAC 주소가 모두 필요**하다.

  이 때, IP 주소만 알고 **MAC 주소를 알지 못해도** ARP를 이용하면 통신이 가능하다.

- 보안상 좀 중요하다. (공격할 수 있음, 요즘엔 대부분 막히긴 했지만..)

- ARP Protocol 구조

  > | byte    | content                                  | content                            |
  > | ------- | ---------------------------------------- | ---------------------------------- |
  > | 0 byte  | HW type (2계층 protocol type)            | protocol type                      |
  > | 4 byte  | MAC주소 길이 / Protocol 주소 길이 (IPv4) | Opcode                             |
  > | 8 byte  | **src HW addr** (MAC 주소)               | **src HW addr**                    |
  > | 12 byte | **src HW addr**                          | <u>src protocol addr</u> (IP 주소) |
  > | 16 byte | <u>src protocol addr</u>                 | **dst HW addr** (MAC 주소)         |
  > | 20 byte | **dst HW addr**                          | **dst HW addr**                    |
  > | 24 byte | <u>dst protocol addr</u> (IP 주소)       | <u>dst protocol addr</u>           |

  - HW type에서는 2계층의 protocol type을 쓴다. 보통 이더넷이며, 이 경우 0x0001을 쓴다.
  - protocol type은 IPv4를 지칭한다. 이 경우 0x0800을 쓴다.
  - MAC 주소 길이에는 0x06, protocol 주소 길이에는 0x04를 쓴다.

- IP는 아는데 MAC 주소 모르는 애의 MAC 찾기 (내부망에서)

  1. 이더넷 껍데기에서 도착 주소를 모르니까, 0xFFFFFFFFFFFF (1로 꽉채워서)로 쓴다. 이렇게 보내면 나중에 broadcast된다. 
  2. ARP 덩어리에서 도착 HW 주소를 죄다 0으로 채운다. IP 주소는 알기 때문에 제대로 쓴다.
  3. 스위치에서는 패킷을 받아보고 2계층 껍데기만 까본다. broadcast이기 때문에 같은 네트워크 상의 모든 노드에게 죄다 보낸다.
  4. 각 노드가 패킷 받아서 dst 주소 111..11이기 때문에 이더넷 껍데기를 까본다. ARP덩어리 내에서 IP 주소를 확인한다. 자기 IP랑 일치하지 않으면 패킷 버린다. 
  5. 일치하면 응답을 보낸다. 이 때는 처음 보낸 애의 MAC, IP 주소 다 알기 때문에 ARP덩어리와 이더넷 껍데기에 잘 싸서 돌려보낸다.
  6. 처음 보낸애가 응답을 받으면 응답 패킷 안에 있는 MAC 주소를 ARP 캐시 테이블에 등록한다.

- ARP 캐시 테이블이란

  - 나와 통신했던 컴퓨터들의 주소를 저장하는 곳
  - 일정 시간 지나면 없어짐
  - 수동으로 등록시 영구하게 남는다.

- ARP 브로드캐스트

  - 내부망 바깥으로 나가지 않고, 그 안의 모든 노드에게만 보냄

    > | Ethernet | ARP     | Padding       |
    > | -------- | ------- | ------------- |
    > | 14 byte  | 28 byte | 00000.....000 |
    >
    > - frame의 최소 크기는 60byte인데, 이를 채우기 위해서 padding에 아무거나 붙인다.
    > - frame의 최대 크기는 일반적으로 1514byte 이다. 
    > - 가끔 패킷 캡쳐를 했을 때 padding 없이 짤막한 frame이 있을 수 있는데, 이는 2계층 위쪽에서 패딩 붙이기 직전에 캡처가 된 것임



### 3계층 [IPv4 protocol 구조]

- 네트워크 상에서 데이터 교환 위한 프로토콜

- **데이터의 정확한 전달을 보장하지 않음**

  따라서 중복된 패킷을 전달하거나, 패킷의 순서를 잘못 전달할 가능성도 있다.

  (이를 악의적으로 이용하면 DoS 공격이 됨)

- 데이터의 정확하고 순차적인 전달은 IPv4보다 상위 프로토콜인 TCP에서 보장한다.

- 프로토콜 구조 (보통 20byte)

  > | byte    | content                              | content                             |
  > | ------- | ------------------------------------ | ----------------------------------- |
  > | 0 byte  | version / 헤더길이 / type of service | Total 길이 (payload 포함)           |
  > | 4 byte  | Identification                       | flag(3bit) / fragment offset(13bit) |
  > | 8 byte  | TTL / 상위 protocol                  | header checksum                     |
  > | 12 byte | **src addr**                         | **src addr**                        |
  > | 16 byte | **dst addr**                         | **dst addr**                        |
  > | 20 byte | IP option (붙을 수도 아닐 수도)      |                                     |

  - version에는 IPv4이므로 **0x4**를 쓴다. IPv6는 아예 패킷 구조가 다르니까 생각 안해도 됨

  - 헤더 길이에는 헤더 길이를 4로 나눈 값을 쓴다. 헤더길이 범위는 20~60이며 헤더를 4로 나눈 값은 보통 **0x5**이다. 

  - type of service는 과거에 쓰던 정보이며 현재는 사용하지 않음. **0x00** 넣으면 됨

  - Idendification은 큰 데이터를 보낼때 패킷을 쪼개서 보내는데 받는 애가 알아보기 위해서 쪼개진 패킷에 동일하게 부여하는 값이다. 

  - flag는 3bit로 구성되어 있음, 각각 X, D, M flag

    1. X는 안씀

    2. D는 "Don't fragmentation"

       패킷 쪼개는 것을 막음. 최대 전송단위(1514byte)보다 큰 데이터를 보낼 때, D flag를 설정하면 쪼개지 못하므로 못보냄

    3. M은 "More fragmentation"

       최대 전송단위보다 클 때 조각화(fragmentation)이 발생하는데, 이를 나타내기 위해 flag 설정함

  - flag offset은 13bit를 사용함. 큰 데이터를 여러 패킷으로 쪼갰을 때, 어느 순서인지 알아야 함. 조각 데이터의 순서를 표시하기 위해 쓴다. 페이로드 데이터의 값을 8byte로 나눈 값을 넣는다.

  - TTL (Time To Live)

    - 패킷이 살아있을 수 있는 시간을 지정 (정확히는 전달 횟수의 한계)
    - 패킷이 빙빙 도는 경우 TTL이 하나씩 감소하고, 이 값이 0이 되면 이 패킷을 더 이상 다른 곳으로 보내지 않음
    - TTL은 운영체제마다 다름 윈도우는 128, 리눅스는 64

  - 상위 프로토콜의 타입을 명시하며, 종류는 ICMP(0x01), TCP(0x06), UDP(0x11)

  - src, dst addr은 IP주소

  - IP option은 하나씩 붙을 때마다 4byte씩 추가됨, 최대 10개 옵션(최대 60byte)



### 3계층 [ICMP protocol 구조]

- Internet Control Message Protocol

- 네트워크 컴퓨터 위에서 돌아가는 운영체제에서 **오류 메세지**를 전송받는데 주로 쓰인다.

- 프로토콜 구조의 Type과 Code를 통해 오류 메시지를 전송 받는다.

- IPv4 패킷 뒤에 붙여서 보냄

  [ Ethernet  [ IPv4 ] [ ICMP ] ]

- 구조

  > | byte  | content     | content  |
  > | ----- | ----------- | -------- |
  > | 0byte | Type / Code | Checksum |
  >
  > - Type : 대분류
  >
  >   1. Echo 통신
  >
  >      0번 : echo reply
  >
  >      8번 : echo
  >
  >   2. 잘못됨
  >
  >      3번 Destination Unreachable 
  >      시간 내에 요청이 도착하지 않음, 네트워크 경로상 문제
  >
  >      11번 Time Exceded
  >      도착 했지만 상대방이 응답하지 않음, 보통 방화벽 때문 
  >
  >   3. 보안
  >
  >      5번 Redirect
  >      옛날에 썼음 라우팅 테이블 원격지에서 수정하는 것. 요새는 안됨
  >
  > - Code : 소분류
  >
  >   대분류에 대한 추가 정보



### 라우팅 테이블

- 패팃 어디로 보내야 하는 지 써있는 지도

- 0.0.0.0으로 기본값 존재

  모르면 일단 문밖(게이트웨이)로 나가라




### DNA 서버

- 도메인 물으면 IP 주소 알려줌



### IPv4의 조각화 (fragmentation)

- 큰 IP 패킷 쪼개는 기준 

  MTU(Maximum transmission unit)이 작은 링크를 통해 전송 시 쪼개야 한다.

- 목적지까지 패킷 전달 과정에 통과하는 각 라우터마다 전송에 적합한 프레임으로 변환하는 것이 필요함

- 일단 조각화가 되면 최종 목적지에 도달하기 전까지 재조립하지 않음

- IPv4는 발신지와 중간 라우터에서 모두 조각화가 가능하다. (중간에 더 작은 MTU 만나면 조각화 발생)

  IPv6는 발신지에서만 조각화가 가능하다.

  재조립은 최종 수신지만 가능하다.

  > 보내려는 데이터 크기가 2379byte, MTU가 980byte인 경우
  >
  > IP 헤더인 20byte 빼주고 데이터를 나눠 계산해야함. → 3개의 조각으로 나뉜다.
  >
  > \[IP헤더] [960] → (flag) MF = 1, offset = 0
  >
  > \[IP헤더] [960] → (flag) MF = 1, offset = 120
  >
  > \[IP헤더] [419] → (flag) MF = 0, offset = 240



### 4계층 [Transport layer]

- **송신자의 프로세스**와 **수신자의 프로세스**를 연결하는 통신 서비스

- 연결 지향 데이터 스트림 지원, 신뢰성 흐름 제어, 다중화 등 서비스

- 1. TCP (전송 제어 프로토콜) : 연결 지향 전송 방식 사용
  2. UDP (사용자 데이터그램 프로토콜) : 비연결 지향형

- 포트 번호

  - 특정 프로세스가 특정 프로세스와 통신하기 위해 사용

  - 하나의 포트는 하나의 프로세스만 사용 가능

    하나의 프로세스가 여러 개의 포트를 사용할 수 있음

  - 포트는 일반적으로 정해져 있지만 꼭 지켜야 하는 것은 아님

    (일반적으로 웹서비스는 80의 포트를 쓰지만 꼭 항상 80일 필요는 없다.)

    1. Well Known port

       > | 서비스 | 포트번호 | 서비스 | 포트번호 |
       > | ------ | -------- | ------ | -------- |
       > | FTP    | 20, 21   | DHCP   | 67, 68   |
       > | SSH    | 22       | TFTP   | 69       |
       > | TELNET | 23       | HTTP   | 80       |
       > | DNS    | 53       | HTTPS  | 443      |

    2. Registered port

       오라클 DB 서버 1521

       MySQL 서버 3306

       MS원격 데스크탑 3389

       등등

    3. Dynamic port : 일반 사용자들이 사용

       49152 ~ 65535



### 4계층 [UDP protocol]

- User(Universal) Datagram Protocol으로 비연결 지향형이다.

- 전송 방식이 너무 단순하며, 서비스의 신뢰성이 떨어진다.

- 데이터그램 도착 순서가 바뀌거나, 중복되거나, 통보 없이 누락될 수 있다.

- 일반적으로 오류의 검사와 수정이 필요 없는 프로그램에서 수행을 가정한다.

- 구조

  > | byte  | content  | content  |
  > | ----- | -------- | -------- |
  > | 0byte | src port | dst port |
  > | 4byte | length   | checksum |



### 4계층 [TCP protocol]

* 전송 제어 프로토콜(Transmission Control Protocol)

* 프로세스간 통신이 안정적이고 순서대로 에러 없이 일어나도록 해준다.

* 안전하지만 느리다. (사용자가 체감할 정도는 아님)

* 구조

  > | byte    | content                                | content        |
  > | ------- | -------------------------------------- | -------------- |
  > | 0 byte  | src port                               | dst port       |
  > | 4 byte  | Seq number                             | Ack number     |
  > | 8 byte  | offset(1B) / reserved(1B) / flags (2B) | window         |
  > | 12 byte | checksum                               | urgent pointer |
  > | 16 byte | TCP option (var length 선택)           |                |
  >
  > * offset : 헤더의 길이를 4로 나눈 값
  >
  > * TCP flags
  >
  >   ~~C E~~ **U A P R S F**
  >
  >   - 앞에 C, E는 사용 안함
  >   - U(urgent flag) 우선순위 높은 급한 데이터, Urgent point(위치 포인터)와 함께 보냄
  >   - **A(Ack flag) 데이터 주고 받는 거 승인**
  >   - P(push flag) 버퍼 신경 쓰지 말고 데이터 넣어라
  >   - R(reset flag) 둘 사이의 연결관계 새로고침하자
  >   - **S(sync flag) 연결 시작할 때 무조건 사용, 중요함 (동기화)**
  >   - **F(fin flag) 연결 끊을 때 사용**
  >
  > * TCP option은 최대 60byte

* 연결 수립 : 3-way handshake

  1. 클라 -(요청 패킷)-> 서버
  2. 서버 -(수락 패킷)-> 클라
  3. 클라 -(최종 수락)->서버

  > | 상태                        | 클라이언트                            | 패킷 방향 | 데이터                                                       | 서버                                 | 상태                            |
  > | --------------------------- | ------------------------------------- | --------- | ------------------------------------------------------------ | ------------------------------------ | ------------------------------- |
  > | CLOSED<br />→SYN_SENT       | **[0xdfa5]\[0x0050 ]<br />포트 전송** | **→**     | flag = SYN<br /><u>S: 100 (임의값)</u><br/>A: 0              |                                      | LISTENING                       |
  > | SYN_SENT                    |                                       | ←         | flag = SYN + ACK<br />**S: 2000(임의값)**<br/><u>A: 101 (S + 1)</u> | **[0x0050]\[0xdfa5]<br />포트 전송** | LISTENING<br />→ SYN_RECEIVED   |
  > | SYN_SENT<br />→ ESTABLISHED | **[0xdfa5]\[0x0050 ]<br />포트 전송** | **→**     | flag = ACK<br /><u>S: 101(A값)</u><br/>**A: 2001 (S + 1)**   |                                      | SYN_RECEIVED<br />→ ESTABLISHED |
  >
  > 




### NAT (Network Address Translation)

- IP 패킷의 TCP/UDP **포트 숫자**와 **소스 및 목적지의 IP 주소** 등을 재기록하면서 라우터를 통해 네트워크 트래픽을 주고받는 기술 (기록하는 곳 : NAT Table)

- 패킷에 변화가 생기기 때문에 IP나 TCP/UDP의 checksum도 다시 계산되어 재기록 해야 함

- NAT를 이용하는 이유

  대개 사설 네트워크에 속한 여러 개의 호스트가 하나의 공인 IP 주소를 사용하여 인터넷에 접속하기 위함

- 하지만 꼭 사설 IP를 공인 IP로 변환하는 데만 사용하지는 않음



### 포트 포워딩

- 패킷이 라우터나 방화벽과 같은 네트워크 장비를 가로지르는 동안 <u>**특정 IP 주소와 포트 번호**</u>의 통신 요청을 <u>**특정 다른 IP와 포트 번호**</u>로 넘겨주는 NAT 응용 기술이다.

- 이 기법은 게이트웨이(외부망)의 반대쪽에 위치한 사설 네트워크에 상주하는 호스트에 대한 서비스를 생성하기 위해 흔히 사용된다. (라우터와 같은 3계층 장비)

- [사설 IP (클라이언트)] → [라우터, 공인 IP 1] → [라우터, 공인 IP 2] → [서버 (클라이언트가 보이지 않음)]

  클라이언트가 서버에 연결하고 싶을 때, 공인IP2의 특정 포트로 전송하면, 공유기가 해당 요청을 내부 네트워크 대역에서 다른 특정 IP의 특정 포트로 전송한다. (대신 전달하는 것)



### 7계층 [HTTP protocol]

- 웹을 만들기 위해 사용되는 다양한 기술들

  | 기술                | 영역        | 비고                          |
  | ------------------- | ----------- | ----------------------------- |
  | HTTP(HTTPS→SSL/TLS) |             | 프론트 엔드의 데이터를 받아옴 |
  | HTML                | 프론트 엔드 | Web 페이지 내용               |
  | JavaScript          | 프론트 엔드 | 기능                          |
  | CSS                 | 프론트 엔드 | 꾸미기                        |
  | ASP/ASP.NET         | 백 엔드     | 서버에서 실행                 |
  | JSP                 | 백 엔드     | 서버에서 실행                 |
  | PHP                 | 백 엔드     | 서버에서 실행                 |
  | DB                  | 백 엔드     |                               |

  언어 : Python, Spring, Jquery, Ajax

- HTTP (HyperText Transfer Protocol)

  - www에서 쓰이는 핵심 프로토콜

  - 문서의 전송을 위해 쓰임

  - 오늘날 거의 모든 웹 애플리케이션에서 사용

    음성, 화상 등 여러 종류 데이터를 MIME으로 정의하여 전송 가능

  - Request/Reponse 동작에 기반하여 서비스를 제공한다는 특징을 갖는다.

- HTTP 1.0

  특징

  1. "연결 수립, 동작, 연결 해제"의 단순함

     하나의 URL은 하나의 TCP 연결

  2. HTML 문서를 전송 받은 뒤 연결을 끊고 다시 연결하여 데이터를 전송

  문제점

  1. 단순 동작이 반복되어 통신 부하 문제 발생
  2. 3-way handshake가 반복되어 비효율적임

  → 해결 : HTTP/1.1

- HTTP 요청 프로토콜 구조

  1. Request line

     [ 요청 타입 ]\[ 공백 ]\[ URI ]\[ 공백 ]\[ HTTP 버전 ]

     - 요청 타입

       GET(URI에 데이터 포함), POST(URI에 데이터 포함X, 중요한 데이터) 등등

     - URI (Uniform Resource Identifier)

       인터넷 상에서 특정 자원(파일)을 나타내는 유일한 주소

       **scheme** ://host[:port]\[/path]\[?query]

       **ftp** ://IP주소:port/파일이름

       **http** ://IP주소(도메인주소):포트(web 브라우저에서는 80, 443로 자동 설정)/폴더 이름/파일 이름

  2. Headers (옵션 다양함)

  3. 공백

  4. Body

- HTTP 응답 프로토콜 구조

  1. Status line

     \[ HTTP 버전 ]\[ 공백 ][ 상태 코드 ]\[ URI ]\[ 상태 문구 ]

     - 상태 코드

       200~299 : 정상 완료

       400~499 : Client 잘못 (서버에 없는 URI 주소, request line, 권한 없는 경우 403)

       500~599 : Server 잘못

  2. Headers

  3. 공백

  4. Body

- HTTP 헤더 포맷

  1. 일반 헤더

     Content_Length

     Content_type

  2. 요청 헤더 (클라 정보)

     Cookie : 서버에서 받은 쿠키를 다시 쿠키에게 돌려줌

     Host : 요청된 URL에 나타난 호스트명을 상세히 표시 (HTTP/1.1은 필수)

     User_Agent : Client Program에 대한 식별 가능 정보

  3. 응답 헤더 (서버 정보)

     Server : 사용하고 있는 웹 서버의 소프트웨어에 대한 정보 포함

     Set_cookie : 쿠키를 생성하고 브라우저에 보낼 때 사용, 해당 쿠키 값을 브라우저가 서버에 다시 보낼 때 사용

  

  




















