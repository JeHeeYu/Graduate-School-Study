
## ARM Design Philosophy (ARM 설계 철학)

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
