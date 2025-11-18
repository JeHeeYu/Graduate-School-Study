
## ARM Design Philosophy (ARM 설계 철학) 

- Variable Cycle Instructions (가변 사이클 명령)
  - LDM/STM(Multiple Load/Store)
  - SWP(swap, atomic)
- Inline Baerrel Shifter (명령어 내장 시프트 기능)
  - 하나의 명령어 안에서 연산 + 시프트 동시 수행 가능
- Thumb (16-bit instruction set)
  - 코드 사이즈를 줄이기 위해 16비트 명령어 집합 제공
  - 성능은 다소 떨어지지만 메모리 절약 큰 장점
- Conditional Execution (조건부 실행)
  - 대부분의 명령어는 조건코드를 붙여 실행 할지말지 결정 가능
- Enhanced DSP Instructions (DSP 연산 강화 기능)
  - DSP 연산을 빠르게 하기 위한 추가된 명령어들을 말함
  - MAC(Multiple-Accumulate), Saturation 연산 포함
 
<br>
<br>

## ARM Core Data Flow Model
<br>

<img width="310" height="391" alt="image" src="https://github.com/user-attachments/assets/401b0a01-f636-4e33-bab1-e8d15bc5b5a1" />

<br>

- Fetch -> Decode -> Execute -> Memory -> Write-back
- Register file, ALU, Shifter, Bus Interface 등이 내부에서 연결됨
- Barrel shifter가 ALU 앞 단계에 붙어 있어, 대부분의 명령에서 shift+연산을 한 번에 수행 가능

포인트
- ARM의 빠른 연산 구조 = 파이프라인 + Barrel Shifter + Load/Store
- RISC 처럼 단순한데 빠른 이유임

<br>
<br>

## 예외 발생 시(When an Exception Occurs) ARM이 수행하는 전체 과정 

전체 흐름 요약
- 이전 상태 보존 (CPSR -> SPSR)
- ARM 상태 강제 전환
- 예외 전용 모드로 진입
- 인터럽트 비트 설정
- 복귀 주소 (LR_mode) 저장
- 백터 주소로 PC 점프

<br>
<br>

## 예외 복귀 (Return From Exception) 과정 
예외가 끝나고 원래 실행하던 코드로 돌아가는 과정을 말함
- CPSR을 SPSR_mode 에서 복원
- PC를 LR_mode 에서 복원

<br>

포인트
- 복귀 시 SPSR 사용

<br>
<br>

## 산술 명령어 예제 
<img width="321" height="203" alt="image" src="https://github.com/user-attachments/assets/5c8d4955-2918-431b-b622-6eb28813bdc9" />

- PRE(실행 전)
  - r0 = 0x00000000
  - r1 = 0x00000005
- 명령어 : Add r0, r1, r1, LSL #1
- POST(실행 후)
  - r0 = 0x0000000f
  - r1 = 0x00000005
 
<br>

ADD 명령어 형태
<pre>
ADD Rd, Rn, Rm {, shift}
</pre>
- Rd : 결과를 저장할 목적지 레지스터
- Rn : 첫 번째 피연산자
- Rm : 두 번째 피연산자
- shift : 두 번째 피연산자에 적용할 배럴 시프터(shift)

따라서 
<pre>
ADD r0, r1, r1, LSL #1
</pre>
은 수식으로 r0 = r1 + (r1 << 1) 이 됨
<br>
즉, 두 번째 r1은 바로 쓰는게 아니라 LSL #1 (왼쪽으로 1비트 시프트)를 먼저 한 다음에 더함
<br>
<br>

