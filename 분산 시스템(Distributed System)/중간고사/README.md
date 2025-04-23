# 1강 Introduction 정리 내용


## 분산 시스템의 투명성(Transparency in a Distributed System)

- 분산 시스템에선 투명성이 중요함
- 투명성은 모두 보이지 않는(Hide) 것을 뜻 함

|타입|설명|
|---|---|
|접근(Access)|데이터의 표현 방식과 자원 접근 방식을 숨김|
|위치(Location)|자원이 어디에 위치하는지 숨김|
|이동(Migration)|자원이 다른 위치로 이동할 수 있음을 숨김|
|재배치(Relocation)|자원이 사용 중 다른 위치로 이동할 수 있음을 숨김|
|복제(Replication)|자원이 복제되어 있음을 숨김|
|동시성(Concurrency)|자원이 여러 경쟁 사용자에 의해 공유될 수 있음을 숨김|
|실패(Failure)|자원의 실패와 복구를 숨김|

## 확장성의 문제점

|개념|예시|
|---|---|
|중앙집중형 서비스(Centralized services)|모든 사용자를 위한 단일 서버(모든 사용자가 하나의 서버에 접속)|
|중앙집중형 데이터(Centralized data)|하나의 온라인 전화번호 북(정보가 한 곳에 모여있음)|
|중앙집중형 알고리즘(Centralized algorithms)|전체 정보를 바탕으로 라우팅 수행(모든 정보를 알고 있는 상태에서 결정|

<br>

### 비중앙 분산 알고리즘의 주요 특성

- 어떤 머신도 시스템 상태에 대한 완벽한 정보를 가지지 않음 (각 노드는 전체 시스템이 아닌 자신이 관찰할 수 있는 일부 정보만 알고 있음)
- 머신은 오직 지역(local) 정보만을 기반으로 결정 내림 (각 노드는 독립적으로 판단하고 동작하며, 중앙 제어 없이 작동)
- 하나의 머신이 실패해도 알고리즘 전체는 망가지지 않음 (일부 노드 실패에도 시스템 전체가 지속적으로 작동 가능)
- 전역 시계(global clock)의 존재를 전제로 하지 않음 (노드 간의 시간 동기화가 필요 없음)

<br>
<br>

## 확장성을 위한 스케일링 기술(Scaling Techniques)

크게 크기, 거리, 관점 세 가지 항목으로 볼 수 있음

- 크기 : 시스템의 노드 수, 데이터 양 등
- 거리 : 캔버스-시-국가-글로벌 단위 등
- 관리 : 중앙집중 (관리 쉬움)

<br>

### 거리의 예시

<img width="704" alt="image" src="https://github.com/user-attachments/assets/bef7597d-5a7f-422c-8729-c127fca80461" />


- a는 서버 측에서 유효성 검사를 하는 경우
  - 구조 : 사용자가 클라이언트에서 폼 작성 시 그 데이터가 그대로 서버로 전송됨
  - 동작 방식
    1. 클라이언트는 오직 입력
    2. 서버가 폼의 유효성 검사(Check form) 수행
    3. 검사 통과 후 폼 처리(Process form)
  - 특징
    1. 서버에 부담 집중
    2. 사용자 경험(UX)이 떨어질 수 있음 (입력 후 대기)
    3. 트래픽 증가 : 불완전한 데이터도 서버로 전송
    4. 확장성이 낮음
       
- b는 클라이어트 측에서 유효성 검사를 하는 경우
  - 구조 : 클라이언트에서 먼저 유효성 검사를 수행한 후, 서버로 전송
  - 동작 방식
    1. 클라이언트가 실시간으로 유효성 검증(ex 이메일)
    2. 문제가 없을 경우 데이터 서버로 전송
    3. 서버는 바로 폼 처리
  - 특징
    1. 서버 부하 감소
    2. 사용자에게 빠른 피드백 UX 향상
    3. 네트워크 효율성 향상
    4. 확장성 좋음

<br>
<br>

## 분산 시스템 개발 시 문제점

- 네트워크가 안전하지 않음
- 토폴로지가 변하지 않음
- 딜레이가 생김
- BandWith(링크)가 제한적임
- 비용이 발생함
- 여러 관리자가 필요함

<br>
<br>


## 분산 시스템 유형(DIS/DCS/DES)

### DIS(Distributed Information System)

- 데이터가 분산되어 있는 정보 시스템
- DIS에서는 여러 노드/서버가 협력해서 트랜잭션 처리
- 예로 TPS(Transaction Processing System), EAI(Enterprise Application Intergration) 등이 있음
- TPS(Transaction Processing System) : 거래 처리 시스템
  - 은행 등 같은 곳에서 건당으로 처리하는 방식
  - TP 모니터를 써서 감시를 많이 함, 밸런스 있게 로드함 (TPS 시스템의 경우 이 방식을 많이 씀)
- EAI(Enterprise Application Intergration)
  - 서버와 클라이언트 사이 미들웨어가 있음
  - 여러 역할을 분담해서 동작하는 방식


<br>

### 트랜잭션

<img width="778" alt="image" src="https://github.com/user-attachments/assets/3d404ea7-dccb-4e80-85b9-138363f9311d" />

- BEGIN_TRANSACTION : 트랜잭션 시작 표시
- END_TRANSACTION : 트랜잭션 종료 후 커밋 시도
- ABORT_TRANSACTION : 트랜잭션 종료 후 이전 값 복원
- READ : 파일, 테이블 또는 기타에서 데이터를 읽음
- WRITE : 파일, 테이블 또는 기타에 데이터 쓰기

<br>

- 트랜잭션의 속성
  - 원자성(Atomic) : 각각의 트랜잭션은 **분리되면 안됨**
  - 일관성(Consistent) : **일관성을 유지**해야 함
  - 격리(Isolated) : 다른 트랜잭션에 **간섭을 주면 안됨**
  - 지속성(Durability) : 트랜잭션 성공 시 **영구적으로 저장**되어야 함

<br>

### 분산된 정보 시스템의 예(트랜잭션 데이터베이스)

<img width="549" alt="image" src="https://github.com/user-attachments/assets/00dedf46-9451-44cc-bb1a-3ed8510a0fc4" />

- 사용자 측면에서 항공사와 호텔 예약이 한번에 되며 각각 분산된 데이터베이스를 가짐
- 일치가 되지 않으면 커밋 안됨

<br>


### 분산 시스템에서의 TP 모니터 역할

<img width="786" alt="image" src="https://github.com/user-attachments/assets/b1b34b5e-1048-4b59-9991-8df64143d6cc" />

- 개별적인 서버에서 각각의 트랜잭션 수행
- TP 모니터에서는 각각의 서브 트랜잭션 요청, 하나의 트랜잭션으로 취급

<br>

### DCS (Distributed Computer System)
- 컴퓨터의 파워를 분산하여 태스크를 수용하는 방식
- 클러스팅 컴퓨터 시스템, 그리드 컴퓨터 시스템이 예로 있음

<br>

### 클러스팅 컴퓨터 시스템

<img width="770" alt="image" src="https://github.com/user-attachments/assets/403fa399-d9a6-4204-9c4e-7cc236a6614f" />

- 마스터와 슬레이브 개념 분리 (슬레이브가 노드)
- 마스터 -> 슬레이브로 태스크를 줌 (병렬 통신)

<br>

### 그리드 컴퓨터 시스템

<img width="686" alt="image" src="https://github.com/user-attachments/assets/24090ca9-5bc0-4135-92ff-f3ce227bdac9" />

- 클러스터와 비슷하지만 서로 다른 컴퓨터, OS가 다르더라도 가능
- 4개의 계층 구조로 되어 있음
  - Collective layer : 전체 자원, 흩어진 자원 관리
  - Connectivity layer : 네트워크
  - Resource layer : 개별적 자원 관리
- 그리드 미들웨어 사용

<br>

### DES(Distributed Embedded System)

- 개별 컴포넌트에 저장되어 있는 시스템으로 전체 시스템에 분산되어 있음
- 어떠한 환경에 변화가 있을 때 이를 처리할 수 있어야 함
- 구성을 바꿔가며 사용 가능
- 공유 인지 기능이 있음
- 최근 분산 시스템이 DES 시스템임

<br>

### 센서 네트워크

- 연구는 되고 있으나 많이 활성화되지 않음
  - 활성화 되지 않는 이유
    1. 센서 제어는 어디서 할 것인가
    2. 네트워크가 끊어지면 어떻게 할 것인가
- 여러 센서들이 동작하나 배터리로 동작하는데 배터리 없으면 답이 없음
- 센서들이 루트 시스템 쪽으로 데이터를 전송함(데이터 중앙 집중형)
- 데이터가 분산되어 각각 센서들이 가지고 있는 형태가 분산 시스템임
  - 각각의 센서들이 정보가 있으니 서로 요청함
  - 중앙 시스템이 없으니 쿼리 등으로 물어봐야 함 (센서는 오직 답변만 함)

<br>
<br>
<br>

# Chapter 2. 아키텍처 정리 내용

## 분산 시스템 구조


## 논리적 구조

논리적 구조는 크게 4가지로 구성됨

- 계층 구조(Layered architectures)
- OOP 구조(Object-based architectures)
- 데이터 기반 구조(Data-centered architectures)
- 이벤트 기반 구조(Event-based architectures)

<br>

### 계층 구조

- 상위 계층 -> 하위 계층 요청
- 하위 계층 -> 상위 계층 응답
- RPC(Remote Procedure Call) 방식
- 대표적으로 OSI 7계층 또는 5층 TCP/IP 계층을 예로 들 수 있음

<img width="480" alt="image" src="https://github.com/user-attachments/assets/a62cce38-3fbe-40c0-9571-2f4fb9418f2b" />


<br>

### OOP 구조

- 오브젝트 기반 구조로 여러 객체(다른 시스템)에서 함수를 콜하며 task 진행
- 원격에서 호출하는 방식
- RMI(Remote Methmod Invocation) 방식
- CORBA, NFS 등이 있음

<img width="426" alt="image" src="https://github.com/user-attachments/assets/7c79361c-623e-4b80-918a-0fca67017fc7" />


<br>

### 데이터 기반 구조

- 하나의 공유 데이터 공간에 구독하여 확인하는 방식
- Publish 되더라도 다른 컴포넌트들이 즉시 수신하지 않아도 됨
- 이벤트 기반 구조에 비해 편리함

<img width="648" alt="image" src="https://github.com/user-attachments/assets/507421c2-e9d9-4833-b8e5-d337dede6315" />

<br>

### 이벤트 기반 구조

- 컴포넌트들이 이벤트 버스 - 관심 토픽에 Subsbribe 해야 함
- 이벤트 버스가 Publshing 시 컴포넌트들이 즉시 수신해야 함

<img width="605" alt="image" src="https://github.com/user-attachments/assets/c20437cd-3a70-4918-8d42-a465b79d8433" />


<br>

## 물리적 구조


물리적 구조는 중앙집중형과 비집중형으로 나뉨

- Centralized(중앙집중형)
  - 클라이언트-서버(Client-Server Model)
  - 어플리케이션(Application) Layering
  - 다계층(Multitier)
- Decentralized(비집중형)
  - Peer-to-Peer
 
<br>

### Client-Server Model

<img width="690" alt="image" src="https://github.com/user-attachments/assets/28acc50a-4d4c-4dc8-9ebe-3844b1cab496" />

- 클라이언트 요청 -> 서버 응답
- 서버의 처리시간 동안 클라이언트 대기
- 서버의 응답을 클라이언트가 수신 후 다음 단계 진행

<br>

### 어플리케이션 구조

- 계층 구조
- UI 레벨
- 처리 레벨
- 데이터 레벨
- 각 계층별 task를 분리함

<br>

<img width="789" alt="image" src="https://github.com/user-attachments/assets/1a11f4a7-9f76-4d7f-810c-cfef9c7c59d4" />

<br>

- 유저, 프로세싱, 데이터 3계층으로 나뉨
- 유저는 키 값을 주고 쿼리제네레이터를 통해 해당 DB로 접근
- 데이터에서 웹 페이지 타이틀과 정보를 랭 킹알고리즘을 통해 리스트화
- HTML 제너레이터를 통해 다시 유저의 화면에 뿌려줌
- 중앙집중은 하나의 데이터 베이스
- 클라이언트와 서버와의 소통

<br>

### 다계층 구조

- 클라이언트와 서버의 구조를 다양한 계층으로 나눔
- 클라이언트 머신의 능력이 중요함
- 가장 대표적으로 UI는 클라이언트, 서버는 어플리케이션과 DB를 담당

<br>

## 비집중형 구조

Peer-to-Peer(P2P) 모델

<br>
<br>

P2P 세대별 구분
- 1세대 : Server-Oriented 방식
  - 서버 Down 시 전체 시스템 Fail 발생
  - Ex) Napster
