# **TCP**

**TCP : 인터넷상에서 메시지를 데이터로 보내기위해 IP와 함께 사용되는 프로토콜로서 연결형, 신뢰성있는 전송 프로토콜이다.**

## **연결 지향적 특징**

<img src="https://user-images.githubusercontent.com/52908154/104708168-add7a580-5760-11eb-925f-c1657aadac80.png" width=30%>

TCP는 연결형 서비스로 위의 그림과 같이 **논리적으로 가상 회선을 만들어 통신에 사용한다.** 데이터를 전송하기전에 반드시 연결이 확립되어야한다. TCP는 **스트림 기반의 전송방식을 사용한다.** 데이터를 임의의 크기로 나누어 연속적으로 전송하는 방식을 사용한다.

<img src="https://user-images.githubusercontent.com/52908154/104708257-cf389180-5760-11eb-8d2a-1ec7897c19af.png" width=50%>

**연결 수립 과정에서 3way handshake**

1\. 클라이언트는 SYN 패킷을 전송하여 접속을 요청한다. 클라이언트는 SYN패킷을 보냄과 동시에 SYN\_SENT상태가 되고 SYN/ACK응답을 기다린다.

2\. 서버는 SYN 패킷을 받고 클라이언트에게 요청을 수락하는 ACK패킷과 SYN패킷을 보낸다. 이때 서버는 SYN\_RCVD상태로 클라이언트의 ACK패킷을 기다린다.

3\. 클라이언트는 ACK패킷을 보낸다. 서버에서 ACK패킷을 받게되면 ESTABLISHED상태가 되고 데이터 통신을 시작한다.

**연결 종료 과정에서 4way handshake**

1\. 서버와 클라이언트가 연결되어있는 상태에서 클라이언트가 접속 종료를 위해 close()함수를 호출한다. close()함수를 호출하면 서버에게 FIN segment를 보내고 **FIN\_WAIT1**상태가 된다.

2\. 서버는 클라이언트가 접속을 종료한다는 것을 알게되고 CLOSE\_WAIT상태로 변경한뒤 ACK 패킷을 보낸다. 

3\. ACK를 받은 클라이언트는 **FIN\_WAIT2**상태가 되고 서버는  close()함수를 호출하여 FIN패킷을 보낸다. 이때, 서버는 **LAST\_ACK**상태가 된다.

4\. 서버가 보낸 FIN패킷을 수신하면 클라이언트는 ACK segment를 보내고 **TIME\_WAIT**상태가 된다.

5\. 서버는 ACK를 받고 **CLOSED**상태가 된다.

**비정상적인 종료 상황**

1\. CLOSE\_WAIT 상태 : 어플리케이션에서 close()함수를 적절하게 호출하지 못하면 TCP포트는 CLOSE\_WAIT상태로 계속 기다리게 된다. CLOSE\_WAIT상태가 많아지게되면 hang이 걸려 더이상 연결을 하지 못하는 경우가 생길 수 있다.

2\. FIN\_WAIT1 상태 : 클라이언트가 종료를 요청했는데 ACK패킷을 응답 받지 못한 상황을 말한다. FIN\_WAIT1상태는 일정 시간이 지나 TIME OUT되면 자동으로 close한다.

3\. FIN\_WAIT2 상태 : 클라이언트가 종료요청을 한 뒤 서버로부터 ACK응답을 받았지만, 서버로부터 종료했다는 FIN응답을 받지 못하는 상황을 말한다. FIN\_WAIT2상태는 일정 시간이 지나 TIME OUT되면 자동으로 close한다.

일정시간이 지나 TIME OUT이되면 FIN\_WAIT1/2상태는 close되지만 TIME OUT시간이 길어 소켓 수가 점점 늘어난다면 메모리 부족으로 더이상 소켓을 오픈하지 못하는 경우가 발생할 수 있다.

## **흐름 제어**

데이터 처리의 속도를 조절하여 수신자의 버퍼 오버플로우를 방지한다. 송신하는 곳에서 감당하기힘들 만큼 많은 데이터를 빠르게 보내는 상황에서 수신하는 곳에 문제가 발생하는 것을 막는다. 수신자는 window size를 조절하여 수신량을 정할 수 있다.

### **Stop and Wait**

<img src="https://user-images.githubusercontent.com/52908154/104708363-f4c59b00-5760-11eb-9754-891084bb1cdf.png" width=20%>

stop and wait은 매번 전송한 패킷에 대해 확인 응답을 받아야만 그 다음 패킷을 전송하는 방식

### **Slide Window**