### LSL #1 의미
LSL # 1 == Logical Shift Left 1bit == 2배 곱하기
<br>
r1의 값은 0x00000005 (십진수 5) 이고 이걸 2진수로 하면
- r1 = 0000 0000 ... 0000 0101 (5, 1bit + 0bit + 1bit == 5)
여기서 왼쪽으로 1비트 시프트(LSL #1)하면
- r1 << 1 = 0000 0000 ... 0000 1010 (십진수 10, 0x0000000A)
즉, 명령어 내부는 r0 = 5 + 10

<br>
<br>

## 최종 결과 계산
- r1 = 5
- r1 << 1 = 10
- r0 = 5 + 10 = 15
- 결과 : 0x0000000f
<br>
POST 상태
- r0 = 0x0000000f
- r1 = 0x00000005 (소스 레제스터라 값이 바뀌지 않음)

<br>
<br>

## CMP 명령어 
<img width="350" height="207" alt="image" src="https://github.com/user-attachments/assets/530d48a9-3618-44e7-9b31-620fd5b91a50" />
<img width="720" height="540" alt="image" src="https://github.com/user-attachments/assets/d78083cb-6093-4935-840e-02200a1f86c4" />

<br>

CMP 정의
- 비교(Compare) 명령어
- 내부적으로 뺼셈(SUB)을 하고,
- 결과값은 어디에도 저장하지 않으며,
- CPSR의 플래그(N, Z, C, V)만 갱신

<br>

CPSR Condition Flags: bits[31:28]
|비트위치|플래그|설명|
|:---:|:---:|:---|
|bit[31]|N(Negative)|연산 결과가 마이너스일 경우 1|
|bit[30]|Z(Zero)|연산한 결과가 0인 경우 1|
|bit[29]|C(Carry)|연산 결과로 자리 올림이 생길 경우 1|
|bit[28]|V(oVerflow)|연산 결과가 오버플로우일 경우 1|

<br>

<pre>
  CMP r0, r1
</pre>

의 논리는
<br>
- r0 - r1 을 계산해서 결과는 버리고,
- 그 연산 결과에 따라 CPSR 의 플래그만 설정

<br>
<br>

### CPSR 문법

<pre>
CMP<cond> <Rn>, #<rotated_immed>
CMP<cond> <Rn>, <Rm> {, <shift>}
; 예) CMP r0, r1   → r0 - r1
</pre>
- Rn : 첫 번째 피연산자
- Rm : 두 번째 피연산자 (또는 즉값)
- {, shift} : 두 번째 피연산자에서 배럴 시프터 적용 가능

<br>

즉, 일반적인 데이터 처리 명령어 형식 (OP Rd, Rn, Operand2 에서)
- Rd가 없고
- 연산은 Rn - Operand2
- 결과는 버린 후 플래그만 설정

<br>
<br>

### CMP 예제 설명

PRE(실행 전)
- cpsr = nzcvqift_USER (플래그 상태 중요 X, 형식만 표시)
- r0 = 4
- r9 = 4

<br>

명령어
<pre>
  CMP r0, r9
</pre>

이 명령어가 하는 계산 : r0 - r9 == 4 - 4 = 0 연산을 수행
- 연산 결과가 0임
  - 결과가 0이면 Z 플래그 = 1 (Set)
  - 결과가 양수/음수에 따라 N 플래그
  - borrow/overflow 여부에 따라 C, V 플래그 결정
- 즉 결과는 다음과 같음
  - 4 - 4 = 0 -> 음수 아님 -> N = 0
  - borrow 없음 -> C = 1
  - overflow 없음 -> V = 0
  - 결과 0 -> Z = 1

<br>

POST(실행 후)
- cpsr의 Z 비트가 1로 세팅
- r0, r9의 값은 절대 변하지 않음
- r0 = 4 (그대로)
- r9 = 4 (그대로)
- CPSR만 바뀌어서 Z = 1이 됨

<br>
<br>

## 단일 레지스터 전송 주소 지정 방식

주소 지정 방식 3가지 정리