- 2세대 : Flooding Query Model (1세대 개선 방식)
  - 쿼리를 채널 내 모든 컴포넌트들에게 전달 (파일 분산)
  - 오버헤드가 큼
  - Ex) Gnutellar
- 3세대 : Super Peer
  - 각 계층별 컴포넌트들을 그룹화 하여 관리
- 4세대 : Structured
  - DHT를 이용하는 것이 대표적
  - Ex) Kedemlia
 
<br>

## 해시테이블과 DHT

<img width="284" alt="image" src="https://github.com/user-attachments/assets/5cdca9fc-5d0f-457b-b9e7-f81b55be0cdf" /> <img width="390" alt="image" src="https://github.com/user-attachments/assets/7ed5f60c-5a8e-4536-bb19-0ab461242013" />

- 해시 테이블
  - 해시 테이블 함수를 정함 (해시 인덱스를 찾기 위해서)
  - modular 방식 - 나머지 계산에 많이 사용
  - 사전형식 처럼 lookup이 빠름
- 분산 해시 테이블(Distributed Hash Table)
  - 사전형식 처럼 lookup이 빠름
  - Hash Table의 빠른 검색속도를 이용
  - Peer 마다 Hash Table 공간 할당
  - 분할 단위 : Bucket
  - Bucket 구성이 중요한 과제

