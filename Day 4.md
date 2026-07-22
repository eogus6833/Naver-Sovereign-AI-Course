# 🚀 NAVER Cloud Sovereign AI Literacy Course - Day 4

> **교육 일자**: 2026.07.21 (화)  
> **강사**: 강태완 강사님  
> **교육 기관**: NAVER Cloud Academy (SCH 순천향대학교)  

---

## 📅 Day 4: 사운드·이미지·문서 AI 및 추천 서비스 실습

### 1. 과정 안내 & 네이버 클라우드 콘솔 이중화/계정 체계

#### 1.1 교육 로드맵 (SCH 순천향대학교)
* **1~3일차 (선행 학습)**: 생성형 AI 입문, NCP AI 개발 환경 구축, 프롬프트 엔지니어링 & CLOVA Chatbot.
* **4~6일차 (메인 코스)**: 사운드·이미지·문서 AI, 음성·언어 AI, Vector DB 및 RAG 기반 통합 실습.
* **7~10일차 (프로젝트 & 평가)**: 서비스 기획·개발, 고도화, 팀별 발표 및 NCP-AI/NCE-AI 자격증 모의시험.

#### 1.2 NCP 계정 구조 및 콘솔 이용 관리
* **메인 계정 vs 서브 계정**:
  * **메인 계정**: 회원 가입 후 결제 정보(신용/체크카드)를 등록하며 클라우드 상품 및 서브 계정을 관리.
  * **서브 계정 (Sub Account)**: 메인 계정으로부터 특정 권한을 부여받아 생성되며, 전용 접속 URL(`https://www.ncloud.com/nsa/{SubKey}`)을 통해 로그인.
* **인증 관리**: 2차 인증(휴대폰, 이메일, OTP, 패스키) 필수 적용.
* **이용 관리 기능**:
  * **서비스 이용 내역**: 월별 청구 금액 및 상세 요금 조회.
  * **서비스 이용 현황**: 리전별(한국, 리전 공통 등) 현재 활성화되어 동작 중인 자원 및 보유 계약 수 확인.
  * **청구 내역 추세**: 최근 6개월간 사용한 비용 추이 그래프 제공.

---

## 2. 사운드 기반 생성형 AI (SUNO AI)

#### 2.1 SUNO AI 개요
* 텍스트 프롬프트를 기반으로 가사(Lyrics), 음악 스타일(Styles), 분위기(Mood), 보컬 성별(Vocal Gender)을 조합하여 음원을 생성하는 생성형 LLM 서비스.
* **요금 플랜 (Basic/Free)**: 매일 50 크레딧 자동 갱신 (하루 약 10곡/5회 생성 가능, 상업적 이용 불가, 최대 8분 오디오 업로드 지원).

#### 2.2 음악 생성 워크플로우 (Public LLM + SUNO)
1. **ChatGPT/CLOVA X 프롬프트 생성**:
   * 원하는 음악 주제, 대상, 장르, 분위기 지정 후 SUNO 전용 **Lyrics** 및 **Style (영어 커스텀 태그)** 추출.
   * *예시 프롬프트*:
     ```text
     Suno AI에서 생성할 노래의 Lyrics와 Styles를 작성해줘.
     노래가사는 희망차고, Style은 요들과 트로트가 섞인 신나는 스타일로 작성해줘.
     ```
2. **SUNO AI Workspace 설정**:
   * `Create` -> `Create New Workspace` 생성.
   * **Lyrics**: 프롬프트 결과 가사 입력 (구절 단위 구분).
   * **Styles**: 장르 키워드 조합 (예: `Upbeat Korean trot mixed with yodeling, bright and cheerful mood, energetic rhythm`).
   * **More Options**: Vocal Gender (Male/Female), Randomness / Style Influence 패러미터 세팅 후 `Create` 실행.

---

## 3. 네이버 클라우드 플랫폼 Application & AI API

#### 3.1 REST API 및 공통 통신 규격
* **REST API**: HTTP 프로토콜 기반의 소프트웨어 아키텍처.
* **HTTP Method**:
  * `GET`: 리소스 조회 (URL 쿼리 스트링 활용, 캐시 가능).
  * `POST`: 리소스 생성/전송 (HTTP Body 활용, 용량 제한 없음, 보안성 우수).
* **NCP API 공통 인증 헤더**:
  * `X-NCP-APIGW-API-KEY-ID`: 발급받은 Client ID.
  * `X-NCP-APIGW-API-KEY`: 발급받은 Client Secret.
  * `Content-Type`: `application/json` 또는 `application/octet-stream`.

#### 3.2 주요 Application Services
* **Maps API**: 정적/동적/벡터 지도, 최적 경로, 주소-좌표 변환(Geocoding) 서비스 제공.
* **SENS (Simple & Easy Notification Service)**:
  * **SMS/LMS/MMS**: 80자 이내(SMS), 2000byte 이내(LMS), 이미지 첨부(MMS).
  * **Biz Message**: 카카오톡 알림톡/친구톡 템플릿 연동 및 치환 태그(`#{치환자}`) 지원.
