
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