<br>

## P2P 구조 아키텍처

### Chord 방식(1차원)

<img width="506" alt="image" src="https://github.com/user-attachments/assets/a1e2a52f-dc10-498a-8300-b872280aba66" />

- 각 Node마다 Key 범위를 할당하여 분할

<br>

### CAN(Context Area Network)

<img width="422" alt="image" src="https://github.com/user-attachments/assets/350fd0d8-0a21-42b1-abb2-843aae6b6f40" /> <img width="418" alt="image" src="https://github.com/user-attachments/assets/7057082b-5811-4e65-9d4e-dc78faf1634b" />


- 각 Node마다 Key면적(2차원)을 할당
- 데이터를 Hashing하여 나타난 좌표(x,y)에 맞는 Node에 저장
- 전체 공간은 고정되어 있으므로 Node 추가시 영역을 분할

<br>

## P2P 비구조 아키텍처

- 쉽게 피어끼리 서로 주고 받는 형식
- 비구조 방식에는 Push와 Pull 두 가지 방식이 있음
- 액티브 방식과 패시브 모드 방식이 있음
- 액티브는 주로 Push, 패시브는 주로 Pull

<br>

### 오버레이 네트워크의 토폴로지 관리

<img width="769" alt="image" src="https://github.com/user-attachments/assets/5678ad90-1c3c-4284-881a-c61e1c9f44d5" /> <img width="785" alt="image" src="https://github.com/user-attachments/assets/538d05eb-7a24-4ceb-a1a9-3c97d928edbe" />

