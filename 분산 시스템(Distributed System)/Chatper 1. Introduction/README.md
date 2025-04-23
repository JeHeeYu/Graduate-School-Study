# 1강 Introduction 정리 내용


## 분산 시스템의 투명성(Transparency in a Distributed System)

- 분산 시스템에선 투명성이 중요함
- 투명성은 모두 보이지 않는(Hide) 것을 뜻 함

|타입|설명|
|---|---|
|접근(Access)|데이터의 표현 방식과 자원 접근 방식을 숨김|
|위치(Location)|자원이 어디에 위치하는지 숨김|
|이동(Migration)|자원이 다른 위치로 이동할 수 있음을 숨김|
|재배치(Relocation)|자원이 사용 중 다른 위치로 이동할 수 있음을 숨김|
|복제(Replication)|자원이 복제되어 있음을 숨김|
|동시성(Concurrency)|자원이 여러 경쟁 사용자에 의해 공유될 수 있음을 숨김|
|실패(Failure)|자원의 실패와 복구를 숨김|

## 확장성의 문제점

|개념|예시|
|---|---|
|중앙집중형 서비스(Centralized services)|모든 사용자를 위한 단일 서버(모든 사용자가 하나의 서버에 접속)|
|중앙집중형 데이터(Centralized data)|하나의 온라인 전화번호 북(정보가 한 곳에 모여있음)|
|중앙집중형 알고리즘(Centralized algorithms)|전체 정보를 바탕으로 라우팅 수행(모든 정보를 알고 있는 상태에서 결정|

## 분산 시스템 유형(DCS/DIS/DES)

