# 🚀 NAVER Cloud Sovereign AI Literacy Course - Day 6

> **교육 일자**: 2026.07.23 (목)  
> **강사**: 강태완 강사님  
> **교육 기관**: NAVER Cloud Academy (SCH 순천향대학교)  

---

## 📅 Day 6: Vector DB & RAG 기반 생성형 AI 서비스 통합 구축

### 1. RAG 기반 생성형 AI 시스템 핵심 개념

#### 1.1 RAG (Retrieval-Augmented Generation) 등장 배경
* **기존 LLM의 한계**:
  * **Hallucination (환각 현상)**: 사전 학습 데이터를 기반으로 확률적 단어를 생성하다 보니 사실이 아닌 내용을 그럴듯하게 거간함.
  * **정보의 최신성 및 보안 제약**: 학습 이후의 최신 정보나 사내 보안 문서, 개인정보 등 비공개 특화 데이터를 직접 답할 수 없음.
* **RAG 해결책**:
  * 외부 데이터 지식베이스(Vector DB 등)에서 사용자 질문과 연관된 최신/전문 정보를 먼저 **검색(Retrieval)**하고, 해당 정보(Context)를 프롬프트에 **보강(Augmented)**하여 LLM이 정확한 근거 기반으로 **답변을 생성(Generation)**하도록 제어함.
  * **의미 기반 (Semantic) 검색의 필요성**: 키워드 단어가 완전히 일치하지 않아도(예: "휴학 신청" vs "학교 쉬려면?") 문장 벡터 간 의미적 유사성을 파악하여 관련 문서를 찾아냄.

#### 1.2 임베딩 (Embedding) & Vector DB 원리
* **임베딩 (Embedding)**:
  * 비정형 데이터(텍스트, 이미지 등)를 의미가 반영된 고차원 숫자 벡터(Vector) 좌표로 변환.
  * 의미가 비슷한 문장/단어일수록 고차원 벡터 공간상에서 거리가 가깝게 위치함 (예: `King` - `Man` + `Woman` $\approx$ `Queen`).
* **Vector Database (VDB)**:
  * 대량의 고차원 벡터 데이터를 효율적으로 인덱싱, 저장, 유사도 검색(Cosine Similarity, Euclidean Distance)을 수행하는 특화 DB.
  * **동작 3단계**:
    1. **Indexing**: HNSW, LSH, IVF 알고리즘 등을 통해 고속 검색 구조 구축.
    2. **Querying**: 질의 벡터와 인덱싱된 벡터 간의 유사도 메트릭 연산.
    3. **Post Processing**: 검색 결과 재정렬 및 필터링 후 프롬프트에 전달.

---

## 2. 네이버 클라우드 DB 서비스 (Cloud DB) 개요

#### 2.1 관리형 DB vs 설치형 DB
* **완전 관리형 (Cloud DB)**: NCP가 백업, 모니터링, 자동 패치, 장애 발생 시 자동 복구(Auto Failover)를 전적으로 전담 운영.
* **설치형 DB**: 사용자가 Compute VM을 생성하여 DBMS를 직접 설치 및 관리 (높은 자유도, 운영 공수 증가).

#### 2.2 Cloud DB 종류별 주요 스펙 및 특징
* **Cloud DB for PostgreSQL**:
  * **특징**: 객체-관계형 DB (ORDBMS), ACID 준수, 복잡한 쿼리 및 JSON/GIS/Vector 처리 가능.
  * **스토리지**: 기본 10GB 제공, 10GB 단위로 최대 6,000GB까지 자동 증가 (HDD/SSD 선택 가능).
  * **고가용성 & 이중화**: Primary DB - Secondary DB 이중화 및 DNS 기반 자동 페일오버 지원.
  * **확장 기능**: **`pgvector`** 익스텐션을 설치하여 RAG용 벡터 데이터베이스로 변환 가능.
* **Cloud DB for MySQL / MSSQL / MongoDB / Cache**:
  * **MySQL**: Read Replica 최대 10대 지원, Load Balancer를 통한 읽기 부하 분산.
  * **MSSQL**: Principal-Mirror DB 이중화, Read Replica 최대 5대 지원.
  * **MongoDB**: BSON 기반 문서 지향 NoSQL, Sharded Cluster & Replica Set 지원.
  * **Cache (Valkey / Redis)**: 초고속 인메모리 DB, BSD 3-Clause 라이선스 기반 Valkey 표준 채택, 16,384개 해시 슬롯 기반 Auto Sharding 지원.

---

## 3. [실습] LLM FIT을 활용한 서버 최적화 모델 탐색

