# Chapter 5. 네이밍 정리 내용

### Names, Identifiers, and Addresses

- Name(이름)
  - 임의의 실체(Entity, Object 등)에 부여
  - 실체를 구분하고 원하는 대상을 찾기 위함
- Identifier(ID, 식별자)
  - 동일한 이름을 가지는 실체를 구분
  - 식별자와 실체는 1:1 관계
  - 동일한 식별자는 항상 동일한 실체를 가리켜야 함
- Address(주소)
  - 실체에 Access 하는 방법
  - Access Point
 
<br>

### Naming 방법

네이밍 방법에는 3가지가 존재함

- Flat
  - Forwarding Pointer
  - HBA(Home-Based Address)
  - DHT(Distributed Hash Table)
  - HA(Hierarchical Approach)
- Structured
  - NS(Name Space): File System, Web
- Attribute-Based
  - LDAP
 
<br>

## Flat 


### Forwarding Pointer

<img width="745" alt="image" src="https://github.com/user-attachments/assets/9db84cb0-cc95-4295-91dc-e5b1525b30d9" />

- Object가 이동 위치마다 Pointer 생성, Chain을 따라 Object 실제 위치 추적, 단 이동이 많은 경우 체인이 너무 길어짐

<br>

<img width="582" alt="image" src="https://github.com/user-attachments/assets/591bb133-1c27-4428-89d4-9b3afd02676e" />

<img width="589" alt="image" src="https://github.com/user-attachments/assets/bc4ad0f8-9a26-4eb5-a8c5-8b5e5e7fc18c" />

- 체인 길어짐을 Shortcut을 이용하여 해결
- 이동이 많은 경우 Chain이 길어지는데, 불필요한 위치를 제거하여 Chain 단축

<br>

### Home-based Approaches

<img width="705" alt="image" src="https://github.com/user-attachments/assets/4e5e3f2b-0e8b-4b08-a3fa-b6b32c717db0" />

- Mobile IP에 응용
- Home Agent(HA)는 Mobile Host(대상)의 현재 위치를 알고 있음
  - HA는 Mobile Host가 Channel에 최초로 가입한 지점
  - 홈페이스가 클라이언트에게 위치를 알려줌
- Mobile Host에 메시지 전송, Access 하려면 HA에 Query를 보내면 됨

<br>

### Distributed Hash Tables

<img width="647" alt="image" src="https://github.com/user-attachments/assets/8b33ec35-9c13-4f7e-9ddf-51f8fff0ca27" />

- 실제 노드가 Data 또는 Object 관리
- P2P, Blockchain에 응용
- Hash Table상 Key는 어떤 Node를 가리킴
- Ex. Chord 구조 DHT

<br>

### Hierarchical Approaches (계층구조)

<img width="785" alt="image" src="https://github.com/user-attachments/assets/46910dd6-441c-424d-a0c9-0ab5d5f9845d" />

<img width="749" alt="image" src="https://github.com/user-attachments/assets/cb1c61fa-17ad-4eda-9164-7b8067a6c235" />

<img width="691" alt="image" src="https://github.com/user-attachments/assets/8f501b85-7ff6-4207-b825-fc3e0399dc98" />

<img width="677" alt="image" src="https://github.com/user-attachments/assets/79bebc67-196f-42aa-918a-b7afd15a6fe9" />

<img width="670" alt="image" src="https://github.com/user-attachments/assets/d636bed0-2da3-42a6-a32a-306682dd4481" />


- Tree형태 Domain 구조
- 서로 다른 Domain 내 같은 Entity 존재 가능
- Domain 관리는 Root가 관리
- 탐색
  - 찾는 Entity가 현재 Domain내에 없다면 상위 Domain으로 이동하여 탐색
  - 한번 Entity를 찾으면 상위 Domain의 Root 만 방문하면 됨
 
<br>

### Name Space

<img width="801" alt="image" src="https://github.com/user-attachments/assets/5c72a5d5-c77a-4c94-96d5-bd2b49f716c9" />

