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
