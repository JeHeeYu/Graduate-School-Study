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
<summary><h2>6. DH(Diffie-Hellman) 키 교환의 원리</h2></summary>

### 기본 개념
- 비대칭키 암호 방식 (공개키 암호)
  - 암호화 : 공개키 사용
  - 복호화 : 개인키 사용
  - 대칭키보다 느리지만, 키 공유 문제 해결
- 보안 기반 : 소인수 분해의 어려움 (큰 수 N을 두 소수 p, q로 나누는 것이 매우 어려움)

<br>

### RSA 키 생성 과정
1. 소수 p, q 선택
- 서로 다른 두 큰 소수 선택 (예 : p=61, q=53)

2. N 계산
- N = p × q (예 : N = 61×53 = 3233)

3. φ(N) 계산 (Euler의 토션 함수)
- φ(N) = (p−1)(q−1) (예 : φ(N) = 60×52 = 3120)

4. 공개 지수 E 선택
- 1 < E < φ(N) 이고, E와 φ(N)이 서로소 (예 : E = 17)

5. 개인 지수 D 계산
- D × E mod φ(N) = 1 (곱셈 역원) (예 : D = 2753)

6. 키 구성
- 공개키: (N, E)
- 개인키: (N, D)

<br>

### RSA 암호화/복호화 공식
- 암호화 : C = P^E mod N
- 복호화 : P = C^D mod N
- P : 평문, C : 암호문, E : 공개 지수, D : 개인 지수, N : 모듈러

<br>

### RSA의 특징 및 장단점

- 장점
  - 키 배송 문제 해결 (비대칭 구조)
  - 디지털 서명 가능
- 단점
  - 연산 느림 (대칭보다 느림)
  - 동일 평문은 동일 암호문 → 패딩 필요 (OAEP)

<br>

### RSA 보안 관련 핵심 개념
- Trapdoor 함수 : E는 공개되어도 D는 구하기 어려움
- 중간자 공격(MITM) 방지 필요 → 공개키 인증서 필요
- 패딩 (RSA-OAEP) : 입력에 난수 추가 → 동일 평문 다른 암호문

</details>
  
<br>



<details>
<summary><h2>9~10. 블록 암호 운영모드 개념 정도</h2></summary>

### 1. ECB (Electronic Code Book)
- 원리 : 각 블록을 독립적으로 암호화
- 암호화식 : Ci = E(K, Pi)
- 장점 : 병렬 처리 가능, 구현 쉬움
- 단점 : 동일 평문 → 동일 암호문 → 패턴 유출, 보안 매우 취약
- 예시 : 이미지 파일을 암호화해도 윤곽이 보임
- **안전하지 않음 절대 사용 불가**

<br>

### 2. CBC (Cipher Block Chaining)
- 원리 : 이전 암호문과 XOR 후 암호화
- 암호화식 : Ci = E(K, Pi ⊕ Ci-1) (C0 = IV)
- 복호화식 : Pi = D(K, Ci) ⊕ Ci-1
- 초기값(IV) : 반드시 필요. 동일해야 복호화 가능
- 장점 : 동일한 평문이라도 다른 암호문 생성 (패턴 깨짐)
- 단점 : 암호화 시 병렬처리 X (복호화는 가능), 에러 전파 있음
- TLS/SSL 등 실제 시스템에서 사용됨

<br>

### 3. CFB (Cipher Feedback)
- 원리 : 암호문을 피드백하여 다음 블록 암호화
- 암호화식 : Ci = Pi ⊕ E(K, Ci-1)
- 복호화식 : Pi = Ci ⊕ E(K, Ci-1)
- 장점 : 패딩 불필요, 스트림 암호처럼 작동
- 단점 : 병렬처리 X, 재전송 공격에 취약
- 현재는 거의 사용 X

<br>

### 4. OFB (Output Feedback)
- 원리 : 암호 알고리즘의 출력을 다시 입력 → 키스트림 생성
- 암호화식 : Ci = Pi ⊕ Oi (Oi = E(K, Oi-1))
- 복호화식 : 동일 (대칭)
- 장점 : 암호화·복호화 구조 동일, 오류 전파 없음
- 단점 : 암호문 블록이 반복되면 키스트림도 반복됨 (보안 취약)
- 실사용 비권장

<br>

### 5. CTR (Counter Mode)
- 원리: nonce + counter → 암호화 → 키스트림 생성
- 암호화/복호화: Ci = Pi ⊕ E(K, counter_i)
- 장점
  - 병렬 처리 가능 (암·복호화 모두)
  - 스트림 암호처럼 빠르고 효율적
  - 패딩 불필요
  - 키스트림 미리 생성 가능
- 최신 암호화 표준에서 가장 권장되는 방식 (TLS 1.3 포함)



</details>
  
<br>


<details>
<summary><h2>11. 타원곡선 암호</h2></summary>

