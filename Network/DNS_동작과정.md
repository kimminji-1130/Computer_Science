##  DNS란?

DNS(Domain Name System)은 **도메인 이름을 IP 주소로 변환하는 인터넷 전화번호부**이다. 우리가 브라우저에 `www.google.com` 같은 이름을 입력하면, DNS가 해당 이름을 IP 주소(예: `142.250.206.100`)로 바꿔준다.

---

## 핵심 개념 요약

1. **분산형 데이터베이스**: DNS는 중앙 서버가 아닌 계층적으로 분산된 구조이다.
2. **캐싱 메커니즘**: 반복 질의 시 빠른 응답 가능 (지연 최소화, 트래픽 감소).
3. **애플리케이션 계층 프로토콜**: 클라이언트가 직접 DNS 질의를 수행.

---

## DNS의 주요 서비스

- **Host aliasing**: 복잡한 이름을 간단한 별칭으로 관리 (예: `server-128-1-3-25.example.com` → `www.example.com`)
- **Mail server aliasing**: 이메일 서버를 위한 별칭 지원 (`MX` 레코드 사용)
- **부하 분산(Load Balancing)**: 여러 서버의 IP를 하나의 도메인에 연결 → 부하 분산 효과 (Round Robin 방식 등)

---

##  DNS 계층구조

<img width="690" alt="DNS 계층구조" src="https://github.com/user-attachments/assets/6c99c17e-cf77-4695-a6d1-02dad36c2456" />

1. **Root DNS server**
   - 최상위 DNS 서버 (13개 root 서버 그룹 존재)
   - TLD 서버의 IP 주소를 가지고 있음
2. **TLD (Top Level Domain) DNS server**
   - `.com`, `.org`, `.net` 같은 도메인 관리
   - 도메인 등록기관이 운영
3. **Authoritative DNS server**
   - 실제 `example.com`에 대한 IP 주소 정보 보관
   - 직접 수정/관리 가능 (회사/서비스 제공자 소유)
4. **Local DNS server (Resolver)**
   - 사용자의 가장 가까운 DNS 서버
   - 자주 찾는 도메인을 캐싱하여 응답 속도 향상

📌 **면접포인트**: "Authoritative DNS와 TLD DNS의 차이점은?" → IP 직접 보유 여부

---

## DNS 동작 원리

<img width="699" alt="DNS 동작 흐름" src="https://github.com/user-attachments/assets/241d6c90-b7aa-4fc0-9254-59ff35a78ba3" />

1. 사용자가 브라우저에 `www.example.com` 입력
2. 브라우저 → Local DNS 서버(보통 ISP 제공)
3. Local DNS → Root DNS 서버 질의
4. Root DNS → `.com` TLD DNS 주소 제공
5. Local DNS → TLD DNS 서버 질의
6. TLD DNS → Authoritative DNS 서버 주소 제공
7. Local DNS → Authoritative DNS 질의 → IP 주소 획득
8. Local DNS → 브라우저에 IP 주소 반환
9. 브라우저 → 해당 IP로 HTTP 요청
10. 웹 서버 → 웹 페이지 응답

📌 **실제는 캐시로 인해 많은 단계를 생략 가능**

---

## 재귀적 쿼리 vs 반복적 쿼리

| 구분 | 재귀적 쿼리 (Recursive) | 반복적 쿼리 (Iterative) |
|------|--------------------------|---------------------------|
| 누가 수행? | 클라이언트 → Local DNS | Local DNS → 상위 DNS |
| 방식 | DNS 서버가 결과를 대신 찾아줌 | 각 DNS 서버가 다음 서버 정보만 제공 |
| 장점 | 클라이언트가 단순함 | 서버 간 부담 분산 |

 **면접포인트**: “왜 두 가지 방식이 존재하나요?”  
→ 효율성, 서버 분산, 캐시 활용 때문

---

## DNS에서 TTL(Time To Live)

- TTL은 **DNS 레코드의 캐시 유효 시간**
- 예: TTL = 3600초 → 1시간 동안 해당 IP 정보를 캐싱함
- TTL이 낮으면 변경이 빨리 반영됨, 그러나 네트워크 부하 증가
- TTL이 0이면, 매번 새로운 질의 필요 → 성능 저하

# TTL 확인 명령어
dig www.naver.com
![image](https://github.com/user-attachments/assets/4d18ad2a-b94e-4808-b568-a3bda54e2a81)
TTL이 0이 된다면 -> DNS 서버는 해당 도메인에 대한 질의 요청을 사용자에게 해주지만 캐싱하지 않고 바로 버린다.
---
<details> <summary><strong>1. DNS가 없으면 웹사이트 접속은 어떻게 될까요?</strong></summary>
도메인 대신 모든 웹사이트를 IP 주소로 직접 입력해야 합니다. → 사용성 저하 + 유지보수 어려움 → DNS는 사용자가 기억하기 쉬운 도메인으로 추상화해주는 역할을 함</details>
<details> <summary><strong>2. DNS는 어느 계층에서 동작하나요?</strong></summary> **애플리케이션 계층**입니다. → DNS는 IP 주소로 변환해주는 네트워크 도우미지만, HTTP/SMTP 같은 애플리케이션 계층에서 호출됩니다.</details>
<details> <summary><strong>3. DNS Spoofing 공격이란?</strong></summary> 악의적인 사용자가 가짜 IP 주소를 DNS 응답에 삽입하여 사용자가 **악성 사이트**로 접속하게 만드는 공격입니다. → 이를 방지하기 위해 DNSSEC(DNS Security Extensions) 도입 </details>
<details> <summary><strong>4. DNS Round-Robin이란?</strong></summary> 하나의 도메인에 여러 IP를 매핑하고, 요청마다 순차적으로 응답하여 **부하 분산**을 하는 기술입니다. → 단순하지만, 서버 상태를 고려하지 못하는 단점 있음 </details>
<details> <summary><strong>5. DNS와 CDN의 관계는?</strong></summary> CDN은 전 세계에 위치한 서버로 콘텐츠를 캐싱함. DNS는 사용자의 위치에 따라 가장 가까운 CDN 서버의 IP 주소를 제공하여 응답속도와 성능을 최적화합니다. </details>
<details> <summary><strong>6. DNS CNAME과 A 레코드 차이는?</strong></summary>
A 레코드: 도메인을 IP 주소에 직접 매핑
CNAME: 도메인을 다른 도메인에 매핑 (간접)
예:
www.example.com → example.com (CNAME)
example.com → 123.123.123.123 (A 레코드)

</details>
