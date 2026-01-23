---
created: 2026-01-23T14:24
updated: 2026-01-23T14:24
---
### 1단계: 3가지 특성 추출

| 특성                | 설명       | 추출 방법                                                                       |
| ----------------- | -------- | --------------------------------------------------------------------------- |
| **Pitch (피치)**    | 음높이 (Hz) | Parselmouth로 F0 추출 후 평균값 (<br><br>```<br>pitch_mean<br>```<br><br>)         |
| **Energy (에너지)**  | 음량 (dB)  | Parselmouth로 Intensity 추출 후 평균값 (<br><br>```<br>energy_mean<br>```<br><br>) |
| **Speaking Rate** | 발화 속도    | ```<br>글자수 / 발화시간<br>```<br><br> (초당 글자 수)                                  |

---

### 2단계: **화자별 정규화 (0~1 범위)**

각 특성을 **화자 개인 기준으로** 정규화합니다 (라인 176-199):

python

# 피치 정규화 (해당 화자의 5%~95% percentile 기준)

pitch_norm = (pitch_mean - pitch_min) / (pitch_max - pitch_min)

# 에너지 정규화

energy_norm = (energy_mean - energy_min) / (energy_max - energy_min)

# 스피킹 레이트 정규화

sr_norm = (speaking_rate - sr_min) / (sr_max - sr_min)

---

### 3단계: **Tone Score 계산** (라인 204)

python

tone_score = (pitch_norm + energy_norm + sr_norm) / 3

→ **3가지 특성의 정규화된 값을 단순 평균**

---

### 4단계: **어조 분류** (라인 209-221)

분위수(percentile) 기준으로 분류:

|분류|조건|의미|
|---|---|---|
|**bright** (밝음)|score ≥ 80%|상위 20%|
|**standard** (일반)|30% ≤ score ≤ 70%|중간 40%|
|**solemn** (무거운)|score ≤ 20%|하위 20%|
|**boundary** (경계)|나머지 구간|20~~30%, 70~~80%|

---

### 요약

> **Tone Score**는 **피치, 에너지, 발화속도** 3가지를 **화자별로 정규화한 후 평균**낸 종합 점수입니다.
> 
> 점수가 **높을수록 "밝은" 어조**, **낮을수록 "무거운" 어조**로 분류됩니다.