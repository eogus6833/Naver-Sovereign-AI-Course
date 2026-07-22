# 🚀 NAVER Cloud Sovereign AI Literacy Course - Day 5

> **교육 일자**: 2026.07.22 (수)  
> **강사**: 강태완 강사님  
> **교육 기관**: NAVER Cloud Academy (SCH 순천향대학교)  

---

## 📅 Day 5: 음성·언어 AI 서비스 (Voice, Speech, Papago, Dubbing)

### 1. CLOVA Voice (Text-to-Speech, TTS)

#### 1.1 TTS 개념 및 기술 발전 역사
* **TTS (Text-to-Speech)**: 텍스트 문장을 분석하여 사람의 목소리와 유사한 자연스러운 음성 파형으로 자동 변환하는 AI 음성 합성 기술.
* **처리 흐름**:
  1. **텍스트 분석 (Text Analysis)**: 문장부호·숫자 정규화, 품사 분석, G2P (Grapheme-to-Phoneme, 발음 기호 변환), 억양 패턴 (Prosody) 결정.
  2. **음향 모델 (Acoustic Model)**: 분석된 특징(Linguistic Features)을 바탕으로 스펙트로그램(Spectrogram) 생성.
  3. **보코더 (Vocoder)**: WaveNet, HiFi-GAN 등의 기술로 스펙트로그램을 고품질 음성 파형(Waveform)으로 최종 합성.
* **기술 진화 흐름**:
  * **1950~1980s**: 규칙 기반 (Formant Synthesis - 수학적 파형 생성, 부자연스러움).
  * **1990~2000s**: 단위 선택 합성 (Unit Selection - 실제 음성 조각 이어붙이기, 감정 표현 제약).
  * **2010s 초반**: 통계 기반 (HMM-based - 로봇 같은 음색).
  * **2016~2020s**: 딥러닝 End-to-End (Tacotron + WaveNet/HiFi-GAN - 사람 수준의 자연스러움).
  * **2020s~현재**: 멀티모달 LLM 연동, 감정/속도/톤/스타일 자유 제어, Cross-lingual (한국어 목소리 톤을 유지한 채 영어 발화) 및 실시간 스트리밍 TTS (대기시간 100ms 이하).

#### 1.2 [실습] Postman을 활용한 CLOVA Voice API 호출
* **Console 사전 설정**:
  * `Services` -> `AI NAVER API` -> `AI NAVER API` -> `+ Application 등록`.
  * Application 이름: `ncloud-voice`, 서비스 선택: `CLOVA Voice - Premium` 체크 후 등록.
  * **인증 정보 확인**: `Client ID` (`X-NCP-APIGW-API-KEY-ID`) 및 `Client Secret` (`X-NCP-APIGW-API-KEY`) 메모장 기록.
* **Postman API 요청 구성**:
  * **Method & URL**: `POST` `https://naveropenapi.apigw.ntruss.com/tts-premium/v1/tts`
  * **Headers**:
    * `X-NCP-APIGW-API-KEY-ID`: `{Client ID}`
    * `X-NCP-APIGW-API-KEY`: `{Client Secret}`
    * `Content-Type`: `application/x-www-form-urlencoded`
  * **Body (raw)**:
    ```text
    speaker=nara&speed=0&emotion=0&format=mp3&text=안녕하세요. 클라우드스퀘어 네이버클라우드아카데미 AI 교육에 오신걸 환영합니다.
    ```
* **결과 처리**:
  * API 호출 성공 시 binary 음성 데이터 수신 (`200 OK`).
  * `Save Response` -> `Save to a file`을 통해 `voice.mp3` 파일로 저장 (CSR 실습의 입력값으로 활용).

---

## 2. CLOVA Speech Recognition (CSR) & CLOVA Speech

#### 2.1 STT (Speech-to-Text) 개념 및 모델 구조
* **STT / ASR (Automatic Speech Recognition)**: 음성 신호의 주파수·세기·속도 등 음향적 특징을 분석하여 문장 형태의 텍스트로 자동 변환하는 기술.
* **동작 프로세스**:
  * `Input` -> `Preprocess` (소음 제거 Noise Suppression, 음성 구간 검출 VAD, 반향 제거 Echo Cancellation) -> `Features` (MFCC, Mel-Spectrogram 추출) -> `Model` (Conformer 기반 CNN+Transformer 연산) -> `Decoder` (Beam Search, 음향+언어 모델 결합) -> `Text` (NLP 기반 띄어쓰기/맞춤법/금칙어/커스텀 사전 후처리).