- Structured Overlay : 토폴로지 특정노드에 대한 링크
- Random Overlay : 랜덤 노드간 아는 정보를 교환
- 각 Node는 자신만의 Network Partial View 를 가짐
- Partial View를 교환할 Node를 임의로 선택
- 다른 Node와 Partial View를 교환하면서 전체 Structure 완성
- 동작 구조
  - Active, Passive의 2개 Thread 존재
  - Active Thread
    - Push Mode : C개의 Partial View를 임의의 P에게 전송
    - Pull Mode : 다른 Node로부터 View를 요청·수신
    - Passive Thread : Pull Mode만 존재

<br>

### 슈퍼 피어(Super Peer)

<img width="614" alt="image" src="https://github.com/user-attachments/assets/cca247a1-d9a7-491c-bf9d-a28e0199e1a1" />


- 그룹을 나누고 그 그룹에서 그룹장(슈퍼피어)을 두고 그룹장이 그룹을 관리함

<br>

### Edge-Server 시스템 (하이브리드)

<img width="751" alt="image" src="https://github.com/user-attachments/assets/09a82a5e-1658-4088-b5b2-dd6c59058058" />

- Structured P2P와 Unstructured P2P의 혼합
- Centralized Architecture
- Ex) Internet

<br>

### Collaborative Distributed Systems (하이브리드)

<img width="810" alt="image" src="https://github.com/user-attachments/assets/404808ca-1384-442b-9300-cb559b71a205" />

