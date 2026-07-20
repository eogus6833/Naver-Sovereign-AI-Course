# 🚀 NAVER Cloud Sovereign AI Literacy Course (Day 1 ~ Day 3)

> **교육 기간**: 2026.07.15 ~ 2026.07.20  
> **강사**: 이장훈 강사님  
> **교육 기관**: NAVER Cloud Academy  

---

## 📅 Day 1: 생성형 AI, 소버린 AI & 네이버클라우드 기초

### 1. 생성형 AI 및 LLM 기본 이해
* **AI / 머신러닝 / 딥러닝 개념**
  * **AI (Artificial Intelligence)**: 사람이 해야 할 일을 기계가 대신 수행하는 모든 자동화 기술
  * **Machine Learning**: 명시적 프로그래밍 없이 데이터로부터 학습하여 패턴을 스스로 발견
  * **Deep Learning**: 인공신경망 기반 모델로, 비정형 데이터에서 특징 추출 및 판단을 한 번에 수행
* **학습 방식 구분**
  * **지도 학습**: 분류(Classification, 예: 손글씨 MNIST) 및 회귀(Regression, 예: 주택 가격 예측)
  * **비지도 학습**: 군집화(Clustering, 예: 유사 특징 그룹화)
  * **강화 학습**: 환경과 상호작용하며 보상(+1 / -1)을 최대화하는 방향으로 학습 (예: 알파고)
* **퍼셉트론과 다층 퍼셉트론 (DNN)**
  * **단층 퍼셉트론**: 입력, 가중치, 임계값을 통한 판단. 단일층으로는 XOR 게이트 문제를 해결하지 못하는 한계 존재
  * **다층 퍼셉트론 (DNN)**: 은닉층(Hidden Layer)을 추가하여 비선형 문제(XOR 등) 해결
* **생성형 AI (Generative AI)**
  * 기존 콘텐츠를 학습하여 텍스트, 이미지, 음성, 영상, 코드를 새로 창작하는 AI
  * **트랜스포머 아키텍처**: 어텐션(Attention) 메커니즘을 통한 중요 단어 집중 및 병렬 처리 지원
  * **작동 프로세스**: 토큰화(Tokenization) -> 임베딩(Embedding) -> 사전 학습(Pre-training) -> 미세 조정(Fine-tuning) -> 추론(Inference)

### 2. 소버린 AI (Sovereign AI) & AI 윤리
* **소버린 AI (Sovereign AI)**
  * **개념**: 국가가 주도하여 자체 인프라, 데이터, 기술로 개발한 AI
  * **필요성**: 글로벌 Big Tech 의존성 탈피, 자국의 언어/문화/맥락 이해, 국가 안보 및 산업 기밀 유출 방지
  * **3대 핵심 요소**: 한국적 AI 모델, 데이터 주권 수호, 자체 기술 인프라
* **AI 윤리 및 규제**
  * **AI 3대 기본원칙**: 인간의 존엄성, 사회의 공공선, 기술의 합목적성
  * **AI 10대 핵심요건**: 투명성, 안전성, 책임성, 데이터 관리, 프라이버시 보호 등

### 3. 네이버클라우드 플랫폼 (NCP) 네트워크 & 서버
* **Region & Zone**
  * **Region**: 지리적으로 떨어진 클라우드 서비스 거점
  * **Zone**: 리전 내 물리적으로 완전히 분리된 데이터센터 세그먼트 (고가용성 확보)
* **VPC & Subnet**
  * **VPC (Virtual Private Cloud)**: 클라우드 상의 논리적으로 격리된 고객 전용 가상 사설망
  * **Public Subnet**: Internet Gateway(IGW)를 통해 외부 인터넷과 직접 통신 (공인 IP 부여 가능)
  * **Private Subnet**: 외부 직접 통신 불가. Outbound 통신 필요 시 NAT Gateway 사용
* **ACG (Access Control Group)**: 서버 NIC 단위로 적용되는 Stateful 가상 방화벽 (Allow 규칙만 가능)

---

## 📅 Day 2: 콘솔 환경 설정, 웹/API 서버 구축 및 Load Balancer

### 1. 서버 접속 및 CLI/API 키
* **SSH 원격 접속**: MobaXterm 또는 VS Code (Remote - SSH) 활용 (기본 22번 포트 사용)
* **NCP CLI & API Key**:
  * IAM 및 API 인증키 관리에서 Access Key ID / Secret Key 발급
  * CLI 바이너리 설치 및 `./ncloud configure` 등록

### 2. 웹 서버 & API 서버 구축 실습
* **Nginx (웹 서버)**: 정적 파일 제공, Reverse Proxy, Load Balancing 역할 수행
  * **Ubuntu 24.04 이슈 해결**: NCP 기본 이미지의 IPv6 소켓 비활성화 이슈로 `/etc/nginx/sites-available/default` 파일에서 `listen [::]:80 default_server;` 구문 주석 처리 후 Nginx 재기동
