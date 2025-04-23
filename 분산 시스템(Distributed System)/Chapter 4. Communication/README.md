# Chapter 4. Communication 정리 내용

프로세서간의 커뮤니케이션(통신모델)이 필요함
<br>
- 통신 모델
  - RPC(Remote Procedure Call) : 원격 프로시저 호출
  - MoM(Message Oriented Middleware) : 메신저를 전송하는 방식
  - Streaming : Audio, Video 전송/처리
  - MC(Multi Casting)

<br>

## 계층간 프로토콜

<img width="618" alt="image" src="https://github.com/user-attachments/assets/9382b5db-09b5-4197-a9e3-809650eb6a4f" />

- 통신모델의 표준으로 채택
- 동등한 레벨끼리 통신
- 실제에서는 사용하지 않음
- 다른 모델의 참고(Reference) 모델

<br>

### 7계층의 헤더

<img width="730" alt="image" src="https://github.com/user-attachments/assets/cd5263f4-33eb-463b-baea-45fd42605b08" />

- 통신모델의 각 Layer별 데이터 구분/처리를 위해 Header 사용
- Header에서 가장 중요한 정보는 주소
- DataLink 계층은 데이터 오류검출을 위해 Trailer 사용

<br>

### 미들웨어 프로토콜

<img width="696" alt="image" src="https://github.com/user-attachments/assets/ef9fea5c-9aa7-49ba-ba9b-fbbeec2cd846" />

- Application Layer와 Transport Layer 사이에 위치
- Transport Layer 이하는 O/S 영역
- 구현은 Application Layer를 2단계로 세분화하여 상위는 Application 영역으로, 하위는 Middleware 영역으로 사용
- Middleware의 목적(기능)
  - 분산처리
  - Real Time Data 처리: Streaming, Sync(동기)
  - Multi Casting
  - Authorization(인증) 등

<br>

#### 저장방법에 따른 분류

- Persistent(영속적) : 통신 시 데이터 보관을 위한 중간 Storage 존재
  - Ex) MailBox (스토리지가 있어 나중에 볼 수 있음)
- Ransient(임시적) : 통신 시 데이터 보관을 위한 중간 Storage 없음, Buffer로 데이터 유지
  - Ex) Web
 
<br>

#### 동기 방식에 따른 분류

- Synchronous(동기) : 통신 시 데이터 교환 과정에 Block 및 Wait 존재
- Asynchronous(비동기) : 통신 시 데이터 교환 과정에 Block 및 Wait 없음
- 클라이언트/서버 모델에서 서버가 어떻게 응답하는가에 따라 Block 및 Wait 시간 단축

<br>

## Conventional Procedure Call(RPC)

<img width="620" alt="image" src="https://github.com/user-attachments/assets/c0d63355-5284-470e-a9f6-ef3268e4e25b" />

- Local 내에서 Procedure 호출 시 매개변수 전달, 복귀주소 보관을 위해 Stack 사용
- Stack에 보관되는 내용
  - 복귀주소
  - Local 변수
  - Parameter 등
 
<br>

### Client and Server Stubs

- 리모트인 경우에는 스택이 없기에 스텁을 이용하여 메시지를 생성
- 클라이언트에서 서버로 Procedure 호출 시 Memory Stack을 사용할 수 없음 (Stub 사용)
- Stub : 원격 Procedure 호출 시 메시지 전달 역할
  - 메시지 내에는 호출할 Procedure 명, 전달
  - 매개변수, Return 값 등을 보관
  - 메시지 생성, 해독, 해제 기능 수행
- Marshalling : 전달 데이터를 메시지로 Pack
- Unmarshalling : 메시지를 데이터로 Unpack

<img width="749" alt="image" src="https://github.com/user-attachments/assets/69efb43c-4123-4cb2-a76f-b63dbffc191b" />

<br>

### Remote Procedure Calls

- 원격에 있는 컴퓨터의 함수를 마치 로컬 함수처럼 호출할 수 있게 해주는 통신 메커니즘

<br>

- RPC 호출 순서

1. 클라이언트 Procedure가 클라이언트 Stub호출
2. 클라이언트 Stub은 메시지를 생성하고 O/S 호출(System Call)
3. 클라이언트 O/S는 메시지를 서버 O/S로 전송
4. 서버 O/S는 메시지를 서버 Stub으로 전달
5. 서버 Stub은 메시지를 해독하여 서버 Procedure 호출 및 매개변수 전달
6. 서버 Procedure는 처리 후 결과를 서버 Stub으로 전달
7. 서버 Stub은 메시지를 생성하고 O/S 호출
8. 서버 O/S는 클라이언트 O/S로 메시지 전송
9. 클라이언트 O/S는 메시지를 클라이언트 Stub으로 전달
10. 클라이언트 Stub은 메시지를 해독하여 클라이언트 Procedure에게 전달