- File을 여러 개의 Node에서 Scattered Query 하여 각 Node가 Partial 전송
- 3종의 Component로 구성
  - 요청(Redirect) : 클라이언트의 요청을 다른 서버로 Redirect 시키는 Component
  - 접근(Analyzing) : 접근 패턴을 분석하는 Component
  - 복제(Replication) : Web Page의 복사본을 관리하는 Component
 
<br>

### 인터셉터

<img width="551" alt="image" src="https://github.com/user-attachments/assets/f4acc18f-185f-43fd-a5a6-93d73bd0cff6" />

- Interceptor: 어떤 Level에서 발생한 쿼리를 처리하기 쉽도록 중간에서 구분/변환하여 다음 Level로 전달
- 변환자(Interceptor)는 각 Level 중간에 위치

<br>

### Feedback Control Model(Adaptive 방식)

<img width="788" alt="image" src="https://github.com/user-attachments/assets/87a70c95-bc5d-4178-87fb-a2a0b4278c93" />

- 노이즈, 외부 사건 등 제어할 수 없는 것들이 발생하면 한계가 있기에 피드백을 통해 보정함

<br>
<br>
<br>

# Chapter 3. 프로세스 정리 내용


Process는 분산시스템에서 중요환 관점이 될 수 있음
<br>
클라이언트/서버 구조를 효율적으로 구현하기 위한 3가지 관점
- 스레드
- 가상화
- 클라이언트-서버 구성


<br>

## 스레드


<br>

### 비 분산시스템 에서의 스레드 사용

<img width="719" alt="image" src="https://github.com/user-attachments/assets/68b0674d-09a9-415c-8606-6c0a0ea034aa" />

- A에서 작업하던 단일코어가 B로 할당
- A에서 작업을 하다가 B로 넘어가기전 기존 A Context를 스택에 저장
- 나중에 사용할 때 Restore
- 기존 프로세스는 서로 다른 Process간 전환 시(또는 정보교환시) Context Switching Overhead가 발생
- Process 전환에 따른 PCB의 Load/Store 필요

<br>

### 스레드 구현

<img width="743" alt="image" src="https://github.com/user-attachments/assets/85dab793-a45d-4096-a1ba-226578a6020e" />

- 경량화된 프로세스라고 할 수 있음
- 하나의 프로세스는 여러개의 스레드를 갖음
- 구현 방법
  - 커널 레벨
  - 유저 레벨
  - 혼합 방식
- 자원 사용을 최소화하기 위해 Thread 사용
- 하나의 Process 내에서 여러 Thread 동작
- 한 Process내 Thread 들은 같은 자원을 공유

<br>

## 멀티스레드 서버

<img width="702" alt="image" src="https://github.com/user-attachments/assets/284b5e8f-e536-43f8-97da-1be67b47a5aa" />

- 요청이 많은 Busy Server에 좋음
- 디스패처 스레드에서 클라이언트 요청을 받음
- 디스패처 스레드에서 다른 스레드에도 일을 분배

<br>

<img width="763" alt="image" src="https://github.com/user-attachments/assets/a414ee79-806d-4ecf-8c68-adf8eddf5123" />

- Thread를 이용한 클라이언트/서버 구성
- Client 요청을 Multithread로 처리하면 WAN 과 같은 RTT가 긴 환경에서 이점을 가짐
  - 자원 관리시간 또는 대기시간이 긴 경우
- Server에서는 매 서비스 요청마다 Dispatcher가 개별 Worker Thread에게 작업을 배분
- 가장 큰 장점은 자원 효율이 좋음(자원 공유)
- 스레드 시스템은 블락되는 형태

<br>

## 가상화(Virtualization)

<img width="767" alt="image" src="https://github.com/user-attachments/assets/87550ffd-928c-4739-bea7-9ffaa9550546" />

<br>

- 기존 SW 개발 시 Platform에 종속됨 (다른 플랫폼으로 개발이되면 동작이 안되는 경우가 발생-따라서 가상화 필요)
- 개발환경과 다른 Platform이라도 기존 SW가 동작하도록 함
  - Platform과 SW계층 사이에 Interface(Virtual Machine)를 두어 다른 Platform이라도 기존 SW 동작
- Platform에 독립적인 시스템 구현 가능

<br>

### 가상화 머신 아키텍처

<img width="754" alt="image" src="https://github.com/user-attachments/assets/77e29679-3532-4787-8483-fd308075f09d" />

- Interface : General Instruction, Privileged, Instruction, System Call, Library, Function(API)

<br>

