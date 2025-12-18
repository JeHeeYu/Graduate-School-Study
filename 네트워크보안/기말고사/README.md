
## xinetd

### 개념
- 기존 inetd룰 확장한 슈퍼 데몬
- 리눅스 커널 2.4 이후 사용
- 요청이 있을 때만 서비스 실행

### 설정
- 설정 디렉터리
<pre>
  /etc/xinetd.d/
</pre>
- 서비스별 설정 파일 생성 (ex telnet)

### 특징
- 자체 접근 제어 가능
- TCPWrapper와 연동 가능
- telnetd, ftp, ssh client 요청 관리


<br>

## Telnet (xinetd 기반 서비스)

### 특징
- 암호화되지 않은 평문 통신
- 보안에 취약
- 실습/교육용 으로 사용

### 설치

<pre>
  apt install xinetd
apt install telnetd
</pre>

### 설정

<pre>
  /etc/xinetd.d/telnet
</pre>

### 주요 옵션

|항목|의미|
|:---:|:---:|
|disable = no|서비스 활성화|
|flags = REUSE|기존 연결 재사용|
|socket_type = stream|TCP 스트림|
|wait = no|다중 클라이언트 처리|
|user = root|실행 사용자|
|server|telnetd 실행 파일|
|log_on_failure += USERID|실패 로그 기록|
|only_from|접속 허용 IP|
|access_times|접속 가능 시간|
|instance|동시 접속 수 제한|

<br>

## 로그

### 파일 위치

<pre>
  /var/log/auth.log
</pre>

### 확인 명령어

<pre>
  tail -f /var/log/auth.log
</pre>

### 로그 내용

- 로그인 성공 : session opned
- 로그인 실패 : authentication failure, FAILED LOGIN
- Telnet, SSH 공통

<br>

## SSH

### 개요

- SSL/TLS와 다른 독립적인 보안 프로토콜
- IETF 표준 (RFC 4251)
- 기능
  - 암호화
  - 인증
  - 무결성
  - 압축

### SSH SSL/TLS 비교

|구분|SSH|SSL/TLS|
|:---:|:---:|:---:|
|용도|원격 로그인, SCP, SFTP|HTTPS, 메일, VPN|
|인증|공개키/패스워드|X.509 인증서|
|구조|자체 프로토콜|TCP 위에서 동작|

<br>

## SSHD(OpenSSH Server)

### 설치

<pre>
  apt install openssh-server
</pre>

### 데몬 특징
- standalone daemon
- xinetd X
- 관리 명령
<pre>
  systemctl restart ssh
</pre>

### 설치 명령어

<pre>
  /etc/ssh/sshd_config
</pre>

### SSH 보안 설정

- 포트 변경

<pre>
  Port 1234
</pre>

- Root 로그인 제한

<pre>
  PermitRootLogin no
</pre>

- 세션 타임아웃

<pre>
// 600 x 3 = 1800초 후 연결 종료
  
  ClientAliveInterval 600
ClientAliveCountMax 3
</pre>

- 접속 제한

<pre>
  MaxAuthTries 3
MaxSessions 2
</pre>


### 인증 방식

- PasswordAuthentication yes/no
- PubkeyAuthentication yes

<br>

## TCP Wrapper

### 개념

- IP 기반 접근 제어
- xinetd + standalone 서비스(sshd 등) 제어 가능

### 설정 파일

<pre>
  /etc/hosts.allow
/etc/hosts.deny
</pre>

### 동작 순서
1. /etc/hosts.allow
2. /etc/hosts.deny
3. 둘 다 없으면 허용

### 설정 예시

<pre>
  # hosts.allow
sshd : 192.168.20.130

# hosts.deny
ALL : ALL
</pre>

<br>

# Sniffing 공격 정리 (교안 요약)

---

## 1. 법적 배경 (정보통신망법 제48조)

- 정보통신망 무단 침입 금지  
  → 5년 이하 징역 또는 5천만원 이하 벌금
- 악성 프로그램 전달·유포 금지  
  → 7년 이하 징역 또는 7천만원 이하 벌금
- 대량 트래픽으로 서비스 장애 유발 금지  
  → 5년 이하 징역 또는 5천만원 이하 벌금
- 보안·인증 절차 우회 프로그램 설치·유포 금지 (2024.1.23 신설)  
  → 5년 이하 징역 또는 5천만원 이하 벌금

---

## 2. Sniffing 공격 개요

- 네트워크 상의 패킷을 도청(Eavesdropping)하는 공격
- Passive Attack (수동적 공격)
- 동일한 네트워크(Subnetwork)에 있으면 정보 수집 가능
- 계정, 패스워드, 메신저 내용, 파일, 웹 활동 정보 획득 가능
- 기술 난이도가 낮지만 효과는 매우 큼

---

## 3. Promiscuous Mode

### 개념
- 정상 NIC: 자신의 MAC/IP 패킷만 처리
- Promiscuous Mode:
  - 모든 패킷을 수신 및 분석
  - 소프트웨어적으로 설정

### 설정
- 인터페이스 확인
  ifconfig

- Promiscuous Mode 설정
  ifconfig eth0 promisc