<br>

### 비동기 RPC

<img width="554" alt="image" src="https://github.com/user-attachments/assets/26cd25ce-4a04-478b-93fa-620d82269a0e" />

<img width="594" alt="image" src="https://github.com/user-attachments/assets/006e62fc-8b0b-4193-b8e3-8bd77ea0eb26" />

<img width="741" alt="image" src="https://github.com/user-attachments/assets/3c669dc8-491f-4900-9fc6-8cbf217e3af7" />

- 서비스 요청 후 완료까지 Block 및 Wait 거의 없음(최소화)
- 서버로 메시지 전송 후 클라이언트는 다른 작업 수행
- Two asynchronous 절차
  1. 클라이언트 Procedure가 서버 Procedure 호출
  2. 서버는 클라이언트에게 Accept 메시지 전송
  3. 클라이언트 Accept 메시지 수신 후 작업 계속 수행
  4. 서버 Procedure의 수행완료 후 결과를 Reply 메시지로 전송
  5. 클라이언트는 현재 수행중인 작업을 Interrupt 한 후 Reply 메시지 수신
  6. 클라이언트는 서버로 ACK 메시지 전송

<br>

## MOM(Message Oriented Middleware)

- Message Oriented Communication의 한 방식
- 비동기 메시지 전달에 기초
- 종류
  - Persistent : MOM, MQM(Message Queuing Model)
  - Transient: MPI(Message Passing Interface)
 
<br>

### MPI(Message-Passing Interface)

<img width="782" alt="image" src="https://github.com/user-attachments/assets/71037114-827a-4cbe-afe1-a94aa8e2882a" />

<img width="744" alt="image" src="https://github.com/user-attachments/assets/68543a4b-27e0-4d9a-8cdf-7d0d17abbaea" />

- 분산시스템 환경에서 프로세스들 사이의 통신을 오직 메시지 송·수신으로만 구현
- Socket 통신(프로그램) 응용
- Transient지만 동기/비동기 통신 모두 지원
- Primitive 중 isend(비동기), issend(동기방식)는 Reference 전송 수행
- 원시적 Socket 기능 보완

<br>

<br>

### MQM (Message Queue Model)

<img width="598" alt="image" src="https://github.com/user-attachments/assets/d11bb411-d024-4606-be0a-5280f3fdcfbb" />

<img width="773" alt="image" src="https://github.com/user-attachments/assets/65083360-69e5-4660-8669-abbbc1665486" />

- Persistent 방식 : 전송경로 중간에 Storage 존재
- Loosely Coupled 형태
  - Sender와 Receiver가 Decoupling 되어있음
  - Sender가 전송한 메시지는 Queue(Storage) 에 저장
  - Receiver는 Sender의 메시지를 사용가능한 시점에 Storage에서 가져옴

<br>

기능 
- Put : 비동기
- Get : 스토리지
- Poll : 메시지확인
- Notify : 메시지가 들어오면 도출되도록 함

<br>

### MQ(Message Queuing) 시스템 아키텍처

<img width="733" alt="image" src="https://github.com/user-attachments/assets/9144a324-3e46-472e-8f09-7a5c48a37aa4" />

<br>

- 큐잉레이어에서 센더측은 DB를 가지고 있음
- 그 DB에 있는 네트워크 주소(TCP/IP and Port) 와 큐주소를 룩업 맵핑
- 리시버로 전송, Sender와 Receiver는 동시에 켜져있을 필요가 없음
- 동일 Network Level인 경우(근거리)

<br>

<img width="823" alt="image" src="https://github.com/user-attachments/assets/80aadcfd-cf98-42a1-b9dc-2c19194a069c" />

<br>

- 송·수신측 Router에도 Queue 존재
- Store-and-Forward 방식 (라우터를 거칠 때 상황에 따라서, 저장 또는 포워드)
- 수신한 메시지는 Queue에 보관하였다가 Network가 사용 가능할 때 전송
- Wide Area : 동일한 서브넷이 있지 않기 때문에 중간 라우터를 통과하여 목적지에 가야 하며, 센더 리시버의 큐 그리고 여러 라우터의 큐를 통과하여 전달

<br>

### MB(Message Broker)

