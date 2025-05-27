# HTTPS
## HTTPS
<img width="259" alt="스크린샷 2025-05-27 오후 9 54 04" src="https://github.com/user-attachments/assets/c46f36c1-3980-487c-b579-b0726cb37d19" />

웹 상에서 데이터를 주고받기 위한 프로토콜

<mark>데이터를 평문으로 전송해 도청 및 데이터 변조의 위험에 있다</mark>

## HTTPS
HTTP + SSL/TLS

SSL/TLS 프로토콜을 활용하여 HTTP 통신의 데이터를 암호화

- 평문 통신을 암호화하여 도청 방지
- 정보가 도중에 변조되지 않도록 보장
- 상대가 신뢰할 수 있는 사이트인지 확인 가능
---
# SSL/TLS
## SSL(Secure Sockets Layer) / TLS(Transport Layer Security)
인터넷 상에서 데이터를 암호화된 형태로 안전하게 주고받기 위한 보안 프로토콜

SSL → TLS

## 동작위치
![image](https://github.com/user-attachments/assets/70c707e9-2d9c-4e3a-8c8f-a818d96ca5cb)
---
# 대칭키 vs 비대칭키

| 🔐 대칭키 | 🗝️ 비대칭키 |
|----------|-------------|
| ![대칭키](https://github.com/user-attachments/assets/8697f69f-805b-4098-a924-ac273874dbcb) | ![비대칭키](https://github.com/user-attachments/assets/ff6c8b73-dcbd-43c4-b1f8-092f388c5696) |
| **장점:** 빠르다 | **장점:** 공개키 노출되어도 개인키로만 복호화 가능 |
| **단점:** 키 노출 시 암호화 무의미 | **단점:** 느리다 |

---

# SSL 핸드셰이크
## 대칭키 + 비대칭키 활용하여 효율적인 통신
<img width="475" alt="스크린샷 2025-05-27 오후 10 04 50" src="https://github.com/user-attachments/assets/1e31b385-1a0f-4395-a311-10f5685dda30" />

### 간단한 버전
A : 클라이언트  |   B : 서버
<img width="488" alt="스크린샷 2025-05-27 오후 10 02 46" src="https://github.com/user-attachments/assets/709db473-11f4-4e13-ab8c-bbd51e09be8c" />
<img width="482" alt="스크린샷 2025-05-27 오후 10 03 12" src="https://github.com/user-attachments/assets/8d4b3fe3-aeaf-435d-9d33-61998ef60507" />

---

# CA 인증서
(ssl 인증서)
### CA(Certificate Authority)

공개키와 공개 DNS명의 연결을 보장하는 공인 인증 기관

### 인증서

[구성요소]

- 도메인 이름
- 공개키
- 유효 기간
- 발급자
- 서명
- 
---

# CS 예상 질문
<details>
<summary><strong>1. SSL/TLS 인증 절차를 상세하게 설명해보세요</strong></summary>

- TLS Handshake 순서:
  1. **ClientHello**: 지원하는 TLS 버전, 랜덤값, Cipher 목록 전달
  2. **ServerHello**: 선택된 Cipher, 인증서, 서버 랜덤값 전달
  3. **Certificate**: 서버의 인증서(공개키 포함) 전달
  4. **Key Exchange**: 클라이언트가 Pre-Master Secret 생성 → 서버 공개키로 암호화하여 전달
  5. **Session Key 생성**: 양측에서 Pre-Master, 랜덤값으로 동일한 Session Key 생성
  6. **Finished** 메시지 교환 후 암호화 통신 시작

- 인증서의 역할: 서버의 신원을 검증 (공개키를 신뢰할 수 있는 기관이 보증)
- 중간자 공격 방지: 공개키 위조를 방지하기 위해 CA 인증서 체계 사용
- TLS 1.3에서는 RSA 기반 대신 ECDHE 기반 키 교환, 더 빠르고 안전한 방식 사용

</details>

<details>
<summary><strong>2. TLS에서 비대칭키와 대칭키를 모두 사용하는 이유</strong></summary>

- **비대칭 암호화 (RSA, ECDHE 등)**:
  - 키 교환 과정에 사용됨
  - 안전하게 Session Key를 전달할 수 있음
  - 느리기 때문에 데이터 통신에는 부적절

- **대칭 암호화 (AES 등)**:
  - 실제 데이터 송수신에 사용
  - 속도가 빠름
  - Session Key만 안전히 교환되면 효율적

- 결론: TLS는 **키 교환은 비대칭**, **데이터 송수신은 대칭**으로 설계

</details>

<details>
<summary><strong>3. HTTP의 구조적 문제점은 무엇인가요?</strong></summary>

- **보안 문제**: 평문 전송 → 도청, 변조 가능
- **Stateless**: 상태 유지를 위해 별도 장치 필요 (예: 쿠키, 세션)
- **성능 문제**:
  - 순차 요청 처리 → 지연 (Head-of-line blocking)
  - 다중 요청 어려움 (Persistent Connection으로도 한계 존재)
- **서버 Push 불가**: 클라이언트 요청 없이 서버가 데이터를 보낼 수 없음

→ 이를 해결하기 위해 **HTTPS, HTTP/2, HTTP/3**이 등장

</details>

<details>
<summary><strong>4. HTTPS는 어떻게 동작하나요?</strong></summary>

- HTTPS = HTTP + TLS
- 3계층 구조: **TCP → TLS → HTTP**
- 포트: 443번 사용
- 인증서 기반 서버 신원 확인
- TLS Handshake 후 세션키 기반 암호화된 HTTP 통신
- 보안 요소:
  - 기밀성: 암호화된 내용
  - 무결성: MAC을 통한 변경 감지
  - 인증: 서버(혹은 클라이언트) 신원 확인

</details>

<details>
<summary><strong>5. 인증서(X.509)에는 어떤 정보가 담겨 있나요?</strong></summary>

- 도메인 정보 (CN, Common Name)
- 공개키
- 발급자 (Issuer)
- 주체 (Subject)
- 유효 기간
- 서명 알고리즘
- 인증서 서명

→ CA 인증서를 통해 체인으로 신뢰 검증

</details>

<details>
<summary><strong>6. 대칭키 암호화 vs 비대칭키 암호화 차이점</strong></summary>

| 항목 | 대칭키 암호화 | 비대칭키 암호화 |
|------|----------------|------------------|
| 키 사용 | 하나의 키 | 공개키/개인키 쌍 |
| 속도 | 빠름 | 느림 |
| 보안성 | 키 노출 시 위험 | 공개키 노출 안전 |
| 키 관리 | 안전한 키 공유 필요 | 공개키 배포 쉬움 |
| 대표 알고리즘 | AES, DES | RSA, ECC |

- TLS에서는 둘을 함께 사용하여 보안성과 성능을 모두 확보함

</details>

<details>
<summary><strong>7. HTTPS가 제공하는 보안 속성은 무엇인가요?</strong></summary>

- **기밀성 (Confidentiality)**: 대칭키 암호화를 통한 데이터 보호
- **무결성 (Integrity)**: MAC(HMAC 등)을 통한 데이터 위변조 감지
- **인증 (Authentication)**: 서버(또는 클라이언트) 인증서로 신원 확인

→ CIA 삼원성의 핵심 보안 속성 모두 포함

</details>