#### 3.1 LLM FIT 개요
* 현재 서버의 컴퓨팅 스펙(CPU 코어 수, RAM 용량, GPU 유무 등)을 자동으로 감지하여 실행 가능한 최적의 오픈소스 LLM 모델 목록 및 성능(tok/s, 쿼드 quantization)을 추천해 주는 CLI 벤치마크 툴.

#### 3.2 Linux 서버 내 LLM FIT 설치 및 실행

```bash
# 1. LLM FIT 바이너리 다운로드 및 설치
curl -fsSL [https://llmfit.axjns.dev/install.sh](https://llmfit.axjns.dev/install.sh) | sh

# 2. 설치 위치 확인 (/usr/local/bin/llmfit) 후 실행
llmfit
```

* **결과 확인**:
  * 현재 서버 시스템 사양 분석 (예: AMD EPYC 2 코어 / 4GB RAM).
  * 추천 모델 검색 (예: `Phi-tiny-MoE-instruct`, `Qwen2.5-0.5B-Instruct`, `SmolLM2-360M` 등) 및 CPU 기반 최적의 Quantization 레벨(Q4_K_M 등) 가이던스 확인.

---

## 4. [실습] RAG 기반 생성형 AI 시스템 풀스택 구축

#### 4.1 시스템 전체 아키텍처
* **VPC 대역**: `10.0.0.0/16`
* **서비스 흐름**:  
  `User (Browser)`  
  -> `Web Server (NGINX, Port 80)` [Subnet: `10.0.10.0/24`]  
  -> `App Server (FastAPI, Port 8000)` [Subnet: `10.0.10.0/24`]  
  -> `RAG Server (Embedding/Retriever, Port 8100)` [Subnet: `10.0.20.0/24`]  
  -> `Vector DB (Cloud DB for PostgreSQL + pgvector, Port 5432)` [Subnet: `10.0.40.0/24`]  
  -> `LLM Server (llama.cpp + Qwen2.5-7B, Port 8001)` [Subnet: `10.0.30.0/24`]

---

#### 4.2 Step 1: 네트워크 (VPC/Subnet/ACG) 및 Vector DB 구축
1. **Subnet 생성 (Zone: KR-2, Public Subnet)**:
   * `lab-web-sub` (`10.0.10.0/24`) - WEB / APP용
   * `lab-rag-sub` (`10.0.20.0/24`) - RAG 엔진용
   * `lab-llm-sub` (`10.0.30.0/24`) - LLM 서버용
   * `lab-db-sub` (`10.0.40.0/24`) - Vector DB용
2. **Cloud DB for PostgreSQL 생성 (`lab-db-server`)**:
   * 버전: PostgreSQL 15.x, 스펙: High CPU (2vCPU, 4GB RAM), Subnet: `lab-db-sub`.
   * 설정: DB User ID=`labuser`, Password=`lab1234!#`, Port=`5432`, DB Name=`labdb`.
3. **`pgvector` Extension 활성화**:
   * 콘솔 -> `Cloud DB for PostgreSQL` -> `DB Service 상세보기`.
   * `Extension 관리` 탭 진입 -> Extension Name: `pgvector`, Database Name: `labdb` 선택 후 `+설치` 클릭 (DB 자동 재부팅 수행).

---

#### 4.3 Step 2: LLM 추론 서버 구축 (`lab-llm-svr001`)
1. **Server 생성**:
   * Ubuntu 24.04-base, Spec: High CPU `c8-g3a` (8vCPU, 16GB RAM), Subnet: `lab-llm-sub`, ACG: `lab-llm-acg` (TCP 1-65535 허용).
2. **환경 세팅 및 Qwen 2.5 7B 모델 서비스 구동**:
   ```bash
   # 서버 기본 패키지 설치
   sudo apt update
   sudo apt install -y net-tools build-essential python3 python3-venv python3-pip

   # LLM 서비스 리소스 다운로드 및 자동 설치
   cd /opt
   wget [https://kr.object.ncloudstorage.com/cs-edulab/edu_rag_llm/chatbot-llm-svc.zip](https://kr.object.ncloudstorage.com/cs-edulab/edu_rag_llm/chatbot-llm-svc.zip)
   unzip chatbot-llm-svc.zip
   chmod 755 -R /opt/chatbot-llm-svc
   cd chatbot-llm-svc
   bash install_llm.sh

   # LLM 모델 (Qwen 2.5 7B Instruct Q4_K_M GGUF) 다운로드 및 서비스 기동
   bash /opt/chatbot-llm/scripts/download_model.sh
   systemctl restart chatbot-llm

   # 헬스체크 확인 (Port 8001)
   curl [http://127.0.0.1:8001/health](http://127.0.0.1:8001/health)
   # 응답 예시: {"status":"ok", "model_path":"/opt/models/qwen2.5-7b-instruct-q4_k_m.gguf", "loaded":true}
   ```

