# 로드밸런싱(Load Balancing)
하나의 인터넷 서비스가 발생하는 트래픽이 많을 때 
둘 이상의 CPU or 저장장치와 같은 컴퓨터 자원들에게 균등하게 작업을 나누는 것  

![image](https://user-images.githubusercontent.com/60323625/105006576-319fd380-5a7a-11eb-802d-eb10a44fe435.png)

* 트래픽을 감당하기에 서버가 부족한 경우 대응 방안
1. Scale-up: 하드웨어의 성능을 올리는 것 - 서버 자체의 성능 향상
2. Scale-out: 기존 서버와 동일하거나 낮은 성능의 서버를 두 대 이상    증설하여 운영하는 것


## 로드 밸런서 종류

### 운영체제 로드밸런서
- 물리적 프로세서 간에 작업을 스케줄링  
### 네트워크 로드밸런서
- 사용 가능한 백엔드에서 네트워크 작업을 스케줄링  

# 네트워크 로드밸런서 종류
OSI 7 계층에서 각각의 계층이 L1/L2/L3‥‥L7에 해당한다.   

## L4(Transport Layer)  
: Transport Layer(IP+Port) Load Balancing  

![image](https://user-images.githubusercontent.com/60323625/105011081-da046680-5a7f-11eb-8f1a-2553b50f68ba.png)
![image](https://user-images.githubusercontent.com/60323625/105014274-914eac80-5a83-11eb-9a0d-cfae05ace555.png)
1. 브라우저에서 도메인 주소 입력
2. 로컬 DNS가 획득한 VIP주소 전송
3. 획득한 정보를 기반으로 VIP로 http 요청
4. 로드밸런서는 최적의 서비스 서버를 알고리즘을 통해 선별하고 요청 전송
5. 전달받은 http결과를 로드밸런서를 통해 client에 전송

## L7(Application Layer)  
: Application Layer(사용자 Request) Load Balancing    

![image](https://user-images.githubusercontent.com/60323625/105011158-f2748100-5a7f-11eb-8f9a-9dc6cc50ca07.png)
1. http header, cookie 등 사용자의 요청을 기준으로 특정 서버에 트래픽 분산이 가능하다.
2. URL에 따라, 쿠키 값에 따라 세분화하여 로드를 분산시킬 수 있다.
3. 특정한 패턴을 가진 바이러스를 감지하고 DDos와 같이 비정상적인 트래픽 필터링이 가능하다.
![image](https://user-images.githubusercontent.com/60323625/105013278-6e6fc880-5a82-11eb-944a-42efa51d5c45.png)

# 로드 밸런서 주요기술
 ## 1. NAT 
private IP 주소 -> public IP 주소
 ## 2. DSR (Dynamic Source Routing protocol)
요청에 대한 응답을 로드밸런서가 아닌 클라이언트의 IP로 전달하여 전달과정을 간단하게 한다.
## 3. Tunneling
데이터를 캡슐화하여 연결된 노드만 캡슐을 해제할 수 있게 만든다

# 로드 밸런싱 동작방식 
## Bridge / Transparent Mode
![image](https://user-images.githubusercontent.com/60323625/105022149-c1e71400-5a8c-11eb-95cb-082c72eb4baf.png)

유저가 서비스를 요청하면 로드밸런서는 전달받은 목적지 IP주소를  실제 서버 IP주소로 변조하고    
 MAC주소도 변조하여 목적지를 찾아간다.


## [다른 동작방식](https://pakss328.medium.com/%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%84%9C%EB%9E%80-l4-l7-501fd904cf05)  

# 로드밸런싱 알고리즘
### 1. 라운드로빈 방식(Round Robin Method)
    : 요청을 순서대로 각 서버에 균등하게 분배하는 방식    
    다른 알고리즘에 비해서 가장 빠름
    L4에서 주로 사용
### 2. 가중 라운드로빈 방식(Weighted Round Robin Method)
    : 서버마다 가중치를 두어 서버에 연결된 Connection 수와 같이 고려하여 할당  

    서버의 트래픽 처리 능력이 상이한 경우 사용
### 3. 최소 연결 방식(Least Connection Method)
    :  서버에 연결되어 있는 Connection 개수를 단순비교하여 가장   적은곳에 연결  

    트래픽으로 인해 세션이 길어지는 경우에 사용
### 4. IP 해시 방식(IP Hash Method)
    : 클라이언트의 IP 주소를 특정 서버로 매핑하여 요청을 처리하는 방식   

      특정 사용자가 항상 같은 서버로 연결되는 것을 보장
     

### 5. 최소 리스폰타임(Least Response Time Method)
    : 서버의 현재 Connection 개수와 응답시간(서버에 요청을 보내고 최초 응답을 받을 때까지 소요되는 시간)을 모두 고려하여 트래픽을 분배    

    가장 적은 연결 상태와 가장 짧은 응답시간을 보이는 서버에 우선적으로 로드가 분배됨


# 로드 밸런싱 failover
로드 밸런서에 문제가 생길 수 있기 때문에 이중화 구성을 한다.    

![image](https://user-images.githubusercontent.com/60323625/105011911-e3420300-5a80-11eb-870f-4ff2170941cb.png)


## 출처
[참고1](https://pakss328.medium.com/%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%84%9C%EB%9E%80-l4-l7-501fd904cf05)
[VIP란](https://run-it.tistory.com/44)