---
created: 2025-12-24T16:10
updated: 2025-12-25T17:02
---
### 📋 상세 순서 및 집중 포인트

| 순서    | 파일               | 시간  | 집중 포인트                                                 |
| ----- | ---------------- | --- | ------------------------------------------------------ |
| **1** | tokens.txt       | 5분  | 심볼 ↔ ID 매핑 구조                                          |
| **2** | KoreanG2P.kt     | 40분 | textToTokens(), 한글 분해, <br><br>intersperse()           |
| **3** | MeloTTSEngine.kt | 50분 | loadModel(), <br><br>synthesize(), 텐서 생성               |
| **4** | AudioPlayer.kt   | 15분 | Float→Short 변환, WAV 구조                                 |
| **5** | MainViewModel.kt | 20분 | TTSUiState, <br><br>```<br>StateFlow<br>```            |
| **6** | MainActivity.kt  | 30분 | Composable 함수들, <br><br>```<br>collectAsState()<br>``` |

![[Pasted image 20251224161427.png]]


## 🗺️ 권장 학습 순서

전체 파이프라인을 **데이터 흐름 순서**로 이해하는 게 가장 효과적입니다:

[1단계: 입력] → [2단계: 변환] → [3단계: 추론] → [4단계: 출력]

---

### 📚 **Phase 1: 큰 그림 먼저** (✅ 완료)

- walkthrough.md에서 전체 구조 파악

---

### 📚 **Phase 2: 데이터 흐름 순서로 학습**

|순서|파일|배우는 것|난이도|
|---|---|---|---|
|**1**|tokens.txt|토큰 매핑이 뭔지|⭐|
|**2**|KoreanG2P.kt|텍스트 → 토큰 변환 로직|⭐⭐⭐|
|**3**|MeloTTSEngine.kt|ONNX 모델 추론 과정|⭐⭐⭐|
|**4**|MainViewModel.kt|상태 관리 (Kotlin/ViewModel)|⭐⭐|
|**5**|MainActivity.kt|UI 연결 (Jetpack Compose)|⭐⭐|
|**6**|AudioPlayer.kt|오디오 재생/저장|⭐⭐|

---

### 📚 **Phase 3: Python 변환 스크립트** (이미 있다면)

|순서|파일|배우는 것|
|---|---|---|
|7|```<br>export-onnx-korean.py<br>```|PyTorch → ONNX 변환|
|8|양자화 스크립트|모델 최적화 과정|

---

## 💡 학습 팁

### 1️⃣ **"왜?"보다 "무엇?"부터**

❌ "왜 StateFlow를 쓰지?" (나중에)

✅ "이 코드가 뭘 하는 거지?" (먼저)

### 2️⃣ **디버그 로그로 흐름 추적**

kotlin

// 각 함수에 로그 추가해서 실행 순서 확인

Log.d("DEBUG", "synthesize() 호출됨, 입력: $text")

### 3️⃣ **작은 수정부터 시도**

- 기본 텍스트 바꿔보기
- 파라미터 기본값 변경해보기
- UI 색상/텍스트 변경해보기

---

## 🎯 추천 학습 방법

**제가 도와드릴 수 있는 것:**

1. **파일별 상세 분석** - 원하는 파일 하나씩 깊게 분석
2. **개념 설명** - ONNX, G2P, 토큰화 등 용어 설명
3. **코드 주석 추가** - 이해를 위한 한글 주석 작성
4. **질문 답변** - 특정 코드가 이해 안 될 때