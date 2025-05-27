# TCP 3-Way Handshake & 4-Way Handshake

TCP는 **신뢰성 있는 데이터 전송을 보장**하기 위해 통신을 시작하기 전 "연결 성립"을, 종료 시에는 "연결 해제"를 위한 과정을 거친다.  
이 과정이 바로 `3-Way Handshake`와 `4-Way Handshake`이다.

---

## 3-Way Handshake - 연결 성립 과정

> TCP는 연결지향형 프로토콜로, 데이터 전송 전 송·수신자 간에 **논리적인 연결을 먼저 설정**해야 한다.

### 요약

| 단계 | 송신자 (Client) | 수신자 (Server) | 설명 |
|------|------------------|------------------|------|
| 1    | SYN, seq=client_isn(seq 초기값)       |     ->             | 연결 요청 (SYN: 연결 시작) 클라이언트->서버 |
| 2    |      <-           | SYN + ACK, seq=server_isn(server 초기값), ack=client_isn+1 | 요청 수락 + 응답 서버->클라이언트|
| 3    | ACK, ack=server_isn+1     |        ->          | 연결 확인 |

- **SYN**: 연결 요청 (Synchronize)
- **ACK**: 응답 확인 (Acknowledge)
- **SEQ / ACK Number**: 데이터 순서를 관리하기 위한 번호

### 그림으로 이해하기
<img width="277" alt="스크린샷 2025-05-27 오후 11 32 49" src="https://github.com/user-attachments/assets/e592c6b5-0f5b-4b59-899b-451e73915d57" />


---


### 3-Way Handshake의 목적
- 클라이언트와 서버가 서로 **연결이 가능한지 확인**
- 양측의 **초기 Sequence Number를 교환**하여 이후 데이터 흐름 제어
- **양방향 통신 스트림 생성**

---

## 4-Way Handshake - 연결 해제 과정

> TCP 연결 해제는 송·수신자가 각자 종료 의사를 **별도로 표현**해야 하므로 4단계로 나뉜다.

### 요약

| 단계 | 송신자 | 수신자 | 설명 |
|------|--------|--------|------|
| 1    | FIN    |   ->     | 연결 종료 요청 (클라이언트->서버)|
| 2    |    <-  | ACK    | 요청 확인 (서버->클라이언트)|
| 3    |    <-  | FIN    | 서버 종료 요청 (서버->클라이언트)|
| 4    | ACK    |   ->     | 종료 확인 (클라이언트->서버)|

### 그림으로 이해하기
<img width="273" alt="스크린샷 2025-05-27 오후 11 41 31" src="https://github.com/user-attachments/assets/3c7f55e0-d99a-4dfb-b2e6-45800afa335d" />

---

### TIME_WAIT 상태
- 마지막 ACK를 보낸 클라이언트는 **TIME_WAIT 상태**에 진입
- 재전송될 수 있는 FIN을 처리하거나, 지연된 패킷이 도착할 가능성을 고려해 일정 시간 대기 후 연결 종료

---

## 면접에서 자주 묻는 질문

<details>
<summary><b>TCP 3-way handshake란 무엇인가요?</b></summary>
TCP는 연결 지향형 프로토콜입니다. 송신자와 수신자가 통신을 시작하기 전에 서로 연결 가능한 상태인지 확인하기 위해 3단계의 handshake 과정을 거칩니다.
</details>

<details>
<summary><b>3-way handshake의 각 단계에서 어떤 패킷이 오고가나요?</b></summary>
1. 클라이언트가 SYN 패킷을 서버로 전송  
2. 서버는 SYN과 ACK 패킷을 함께 전송  
3. 클라이언트는 ACK 패킷을 서버로 전송  
이후에 연결이 성립되고 데이터 송수신이 가능합니다.
</details>

<details>
<summary><b>4-way handshake 과정은 어떻게 이루어지나요?</b></summary>
1. 클라이언트가 서버로 FIN 요청을 보냄  
2. 서버는 ACK를 보내고 CLOSE_WAIT 상태  
3. 서버가 데이터 처리를 끝낸 후 FIN을 전송  
4. 클라이언트가 ACK를 보내고 TIME_WAIT 후 종료  
</details>

<details>
<summary><b>왜 연결 해제는 4단계인가요?</b></summary>
TCP는 양방향 통신이므로 **각 방향의 연결을 따로 종료**해야 합니다. 클라이언트 → 서버 종료 요청, 서버 → 클라이언트 종료 요청이 따로 처리됩니다.
</details>

<details>
<summary><b>3-way handshake 과정 중에 발생할 수 있는 취약점은?</b></summary>
**SYN Flooding 공격**: 클라이언트가 악의적으로 다수의 SYN만 보내고 ACK를 생략하여 서버의 자원을 고갈시키는 공격.  
방지 방법: SYN 쿠키, 방화벽 설정, backlog 큐 조정 등
</details>

<details>
<summary><b>UDP는 왜 handshake가 없나요?</b></summary>
UDP는 **비연결지향 프로토콜**이기 때문에 연결 설정 과정 없이 바로 데이터를 전송합니다. 빠른 전송이 필요한 서비스(예: DNS, 실시간 스트리밍)에 주로 사용됩니다.
</details>

---

## 추가 설명

- **TCP 세그먼트**: 데이터를 전송하기 위한 TCP의 기본 단위
- **Sequence Number**: 전송된 바이트의 순서를 식별
- **Acknowledgement Number**: 수신 측이 다음에 받을 것으로 예상하는 Sequence Number

