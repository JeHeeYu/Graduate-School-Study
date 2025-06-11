# 동기화


## Vector Clocks (인과관계)
- **메시지의 전후관계 뿐만 아니라 인과관계도 고려하여 시간 동기화**
- 표현: VCi=(Px, Py, Pz)
- 동작 
1) 어떤 Process가 다른 Process에게 메시지 를 보낼 경우 Vector 값 중 자신에게 해당 되는 부분을 1증가 시키고 메시지와 같이 전송 
2) 수신한 Process는 자신의 Vector와 비교하 여 일관성에 문제가 없을 경우 Vector 업 데이트 
3) 만약 수신한 메시지의 Vector 일관성이 없 다면 대기시키고, 일관성이 일치하는 다른 Vector를 가진 메시지를 Update 수행

<br>
<br>

## Mutual Exclusion (독점)

### Mutual Exclusion의 종류

1. Centralized - 코디네이터 / 큐 존재
- Coordinator는 각 Process의 자원사용을 중재
- Queue를 사용하여 사용순서 결정

2. Distributed - Peer 관계에서 자원공유
- 
