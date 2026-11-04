# ⚖️ VERDICT COURT — AI Debate Arena

**레트로 2D 법정 비주얼노벨 스타일의 Streamlit AI 찬반 토론 게임**

판사는 **사람인 사용자**입니다. 찬성 측 **변호사 AI**와 반대 측 **검사 AI**가 논쟁하고, **기록관 AI**가 참고용 분석을 제공합니다. 최종 판결은 사용자만 내릴 수 있습니다.

## 주요 기능

- **텍스트 슬롯형 랜덤 쟁점 추첨**: 12개 주제가 빠르게 하나씩 바뀌다가 점점 느려지며 멈춥니다. 원형 룰렛이 아닙니다. 추첨은 API를 호출하지 않습니다.
- **사전 의견 선택**: 판사는 토론 전 찬성/반대/판단 보류를 선택합니다.
- **AI 법정 토론**: 변호사 AI(찬성) vs 검사 AI(반대), 말투 선택, 1~3라운드.
- **레트로 법정 게임 화면**: 법정 배경과 사람 판사·변호사·검사 오리지널 2D 캐릭터 스프라이트, 발언 시 바뀌는 고전 게임 대사창.
- **기록관 AI 분석**: Gemini가 양쪽의 논리성·근거 적절성·반박력을 JSON으로 평가합니다. 점수는 주관적 참고 의견이지 검증된 사실 점수가 아닙니다.
- **최종 질문**: **“존경하는 판사님, 누구 말이 맞는건가요?”**
- **사람 판사의 판결**: 사용자만 최종 판결 버튼을 누르고, 판결 내용이 법정 대사창에 표시됩니다.
- **법정 기록 다운로드**: 토론 내용과 판단을 UTF-8 TXT 파일로 내려받습니다.

## GitHub에 들어가야 할 파일 구조

```text
verdict-ai-arena/        ← GitHub 저장소 최상단(루트)
├── app.py               ← 파일 이름 정확히 app.py
├── requirements.txt
├── README.md
├── .gitignore
├── .streamlit/
│   └── config.toml
└── assets/
    ├── courtroom.webp
    ├── judge.webp
    ├── defense.webp
    └── prosecutor.webp
```

**중요**: `app (1).py`나 `app.py.py`는 잘못된 이름입니다. ZIP 자체를 업로드하는 게 아니라 **압축을 푼 후 파일 및 폴더 내용물**을 올리세요. 저장소 최상단에서 `app.py`가 보여야 합니다. `.streamlit/secrets.toml`은 업로드하지 마세요.

## 기존 GitHub 저장소에 적용하기

현재 저장소: https://github.com/seeun-jang/verdict-ai-arena

1. GitHub 저장소에서 이름이 잘못된 **`app (1).py`** 파일을 제거합니다.
2. 이 ZIP을 PC에서 풀고 **안에 있는 모든 파일과 두 폴더**를 GitHub 저장소 최상단에 업로드합니다. `app.py` / `requirements.txt` / `assets/`가 루트에 보여야 합니다.
3. 변경 사항을 `main` 브랜치에 Commit 합니다.
4. Streamlit Community Cloud에서 기존 앱의 **Main file path가 `app.py`인지 확인**합니다. 설정이 잘못 연결되어 수정할 수 없다면 동일 저장소의 `main` / `app.py`로 새 배포를 생성합니다.
5. 앱의 **Settings → Secrets**에서 아래 형식으로 키를 등록하고 Save 합니다. 키를 GitHub에 올리지 않습니다.

   ```toml
   GEMINI_API_KEY = "본인의_실제_API_키"
   ```

6. 기존 앱을 재시작(Reboot)하거나 재배포 후, 다음 링크에서 확인합니다: https://verdict-ai-arena-seeun.streamlit.app

## 패키지 / 환경

`requirements.txt`:

```text
streamlit==1.66.1
google-genai==2.12.1
```

강의자료의 `gemini-3.5-flash-lite`, Gemini **Interactions API**, `store=False`, `st.secrets` 방식을 사용합니다. Streamlit Community Cloud에서는 `main` 브랜치와 `app.py`를 지정합니다.

로컬에서 실행하려면 Python 환경에 `pip install -r requirements.txt` 후 `streamlit run app.py`로 실행합니다. 로컬 키는 `.streamlit/secrets.toml`에 저장하고 공유하지 마세요.

## 오류가 나면

- **Oh no / Error running app**: 앱 관리(Manage app) → Logs에서 마지막 빨간 오류 줄을 확인합니다. 흔한 원인은 파일명 오류, 잘못된 앱 실행 경로, 패키지 설치 오류입니다.
- **이미지가 안 나옴**: `assets` 폴더와 4개 `.webp` 파일이 GitHub에 있는지 확인합니다.
- **GEMINI_API_KEY가 없음**: Streamlit 앱의 Secrets 설정을 확인합니다.
- **429 오류**: API 호출 한도 초과. 재실행 연타를 피합니다.
- **API 응답이 비거나 서버 오류**: Gemini 모델과 사용 권한, 서비스 상태를 확인합니다.

## 요청 횟수와 과제 제출

- 주제 추첨, 사전·최종 판단, 기록 다운로드: **0회** AI 호출.
- 토론 1회: 첫 발언 1 + 각 라운드 2 + 기록관 분석 1 = **2 + 2 × 라운드** 호출. 기본 2라운드 = 6회.
- 강의자료 제출 형식에 맞춰 **12주차 노트북(.ipynb) 실행 결과, 6개 조건 점검표, 앱 URL, GitHub URL, API 키 비노출 점검**을 별도로 제출하세요.

> API 요청에는 개인정보를 포함하지 않는 것이 좋습니다. 기록은 현재 Streamlit 세션에만 보관되므로 페이지 새로고침 시 사라질 수 있습니다.

## 캐릭터 저작물 안내

이미지는 이번 앱용으로 별도 생성한 **오리지널 2D 캐릭터/법정 배경**입니다. 다른 게임의 스크린샷·캐릭터 원본은 포함하지 않습니다.