- 해제
  ifconfig eth0 -promisc

---

## 4. Sniffing 공격 도구

### TCP Dump
- 리눅스 기본 Sniffing 도구
- 네트워크 관리 목적에서 개발
- 수집한 증거는 법적 효력 인정 가능

설치
apt install tcpdump

실행 예
tcpdump -i eth0 -xX -w telnet.pcap

옵션 설명
- -i : 인터페이스 지정
- -xX : hex + ascii 출력
- -w : pcap 파일로 저장

---

## 5. Hub 환경에서의 Sniffing

- Hub는 모든 포트를 브로드캐스트
- 목적지 외 모든 PC가 프레임 수신
- 충돌 많고 보안 취약
- Sniffing 매우 용이

---

## 6. 실습 환경 구성

- Attacker : Kali Linux
- Client : WinClient
- Server : Ubuntu 24.04 (Telnet)
- VMware NAT (Vmnet8)
- Hub 환경 가정

---

## 7. Telnet Sniffing 실습 흐름

1. Ubuntu에 telnet 서버 설치
   apt install xinetd telnetd

2. xinetd telnet 설정
   /etc/xinetd.d/telnet

3. 방화벽 설정
   ufw allow 23

4. Kali에서 패킷 수집
   tcpdump -i eth0 host 192.168.20.110 -xX -w telnet.pcap

5. WinClient에서 telnet 접속
   telnet 192.168.20.110

6. Wireshark로 계정/비밀번호 확인 가능 (평문)

---

## 8. Wireshark Promiscuous Mode 설정

- Capture > Options
- Enable promiscuous mode on all interfaces 체크

---

## 9. Switching 환경에서의 Sniffing 대응책

### 1) SSL
- 웹 트래픽 암호화
- 128bit / 256bit 암호화

### 2) SSH
- Telnet 대체용 암호화 서비스
- OpenSSL Library 사용
- Telnet보다 훨씬 안전

### 3) VPN
- 암호화된 터널 제공
- 단, VPN 장비가 해킹되면 암호화 이전 데이터는 노출 가능

---

## 10. SSH Sniffing 결과

- Wireshark로 확인 시 데이터는 암호화됨
- Elliptic Curve Diffie-Hellman 키 교환 사용
- 계정/비밀번호 확인 불가

---

## 핵심 정리

- Sniffing은 Passive Attack
- Promiscuous Mode 필요
- Telnet은 평문 → Sniffing 취약
- SSH는 암호화 → Sniffing 안전
- Hub 환경은 보안 취약
- 대응책은 암호화(SSL, SSH, VPN)

<br>

# Switching 환경에서의 Sniffing 정리
ARP Spoofing / ARP Redirect / fragrouter

---

## 1. Switching 환경과 Sniffing

- Switch는 각 장비의 MAC 주소를 학습하여 포트에 할당
- 목적지 MAC이 아닌 패킷은 전달되지 않음
- 결과적으로 Sniffing이 어려운 환경
- Switch는 Sniffing 방지 목적 장비는 아니나 결과적으로 저지 효과 발생

---

## 2. ARP Redirect와 ARP Spoofing

### ARP Redirect
- 공격자가 자신을 **Router인 것처럼 속이는 공격**
- 2계층(Layer 2) 공격
- LAN 내부에서 수행
- 공격자는 실제 Router의 MAC 주소를 알고 있어야 함
- 수신한 패킷을 다시 원래 Router로 전달(relay)해야 통신 유지 가능

### ARP Spoofing
- Host to Host 공격
- ARP Redirect와 유사하지만 공격 대상 범위가 다름

차이 요약
- ARP Spoofing : Host ↔ Host
- ARP Redirect : Host ↔ Router (LAN 전체 영향)

---

## 3. 실습 6-4 : ARP Redirect 공격

### 실습 환경
- Attacker : Kali Linux 2024.4
- Target : WinClient2 (192.168.20.130)
- Router 역할 : Ubuntu 24.04 (192.168.20.110)
- 필요 도구 : arpspoof, fragrouter
- Switch 환경 가정

---

## 4. 공격 전 상태 확인

- 공격 대상 ARP 테이블 확인
```
arp -a
```
- 정상 상태에서는 Router IP에 정상 MAC 주소 등록

---

## 5. ARP Redirect 공격 수행 절차

### 1) 패킷 Relay 설정 (필수)
- 세션 유지를 위해 패킷을 다시 전달해야 함
```
apt install fragrouter
fragrouter -B1
```

### 2) ARP Redirect 공격 실행
```
arpspoof -i eth0 -t 192.168.20.130 192.168.20.110
```
- WinClient2에게 Ubuntu(Router)의 MAC 주소가 공격자(Kali)의 MAC이라고 속임

---

## 6. 공격 중 동작 확인

- fragrouter 실행 창에서 패킷이 Kali를 경유하는 것 확인
- WinClient2에서 테스트
  - ping 192.168.20.110
  - 웹 브라우저 접속 (Apache2 서버)

---

## 7. 공격 결과 확인