### 기본 개념
- 타원곡선 암호(ECC)는 이산대수 문제(DLP)의 변형인 타원곡선 이산대수 문제(ECDLP)를 기반으로 한 공개키 암호
- CC는 RSA보다 작은 키 길이로도 동등한 보안성을 제공함

<br>

### 핵심 개념 : ECDLP
- Q = k × G일 때, G와 Q가 주어져도 k를 구하는 건 매우 어려움
- 이게 Elliptic Curve Discrete Logarithm Problem (ECDLP)

<br>

### ECC 암호 방식의 주요 활용

|구분|내용|
|:---:|:---|
|ECDH|타원곡선 기반의 키 교환 (Diffie-Hellman 응용)|
|ECDSA|디지털 서명 방식|
|ECIES|타원곡선 통합 암호화 방식 (평문 암호화 지원)|

<br>

### ECC 장점
- 짧은 키 길이로도 강한 보안 제공
  - 예 : RSA 2048bit ≈ ECC 224bit
- 연산 속도 빠름, 자원 절약 (모바일/IoT에 적합)
- 전자서명, 세션 키 생성, 블록체인에 많이 활용

</details>
  
<br>

<details>
<summary><h2>12, 16. 해시함수의 특징</h2></summary>

### 해시 함수(Hash Function)의 특징

#### 1. 일방향성 (One-Way Property)
- 입력값(M)으로부터 해시값 h(M)을 쉽게 계산할 수 있지만,
- 해시값 h(M)으로부터 입력값 M을 역으로 구하는 것은 불가능에 가까움 (Preimage resistance).

<br>

#### 2. 고정 길이 출력
- 입력값 크기(1비트 ~ 1GB)는 상관없이, 출력은 항상 고정된 길이 (예: SHA-256 → 256비트).
- 이 고정된 해시값이 디지털 지문 역할.

<br>

#### 3. 빠른 계산 속도
- 어떤 크기의 메시지든지 해시값 계산이 빠름.
- 파일 무결성 검사, 디지털 서명 등 실시간 처리에 적합.

<br>

#### 4. 민감한 변화 반응 (Avalanche Effect)
- 입력값이 1비트만 바뀌어도 완전히 다른 해시값이 나옴.
- 변경 여부를 확실하게 감지할 수 있음.

<br>

#### 5. 충돌 저항성 (Collision Resistance)
- 약한 충돌 내성 (Weak Collision Resistance)
  - M과 h(M)을 알고 있을 때, h(M) = h(M′) 인 M′ ≠ M을 찾는 것이 어려움.
- 강한 충돌 내성 (Strong Collision Resistance)
  - 서로 다른 두 입력 M, M′에 대해 h(M) = h(M′)이 되도록 하는 것이 매우 어려움.


<br>

#### 6. 무결성 검증에 사용
- 데이터를 직접 비교하지 않고도 해시값으로 파일이나 메시지의 위변조 여부를 판별 가능.
- 예: 다운로드 파일, 암호화된 패스워드 확인 등.


<br>

#### 7. 비밀번호 저장에 사용
- 비밀번호를 해시값으로 변환해서 저장 → 원래 비밀번호는 저장하지 않음.
- 해킹 당해도 역으로 해시 풀기 어려움.

<br>

#### 8. 전자서명, 인증서, 블록체인 등 활용
- 메시지 인증코드(MAC), 디지털 서명, TLS, 블록체인 거래 해시 등 보안 분야 핵심 기술.

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



<details>
<summary><h2>15. 비트코인의 주소 생성 과정</h2></summary>

### 1. 개인키 (Private Key) 생성
- 256비트 난수(random number) 생성
  - 예 : k = 0xAB23... (64자리 16진수, 256비트)
- 완전 랜덤, 소유자의 비밀키 → 지갑에서 생성됨

<br>

### 2. 2단계: 공개키 (Public Key) 생성
- 개인키 k를 바탕으로 타원곡선 곱셈 (ECC)
  - Q = k * G
      - G: 기준점 (Elliptic Curve secp256k1 상의 고정점)
      - Q = (x, y): 512비트의 공개키 (x,y 좌표 256비트씩)

- 공개키만 알면 개인키를 알 수 없다 (일방향)

<br>

### 3. 공개키 → 해시 (Public Key Hash)

- 공개키를 압축된 주소로 만들기 위해 해싱
  - 1차 해시: SHA256(PubKey) → 256bit
  - 2차 해시: RIPEMD160(SHA256(PubKey)) → 160bit
  - 이 결과를 PubKeyHash라고 함

<br>

### 4. 4단계: 주소 포맷 변환 (Base58Check 인코딩)
- Base58Check 인코딩:
  - Prefix (네트워크 타입):
    - 메인넷: 0x00 (즉, 시작문자 1)
  - Payload: PubKeyHash
  - Checksum: SHA256(SHA256(Prefix + Payload))의 앞 4바이트
- 위 모두를 합쳐 Base58 인코딩 → 사람 읽기 가능한 주소 생성
  - 예: 1Cdid9KFAaatwczBwBttQcwXYCpvK8h7FK
 

</details>
  
<br>
