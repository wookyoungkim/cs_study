NIC : network interface card

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3b40a48d-4679-40a8-ab22-3bd5e94d23d6/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3b40a48d-4679-40a8-ab22-3bd5e94d23d6/Untitled.png)

네트워크 상에는 수많은 서로 다른 종류의 디바이스들이 존재한다.

이러한 여러 디바이스들은 서로 다른 하드웨어/소프트웨어로 구성되어있기 때문에 네트워크에 관여하는 요소들의 기능별 호환성이 보장되어야 네트워크 연결이 성립한다.

이러한 복잡한 장비 요소들을 기능에 따라 보다 명확하게 구분하기 위해 OSI가(Open Systems Interconnection Reference Model) 정의되었다.

OSI 7 계층

1. Application Layer
2. Presentation Layer
3. Session
4. Transport
5. Network
6. Data link
7. Physical

# Application Layer

network applications

유저가 사용하는 서비스

ex) 구글, 크롬, outlook, skype, ...

사용되는 프로토콜 : HTTP, HTTPs, FTP, NFS, FMTP, DHCP, SNMP, TELNET, POP, IRC, NNTP

File Transfer <> FTP

Web Surfing <> HTTP / HTTPs

Emails <> SMTP

Virtual Terminals <> Telnet

# Presentation layer

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eccc203e-c125-4847-9f4d-04f76d842ed7/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eccc203e-c125-4847-9f4d-04f76d842ed7/Untitled.png)

Application 계층으로부터 숫자 또는 문자 형태의 data를 전달받고

1. Transition : 데이터 형태를 변환한다.
2. Data Compression : 데이터를 압축한다.
3. Encryption/Decryption : SSL과 같은 Secure Sockets Layer를 통해 data를 암호/복호화한다.

# Session layer

APIs, NETBIOS와 같은 helper를 통해 유저 간의 연결에서 송수신을 가능케 한다.

Session을 통해 server에 접속하면

authentication : name/password를 통해 유저를 인증하고

authorization : 유저가 server가 가지고 있는 리소스에 대한 접근 권한이 있는지 확인한다.

session management : 유저가 요청에 의해 응답한 리소스의 형태에 따라 어떻게 리소스를 처리할 것인지를 관리한다.

# Transport layer

segmentation

data를 segments로 쪼갠다.

각 segment는 소스와 도착지에 대한 port number와 segment의 순서 정보를 포함한다.

쪼개어진 segment는 포트 번호를 통해 알맞는 application으로 전달될 수 있고, 송신단에서 쪼개어진 segment는 수신단에서  순서 정보에 의해 원 데이터로 합쳐질 수 있다.

flow control

송수신단 사이의 데이터 흐름을 제어한다.

error control

송수신 시 segments 유실/손상/순서 섞임/중복과 같은 에러가 발생할 경우 이에 대해 피드백한다.

[TCP의 error control](https://www.geeksforgeeks.org/error-control-in-tcp/)

사용하는 프로토콜

TCP ( transmission control protocol )

- connection - oriented transmittion
- 데이터 손실이 발생하면 안 되는 WWW, email, FTP에서 사용된다.

UDP ( user datagram protocol )

- connectionless transmission
- feedback이 없기 때문에 TCP보다 빠르다
- 속도가 우선시 되는 스트리밍, TFTP, DNS에서 사용된다.

# Network layer

network layer는 Transport layer가 전달한 segments를 전달받으며 이를 패킷이라 한다.

Logical Addressing

- IPv4 & IPv6
- 각 패킷에 IP 주소를 첨부해 데이터가 목적지에 도착할 수 있도록 한다

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d9e4b5e0-c5c4-4f2a-8917-a53dec512f2a/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d9e4b5e0-c5c4-4f2a-8917-a53dec512f2a/Untitled.png)

Path determination

- 최적의 경로를 결정한다.
- OSPF , BGP , IS-IS
- QoS (Quality of Service)

Routing

- logical addressing 기능을 맡는 IPv4 & IPv6와 mask를 이용해 라우팅한다.

mask : 225.225.225.0

도착지 ip : 192.168.2.1

mask는 IP의 앞 3 조합은 network, 마지막은 host B임을 설명한다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b4cc8cd9-0548-4792-95e7-64388658e4f6/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b4cc8cd9-0548-4792-95e7-64388658e4f6/Untitled.png)

# Data Link Layer

Logical adressing ( from network layer)

Physical addressing

data link layer는 NIC(network interface controller)에 소프트웨어로 임베디드되어 네트워크 계층에서 전달받은 data packet에 MAC 주소 정보와 tail 정보를 더해 frame 조각으로 캡슐화해 다른 컴퓨터로 전송한다.

- MAC : 12자의 알파벳과 숫자로 이루어진 디바이스 주소

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0697259a-d7a6-4679-a102-7d9ce96c18b3/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0697259a-d7a6-4679-a102-7d9ce96c18b3/Untitled.png)

미디어로부터 전달받은 데이터를 연결된 디바이스에 전달한다.(MAC, Error Detection)

Access the media (Framing)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0f6ec9d9-fa3a-4078-959e-3c1c695b2cf9/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0f6ec9d9-fa3a-4078-959e-3c1c695b2cf9/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/35fd5acd-b4f2-4e48-9e1c-5fcf55b2e361/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/35fd5acd-b4f2-4e48-9e1c-5fcf55b2e361/Untitled.png)

# Physical Link Layer

2진 형태의 data를 전기/광/전자 신호로 변환해주는 물리적 전송 매개물을 의미한다.

simplex, half duplex, full duplex와 같은 전송 모드 또한 physical layer에서 정의되고 USB, Bluetooth, Ethernet 규범 또한 Physical layer 명세에 포함된다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/343cbe49-30c8-437c-bacb-a15605dc2b6a/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/343cbe49-30c8-437c-bacb-a15605dc2b6a/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/29ba888d-b109-4f78-ae6a-be7d15cb5d2a/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/29ba888d-b109-4f78-ae6a-be7d15cb5d2a/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d3870a64-f0e9-439d-9ae5-a050d80d97fc/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d3870a64-f0e9-439d-9ae5-a050d80d97fc/Untitled.png)

MAC : Media Access Control

ARP : address solution protocol

PPP : WAN protocol

이더넷 802 : wire protocol

802.11 : wireless protocol

SONET/SDH : 광통신

ICMP : message

IPSec : ip security로 하위 레이어에 내장됨

OSPF, EIGRP : 라우팅 프로토콜

user가 사용하는 프로토콜들

SSH : remote connection

SMTP : mail

POP, IMAP : mailbox

SNMP : network management

TLS/SSL : security socket layer

BGP , RIP: 라우팅