- WinClient2에서 ARP 테이블 재확인
```
arp -a
```
- Router IP에 공격자 MAC 주소가 등록됨
- arpspoof 중지 시 ARP 테이블 원상 복구

---

## 8. 핵심 정리

- Switching 환경에서는 기본적으로 Sniffing 불가
- ARP Redirect는 Switch 환경에서도 Sniffing 가능하게 함
- ARP Redirect는 공격자가 중간자(MITM)가 되는 공격
- fragrouter는 세션 유지를 위한 필수 도구
- ARP 공격은 Layer 2 공격

---

## 시험 포인트 요약

- Switch 환경 → Sniffing 어려움
- ARP Redirect → Router 사칭
- ARP Spoofing → Host 간 공격
- fragrouter 역할 → 패킷 relay
- ARP 공격 계층 → Layer 2

<br>

# ARP Spoofing / ARP Redirect / ICMP Redirect / IP Spoofing 정리

---

## 1. ARP Spoofing 과 ARP Redirect

### ARP Spoofing
- 위조된 ARP Reply를 전송하여 피해자의 ARP 테이블을 변조하는 공격
- 공격자는 자신을 게이트웨이처럼 속임
- ARP는 인증 절차가 없어 공격에 취약
- ARP Cache Poisoning이라고도 불림

목적
- 피해자의 트래픽을 공격자 PC로 유도

예시
arpspoof -i eth0 -t [공격대상IP] [GW IP]

---

### ARP Redirect
- ARP Spoofing 이후 트래픽을 다시 목적지로 전달하여 정상처럼 보이게 하는 공격
- 공격자가 중간자(MITM)가 됨
- IP Forwarding 또는 fragrouter 필요

특징
- 트래픽을 가로채고 다시 전달
- 완성형 MITM 공격

---

### ARP Spoofing vs ARP Redirect 비교

- ARP Spoofing
  - ARP Reply 위조
  - ARP 테이블 변조
  - 트래픽을 공격자로 유도
  - 단독으로는 불완전한 MITM

- ARP Redirect
  - Spoofing + 중계
  - 트래픽이 공격자를 경유
  - IP Forwarding 필요
  - 완성형 MITM

---

## 2. ARP Spoofing / Redirect 방어 대책

- 정적 ARP 테이블 설정
  arp -s IP MAC
- Dynamic ARP Inspection (DAI) 사용
- ARP 감시 도구 사용
  - arpwatch (Linux)
  - XArp (Windows)
- arptables로 비정상 ARP 패킷 차단

예시
sudo arptables -A INPUT --source-ip 192.168.20.10 ! --source-mac 00:0c:29:30:b7:a2 -j DROP

---

## 3. ICMP Redirect

### ICMP 개요
- IP의 오류 및 제어 메시지를 전달하는 프로토콜
- Type 5 : Redirect 메시지

### ICMP Redirect 목적
- 호스트의 라우팅 경로 변경
- 더 효율적인 경로 안내 목적

---

### ICMP Redirect 공격 원리
- 공격자가 자신을 라우터로 속여 ICMP Redirect 메시지 전송
- 피해자는 인증 없이 라우팅 테이블을 변경
- 특정 목적지 IP에 대해서만 경로 변경

특징
- ARP Redirect보다 범위가 좁음
- 특정 목적지에 대해서만 Redirect 수행

---

### 정상 ICMP Redirect 흐름
1. Host → RouterA
2. RouterA → ICMP Redirect (RouterB 추천)
3. Host → RouterB

---

### ICMP Redirect 공격 흐름
1. 공격자가 가짜 Redirect 메시지 전송
2. 피해자의 라우팅 테이블 변경
3. 트래픽이 공격자를 경유

---

### ICMP Redirect 설정 (Victim)

Windows
- 방화벽 비활성화
- 레지스트리 설정
  HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters
  EnableICMPRedirect = 1
- 재부팅 필요

Linux
cat /proc/sys/net/ipv4/conf/all/accept_redirects
sysctl -w net.ipv4.conf.all.accept_redirects=1

---

## 4. IP Spoofing

### 개념
- 출발지 IP 주소를 위조하여 패킷 전송

### 특징
- 응답을 받을 수 없음
- 비연결형 프로토콜에서만 사용
  - UDP
  - ICMP

### 활용
- DoS / DDoS 공격
- Land Attack

예시
hping3 -a 192.168.20.110 192.168.20.110 --icmp --flood

---

## 5. 공격 기법 비교 요약

- ARP Spoofing
  - Layer 2 공격
  - ARP 테이블 변조
  - 트래픽 유도

- ARP Redirect
  - Layer 2 공격
  - MITM 완성
  - 트래픽 중계

- ICMP Redirect
  - Layer 3 공격
  - Routing Table 변조
  - 특정 목적지 Redirect

- IP Spoofing
  - Layer 3 공격
  - 출발지 IP 위조
  - 응답 수신 불가

---

## 시험 핵심 요약

- ARP는 인증 없음 → Spoofing 취약
- ARP Redirect = Spoofing + Relay
- ICMP Redirect = Routing Table 공격
- IP Spoofing은 UDP/ICMP에서만 의미 있음
- MITM 완성형 공격은 ARP Redirect

