## Buffer Overflow - 보안 기법

### NX(No-eXecute bit)
- Stack, Heap 과 같은 Data 영역에서 execute 권한을 부여하지 않음
- Shellcode를 넣고 실행 금지 (Segmentation fault, cored dump)

<br>

## ASLR(Adress Space Layout Randomization)
- 메모리 영역의 주소를 랜덤화, BOF 에서의 특정주소 공격 방지

<br>

## Stack Canary
- Return 직전 값이 바뀌었으면 attack 이라고 판단하고 강제 종료
- Stack smashing detect, terminated

<br>
<br>

## 버퍼

### 버퍼란
- 임시로 데이터를 저장하기 위한 메모리 공간
- 버퍼는 연속된 공간을 의미함 (배열)

<br>
<br>

### 버퍼 3가지 요소
- 값(Value) : 실제 저장된 데이터
- 크기(Size) : 버퍼가 차지하는 메모리 크기
- 주소(Addres) : 메모리에서의 위치

<br>

### 메모리 배치
- 낮은 주소 -> 높은 주소 방향으로 쌓임
- 먼저 선언된 변수일 수록 낮은 주소

<pre
  char a;
  char b;
  char c;
  char d;
높은 주소
[d]
[c]
[b]
[a]
낮은 주소
  </pre>


<br>

### 함수 내부 버퍼
- 함수 안에 선언된 버퍼는 스택에 위치함
- Stack Frame 구조
  - 지역 변수 (버퍼 포함)
  - SFP(Stack Frame Pointer)
  - Return Adress
 
- 