<img width="794" alt="image" src="https://github.com/user-attachments/assets/e1901ecf-11c6-4704-9c6b-e47a68264475" />

- File System, Domain Name(인터넷)에 응용
- Tree 형태 Directory 사용
- Unix의 File System은 Name과 inode 속성을 가짐
  - inode(index node): index node, 식별자이며 DISK 주소값을 가짐, 이름만으로도 File을 혼동할 수 있으므로 사용
- 원격으로 Storage가 흩어져있는 경우 분산 File System(Linking and Mounting) 사용

<br>

### Structured - Name Space - Linking and Mounting

<img width="806" alt="image" src="https://github.com/user-attachments/assets/56a97cee-df65-4680-a3c3-24f0999433a7" />

<img width="757" alt="image" src="https://github.com/user-attachments/assets/8562a0e1-0adb-4685-a4b7-81a0da23daeb" />

<img width="640" alt="image" src="https://github.com/user-attachments/assets/77019aa1-3712-4438-8e30-f66ba198fca6" />

- 원격에 있는 File을 Local에 있는 것처럼 사용
- 자신의 Directory Tree에 원격의 File System을 연결하고 Mounting
  - Mounting에 필요한 정보: Access Protocol, 서버 이름, Mounting 지점에 관한 외부 Name Space


<br>

### Structured - Name Space - Name Space Distribution

<img width="705" alt="image" src="https://github.com/user-attachments/assets/3ecee2d4-43f8-499a-909c-ac79e747e8ff" />

<img width="770" alt="image" src="https://github.com/user-attachments/assets/a9dab570-294d-4e11-aad9-c4f3404380e5" />

- DNS, Web Site에 응용
- 사람이 이용하기 쉽도록 이름을 통해 Entity 접근
  - Flat Model보다 사람이 이용하기 쉽도록 고안된 것이 Structured (naming) Model
- DNS: 이름을 Entity의 IP 주소로 Mapping
- 구현 시 Global Level에서는 가용성이, Manage Level에서는 성능이 중요

<br>

### Implementation of Name Resolution

<img width="820" alt="image" src="https://github.com/user-attachments/assets/b73131d0-7435-4477-9cf5-637440352685" />

<img width="823" alt="image" src="https://github.com/user-attachments/assets/c8451a32-a313-4f93-8c7d-ae8e7765fee4" />

<img width="819" alt="image" src="https://github.com/user-attachments/assets/429b2921-660d-48f6-bc9d-f622a9575475" />

- DNS Query 동작
  - Iterative: 클라이언트가 직접 Root부터 각 Level 서버를 Query
  - Recursive: 클라이언트는 Root 서버만 Query, 이후 Root 서버에서 각 Level 서버로 Query 순회
- 원거리 통신의 경우 Iterative 방식은 Recursive보다 Delay 발생
- Ex. www.hanyang.ac.kr
  - 점을 구분으로 하여 뒤에서부터 탐색
  - Root Level: kr
  - 2nd Level: ac
  - 3rd Level: hanyang
  - 4th Level: www
 
<br>

## DNS(Domain Name Space)

<img width="818" alt="image" src="https://github.com/user-attachments/assets/d36abcb0-97e7-47ce-b9a8-69d96937e709" />

<img width="782" alt="image" src="https://github.com/user-attachments/assets/916640cb-720d-468b-9aee-e6cf16c13567" />

<br>

### Name Resolution 방식 (이름 해석 방식)

#### Iterative Resolution

- 클라이언트가 직접 각 단계의 네임 서버에 요청
- 각 네임 서버는 다음 서버 주소를 알려주고 클라이언트가 직접 재요청함
- 통신 비용은 클라이언트가 부담 (각 요청 마다 네트워크 이동)
- 네트워크 분산, 효율적

<br>

#### Recursive Resolution

- 클라이언트는 첫 네임 서버에 한 번만 요청
- 해당 서버가 책임지고 하위 네임 서버들에 순차적으로 요청하고 결과 반환
- 클라이언트 통신 횟수는 적지만, **서버 부하 증가**

<br>

