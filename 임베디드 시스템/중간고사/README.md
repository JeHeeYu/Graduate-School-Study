
## ARM Design Philosophy (ARM 설계 철학) - 시험 문제

- Variable Cycle Instructions (가변 사이클 명령)
  - LDM/STM(Multiple Load/Store)
  - SWP(swap, atomic)
- Inline Baerrel Shifter
  -  하나의 명령어 안에서 연산 + 시프트 동시 수행 가능
-  Thumb (16-bit instruction set)
  - 코드 사이즈를 줄이기 위해 16비트 명령어 집합 제공
  - 성능은 다소 떨어지지만 메모리 절약 큰 장점
- Conditional Execution (조건부 실행)
  - 대부분의 명령어는 조건코드를 붙여 실행 할지말지 결정 가능
- Enhanced DSP Instructions
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

## 예외 발생 시(When an Exception Occurs) ARM이 수행하는 전체 과정 - 시험 문제

전체 흐름 요약
- 이전 상태 보존 (CPSR -> SPSR)
- ARM 상태 강제 전환
- 예외 전용 모드로 진입
- 인터럽트 비트 설정
- 복귀 주소 (LR_<mode>) 저장
- 백터 주소로 PC 점프

<br>
<br>

## 예외 복귀 (Return From Exception) 과정 - 시험 문제
예외가 끝나고 원래 실행하던 코드로 돌아가는 과정을 말함
- CPSR을 SPSR_<mode> 에서 복원
- PC를 LR_<mode> 에서 복원

<br>

포인트
- 복귀 시 SPSR 사용

<br>
<br>

## 산술 명령어 예제 - 시험 문제
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
Rd : 결과를 저장할 목적지 레지스터
Rn : 첫 번째 피연산자
Rm : 두 번째 피연산자
shift : 두 번째 피연산자에 적용할 배럴 시프터(shift)

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
- r0 = 5 + 10 + 15
<br>
POST 상태
- r0 = 0x0000000f
- r1 = 0x00000005 (소스 레제스터라 값이 바뀌지 않음)
