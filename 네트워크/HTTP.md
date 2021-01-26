TCP/IP 4계층

 



![img](https://blog.kakaocdn.net/dn/cv37i5/btqURuFCNST/HkY7Q4Qkj1qScH0Fn9ksVk/img.png)



 

IP 프로토콜

 

지정한 IP 주소에 데이터 전달

Packet 단위 통신을 통한 데이터 전달

 

문제점 :

\1. 비연결성

패킷을 받을 대상이 통신 불가능 상태일때도 패킷을 전송함

\2. 비신뢰성

패킷이 제대로 전달되었는지 알 수 없음

\3. 프로그램 구분 불가능

실행중인 애플리케이션이 여러 개일때 어떤 프로그램에서 사용해야하는지 구분 불가

 

이러한 문제점들을 해결하기 위해 TCP와 UDP가 등장

TCP 특징

 

연결지향 – 3 way handshake

데이터 전달 보증, 순서 보장

 

HTTP

 

TCP를 전송 프로토콜로 사용 (HTTP 1.1과 HTTP 2.0에서는 TCP 를 사용하지만 최근에 진행중인HTTP/3에서는 TCP 대신 UDP를 사용)

 

HTTP 특징

 

\1. 클라이언트 서버 구조

Request Response 구조

클라이언트는 서버에 요청을 보내고 응답을 대기

서버는 클라이언트의 요청에 대한 결과를 만들어서 응답

 

\2. 비상태 프로토콜 (stateless protocol)

서버가 클라이언트의 상태를 보존하지 않음

서버 확장성이 높음

클라이언트가 추가 데이터를 전송해야함

è 이에 대한 성능 개선을 위해 쿠키를 사용함

 

\3. 비지속 연결(non-persistent connection)

기본적으로 연결을 유지하지 않으므로 서버 자원을 효율적으로 사용

TCP/IP 연결을 매번 새로 해줘야함

현재는 HTTP 지속 연결로 문제 해결

 

HTTP/3

 

3way handshack 최적화

HOLB(Head of line Blocking) 문제를 해결하기 위해 (TCP를 사용하면 발생할 수 밖에 없음)

![gcp-cloud-cdn-performance](https://evan-moon.github.io/95f5c7e411d0b7f96d182abe284be551/gcp-cloud-cdn-performance.gif)





 

 

 

첫번째 handshack를 거칠 때, 연결 설정에 필요한 정보와 함께 데이터도 보내버리는 방식으로 최적화

 

멀티플렉싱 지원

![HTTP-vs-with-Push-HTTP1](https://freecontent.manning.com/wp-content/uploads/HTTP-vs-with-Push-HTTP2.gif)

![HTTP-vs-with-Push-HTTP2](https://freecontent.manning.com/wp-content/uploads/HTTP-vs-with-Push-HTTP1.gif)

클라이언트 ip가 바뀌어도 연결이 유지됨

 

Connection id 를 사용하여 연결을 유지 이는 클라이언트의 ip와는 전혀 무관한 데이터이므로 연결을 유지시킬 수 있다.