|방식|Effective Address|Base Register(r1 등)|예제 형태|
|:---:|:---:|:---:|:---:|
|Preindex|base + offset|base (변화 없음)|LDR r0, [r1, #4]|
|Preindex with writeback|base + offset|base ← base + offset|LDR r0, [r1, #4]!|
|Postindex|base|base ← base + offset|LDR r0, [r1], #4|

<pre>
Preindex :                    주소 = r1 + 4,   r1 그대로
Preindex with writeback :     주소 = r1 + 4,   r1 = r1 + 4
Postindex :                   주소 = r1,       r1 = r1 + 4
</pre>

<br>
<br>

<img width="591" height="312" alt="image" src="https://github.com/user-attachments/assets/e1da0d93-89a6-461a-a93d-623a1e734c6c" />

<br>

### 예제 1 — Preindex

<pre>
  명령어
  LDR r0, [r1, #4]

  1단계 : 주소 계산
  preindex는 먼저 offset을 더함
  addr = r1 + 4 = 0x9000 + 0x4 = 0x9004

  2단계 : 메모리 로드
  r0 = mem32[0x9004] = 0x02020202

  3단계: r1 변화 없음
  preindex는 writeback이 없기 때문에 r1 그대로.

  실행 결과
  r0 = 0x02020202
  r1 = 0x00009000
</pre>

<br>

### 예제 2 — Preindex with writeback

<pre>
  명령어
  LDR r0, [r1, #4]!

  1단계 : 주소 계산
  preindex니까 똑같이 먼저 더함
  addr = r1 + 4 = 0x9004

  2단계 : 메모리 로드
  r0 = mem32[0x9004] = 0x02020202

  3단계: r1 업데이트 (writeback)
  r1 = r1 + 4 = 0x9004

  실행 결과
  r0 = 0x02020202
  r1 = 0x00009004
</pre>

<br>

예제 3 — Postindex
<pre>
  명령어
  LDR r0, [r1], #4

  1단계 : 메모리 접근 먼저 (r1 그대로 사용)
  addr = r1 = 0x9000
  r0 = mem32[0x9000] = 0x01010101

  2단계 : r1 업데이트 (나중에 offset 적용)
  r1 = r1 + 4 = 0x9004

  실행 결과
  r0 = 0x01010101
  r1 = 0x00009004
</pre>

<br>
<br>

## 조건부 실행 : 최대 공약수 예제 

<pre>
  gcd
    CMP     r1, r2        ; r1 - r2 결과로 플래그 설정
    BEQ     complete      ; r1 == r2 이면 종료로 이동
    BLT     lessthan      ; r1 < r2 이면 lessthan 레이블로 이동
    SUB     r1, r1, r2    ; r1 > r2 인 경우 r1 = r1 - r2
    B       gcd           ; 다시 반복

lessthan
    SUB     r2, r2, r1    ; r2 > r1 인 경우 r2 = r2 - r1
    B       gcd           ; 다시 반복

complete
    ...                   ; GCD 계산 완료
</pre>

### 레이블(Label) 의미 요약표

| 레이블 이름 | 원뜻(약자) | 의미 | 언제 실행되는가 |
|-------------|------------|-------|------------------|
| **gcd** | Greatest Common Divisor | GCD 메인 루프 시작 위치 | 매 반복마다 `B gcd` 로 돌아올 때 |
| **lessthan** | Less Than | r1 < r2 인 경우 r2 = r2 - r1 수행 | `BLT lessthan` 조건 만족 시 |
| **complete** | Complete | 최대공약수 계산 완료 지점 | `BEQ complete` 조건 만족 시 |

---

### 조건 분기 명령어 요약

| 명령어 | 의미(약자) | 동작 조건 | 사용 플래그 |
|--------|------------|-----------|--------------|
| **CMP r1, r2** | Compare | r1 - r2 수행 후 플래그 설정 | N, Z |
| **BEQ label** | Branch if Equal | r1 == r2 → Z=1 일 때 분기 | Z |
| **BLT label** | Branch if Less Than | r1 < r2 → N=1 일 때 분기 | N |
| **B label** | Branch | 무조건 분기 | 없음 |

### 상세 설명

CMP r1, r2
- 실제 뺄셈을 수행하진 않음
- 내부적으로 r1 - r2 계산 후 플래그(N, Z, C, V) 를 설정함
  - Z = 1 → r1 == r2
  - N = 1 → r1 < r2
  - N = 0 → r1 > r2
 
<br>

BEQ complete
- Z 플래그가 1이면(branch if equal) 이동
- 즉, r1 == r2 → GCD 완료

<br>

BLT lessthan
- N 플래그가 1이면(branch if less than) 이동
- 즉, r1 < r2 → 작은 쪽(r1)을 빼는 게 아니라 큰 쪽(r2)에서 작은 쪽(r1)을 빼기 위해 lessthan으로

<br>

SUB r1, r1, r2
- r1 > r2 인 경우만 도달함
- r1 = r1 - r2

<br>

B gcd
- 레이블 gcd 로 다시 점프
- 반복 진행

<br>

lessthan 레이블
<pre>
  SUB r2, r2, r1   ; r2 = r2 - r1
  B   gcd
</pre>
- 이 부분은 r1 < r2 인 경우
- 큰 값(r2)에서 작은 값(r1)을 뺀 뒤 다시 반복

<br>

complete
- r1 과 r2 가 동일해진 순간 → 그 값이 GCD
- 여기서 함수 종료 또는 결과 반환 로직 수행

<br>
<br>

## 조건부 코드 설명

|코드|약자|설명|
|:---:|:---:|:---|
|ADDEQ|EQ (Equal)|r0 == r1 일 때만 실행|
|ADDNE|NE (Not Equal)|r0 != r1 일 때만 실행|
|ADDGT|GT (Greater Than)|r0 > r1 일 때만 실행|
|ADDLT|LT (Less Than)|r0 < r1 일 때만 실행|
|ADDGE|GE (Greater or Equal)|r0 ≥ r1 일 때만 실행|
|ADDLE|LE (Less or Equal)|r0 ≤ r1 일 때만 실행|