<img width="673" alt="image" src="https://github.com/user-attachments/assets/cbf22bbd-396f-4ea5-b589-74a6fca8d254" />


- MOM  센더와 리시버가 메시지 포멧들이 다른 경우에는 포멧들을 변환할 수 있는 메시지 브로커가 필요
- 중간 메시지 브로커를 이용하여 목적지까지 이동

<br>

### 대표적 Queuing System - IBM

<img width="803" alt="image" src="https://github.com/user-attachments/assets/b70e56b4-9081-428c-842c-de248c2af5e2" />

- Sending Client & Receiving Client
- 네트워크 중간에 큐매니저가 존재
- 동일망 (Subnet) 내의 클라이언트와 큐매니저는 RPC 통신(동기방식)으로 전달
- 참고로 RPC는 Stub을 사용하고 역시 리시버 클라이언트도 Stub하고 마쉘링/언마쉘링을 사용

<br>

### Message Trasnsfer 

<img width="812" alt="image" src="https://github.com/user-attachments/assets/a17c4d99-0344-4f35-9faa-b38145d6dbd4" />

- 라우팅 테이블과 별칭을 사용하는 MQ 대기열 네트워크의 일반적인 구성
  - QMA : Queue Manager A
  - QMB : Queue Manager B
  - QMC : Queue Manager C
  - QMD : Queue Manager D
- 큰 직사각형 4개는 Subnet 을 의미
- 각 서브넷마다 Queue Manager 존재
- 원거리 전송 시 Queue Manager가 Relay 역할
- Routing Table에 따라 전달 경로 결정 - Routing Table이 중요한 역할

<br>

## QoS(Quality of Service)

QoS : QoS는 네트워크 상에서 데이터 전송의 품질을 보장하기 위한 기술 및 개념

<br>
<br>

### QoS 속성
- Bitrate(전송률)
- Delay: Session Setup, End to End, Variance, Jitter, RTT(왕복이동)

<br>

## Overlay Construction (Ap-level)

<img width="675" alt="image" src="https://github.com/user-attachments/assets/7d767d7e-059a-47a5-aec8-975e7b952388" />

- Application Level에서 수행
- Physical 망 위에 Logical Tree(전송경로)를 생성하여 이에 따라 메시지 전송
- 각각의 Multicast Group마다 Multicast Tree 생성- Multicast Tree 생성에는 Dijkstra 알고리즘 사용

<br>

<img width="750" alt="image" src="https://github.com/user-attachments/assets/98e4b97b-7ebb-415e-b03b-8019b01d6a70" />

- Flooding 방식
    - 메시지를 수산하면 다른 Node에 전달 (단순)
    - Epidemic 알고리즘이 대표적 (중앙코드네이터없이 분산적으로 빠르게 퍼트림)
- 목표 : 정보를 가장 빠르게 네트워크 내 모든 노드에게 전달
- 단점 : 오버헤드가 큼
- 노드타입 : 감염/미감염/감염이됐지만 전파능력이 없거나 하고싶지 않은 노드

<br>

<img width="648" alt="image" src="https://github.com/user-attachments/assets/093dbcd9-97f8-4239-ab08-854c71e5728d" />

전파모델: Anti-Entropy, 임의의 Node를 선택하여 메시지 전달
- P라는 노드가 Q라는 노드를 랜덤으로 지정
- Push: Infected Node(P)가 Susceptible Node(Q)에게 메시지전달 (자신이 감염이 되어있어야 전달)
- 한계 : Infected가 될 수록 감염 안된 (Susceptible) 노드를 찾기 어려움
- Pull: Susceptible Node(P)가 Infected Node(Q)에서 메시지 를 가져옴 (비감염이  감염으로 부터 정보를 가져옴)
- P와 Q는 상황에 따라 Push/Pull 한다. (가장좋은 전파 모델)

<br>

<img width="610" alt="image" src="https://github.com/user-attachments/assets/08a53cfa-557b-4129-911c-1188c0f04cfd" />

- 메시지 전파속도: O(log2n) (처음 하나의노드로 시작해서 2의배수로 진행되어감)
  - Ex) 1-2-4-8 8개의 노드를 3번만에 모두감염
- n은 전체 Node의 개수
- Scalable함
- 전파 중 Stop될 확률이 Rumor Spread와 유사
- 모든 Node에게 메시지가 다 전파되지 않을 수 있음

<br>

<img width="576" alt="image" src="https://github.com/user-attachments/assets/a1d4540f-c7ea-4d14-a05b-9e409c260634" />