* **Cloud Outbound Mailer**: 대량 메일 발송, 법적 필수 문구(광고/수신거부) 자동화, 스마트 에디터, 사용자 정의 치환 태그(`${태그명}`) 제공.
* **NAVER API HUB**: 하나의 발급 키로 네이버의 다양한 서비스 API(검색, 쇼핑인사이트, Data Lab 트렌드 등)를 종량제로 연동·관리하는 통합 플랫폼.

#### 3.3 주요 AI Services
* **CLOVA Speech (STT)**: NEST 기술 기반 음성-텍스트 변환, 장문/단문 인식, 화자 분리(Diarization), 타임스탬프 제공.
* **CLOVA Voice (TTS)**: 텍스트-음성 변환, 100여 가지 합성음, Volume/Speed/Pitch/Emotion 등 감정 표현 제어.
* **CLOVA AiCall vs CLOVA CareCall**:
  * **AiCall**: 기업/기관의 고객센터 인바운드/아웃바운드 콜 자동화 (STT + NLU + TTS 통합).
  * **CareCall**: 노인·취약계층 대상 AI 돌봄 전화봇 (정기 안부 확인, 건강 이상 및 이상 패턴 감지 시 담당자 자동 알림).
* **NCLUE**: HyperCLOVA X 기반 사용자 행동 시퀀스 분석 및 예측(구매 예측, 이탈 방지, 사용자 프로파일링) SaaS.

---

## 4. CLOVA OCR & AITEMS 개념 원리

#### 4.1 Optical Character Recognition (OCR)
* **구성 요소**: 문자 탐지 (Text Detection) + 문자 인식 (Text Recognition).
* **기술의 발전 과정**: 원형 정합 (Template Matching) -> 통계적 방법 -> 구조 분석적 방법 -> 인공신경망 (Neural Network).
* **OCR 분류**:
  1. **General (Text) OCR**: 이미지 내 모든 텍스트 영역을 감지하고 추출.
  2. **Template OCR**: 지정된 위치/양식(체크박스, 멀티박스 등)에서 필드별 Key-Value 값을 추출.
  3. **Document OCR**: 영수증, 신용카드, 사업자등록증, 명함, 신분증 등 비정형 문서에서 AI가 구조를 이해해 Key-Value 자동 추출 (사전 승인 필요).
  4. **eKYC**: Document OCR + 제3자 인증기관 연동을 통한 비대면 신분증 진위 확인 원패스 서비스.

#### 4.2 AITEMS (개인화 추천 시스템)
* **개념**: 머신러닝 전문 지식 없이 사용자의 구매/클릭 이력을 기반으로 개인 맞춤형 상품을 추천하는 완전 관리형 서비스.
* **3대 데이터셋 스키마 (Schema)**:
  * **User**: `USER_ID` (String) [필수]
  * **Item**: `ITEM_ID` (String), `CATEGORY` (String) [필수], `CREATION_TIMESTAMP` (long - Unix Time)
  * **Interaction**: `USER_ID` (String), `ITEM_ID` (String), `TIMESTAMP` (long) [필수], `EVENT_TYPE` (String)
* **추천 모델 방식**: 개인화 추천, 연관 항목 추천, 인기 항목 추천 (Hyperparameter Optimization - HPO 지원).

---

## 5. [실습] CLOVA OCR 웹 서비스 구축 & Batch 처리

#### 5.1 OCR 웹 서비스 아키텍처
* **구조**: User Client (Browser) -> FastAPI Web Server (Port 8000) -> API Gateway (Invoke URL) -> NAVER Cloud CLOVA OCR (Port 443).

#### 5.2 콘솔 인프라 세팅
1. **VPC 및 Subnet 생성**:
   * VPC: `lab-vpc-10` (`10.0.0.0/16`, NORMAL)
   * Subnet: `lab-web-sub` (`10.0.10.0/24`, Zone: `KR-2`, Public Subnet)
2. **ACG 설정 (`lab-ocr-acg`)**:
   * Inbound Rule: Protocol `TCP`, Source `0.0.0.0/0`, Port Range `1-65535`.
3. **Init Script 생성 (`Portadd-script-ubuntu`)**:
   * SSH 22번/2200번 포트 동시 오프닝 설정:
     ```bash
     #!/bin/bash
     set -e
     mkdir -p /etc/systemd/system/ssh.socket.d
     cat > /etc/systemd/system/ssh.socket.d/override.conf <<'EOF'
     [Socket]
     ListenStream=
     ListenStream=22
     ListenStream=2200
     EOF
     sed -i '/^Port/d' /etc/ssh/sshd_config
     sed -i '1i Port 2200' /etc/ssh/sshd_config
     sed -i '1i Port 22' /etc/ssh/sshd_config
     systemctl daemon-reload
     systemctl restart ssh.socket
     systemctl restart ssh
     ```