#### 2.2 CSR vs CLOVA Speech 상세 비교
| 구분 | CLOVA Speech Recognition (CSR) | CLOVA Speech |
| :--- | :--- | :--- |
| **음성 길이** | **60초(1분) 이내** 단문 음성 | **장문 오디오/비디오** (최대 수 시간) |
| **주요 용도** | 음성 명령, 비서 앱, 챗봇 입력, 음성 검색 | 회의록 자동 작성, 방송 자막 생성, 통화 녹취 분석 |
| **화자 분리 (Diarization)** | 미지원 | **지원** (발언자 A, B, C 구별) |
| **타임스탬프 (Timestamp)** | 미지원 | **지원** (단어/문장별 시작 및 종료 시간 표기) |
| **키워드 부스팅** | 미지원 | **지원** (고유명사/전문용어 가중치 부여로 인식률 향상) |
| **결과 편집 및 수출** | REST API 응답 | 웹 에디터 제공 (텍스트/화자 수정 후 CSV, SRT, SMI 추출) |

#### 2.3 [실습] Postman을 활용한 CSR (단문 STT) 호출
* **Console 설정**:
  * 기존 생성한 `ncloud-voice` Application 수정 -> `CLOVA Speech Recognition (CSR)` 추가 선택 후 저장.
* **Postman API 요청 구성**:
  * **Method & URL**: `POST` `https://naveropenapi.apigw.ntruss.com/recog/v1/stt?lang=Kor`
  * **Params**: `lang` = `Kor`
  * **Headers**:
    * `X-NCP-APIGW-API-KEY-ID`: `{Client ID}`
    * `X-NCP-APIGW-API-KEY`: `{Client Secret}`
    * `Content-Type`: `application/octet-stream`
  * **Body (binary)**: `Select file` -> 이전 실습에서 저장한 `voice.mp3` 선택.
* **결과 확인**:
  ```json

#### 2.4 [실습] CLOVA Speech (장문 STT) 콘솔 & API 구축

1. **Object Storage 버킷 생성**:
   * 소스 버킷: `ncloud-origin-xxx0000` (비공개)
   * 결과 버킷: `ncloud-result-xxx0000` (비공개)
   * 테스트 파일 업로드: `Tvpc-demo.mp4` 비디오 파일 업로드.
2. **CLOVA Speech Domain 생성**:
   * `Services` -> `CLOVA Speech` -> `+ 도메인 생성` -> `장문 인식 도메인 생성`.
   * 도메인 이름/코드: `ncloud-speech-xxx0000`, 플랜: `Basic`.
   * 추가 기능: `화자인식` (사용), `이벤트 탐지` (미사용).
   * Storage 연동: 인식 대상 경로(`ncloud-origin-xxx0000`), 결과 저장 경로(`ncloud-result-xxx0000`).
3. **키워드 부스팅 (Keyword Boosting) 설정**:
   * 빌더 실행 -> `키워드 부스팅` 메뉴 진입.
   * 전문 기술 용어 가중치 등록:
     * `Transit VPC` (가중치: 5)
     * `Security VPC` (가중치: 5)
     * `Firewall` (가중치: 4)
4. **인식 작업 실행 & 웹 에디터 편집**:
   * `작업목록` -> `+ 인식 작업 요청` -> Object Storage의 `Tvpc-demo.mp4` 선택 후 요청.
   * 작업 완료 후 `인식 결과 편집` 진입:
     * 신뢰도 색상 확인 및 텍스트 직접 수정 (수정 문장은 초록색으로 하이라이팅).
     * `화자 추가(B)`를 통해 발언자 변경 적용.
     * `내보내기` -> `CSV` 포맷 다운로드 (`demo-speech.csv`).
5. **Postman API 호출 방식 테스트**:
   * **Object Storage 내부 경로 방식 (`/recognizer/object-storage`)**:
     * URL: `POST` `{Invoke_URL}/recognizer/object-storage`
     * Header: `X-CLOVASPEECH-API-KEY`: `{Secret_Key}`, `Content-Type`: `application/json`
     * Body (raw JSON):
       ```json
       {
         "dataKey": "Tvpc-demo.mp4",
         "language": "ko-KR",
         "completion": "sync",
         "callback": "",
         "fullText": true
       }
       ```
   * **로컬 파일 직접 업로드 방식 (`/recognizer/upload`)**:
     * URL: `POST` `{Invoke_URL}/recognizer/upload`
     * Header: `X-CLOVASPEECH-API-KEY`: `{Secret_Key}`, `Content-Type`: `multipart/form-data`
     * Body (form-data):
       * `media`: File (`Tvpc-demo.mp4`)
       * `params`: Text (`{"language":"ko-KR","completion":"sync","callback":"","fullText":true}`)
       * `type`: Text (`application/json`)

---

## 3. Papago Translation (NMT & Image Translation)

#### 3.1 Papago NMT (Neural Machine Translation) 기술 원리
* **Transformer Encoder-Decoder 구조**:
  * **Encoder**: 입력 문장을 의미 벡터 시퀀스로 변환.
  * **Cross-Attention**: 원문과 타겟 언어 간의 구조적 매핑 및 문맥 맥락 파악.
  * **Decoder**: 타겟 언어로 최적의 어순과 표현 생성.
* **주요 특징**:
  * 한국어 어순, 조사, 높임말 체계(`honorific=true`)에 최적화.
  * 용어집(Glossary) 연동으로 사내 전문 용어 및 브랜드 고유명사 번역 통일.
  * 텍스트 번역, 문서 번역(.docx, .pptx, .xlsx, .pdf, .hwp 포맷 및 레이아웃 유지), 웹사이트 번역, 언어 감지 지원.

#### 3.2 [실습] Postman을 활용한 Papago Text & Image Translation
* **Console 설정**:
  * `Services` -> `Papago Translation` -> `이용 신청` 및 `+ Application 등록`.
  * App 이름: `ncloud-papago` (Text/Doc Translation 선택) & `ncloud-image-text` (Image Translation 선택).
* **Text Translation API 호출**:
  * **Method & URL**: `POST` `https://papago.apigw.ntruss.com/nmt/v1/translation`
  * **Headers**: `X-NCP-APIGW-API-KEY-ID`, `X-NCP-APIGW-API-KEY`, `Content-Type: application/x-www-form-urlencoded`
  * **Body (raw)**: `source=ko&target=en&text=안녕! 내 이름은 파파고야!`
  * **응답 결과**:
    ```json
    {
      "message": {
        "result": {
          "srcLangType": "ko",
          "tarLangType": "en",
          "translatedText": "Hi! My name is Papago."
        }
      }
    }
    ```
