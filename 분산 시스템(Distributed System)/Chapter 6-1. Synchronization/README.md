# Chapter 6-1 동기화 정리 내용

<img width="781" alt="image" src="https://github.com/user-attachments/assets/248cd6aa-5c0a-4d05-9e55-9e4c33e7de05" />

- 서로 다른 프로그램 (컴파일러와 에디터)끼리의 로컬클록이 다르기에 동기가 필요함

<br>
<br>

#### 시간(Clock or Time)의 중요성

- 컴퓨터에서 일어난 이벤트 발생시간 파악
- Data Consistency 유지 등 각종 시스템 알고리즘 구현에 이벤트 시간이 중요

<br>

#### Clock Sync(시간 동기)의 필요성
- 각각의 컴퓨터마다 자체 Clock을 사용
- 자체 Clock 사용으로 인하여 분산시스템상에서 각 작업 간 Clock 및 시간 동기가 어긋나 문제 발생

<br>

#### Clock 종류
- Physical(물리적): 태양력, 원자시계(TAI)
- Logical(논리적)
  - Lamport : 시간의 선/후 파악
  - Vector Clock : Event의 인과관계 파악
 
<br>

## Physical Clocks

### 태양력

<img width="727" alt="image" src="https://github.com/user-attachments/assets/abcf8b24-7cb6-4c7b-8979-a2757d7bae9c" />

- 지구의 공전주기 활용
- 하루 중 태양이 가장 높이 뜬 시점부터 측정
- n번째 날짜까지 초 단위로 Count
- 영국 그리니치 천문대가 표준 측정지점
- 360도 == 1년
- 하루 == 태양이 가장 높이 떳을때 오늘과 내일의 시간차

<br>

### 원자시계

<img width="809" alt="image" src="https://github.com/user-attachments/assets/adcfc71d-f9b8-42d9-bc89-7e7adf1dff31" />

- 1948년 고안
- 세슘 원자의 진동수를 기준으로 시간 측정
- (초당 92억회 진동)0
- TAI; International Atomic Time의 역자
- 세계표준시(UTC, Universal Time Coordinate)
- TAI 방식으로 측정

<br>

### GPS

<img width="725" alt="image" src="https://github.com/user-attachments/assets/cdd7fd16-37e7-466c-ae83-281a01de89aa" />

<img width="716" alt="image" src="https://github.com/user-attachments/assets/bc5013bf-ef4d-4bfa-bd7c-8b9845fb4775" />

- GPS는 4개의 원자시계를 사용함
- 저궤도 인공위성을 사용하여 3각 측량법으로 위치측정, 이를 이용한 특정 Node와 GPS 간 시간 동기 수행
- GPS 시간 동기 Issue
  - 전파지연 : 위성에서 지구까지 전파가 도달하는 시간 보정 필요 
  - Clock차이 : 수신자와 위성의 Clock 차이보정
 
<br>

### NTP(Network Time Protocol)


<img width="644" alt="image" src="https://github.com/user-attachments/assets/2945cf4f-099a-4d72-a126-063ccc5bfda0" />

<br>

- PASSIVE 형식 : 물어볼때 알려줌
- 유선 인터넷에서 사용
- 원자시계 수준의 정밀한 시간을 유지하는 Time Server 존재 
  - Server의 동작은 Passive 방식

 <br> 


### The Berkeley Algorithms 

- 서버가 주기적으로 자기 시간을 알려주어 동기화
- 클라이언트가 서버에게 나는 얼마나 늦는지에 대한 값을 알려줌
- 3번 왔다 갔다 함
  - 먼저 몇시에 동기화 할지(ex 3시)
  - 왼쪽은 10분 느리다고, 오른쪽은 25분 빠르다고 클라이언트가 서버로 알림
  - 서버에서 계산해서 왼쪽은 +15분, 오른쪽은 -20분 하라고 알림

<br>

## 클럭 동기화 알고리즘(무선)

- 메시지 준비 → NIC(Network Interafce Card)가 전송 → 어플리케이션이 받아서 처리 → 마지막에 시간 계산
- 실제로 메시지 준비 부분부터 시간 계산을 하면 오래 걸리니 NIC가 전송 했을 떄부터 시간 계산