<img width="463" alt="image" src="https://github.com/user-attachments/assets/ef551156-b1f9-493d-b9b2-4e3adb6a2f6f" />

- 프로세스 가상 머신으로, (응용 프로그램, 런타임) 조합의 여러 인스턴스가 있음
- API레벨에서 가상화 OS는 하나이며 APPLICATION환경에 맞게 런타임을 이용화하여 가상화

<br>

<img width="476" alt="image" src="https://github.com/user-attachments/assets/398dcc44-4f8f-4f74-b38f-64ede4b2eace" />

- 여러 인스턴스가있는 가상 컴퓨터 모니터 (응용 프로그램, +-운영 체제) 조합
- 다양한 OS와 그에 맞는 APPLICATION 가상화 가능

<br>

## 클라이언트 서버 구성

<img width="609" alt="image" src="https://github.com/user-attachments/assets/02be08be-bd19-498c-bc66-cada5eb48ff0" />

<img width="650" alt="image" src="https://github.com/user-attachments/assets/f85044b3-93cb-49bc-bc8e-f60c2a5e33ec" />

- 특정 어플리케이션에 한정적
- 어플리케이션에 종속되지 않음

<br>

### 클라이언트 서버 바인딩 방식

<img width="739" alt="image" src="https://github.com/user-attachments/assets/7f4fe0bc-2ce0-4f7a-91e1-df16261f09d2" />

- 모든 서비스 서버가 동작
- Background에서 Daemon이 구동
- Daemon은 Endpoint(주소개념) Table을 가지고 있음 (demon은 주소록 개념)
- 클라이언트에서 서비스 요청 시 Daemon이 해당 서비스에 맞는 Endpoint를 연결

<br>

### 슈퍼 서버 방식

<img width="750" alt="image" src="https://github.com/user-attachments/assets/1fdaf0e0-d045-47e3-8531-0a81bf150762" />

- 서비스 요청에 대응하는 Super-Server 존재
- Super-Server는 요청에 따라 실제 서버를 Binding (Ex. 메일을 쓰고싶을 경우 그 때만 메일서버를 바인딩)
- 요청에 따라 필요한 Server만 동작하기때문에 자원소모가 절약 (기존 Demon방식은 서버가 모두 동작하여 살아있어야함. 자원소모가 큼)

<br>

### Server Clusters

- 동일 서비스를 제공하는 여러 개의 서버를 사용
- 개별 서버보다 Computing 능력이 Powerful함
- 클라이언트의 요청을 각 서버로 배분할 수있는 Switch 필요

<br>

<img width="799" alt="image" src="https://github.com/user-attachments/assets/3f06ece6-a7dd-4e07-b5e8-c49eb4896fb0" />

1. Dispatcher, 요청을 서버로 분배
2. Server Cluster, 서비스 서버
3. DB Cluster, 데이터 서버

<br>

<img width="669" alt="image" src="https://github.com/user-attachments/assets/fe39316f-015a-4d59-b283-956cf6742f72" />

- TCP Handoff 기술
- 서비스 요청 시 Switch가 부하가 적은 서버로 Binding
- TCP Network 사용
- 응답은 단일 Connection(클라이언트로 직접 전송)
- 구성이 복잡함

<br>

### 분산 서버(Distributed Servers)

- 서버 Clustering 시 Switch가 Fail(고장/실패)일 때 전체 시스템이 Down 되는 것을 서버를 분산시켜 보완
  - Ex) 분산서버- 스위치 개념이 없음
  - 서버1이 죽으면 서버2로 이동
- 각 서버의 실제 IP 주소는 다르지만 공통 Home Address(HA)를 갖는 서버 Cluster를 구성
- Application Level에서는 서비스를 HA로 요청하지만, MIPv6 Level에서 실제 서비스 서버 IP로 Binding
- MIP(MobileIP)는 HA와 CoA(Core of Address, 실제주소) 정보를 가짐

<br>

<img width="691" alt="image" src="https://github.com/user-attachments/assets/5dc11a21-fc67-4d89-9a6b-b318a3093705" />


<br>

### 서버 클러스터 관리

<img width="666" alt="image" src="https://github.com/user-attachments/assets/9960a82a-06e5-405e-abfe-3d542a6b9958" />

- 가상화를 이용 Server Cluster를 관리
- 원래 O/S에 VMMonitor를 추가하여 필요한 환경 제공

<br>
<br>
<br>

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