* **FastAPI (API 서버)**: Python 기반 API 애플리케이션 구축
  ```bash
  mkdir -p ~/fastapi-demo && cd ~/fastapi-demo
  python3 -m venv .venv
  source .venv/bin/activate
  pip install fastapi[standard]
  uvicorn main:app --host 0.0.0.0 --port 8000 --reload

* **HTTP 프로토콜 핵심**[cite: 1]
  * `GET` (조회) / `POST` (생성) / `PUT` (전체 수정) / `PATCH` (부분 수정) / `DELETE` (삭제)[cite: 1]
  * 주요 상태 코드: `200 OK`, `400 Bad Request`, `404 Not Found`, `500 Internal Server Error`, `502 Bad Gateway`[cite: 1]

### 3. Postman API 테스트
* TODO List API 서비스 엔드포인트를 대상으로 CRUD 메서드 테스트 및 JSON 응답 확인[cite: 1]

### 4. Load Balancer (ALB) 연동
* **로드밸런서 개념**: 트래픽 분산 처리를 통한 고가용성 및 확장성 확보[cite: 1]
* **분배 알고리즘**:
  1. **Round Robin**: 순차적으로 요청 분배[cite: 1]
  2. **Least Connection**: 현재 연결 수가 가장 적은 서버로 분배[cite: 1]
  3. **Source IP Hash**: 클라이언트 IP 해시값 기반 매핑[cite: 1]
* **핵심 요소**: Listener (포트 수신) -> Target Group (요청 처리 대상 집합) -> Health Check[cite: 1]

---

## 📅 Day 3: 프롬프트 엔지니어링, Clova Studio & Chatbot, 로컬 LLM

### 1. AI 최신 용어 및 에이전트 개념
* **Agent / Agentic AI**: 사용자 목표 제시 시 사람의 개입 없이 스스로 계획 수립, 정보 수집, 도구 활용을 수행하여 과업을 완수하는 자율 시스템[cite: 2]
* **MCP (Model Context Protocol)**: LLM이 외부 데이터 소스(API, DB, 파일)와 안전하고 쉽게 연동하도록 지원하는 오픈소스 표준 프로토콜[cite: 2]
* **RAG (Retrieval-Augmented Generation)**: 외부 데이터베이스에서 관련 정보를 검색(Retrieval) 후 LLM에 전달하여 정확도 향상 및 환각(Hallucination) 방지[cite: 2]
* **Harness & Loop Engineering**: 에이전트의 수행 환경·권한 제어 및 "관찰 -> 판단 -> 실행 -> 검증" 반복 루프 제어[cite: 2]

### 2. 프롬프트 엔지니어링 & 파라미터 조절
* **프롬프트 4대 핵심 요소**: 페르소나 (Persona), 맥락 (Context), 지시 (Instruction), 출력 형식 (Output)[cite: 2]
* **프롬프팅 기법**: Zero-shot, Few-shot, CoT (Chain of Thought), ToT (Tree of Thoughts)[cite: 2]
* **주요 하이퍼파라미터**:
  * **Temperature**: 낮을수록 일관되고 정형적인 답변, 높을수록 다양하고 창의적인 답변 (무작위성 조절)[cite: 2]
  * **Top P / Top K**: 후보 단어 추출 범위 조절 (Top P: 누적 확률 기준, Top K: 상위 K개 기준)[cite: 2]
  * **Repetition Penalty**: 동일 단어나 문구의 반복을 억제[cite: 2]

### 3. Clova Studio & Clova Chatbot 실습
* **Clova Studio (HCX-005, HCX-007)**: 테스트 API Key 발급 후 Postman으로 `/v3/chat-completions` API 호출 실습[cite: 2]
* **Clova Chatbot**: Domain 생성 -> 대화/인텐트/엔티티(Entity)/태스크(Task)/Form 설계 -> 대화 모델 빌드 및 테스트[cite: 2]

### 4. 로컬 LLM 실행 실습 (`llama.cpp`)
* 네이버클라우드 Linux 서버 (2vCPU / 8GB RAM) 환경에서 CPU 추론을 통한 오픈소스 LLM 실행[cite: 2]
  ```bash
  # llama.cpp 빌드 및 Llama 3.2 1B Instruct (Q4_K_M 양자화) 모델 실행
  cd /root/llama.cpp
  ./build/bin/llama-server \
    -m /root/llama.cpp/models/Llama-3.2-1B-Instruct-Q4_K_M.gguf \
    --alias llama-3.2-1b \
    --host 0.0.0.0 \
    --port 8080 \
    -c 2048 \
    --api-key ncloud-llm-demo \
    --webui
  ```[cite: 2]
