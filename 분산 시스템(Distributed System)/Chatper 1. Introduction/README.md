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
