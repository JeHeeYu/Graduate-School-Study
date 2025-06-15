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


