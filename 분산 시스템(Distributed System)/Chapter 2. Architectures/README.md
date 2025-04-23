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