* **Image Translation API 호출 (OCR + NMT 통합)**:
  * **Method & URL**: `POST` `https://papago.apigw.ntruss.com/image-to-text/v1/translate`
  * **Headers**: `X-NCP-APIGW-API-KEY-ID`, `X-NCP-APIGW-API-KEY`, `Content-Type: multipart/form-data`
  * **Body (form-data)**:
    * `source`: `auto` (자동 언어 감지)
    * `target`: `ko`
    * `image`: File (영문 사용 가이드 캡처 이미지 `ImageT.png`)
  * **응답 결과**: 이미지 내 영문 텍스트 추출(`sourceText`) 및 한국어 번역문(`targetText`)이 단락 단위로 자동 구조화되어 반환됨.

---

## 4. CLOVA Dubbing (AI 더빙 & 미디어 파이프라인)

#### 4.1 CLOVA Dubbing 개념 및 비즈니스 가치
* **개념**: 동영상, 이미지, PDF 콘텐츠에 자연스러운 CLOVA Voice AI 보이스와 효과음을 합성하여 다국어 더빙 영상을 제작하는 완전 관리형 서비스.
* **자동화 처리 파이프라인**:
  `원본 미디어 업로드` -> `1단계: STT (CLOVA Speech 스크립트/타임스탬프 추출)` -> `2단계: 번역 (Papago NMT 다국어 변환)` -> `3단계: TTS (CLOVA Voice 화자/감정/속도 매핑)` -> `4단계: 싱크 및 오디오 믹싱 (BGM 유지 후 타임라인 정렬)` -> `결과 Export`.

#### 4.2 [실습] CLOVA Dubbing 프로젝트 구축 & 음성/효과음 합성
1. **서비스 이용 신청**:
   * `Services` -> `CLOVA Dubbing` -> `Subscription` -> `+ 이용 신청`.
   * 플랜 선택: `STANDARD`, 프로젝트 그룹 이름/코드: `ncloud-dubbing-xxx0000`.
   * 파일 저장 경로: Object Storage `ncloud-result-xxx0000/` 선택.
2. **더빙 프로젝트 생성 & 영상 업로드**:
   * 콘솔 `바로가기` 클릭하여 빌더 진입 -> `+ 프로젝트 추가` -> `ncloud-dubbing-xxx0000` 생성.
   * `파일 업로드` -> 이전 CLOVA Speech 실습에서 사용한 `Tvpc-demo.mp4` 파일 업로드.
3. **대본 입력 및 보이스/효과음 매핑**:
   * 이전 CLOVA Speech 결과 CSV 파일에서 정제된 텍스트 구절 복사.
   * 빌더 우측 `더빙 추가` 영역에 텍스트 붙여넣기.
   * AI 보이스 선택 (`아라`, `민상`, `다인`, `하준` 등) 후 `+ 더빙 추가` 클릭 -> 타임라인에 음성 블록 배치.
   * 타임라인 재생 헤드를 특정 시간대(`00:20.05`)로 이동 후 우측 `효과음 추가`에서 `고양이` / `관중 웃음` 효과음 삽입.
4. **결과 파일 다운로드 (Export)**:
   * 상단 `다운로드` 클릭:
     * **개별 더빙 파일**: 각 보이스 합성음만 개별 `.mp3`로 저장.
     * **음원 파일**: 더빙 음성과 효과음이 합쳐진 단일 오디오 파일.
     * **영상 파일**: 원본 비디오에 더빙 오디오가 입혀진 최종 더빙 동영상 다운로드.
  {
    "text": "안녕하세요 클라우드 스퀘어 네이버 클라우드 아카데미 ai 교육에 오신 걸 환영합니다"
  }
