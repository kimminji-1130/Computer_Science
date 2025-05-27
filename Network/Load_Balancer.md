# 💡 Load Balancer란? | CS 면접 대비 정리

로드 밸런서는 클라이언트 요청을 여러 서버에 **분산**시켜 특정 서버에 과도한 부하가 가지 않도록 하는 **트래픽 분산기**입니다.

---

## 📌 왜 알아야 할까?
<img width="511" alt="스크린샷 2025-05-27 오후 8 22 42" src="https://github.com/user-attachments/assets/f4c3720b-b216-408b-b9f0-0c09b397f120" />


- 로드 밸런싱은 클라이언트와 서버 그룹 사이에 위치해서 서버에 가해지는 **트래픽*을 여러 대의 서버에 분배하여 특정 서버의 부하를 덜어준다.
- DevOps, 클라우드, 백엔드 실무에서 자주 등장하며, 시스템 설계 면접에서 자주 묻는 주제다.
- 실무에서 잘 사용되는 기술

---

## ⚙️ Load Balancing Algorithm (분산 기준)

### ✅ 정적 알고리즘 (Static)
![load%20balancing](https://github.com/user-attachments/assets/d08e9eea-4e88-48a5-b809-d3c71b8df000)

- **Round Robin**  
  → 요청을 서버 그룹에 순차적으로 분산시킴 (즉, 동일 사용자의 여러 요청이 같은 인스턴스로 전달될 것이라는 보장은 없음) 
- **Sticky Round Robin**  
  → 라운드 로빈의 단점을 보완한 방식 (동일 사용자로부터 발생한 요청은 항상 같은 인스턴스로 전달됨)
- **Hash-Based**  
  → 요청의 해시 키 값을 기준으로 분산 처리 (이때, 해시 키는 IP 주소나 요청의 URL 등이 될 수 있음)
- **Weighted Round Robin**  
  → 각 서버에 가중치(Weight)를 부여하여 트래픽 분산 (가중치가 높은 서버에 더 많은 요청이 전달됨) → 서버마다 처리 능력이 다른 환경에 적합

📌 *단점: 갑작스러운 트래픽에 유연하게 대응하기 어려움*

---

### ✅ 동적 알고리즘 (Dynamic)
- 최소 연결 수(**Least Connections**)  
  → 연결 수가 가장 적은 서버 선택
- 최소 응답 수(**Least Response Time**)  
  → 응답 속도가 가장 빠른 서버 선택

📌 *트래픽 변화에 실시간으로 대응 가능*

---

## 🔀 Load Balancer의 유형

### 🌐 L4 Load Balancer (Transport Layer)
<img width="822" alt="스크린샷 2025-05-27 오후 9 45 56" src="https://github.com/user-attachments/assets/a0fadf93-a086-47a6-9803-d2ef15969966" />

- 주로 TCP/UDP 프로토콜의 IP 주소와 포트 정보를 기반으로 트래픽을 여러 서버로 분산
- 패킷의 헤더 정보만을 확인하기 때문에 처리 속도가 빠르고 효율적
- 실시간 트래픽 처리나 단순 분산이 필요한 서비스에 적합
- 다만, 애플리케이션 계층의 정보(예: URL, 쿠키 등)는 인식하지 못해 세밀한 라우팅이나 다양한 정책 적용에는 한계가 있음.


### 🌐 L7 Load Balancer (Application Layer)
<img width="566" alt="스크린샷 2025-05-27 오후 9 47 17" src="https://github.com/user-attachments/assets/175b9a17-d905-44bb-9bf2-e8936cd2fc80" />

- HTTP/HTTPS 등 프로토콜의 요청 내용(예: URL, 헤더, 쿠키, 페이로드 등)을 분석해 트래픽을 분산
    - 이를 통해 특정 경로나 리소스, 사용자 속성에 따라 세밀한 라우팅, 캐싱, 압축, 비정상 트래픽 필터링 등 다양한 기능을 제공
    - 패킷의 내용을 분석해야 하므로 L4에 비해 처리 속도가 느리고, 비용이나 리소스 소모가 더 큼.


# 어떤 걸 써야 할까?
| 구분     | L4 Load Balancer       | L7 Load Balancer      |
| ------ | ---------------------- | --------------------- |
| 라우팅 기준 | IP, Port               | URL, 헤더, 쿠키 등         |
| 속도     | 빠름                     | 느림                    |
| 유연성    | 낮음                     | 높음                    |
| 활용 예시  | 게임 서버, DB              | 웹 애플리케이션, API Gateway |
| 실무 사용  | ✅ 보통 **L4 + L7** 혼합 사용 |                       |

---

<details> <summary><strong>Q1. Load Balancer란 무엇이고 왜 사용하는가요?</strong></summary> 
A. 클라이언트 요청을 여러 서버에 분산시켜 시스템 부하를 줄이고, 가용성과 확장성을 높이기 위해 사용합니다. </details> 
<details> <summary>
<strong>Q2. 라운드 로빈과 해시 기반 분산 방식의 차이점은?</strong></summary> 
A. 라운드 로빈은 순차적으로 요청을 분배하고, 해시 기반은 IP나 URL 등으로 해시 값을 생성하여 분산합니다. </details> 
<details> <summary><strong>Q3. L4와 L7 로드 밸런서의 차이는 무엇인가요?</strong></summary> 
A. L4는 IP/포트 기반으로 빠르게 분산하고, L7은 애플리케이션 레벨의 정보를 기반으로 정밀한 라우팅이 가능합니다. </details> 
<details> <summary><strong>Q4. 실무에서 L4와 L7 중 어떤 걸 사용하는 게 좋나요?</strong></summary> 
A. 보통 L4는 빠른 분산에, L7은 정밀 제어에 쓰며, **둘을 함께 조합**하여 사용하는 경우가 많습니다. </details> 
<details> <summary><strong>Q5. Least Connections 알고리즘은 어떤 경우에 유리한가요?</strong></summary> 
A. 서버마다 요청 처리 시간이 다를 때, 현재 가장 덜 바쁜 서버에 요청을 보내 부하를 줄이기에 유리합니다. </details>