slide window은 수신측에서 설정한 윈도우 크기만큼 송신측에서 확인응답 없이 세그먼트를 전송할 수 있게 하여 데이터 흐름을 동적으로 조절하는 제어 기법. window는 수신측과 송신측에 둘 다 있으며 TCP header에 window size 필드로 window size가 지정된다. 

<img src="https://user-images.githubusercontent.com/52908154/104708393-fe4f0300-5760-11eb-8bd4-45827a458f35.png" width=80%>

먼저 윈도우에 포함되는 패킷을 전부 전송하고, 그 패킷의 전송이 확인되는대로 이 윈도우를 옆으로 slide하여 그 다음 패킷을 전송한다. 수신측은 window의 범위에 벗어난 패킷은 무시하며, window 순서에 맞지 않게 패킷이 도착한 상황에서는 버퍼에 보관한뒤 재조립한다. 수신순서에 맞게 된다면 window는 slide하여 다음 패킷에 대해 수신을 대기한다.

## **TCP  오류제어**

TCP를 사용하는 송수신 측에서 오류를 확인할 수 있는 상황은 다음과 같다.

1\. 수신측에서 NACK를 보낸 경우

2\. 수신측으로부터 ACK가 도착하지 않는 경우 (time out)

3\. 중복된 ACK가 계속 오는경우

### **Go-Back N**

<img src="https://user-images.githubusercontent.com/52908154/104708482-1fafef00-5761-11eb-8936-6906c3c9aa24.png" width=50%>

Go-Back-N은 이름 그대로 오류가 난 지점부터 다시 돌아가 패킷을 재전송하는 방식이다. 오류가 난 패킷 이후 순서에서 성공적으로 도착한 패킷은 전부 폐기되고 새롭게 패킷을 수신하게된다. 1 ~ 1000 순서의 패킷을 전송하는 상황에서 10을 제외한 모든 패킷이 정상적으로 도달했다면, 10의 패킷으로 인해 10~1000의 패킷이 전부 재전송하게 된다. 

### **Selective Repeat**

<img src="https://user-images.githubusercontent.com/52908154/104708500-23dc0c80-5761-11eb-857e-e4ae478e1276.png" width=50%>

Selective Repeat는 문제가 생긴 패킷에 대해서만 재전송을 한다. 1, 2, 3, 4, 5을 수신하고 1, 2, 4, 5패킷만 도착한다면 송신측은 3의 패킷만 다시 보내면 된다. 하지만 3의 패킷에 대해서는 기존의 위치에 끼워 넣기위해서 버퍼에서 재정렬이 필요하다.

### **Go Back N vs Selective Repeat**

go back n 방식은 문제가 생긴 패킷부터 다시 전체를 재전송하는 방식이다. 수신측은 문제가 생긴 버퍼 이후에 도착했던 패킷을 폐기한다. 따라서 추가적인 버퍼가 필요하지 않다. selective repeat방식은 문제가 생긴 패킷을 재전송하는 방식이다. 수신측은 정상적으로 도착한 패킷을 폐기하지 않고 재배열하여 버퍼에 담아둔다. 추가적인 버퍼 관리가 필요하다. 

## **혼잡 제어**

네트워크 내의 패킷 수가 넘치게 증가하지 않도록 방지한다. TCP의 오류제어는 재전송을 기반으로 한다. 여러 곳에서 반복되는 재전송으로 네트워크가 혼잡해지면 상황이 점점 악화된다. TCP의 혼잡제어는 송신측의 윈도우를 조절해 데이터 전송량을 강제로 줄인다.

### **혼잡 윈도우(congestion window)**

송신측은 최종 윈도우 사이즈를 결정할 때, 수신측이 보내준 윈도우 크기와 네트워크의 혼잡도를 고려해 결정된 윈도우 크기중 작은 것을 선택한다. 

통신을 시작하면 ack응답이나 time out등을 고려해 네트워크 혼잡도를 파악할 수 있지만 처음 통신을 진행할 때는 알 수 없다. 이때, MSS(Maximum Segement Size)가 사용된다.

> **MSS**\= MTU - (IP헤더길이 + IP옵션길이) - (TCP헤더길이 + TCP옵션길이)

MTU(Maximum Transmission Unit)는 한번 통신 때 보낼 수 있는 최대 단위이다.  즉, MSS는 한번에 보낼 수 있는 최대 단위가 정해진 상황에서 진짜 데이터를 담을 수 있는 공간이 얼마나 있는지를 의미한다.

### **혼잡 회피 방법**

### **AIMD(Additive Increase / Multicative Decrease)**