---

#### 4.4 Step 3: RAG 서버 구축 & 카페 데이터 문서 임베딩 (`lab-rag-svr001`)
1. **Server 생성**:
   * Ubuntu 24.04-base, Spec: Standard `c2-g3` (2vCPU, 4GB RAM), Subnet: `lab-rag-sub`.
2. **RAG 파이프라인 패키지 설치**:
   ```bash
   sudo apt update
   sudo apt install -y net-tools build-essential python3 python3-venv python3-pip postgresql-client

   cd /opt
   wget [https://kr.object.ncloudstorage.com/cs-edulab/edu_rag_llm/chatbot-rag-svc.zip](https://kr.object.ncloudstorage.com/cs-edulab/edu_rag_llm/chatbot-rag-svc.zip)
   unzip chatbot-rag-svc.zip
   chmod 755 -R chatbot-rag-svc
   cd chatbot-rag-svc

   # RAG 서비스 설치 (DB Host, Port, DB Name, DB User, DB Password)
   bash install_rag.sh {DB_Private_Domain} 5432 labdb labuser lab1234!#

   # RAG 서비스 헬스체크 (Port 8100)
   curl [http://127.0.0.1:8100/health](http://127.0.0.1:8100/health)
   ```
3. **문서 데이터 벡터 임베딩 (Embedding Data Ingestion)**:
   ```bash
   # 카페 스태프 근무/음료 제조 정보 JSON 데이터 확인
   cat /opt/chatbot-rag/rag/data/cafe_staff.json

   # 임베딩 모델(intfloat/multilingual-e5-large) 구동 및 Vector DB 적재 실행
   source /opt/chatbot-rag/venv/bin/activate
   python /opt/chatbot-rag/rag/embed_cafe_data.py
   # 출력: "임베딩 데이터 적재 완료: 20건"

   # PostgreSQL DB 직접 접속 후 임베딩 테이블 검증
   psql -h {DB_Private_Domain} -U labuser -d labdb
   # SQL> select count(*) from cafe_embedding;
   # SQL> select id, left(content, 40), left(embedding::text, 120) from cafe_embedding limit 3;
   ```

---

#### 4.5 Step 4: WEB/WAS 서버 구축 및 전체 서비스 연동 (`lab-web-svr001`)
1. **Server 생성**:
   * Ubuntu 24.04-base, Spec: Standard `c2-g3`, Subnet: `lab-web-sub`, ACG: `lab-web-acg` (TCP 80, 8000, 2200 오픈).
2. **Nginx & FastAPI 연동 배포**:
   ```bash
   sudo apt update
   sudo apt install -y net-tools build-essential python3 python3-venv python3-pip nginx

   cd /opt
   wget [https://kr.object.ncloudstorage.com/cs-edulab/edu_rag_llm/chatbot-webwas-svc.zip](https://kr.object.ncloudstorage.com/cs-edulab/edu_rag_llm/chatbot-webwas-svc.zip)
   unzip chatbot-webwas-svc.zip
   chmod 755 -R chatbot-webwas-svc
   cd chatbot-webwas-svc

   # Web/WAS 스크립트 실행 (RAG 서버 사설 IP와 LLM 서버 사설 IP 바인딩)
   bash install_webwas.sh {RAG_Server_Private_IP} {LLM_Server_Private_IP}

   # Nginx IPv6 바인딩 예외 처리 후 재기동
   sed -i '/listen \[::\]:80/d' /etc/nginx/sites-enabled/default 2>/dev/null || true
   sed -i '/listen \[::\]:80/d' /etc/nginx/sites-available/default 2>/dev/null || true
   systemctl restart nginx
   ```
3. **최종 서비스 검증 (Cafe Assistant 챗봇)**:
   * 웹 브라우저 접속: `http://{WEB_Server_Public_IP}`
   * **질문 테스트**:
     * *질문*: "오전 파트타이머 누구야?"
     * *RAG 동작*: Vector DB 유사도 검색을 통해 `지민`, `서준`, `건우` 스태프 정보 컨텍스트 추출.
     * *LLM 생성*: Qwen2.5 모델이 추출된 컨텍스트를 종합하여 "오전 파트타이머는 지민, 서준, 건우입니다." 자연어 최종 답변 생성.
