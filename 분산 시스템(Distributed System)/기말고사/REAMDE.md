# 동기화


## Vector Clocks (인과관계)
- **메시지의 전후관계 뿐만 아니라 인과관계도 고려하여 시간 동기화**
- 표현: VCi=(Px, Py, Pz)
- 동작 
1) 어떤 Process가 다른 Process에게 메시지 를 보낼 경우 Vector 값 중 자신에게 해당 되는 부분을 1증가 시키고 메시지와 같이 전송 
2) 수신한 Process는 자신의 Vector와 비교하 여 일관성에 문제가 없을 경우 Vector 업 데이트 
3) 만약 수신한 메시지의 Vector 일관성이 없 다면 대기시키고, 일관성이 일치하는 다른 Vector를 가진 메시지를 Update 수행

<br>
<br>

## Mutual Exclusion (독점)

### Mutual Exclusion의 종류

1. Centralized - 코디네이터 / 큐 존재
- Coordinator는 각 Process의 자원사용을 중재
- Queue를 사용하여 사용순서 결정

<br>
<br>

2. Distributed - Peer 관계에서 자원공유
- Coordinator 없이 Peer-to-Peer 방식
- 자원사용을 경쟁할 때 각 Process의 Timestamp 값으로 결정

<br>
<br>

3. Token Ring
- Network를 순회하는 Token을 가진 Peer가 자원을 사용함
- 자원사용이 종료되면 Token을 해제하고 다음 Peer에게 넘김

<br>
<br>

### Election Algorithms
- Centralized 분산시스템 환경에서 Coordinator 역할을 수행할 Peer를 선정하는 방법
- 종류
  - The Bully Algorithm
  - A Ring Algorithm

1. The Bully Algorithm
- 각 Peer가 고유하게 갖는 timestmap 또는 priority 사용
- Coorninator는 가장 큰 priority를 가짐

<br>
<br>

2. A Ring Algorithm
- 각 Peer들은 Logical한 Ring에 연결되어 있다고 가정함
- Ring을 따라 순회하며 Coordinator 선정


<br>
<br>

## 일관성 및 복제

<br>
<br>

### 복제 이유 (신뢰성/성능)
- 성능을 위한 복제 (병렬 처리)
- 숫자의 크기 조정
- 지리적 영역의 확장 경고
- 실적 향상
- 복제 유지 비용 증가 대역폭

<br>
<br>

### Replication(복제) : 분산시스템에서 성능과 신뢰성 확보 방법
- 성능 : 병렬처리, 캐싱
- 신뢰성 : 자료 백업

<br>

### 복제의 문제점
- 데이터의 일관성이 깨질 수 있음
- 시스템 운영/관리 비용 증가

<br>
<br>

### Consistency Model(일관성 유지 모델) 

- Data-Centric(데이터 중심) 
- Client-Centric(클라이언트 중심) - MONOTONIC READ /

<br>
<br>

### Data-Centric Consistency Models

- 클라이언트에 상관없이 Data 일관성 유지

1. Continuous Consistency : 클라이언트에 상관없이 Data 일관성 유지
2. Sequential Consistency : Data를 읽고 쓰는데 순차적으로 일관성 유지
3. Causal Consistency : Update 의인과 관계가 성립 여부 검사
4. Grouping Operations : 공유자원의 배타적 사용에 관한 논의
5. Eventual Consistency : 어떤 이벤트 수행 후 마지막에 데이터의 일관성을 갖도록 함

<br>
<br>

### Eventual Consistency
- 어떤 Event의 수행 후 마지막에 데이터 일관 성을 갖도록 함
- Update가 가끔 일어나는 상황에 유용

#### Client Centeric MODEL
1. Monotonic reads
- Read 상황에서 한결같은 일관성 제공
- 동일 Input이 전파되어 Client 별로 Read 시 최종 결과에 반영되어야 함 (최종값을 읽어야 함)
2. Monotonic Writes
- Write 상황에서 한결같은 일관성 제공
- 동일 Input이 전파되어 다른 Client의 Write 최종 결과에 반영되어야 함 
3. Read Your Writes
- 특정 Client에 대해 Write 결과가 반영된 Read 수행
4. Write Follow Reads
- 특정 Client가 Read 한 이후 다른 Client가 Write 수행

<br>
<br>

### Content Replication and Placement 

#### Replica 생성 유형 
- Permanent   (처음부터 복사본수를 미리 정함)
  - 처음부터 일정 숫자를 복제
  - 시스템 최초 형성 시 관리자 가복제 수량 결정
- Server-Initiated (서버가 복사)
- Client-Initiated (클라이언트가 복사((예) 캐시를 사용하는 것))

<br>
<br>

#### Server-Initiated Replicas (파일마다 Access Count 저장)
- File-Access를 Count 하여 Threshold를 정함

<br>
<br>

### CDN(Content Distribution Network)
- Access 시간을 단축시키기 위해 주로 사용
- 여러 위치에 분산된 데이터를 요청할 때 실행 분산
- 접근 시간 단축을 위해 많이 사용

1. State보다 Operation
- 업데이트 메시지 전파
- 복제에서 다른 복제로 데이터 전송
- 다른 복제에서 업데이트 Operation 전파

<br>

2. pull vs protocols

#### Pull
- 서버에 Update Data를 요청하여 수신 (업데이트를 상대에게 알려주고 가져옴)
- 서버의 상태를 유지하지는 않음 
- Push보다 오래걸림 (Fetch Update 시간 필요)

#### Push
- 서버로부터 Update Data를 받음 
- 서버 상태, 복제 목록, Data 저장을 위한 Cache 필요 
- 즉시 실행 가능

<br>
<br>

#### Remote-Write Protocols (primary based)
- Primary Backup(Replica) 서버 존재
- Synchronous 방식  - Write 시간이 오래 걸리며, 그동안 Client는 Block 됨
- Read는 Client에서 가까운 Backup 서버에서 수행

<br>

### Local-Write Protocols
- Client로부터 Write를 요청받은 Backup이 Primary가 됨
- Remote-Write보다 Write 시 Client의 Block 시간이 짧음
- Read 절차는 Remote-Write와 동일

<br>
<br>

### Quorum-Based Protocols (Replicated Protocols) 
- 일정 정족수(과반수) 이상을 만족할 경우 Update 수행 
- 중복이 허용됨; Read Peer와 Write Peer가 중복될 수 있음

<br>
<br>

## 분산시스템의 결함 허용 (Fault Tolerance)

### Fault Tolerance : 분산시스템상 어느 정도 Fault가 발생해도 시스템이 Crash 되지 않음

<br>
<br>

### Fault Tolerance의 요소
- 가용성(Availability)
- 신뢰성(Reliability)
- 안정성(Safety)
- 유지보수성(Maintenance)

<br>
<br>

### Failure Models
1. crash
2. omission (생략)
3. timing
4. response 
5. arbitary (임의)

<br>
<br>

### failure 극복 방안 (Failure Masking by Redundancy )

- 어떤 node의 fail이 발생하면 vote에 연결된 Redundancy의 절반 이상이면 정상, 아니면 fail

<br>
<br>

### 분산시스템 내 합의의 고려사항 (Failure Masking by Redundancy)