네트워크에 문제가 없다면 혼잡 윈도우의 크기를 1씩 늘리고, 데이터가 유실되거나 응답이 오지않는 상황에서는 윈도우 크기를 절반으로 줄인다.

<img src="https://user-images.githubusercontent.com/52908154/104708615-4a01ac80-5761-11eb-9657-e40ec18f02e7.png" width=30%>

일찍 네트워크에서 통신하고 있던 A는 혼잡 윈도우가 큰 상태가 된다. 새로 진입하는 B는 혼잡 윈도우가 작은 상태에서 시작한다. 시간이 지남에 따라 A의 혼잡 윈도우는 절반으로 감소해가고 B의 혼잡 윈도우는 점점 커지게된다. 그렇게 모든 호스트의 혼잡 윈도우는 평행상태로 접어든다. 이 방식의 문제점은 네트워크 대역이 남아도는 상황에서 너무 천천히 혼잡 윈도우의 크기를 늘려간다는 것이다. AIMD 방식은 그런 이유로 네트워크 대역을 활용하여 제대로 된 속도로 통신하기까지 시간이 소요된다.

### **Slow Start**

slow start는 AIMD방식과 유사하다. 차이점은 혼잡 윈도우 크기를 지수적으로 증가시키다가 혼잡이 감지되면 윈도우 크기를 1로 만든다.
<img src="https://user-images.githubusercontent.com/52908154/104708636-4f5ef700-5761-11eb-9e1c-f18f8afb7b94.png" width=50%>

AIMD방식과는 달리 빠른 속도로 혼잡 윈도우 크기를 늘릴 수 있다.

### **혼잡 제어 정책**

혼잡 제어의 공통적인 전략은 혼잡한 상황에서는 윈도우 크기를 줄이거나 증가시키지 않으며 혼잡을 회피하는 것이다.

유명한 정책으로 Tahoe와Reno가 있다. Tahoe와Reno는 기본적으로 slow start 방식을 사용하다가 혼잡한 상황에서 AIMD방식으로 전환한다.

<img src="https://user-images.githubusercontent.com/52908154/104708652-538b1480-5761-11eb-817c-6d8b79a548ff.png" width=50%>

**3 ACK Duplicated, Timeout**은 해당 시나리오가 감지되면 윈도우 사이즈를 줄인다. 혼잡 발생을 감지하는 기본적인 정책이다.

Timeout은 송신 측에서 보낸 데이터가 유실되거나, 수신측이 보낸 ack응답이 유실된 상황을 말한다. 3 ACK Duplicated는 같은 번호의 ack가 3번이 중복되었다는 의미이다. 수신측은 정상적으로 처리한 데이터에 대해 ack를 보내는데, 같은 번호가 3번이나 왔다는 것은 특정 시퀸스 이후에 데이터를 제대로 처리하지 못함을 의미한다. TCP에서는 현재까지 처리한 누적된 데이터중 마지막 데이터에대한 승인 번호를 전송하므로 같은 번호가 반복되면 무언가 문제가 생겼다는 것을 알 수 있다. TCP 통신에서 패킷의 순서가 늘 순서대로 도착하지는 않으므로  한 두번의 같은 ACK를 가지고 네트워크가 혼잡하다고 판단하지는 않는다. 이런 오류 상황의 경우 go back n, selective n등 오류제어 기법을 사용하여 timeout이 지나지 않아도 해당 패킷을 재전송한다(fast transmit).

**slow start threshold**

그래프의 threshold는 여기까지만 slow start를 사용하겠다는 것을 의미한다. threshold가 없는 경우 윈도우 크기는 지수적으로 증가하여 제어하기 어려워진다. 네트워크가 혼잡한 상황에서는 값을 천천히 증가시키는 것이 안전하다. threshold를 넘어가면 AIMD방식을 사용해 혼잡 윈도우 크기를 선형적으로 증가시킨다.

### **TCP Tahoe**

TCP Tahoe는  slow start를 기반한 정책으로 빠른 재전송(fast transmit)이 적용된 정책이다. 처음에는 혼잡 윈도우 크기를 지수적으로 증가시키다가 threshold를 넘어가면 AIMD방식을 사용하여 선형적으로 증가시킨다. 그러다 ACK Duplicated, Timeout와 같이 혼잡한 상황이 감지되면 threshold와 혼잡 윈도우의 크기를 수정한다.

<img src="https://user-images.githubusercontent.com/52908154/104708667-56860500-5761-11eb-8a18-daf422639888.png" width=30%>

### **TCP Reno**

