# OSI 7계층

[https://www.youtube.com/watch?v=vv4y_uOneC0](https://www.youtube.com/watch?v=vv4y_uOneC0)

NIC : network interface card

![image](https://user-images.githubusercontent.com/56799176/105854022-ab5f3080-6029-11eb-8f02-462dd335f05a.png)

네트워크 상에는 수많은 서로 다른 종류의 디바이스들이 존재한다.

이러한 여러 디바이스들은 서로 다른 하드웨어/소프트웨어로 구성되어있기 때문에 네트워크에 관여하는 요소들의 기능별 호환성이 보장되어야 네트워크 연결이 성립한다.

이러한 복잡한 장비 요소들을 기능에 따라 보다 명확하게 구분하기 위해 OSI가(Open Systems Interconnection Reference Model) 정의되었다.

OSI 7 계층

7. Application Layer

6. Presentation Layer

5. Session

4. Transport

3. Network 

2. Data link

1.  Physical

# Application Layer

network applications

유저가 사용하는 서비스로 네트워크 자원에 대한 접근을 제공한다.

ex) 구글, 크롬, outlook, skype, ...

사용되는 프로토콜 : HTTP, HTTPs, FTP, NFS, FMTP, DHCP, SNMP, TELNET, POP, IRC, NNTP

File Transfer <> FTP

Web Surfing <> HTTP / HTTPs

Emails <> SMTP

Virtual Terminals <> Telnet

# Presentation layer

![image](https://user-images.githubusercontent.com/56799176/105854030-ae5a2100-6029-11eb-9ad2-1931394950ca.png)

Application 계층으로부터 인간이 이해할 수 있는 data를 전달받고,
네트워크 전송이 가능하며 이에 용이한 형태로 데이터를 변환한다.

1. Transition : 데이터 형태를 변환한다.
2. Data Compression : 데이터를 압축한다.
3. Encryption/Decryption : SSL과 같은 Secure Sockets Layer를 통해 data를 암호/복호화한다.

프로토콜 : JPEG, MPEG, SMB, ...

# Session layer

APIs, NETBIOS와 같은 helper를 통해 네트워크 노드 간의 소통을 위한 연결 시스템을 구축한다.

Session을 통해 server에 접속하면

1. authentication : name/password를 통해 유저를 인증하고
2. authorization : 유저가 server가 가지고 있는 리소스에 대한 접근 권한이 있는지 확인한다.
3. session management : 세션은 송수신되는 data를 추적하며 어떻게 이를 처리할 것인지 결정해준다.

# Transport layer

1. segmentation

data를 segments로 쪼갠다.

각 segment는 소스와 도착지에 대한 port number와 segment의 순서 정보를 포함한다.

쪼개어진 segment는 포트 번호를 통해 알맞는 application으로 전달될 수 있고, 송신단에서 쪼개어진 segment는 수신단에서  순서 정보에 의해 원 데이터로 합쳐질 수 있다.

2. flow control

송수신단 사이의 데이터 흐름을 제어한다.

3. error control

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

network layer는 Transport layer가 전달한 segments를 전달받는다.

1. Logical Addressing

- IPv4 & IPv6
- 각 segment에 IP 주소를 부여해 데이터가 목적지에 도착할 수 있도록 하며 이러한 조각을 packet이라 한다.

![image](https://user-images.githubusercontent.com/56799176/105854033-b0bc7b00-6029-11eb-8ee3-f247789d01c2.png)

2. Path determination

- 최적의 경로를 결정한다.
- OSPF , BGP , IS-IS
- QoS (Quality of Service)

3. Routing

- IPv4 & IPv6와 mask를 이용해 라우팅한다.

mask : 225.225.225.0 

도착지 ip : 192.168.2.1 

mask는 IP의 앞 3 조합은 network, 마지막은 host B임을 설명한다.

![image](https://user-images.githubusercontent.com/56799176/105854038-b2863e80-6029-11eb-9789-fbe9d7f2f9d2.png)

# Data Link Layer

Logical adressing ( from network layer)

Physical addressing

data link layer는 NIC(network interface controller)에 소프트웨어로 임베디드되어 네트워크 계층에서 전달받은 data packet에 MAC 주소 정보와 tail 정보를 더해 frame 조각으로 캡슐화해 다른 컴퓨터로 전송한다.

- MAC : 12자의 알파벳과 숫자로 이루어진 디바이스 주소

![image](https://user-images.githubusercontent.com/56799176/105854041-b4500200-6029-11eb-984b-c5403e530ab2.png)

미디어로부터 전달받은 데이터를 연결된 디바이스에 전달한다.(MAC, Error Detection)

Access the media (Framing)

![image](https://user-images.githubusercontent.com/56799176/105854048-b619c580-6029-11eb-8564-2183984c8bb6.png)

![image](https://user-images.githubusercontent.com/56799176/105854052-b7e38900-6029-11eb-9048-632ef920d7be.png)

# Physical Link Layer

2진 형태의 data를 전기/광/전자 신호로 변환해주는 물리적 전송 매개물을 의미한다.

simplex, half duplex, full duplex와 같은 전송 모드 또한 physical layer에서 정의되고 USB, Bluetooth, Ethernet 규범 또한 Physical layer 명세에 포함된다.

![image](https://user-images.githubusercontent.com/56799176/105854060-ba45e300-6029-11eb-9b07-004ce4e65f8e.png)


프로토콜

- MAC : Media Access Control
- ARP : address solution protocol
- PPP : WAN protocol
- 이더넷 802 : wire protocol
- 802.11 : wireless protocol
- SONET/SDH : 광통신
- ICMP : message
- IPSec : ip security로 하위 레이어에 내장됨
- OSPF, EIGRP : 라우팅 프로토콜
- user가 사용하는 프로토콜들
- SSH : remote connection
- SMTP : mail
- POP, IMAP : mailbox
- SNMP : network management
- TLS/SSL : security socket layer
- BGP , RIP: 라우팅