4. **Server 생성 (`lab-ocr-svr001`)**:
   * OS: `Ubuntu 24.04-base` (KVM 하이퍼바이저)
   * Spec: `Standard c2-g3` (2vCPU, 4GB RAM), Storage 50GB
   * Network: Public IP 할당, Init Script 적용.

#### 5.3 CLOVA OCR Domain & 템플릿 세팅
1. **General OCR Domain 생성**:
   * Domain 명: `ai-ncai-ocr`, 지원 언어: 한국어, 서비스 플랜: General.
   * 옵션: `표 추출 여부` 활성화.
2. **Template OCR Domain 생성**:
   * Domain 명: `ncai-ocr-template`, 서비스 타입: 템플릿, 인식 모델: Premium, 플랜: Advanced.
   * **템플릿 빌더 세팅**:
     * 대표 샘플 업로드 (`sample_insurance_claim.png`).
     * 대표 샘플명: `보험청구서` (유사어: `보험 청구서`).
     * 판독 필드 지정: `성명` (일반 필드), `생년월일` (멀티박스), `상해`/`질병`/`교통사고` (체크박스: True='예', False='아니오').
     * 결합 필드: `휴대전화1` - `휴대전화2` - `휴대전화3` 결합 추출.
     * **배포**: `베타 배포` 후 테스트 수행 -> `서비스 배포` 최종 완료.
3. **API Gateway 연동**:
   * CLOVA OCR 콘솔 내 `API Gateway 연동` 클릭 -> `자동 연동` 수행.
   * `Secret Key` 생성 및 `APIGW Invoke URL` 복사.

#### 5.4 Linux 서버 환경 구축 및 FastAPI 애플리케이션 실행

```bash
# 1. 서버 기본 패키지 및 Python 환경 설치
apt update
apt install -y python3-pip python3-venv unzip

# 2. 서비스 디렉토리 생성 및 가상환경 구성
mkdir -p /opt/ocr-web && cd /opt/ocr-web
python3 -m venv venv
source venv/bin/activate

# 3. 소스코드 다운로드 및 의존성 패키지 설치
wget [https://kr.object.ncloudstorage.com/cs-edulab/ocr/ocr_web.zip](https://kr.object.ncloudstorage.com/cs-edulab/ocr/ocr_web.zip)
unzip ocr_web.zip
pip install -r requirements.txt

# 4. API 연동 환경변수 (.env) 설정
cat > /opt/ocr-web/.env << 'EOF'
OCR_INVOKE_URL=[https://xxxxxx.apigw.ntruss.com/custom/v1/xxxxx/xxxxxxxxxx/general](https://xxxxxx.apigw.ntruss.com/custom/v1/xxxxx/xxxxxxxxxx/general)
OCR_SECRET_KEY=여기에SecretKey입력
EOF
chmod 600 /opt/ocr-web/.env

# 5. Systemd 서비스 등록 (자동 재기동 설정)
cat > /etc/systemd/system/ocr-web.service << 'EOF'
[Unit]
Description=CLOVA OCR Web Service (FastAPI)
After=network.target

[Service]
WorkingDirectory=/opt/ocr-web
EnvironmentFile=/opt/ocr-web/.env
ExecStart=/opt/ocr-web/venv/bin/uvicorn main:app --host 0.0.0.0 --port 8000
Restart=always
User=root

[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reload
systemctl enable --now ocr-web
systemctl status ocr-web
```

#### 5.5 OCR Batch (대량 일괄 처리) 생성 및 실행 실습

* **개념**: Object Storage와 연동하여 대용량 이미지 파일의 OCR 판독 작업을 자동 일괄 처리하는 기능.
* **Step 1. Object Storage 버킷 생성**:
  * 입력 버킷 생성: `ocr-edu-batch-source-xxx` (잠금/암호화 비활성화, 권한: 비공개)
  * 출력 버킷 생성: `ocr-edu-batch-output-xxx` (잠금/암호화 비활성화, 권한: 비공개)
* **Step 2. Batch 생성 및 연동**:
  * 콘솔 경로: `CLOVA OCR` -> `Batch` -> `+배치 생성` 클릭.
  * **배치 이름**: `ocr-edu-batch100`
  * **대상 도메인**: `ai-ncai-ocr` (General 또는 Template 도메인)
  * **언어 / 파일 형식**: 한국어 / `json`
  * **인식 대상 저장 경로**: `ocr-edu-batch-source-xxx` 버킷 지정.
  * **결과 파일 저장 경로**: `ocr-edu-batch-output-xxx` 버킷 지정.
* **Step 3. Batch 실행 및 결과 파일 확인**:
  * `ocr-edu-batch-source-xxx` 버킷에 판독할 이미지 파일들(또는 폴더)을 Drag & Drop으로 업로드.
  * 처리 완료 시 source 버킷의 이미지들은 batch 전용 폴더로 자동 이관됨.
  * `ocr-edu-batch-output-xxx` 버킷에 각 이미지별 판독 결과인 `파일명.json`이 생성되며, 성공/실패 여부를 정리한 `batch명_년월일.csv` 로그 파일이 자동 생성됨.