Tahoe방식과 유사하게 slow start로 시작하여 threshold를 넘으면 합증가 방식으올 변경하는 구조이다. 그러나 Reno방식은 ACK Duplicated, Timeout을 구분한다. 3 ACK Duplicated가 발생했을 때 윈도우 크기를 1로 줄이는 것이 아니라 AIMD처럼 반으로 줄인다. threshold는 줄어든 윈도우 크기가 된다. 그렇기 때문에 작아진 혼잡 윈도우도 빠르게 회복이 가능하다. Timeout이 발생한 경우 윈도우 크기를 1로 줄이고 slow start를 진행한다. 이때, threshold는 변경하지 않는다. ACK Duplicated상황이 Timeout에 비해 그리 혼잡한 상황이 아니라고 판단하여 대처하고 있음

<img src="https://user-images.githubusercontent.com/52908154/104708680-5980f580-5761-11eb-99de-8510126254b0.png" width=30%>

# **UDP**

**UDP : 데이터를 데이터그램 단위로 처리하는 프로토콜로서 비연결형, 비 신뢰성 전송 프로토콜이다.**

<img src="https://user-images.githubusercontent.com/52908154/104708695-5d147c80-5761-11eb-8a5d-00cdc989d60e.png" width=30%>

데이터그램은 독립적인 관계를 지닌 패킷으로 UDP는 비연결형 프로토콜로 패킷 전송에 사용되는 논리적인 경로가 없다. 각 패킷은 독립적인 경로로 전송되고 독립적인 관계를 갖는다.

## **비연결형 서비스**

UDP는 비연결 서비스로 연결을 설정하고 해제하는 과정이 없다. 서로 다른 경로에서 패킷을 독립적으로 전송하지만, 패킷에 순서를 부여하여 재조립하거나 흐름 제어, 혼잡 제어같은 기능도 처리하지 않기 때문에 **TCP보다 속도가 빠르고 네트워크 부하가 적다.** 하지만, 위와 같은 이유로 신뢰성있는 데이터 전송이 불가능하여 **신뢰성보다 연속성이 중요한 서비스(ex 스트리밍 서비스)에 사용된다.  **

## **TCP vs UDP**

**공통점**

-   포트 번호를 이용하여 주소를 지정한다.
-   데이터 오류검사를 위한 체크섬이 사용된다.

**차이점**

<table style="border-collapse: collapse; width: 100%; height: 172px;" border="1" data-ke-style="style12"><tbody><tr style="height: 20px;"><td style="width: 29.1861%; height: 20px;">&nbsp;</td><td style="height: 20px; width: 37.5582%; text-align: center;"><b>TCP</b></td><td style="width: 33.2557%; text-align: center; height: 20px;"><b>UDP</b></td></tr><tr style="height: 19px;"><td style="width: 29.1861%; height: 19px;"><b>연결방식</b></td><td style="width: 37.5582%; height: 19px;">연결형서비스</td><td style="width: 33.2557%; height: 19px;">비 연결형 서비스</td></tr><tr style="height: 19px;"><td style="width: 29.1861%; height: 19px;"><b>패킷 교환 방식</b></td><td style="width: 37.5582%; height: 19px;">가상 회선 방식</td><td style="width: 33.2557%; height: 19px;">데이터그램 방식</td></tr><tr style="height: 19px;"><td style="width: 29.1861%; height: 19px;"><b>전송 순서</b></td><td style="width: 37.5582%; height: 19px;">전송 순서 보장</td><td style="width: 33.2557%; height: 19px;">전송 순서가 바뀔 수 있음</td></tr><tr style="height: 19px;"><td style="width: 29.1861%; height: 19px;"><b>수신 여부 확인</b></td><td style="width: 37.5582%; height: 19px;">수신 여부를 확인함</td><td style="width: 33.2557%; height: 19px;">수신 여부를 확인하지 않음</td></tr><tr style="height: 38px;"><td style="width: 29.1861%; height: 38px;"><b>통신 방식</b></td><td style="width: 37.5582%; height: 38px;">1:1 통신만 가능</td><td style="width: 33.2557%; height: 38px;">1:1 / 1:N / N:N 통신 모두 가능</td></tr><tr style="height: 19px;"><td style="width: 29.1861%; height: 19px;"><b>신뢰성</b></td><td style="width: 37.5582%; height: 19px;">높음</td><td style="width: 33.2557%; height: 19px;">낮음</td></tr><tr style="height: 19px;"><td style="width: 29.1861%; height: 19px;"><b>속도</b></td><td style="width: 37.5582%; height: 19px;">느림</td><td style="width: 33.2557%; height: 19px;">빠름</td></tr></tbody></table>
