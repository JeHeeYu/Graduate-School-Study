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

<pre>
char a;
char b;
char c;
char d;
</pre>
<pre>
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

<pre>
높은 주소
[ Return Address ]
[ SFP ]
[ 지역 변수 / buffer ]
낮은 주소
</pre>

<br>
<br>

## 메모리

메모리 맵

<img width="1417" height="379" alt="image" src="https://github.com/user-attachments/assets/f5536c72-be96-4456-9a99-ec8d72c43527" />

<br>

메모리 변조 : 버퍼 사이즈를 넘어선 입력으로 옆에 있는 메모리 값이 바뀌는 것
<br>
- 스택 : 위 -> 아래 방향
- 힙 : 아래 -> 위 방향

<br>
<br>

리틀 엔디안은 정수는 메모리에 반대 방향으로 저장됨

<br>
<br>

## 스택

<img width="1421" height="354" alt="image" src="https://github.com/user-attachments/assets/a8aacfc2-e0d9-4752-b1d4-1fdf070f5d1f" />

가장 먼저 처리해야 될 것을 가장 가까운 곳에 저장함
<br>
높은 주소 -> 낮은 주소 방향으로 자람

<br>

Stack Frame 구성
- 지역 변수 (buffer 포함)
- SFP (Stack Frame Pointer)
- Return Address

<br>

<pre>
  높은 주소
[ Return Address ]
[ SFP ]
[ 지역 변수 / buffer ]
낮은 주소
</pre>

