# 정보 보호 기말 고사 정리 내용

<details>
<summary><h2>6. DH(Diffie-Hellman) 키 교환의 원리</h2></summary>

### 교환 개념 요약
**두 당사자(앨리스, 밥)** 가 서로의 비밀키를 공개하지 않고도 공유된 비밀키(Session Key)를 안전하게 합의할 수 있도록 하는 방법
<br>
- 큰 소수 p (prime number)
- 원시근 g (primtive root mod p)
p, g 모두 공개값

<br>
<br>



### 수학적 원리

1. 앨리스와와 밥은 둘 다 p, g를 알고 있음
2. 앨리스는 비밀값 a를 생성하고 A = g^a mod p를 계산해 밥에게 전송 (이걸 half-key 또는 공개키 라고 부름)
3. 밥은 비밀값 b를 생성하고 B = g^b mod p를 계산해 앨리스에게 전송
4. 이제 둘 다 상대방의 공개키와 자신의 비밀값으로 공유키 계산
  - 앨리스 : K = B^a mod p = (g^b)^a = g^(ab) mod p
  - 밥 : K = A^b mod p = (g^a)^b = g^(ab) mod p
<br>
<br>

- 결론적으로 둘은 같은 K (공유키)를 가짐
- 공격자는 중간에 A와 B는 알 수 있지만 a 또는 b는 알 수 없음
- 이는 이산대수 문제(Discrete Logarithm Problem, DLP)가 어렵기 때문임

<br>
<br>

### 키 교환 예시
- p = 13, g = 2
- 앨리스 비밀키 A = 9 → RA = g^9 mod 13 = 2^9 mod 13 = 6
- 밥 비밀키 B = 7 → RB = g^7 mod 13 = 2^7 mod 13 = 11
- 공유 키 계산 방법
  - K = RB^A mod p = 11^9 mod 13 = 6
  - K = RA^B mod p = 6^7 mod 13 = 6
- 둘 다 K = 6이란 공유키를 갖음

<br>
<br>

### 안전한 이유
- 공격자는 g, p, RA, RB를 볼 수 있지만 a 또는 b는 알 수 없음
- 왜냐하면 g^x mod p = y일 때, x를 찾는 건 **DLP(이산대수 문제)** 로 매우 어렵기 때문

<br>
<br>

### ElGamal과의 관계
- ElGamal 암호 방식은 DH 키 교환을 기반으로 만들어 짐
- ElGamal은 여기에 메시지를 암호화하는 방식을 덧붙인 것 뿐

<br>
<br>

### 요약

| 항목 | 설명 |
|:----:|:-----|
|목적|	공개 채널에서 안전하게 공유 비밀키를 생성|
|기반|이산대수 문제 (Discrete Log Problem, DLP)|
|공개값|	큰 소수 p, 원시근 g|
|비공개값|앨리스의 비밀 a, 밥의 비밀 b|
|공유키 계산|g^ab mod p (서로 다른 방식으로 동일 결과 도출)|
|보안성|g^x mod p = y일 때, x를 유추하는 것이 어렵다는 수학적 성질|
|활용|ElGamal, SSL/TLS, IPSec, PGP, ECDH 등|



</details>
  
<br>


<details>
<summary><h2>13. 전자서명</h2></summary>

### 전자 서명이란

- 디지털 환경에서 사람의 서명 역할
- 수신자가 송신자를 신뢰할 수 있게 해줌
- 위조 방지 + 부인 방지 기능 포함

<br>
<br>

### 목적

|보안 속성|설명|
|:---:|:---|
|무결성|메시지가 중간에 바뀌지 않았다는 증거|
|인증|서명자가 누구인지 검증 가능|
|부인 방지|서명 후 부인 불가|

<br>
<br>

### MAC과 서명의 차이

|항목|MAC|전자 서명|
|:---:|:---:|:---:|
|키|공유 키 사용|비대칭 키 사용|
|부인방지|불가|가능|
|제3자 검증|불가|가능|
|사용 방식|HMAC|RSA/DSA/ECDSA 등|

<br>
<br>

### 동작 원리

- 서명
  - 메시지 작성
  - 메시지 -> 해시 값 계산
  - 해시 값 -> 개인키로 암호화 (이게 전자 서명임)
- 검증
  - 메시지 -> 해시 값 계산
  - 전자 서명 -> 공개키로 복호화 -> 복호화된 해시 값 비교
  - 같으면 무결성 + 인증 + 부인 방지 성립

<br>
<br>

### 전자 서명 방법

|방법|설명|
|:---:|:---|
|메시지 직접 서명|전체 메시지를 개인키로 서명 -> 비효율적|
|해시 값 서명|메시지를 해시 후 해시값에 서명 -> 일반적, 효율적|

<br>
<br>

### RSA 전자 서명 예시

- 서명자 공개키 (E, N) / 개인키 (D, N)
- 메시지 M = 123
- 서명 = M^D mod N (ex. 123^29 mod 323 = 157)
- 검증 = 서명^E mod N = 157^5 mod 323 = 123
- 메시지와 동일하므로 검증 성공

</details>
  
<br>


<details>
<summary><h2>14. TLS : 마스터 secret 생성 과정</h2></summary>

### 순서

|단계|설명|
|:---:|:---|
|ClientHello|client_random 전송|
|ServerHello|server_random + 인증서 + 서버 공개키 전송|
|ClientKeyExchange|클라이언트 공개키 전송|
|양측 공유키 생성|ECDHE 방식으로 공유 Premaster Secret 생성|
|Master Secret 생성|	PRF(pre_master, "master secret", client_random + server_random)|
|Key Block 생성|	PRF(master_secret, "key expansion", server_random + client_random)|

<br>
<br>

### 구성 요소 설명

|요소|설명|
|:---:|:---|
|client_random|클라이언트가 생성한 32바이트 랜덤|
|server_random|서버가 생성한 32바이트 랜덤|
|Premaster Secret|ECDHE 또는 RSA로 교환한 임시 비밀|
|Master Secret|	PRF를 통해 생성 (총 48바이트)|
|Key Block|암호화 키, MAC 키, IV 등을 포함한 키 집합|

  


</details>
  
<br>
