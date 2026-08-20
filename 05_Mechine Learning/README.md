# 🤖 Machine Learning

머신러닝에서 학습한 핵심 개념과 실습 내용을 정리합니다.

---

# 📚 전체 학습 흐름

```text
데이터
   ↓
Train / Test 분리
   ↓
전처리
   ↓
불균형 데이터 처리
   ↓
모델 학습
   ↓
평가 지표
   ↓
Cross Validation
   ↓
하이퍼파라미터 탐색
   ↓
후보 선택
   ↓
최종 모델 학습
   ↓
Sealed Test
```

---

# 1️⃣ Train / Validation / Test

머신러닝에서는 데이터를 목적에 따라 나누어 사용합니다.

| 데이터 | 역할 |
|---|---|
| Train | 모델 학습 |
| Validation | 모델과 하이퍼파라미터 선택 |
| Test | 최종 모델 성능 확인 |

쉽게 생각하면 다음과 같습니다.

```text
Train
= 공부

Validation
= 모의고사

Test
= 최종시험
```

## 핵심

```text
Train → 학습

Validation → 모델 선택

Test → 최종 평가
```

---

# 2️⃣ 데이터 누수 (Data Leakage)

## 데이터 누수란?

실제 예측 시점에는 알 수 없는 정보가 학습 과정에 들어가는 현상입니다.

예를 들어 전체 데이터를 먼저 `StandardScaler`로 학습시키면 Test 데이터의 평균과 분산 정보가 전처리에 포함될 수 있습니다.

---

## ❌ 잘못된 방법

```text
전체 데이터
   ↓
Scaler.fit()
   ↓
Train / Test 분리
```

이 경우 Test 데이터의 정보가 이미 전처리에 사용됩니다.

---

## ✅ 올바른 방법

```text
Train / Test 분리
   ↓
Train → fit + transform
   ↓
Test → transform만
```

### 예제 코드

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

# Train 데이터로 평균과 표준편차를 학습
X_train_scaled = scaler.fit_transform(X_train)

# Test 데이터는 기존 기준으로 변환만 수행
X_test_scaled = scaler.transform(X_test)
```

## 핵심

```text
Train → fit + transform

Test → transform
```

---

# 3️⃣ Pipeline

Pipeline은 전처리부터 모델 학습까지의 과정을 하나로 연결하는 기능입니다.

```text
데이터
   ↓
전처리
   ↓
모델
```

## 예제 코드

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

pipeline = Pipeline([
    ("scale", StandardScaler()),
    ("model", LogisticRegression())
])
```

모델 학습:

```python
pipeline.fit(X_train, y_train)
```

예측:

```python
pred = pipeline.predict(X_test)
```

## Pipeline의 장점

- 전처리와 모델을 하나로 관리
- 코드가 깔끔해짐
- Cross Validation 과정에서 데이터 누수를 줄일 수 있음

---

# 4️⃣ 불균형 데이터

불균형 데이터란 특정 클래스의 데이터가 매우 적거나 많은 경우입니다.

예시:

```text
정상 : 990개

불량 : 10개
```

이 경우 모델이 모든 데이터를 `정상`이라고 예측해도 Accuracy가 높게 나올 수 있습니다.

```text
정상 데이터가 많음
      ↓
모든 데이터를 정상이라고 예측
      ↓
Accuracy는 높게 나올 수 있음
```

---

# 5️⃣ 불균형 데이터 처리 방법

## 1. class_weight

소수 클래스의 오류에 더 큰 가중치를 줍니다.

```python
from sklearn.linear_model import LogisticRegression

model = LogisticRegression(
    class_weight="balanced"
)
```

---

## 2. Undersampling

많은 클래스의 데이터를 줄이는 방법입니다.

```text
정상 데이터
1000개
   ↓
일부 제거
   ↓
100개
```

단점:

```text
유용한 데이터를 잃을 수 있음
```

---

## 3. SMOTE

SMOTE는 소수 클래스 주변에 새로운 데이터를 생성하는 방법입니다.

```text
소수 클래스 데이터
      ↓
SMOTE
      ↓
새로운 합성 데이터 생성
```

### 예제 코드

```python
from imblearn.over_sampling import SMOTE

smote = SMOTE()

X_resampled, y_resampled = smote.fit_resample(
    X_train,
    y_train
)
```

## 가장 중요한 규칙

```text
SMOTE는 Train 데이터에만 적용한다.
```

```text
Train → SMOTE 적용

Test → 적용하지 않음
```

---

# 6️⃣ Confusion Matrix

분류 모델의 예측 결과를 다음과 같이 나눌 수 있습니다.

| 실제 / 예측 | Positive | Negative |
|---|---|---|
| Positive | TP | FN |
| Negative | FP | TN |

---

## TP

```text
실제 Positive
예측 Positive
```

Positive를 정확하게 예측한 경우입니다.

---

## TN

```text
실제 Negative
예측 Negative
```

Negative를 정확하게 예측한 경우입니다.

---

## FP

```text
실제 Negative
예측 Positive
```

실제로는 Negative인데 Positive라고 잘못 예측한 경우입니다.

---

## FN

```text
실제 Positive
예측 Negative
```

실제로는 Positive인데 Negative라고 잘못 예측한 경우입니다.

---

# 7️⃣ Accuracy

전체 데이터 중 정확하게 맞춘 비율입니다.

```text
Accuracy

=
전체 중 맞힌 개수
──────────────
전체 데이터
```

하지만 불균형 데이터에서는 Accuracy만 보면 모델 성능을 잘못 판단할 수 있습니다.

---

# 8️⃣ Precision

Positive라고 예측한 것 중 실제 Positive의 비율입니다.

```text
Precision

=
TP
───────
TP + FP
```

FP가 중요한 경우 확인합니다.

---

# 9️⃣ Recall

실제 Positive 중에서 모델이 찾아낸 비율입니다.

```text
Recall

=
TP
───────
TP + FN
```

FN이 중요한 경우 확인합니다.

---

# 🔟 F1 Score

Precision과 Recall을 함께 고려하는 평가 지표입니다.

```text
Precision
      +
Recall
      ↓
F1 Score
```

---

# 1️⃣1️⃣ PR-AUC

Precision과 Recall의 관계를 이용해 모델 성능을 평가하는 지표입니다.

특히 Positive 데이터가 적은 불균형 데이터에서 활용할 수 있습니다.

```text
Precision
     +
Recall
     ↓
PR Curve
     ↓
PR-AUC
```

---

# 1️⃣2️⃣ ROC-AUC vs PR-AUC

## ROC-AUC

ROC-AUC는 모델의 전체적인 분류 능력을 평가합니다.

```text
TPR
=
Recall
```

과

```text
FPR
```

의 관계를 사용합니다.

---

## PR-AUC

PR-AUC는 다음 관계를 사용합니다.

```text
Precision
     +
Recall
```

Positive 클래스가 매우 적은 불균형 데이터에서 특히 중요하게 사용할 수 있습니다.

---

# 평가 지표 핵심 정리

```text
불균형 데이터

Accuracy만 확인 ❌

Confusion Matrix
+
Precision
+
Recall
+
F1 Score
+
PR-AUC

함께 확인 ⭕
```

---

# 1️⃣3️⃣ Cross Validation

Cross Validation은 Train 데이터를 여러 부분으로 나누어 모델을 반복적으로 평가하는 방법입니다.

예를 들어 4-Fold Cross Validation:

```text
전체 Train 데이터

Fold 1 → Train / Validation

Fold 2 → Train / Validation

Fold 3 → Train / Validation

Fold 4 → Train / Validation

        ↓

     평균 성능
```

각 Fold에서 모델을 학습하고 성능을 계산합니다.

```text
Fold 1 → 점수

Fold 2 → 점수

Fold 3 → 점수

Fold 4 → 점수

      ↓

평균 점수
```

---

# 1️⃣4️⃣ CV Fit 수

CV에서 모델이 실제로 학습되는 횟수는 다음과 같이 생각할 수 있습니다.

```text
CV Fit 수

=

후보 수 × Fold 수
```

예시:

```text
후보 12개

4-Fold

12 × 4

=

48 CV Fits
```

---

# 1️⃣5️⃣ Grid Search

Grid Search는 지정한 하이퍼파라미터의 모든 조합을 탐색합니다.

## 예제

```python
param_grid = {
    "n_estimators": [100, 200],
    "max_depth": [5, 10]
}
```

가능한 조합:

```text
n_estimators = 100
max_depth = 5

n_estimators = 100
max_depth = 10

n_estimators = 200
max_depth = 5

n_estimators = 200
max_depth = 10
```

## 장점

```text
모든 조합을 빠짐없이 탐색
```

## 단점

```text
탐색 공간이 커지면
시간이 오래 걸릴 수 있음
```

---

# 1️⃣6️⃣ Random Search

Random Search는 모든 조합을 탐색하지 않고 일부 후보를 랜덤하게 선택합니다.

```text
전체 후보

● ● ● ● ● ● ● ● ●

      ↓

랜덤 선택

●     ●      ●
```

## 장점

```text
제한된 시간 안에

넓은 탐색 공간을

효율적으로 탐색
```

---

# Grid Search vs Random Search

| 구분 | Grid Search | Random Search |
|---|---|---|
| 탐색 방식 | 모든 조합 | 일부 후보 랜덤 선택 |
| 적합한 경우 | 탐색 공간이 작음 | 탐색 공간이 큼 |
| 장점 | 빠짐없이 확인 | 넓은 범위를 빠르게 탐색 |
| 단점 | 시간이 오래 걸릴 수 있음 | 모든 조합을 확인하지 않음 |

---

# 1️⃣7️⃣ Optuna

Optuna는 하이퍼파라미터 탐색을 자동화하는 도구입니다.

기본적으로 여러 하이퍼파라미터 후보를 제안하고 성능을 확인합니다.

```text
하이퍼파라미터 후보
        ↓
Cross Validation
        ↓
성능 계산
        ↓
결과 기록
        ↓
다음 후보 탐색
```

---

# 1️⃣8️⃣ TPE

TPE는 이전 Trial 결과를 참고해서 다음 후보를 제안하는 방식입니다.

```text
이전 결과
   ↓
좋았던 후보 분석
   ↓
좋지 않았던 후보 분석
   ↓
다음 후보 제안
```

즉, 무작정 랜덤으로 탐색하는 것이 아니라 이전 결과를 참고하여 다음 후보를 탐색합니다.

---

# Optuna + TPE 전체 흐름

```text
하이퍼파라미터 후보 제안
        ↓
Cross Validation 수행
        ↓
mean_AP 계산
        ↓
결과 기록
        ↓
다음 후보 제안
        ↓
반복
```

## 중요한 규칙

```text
Test 데이터를

Optuna objective에

사용하면 안 된다.
```

Test 데이터는 마지막 평가까지 사용하지 않습니다.

---

# 1️⃣9️⃣ Random Forest

Random Forest는 여러 개의 Decision Tree를 사용하는 앙상블 모델입니다.

```text
데이터
  ↓

Tree 1
Tree 2
Tree 3
Tree 4

  ↓

결과 종합

  ↓

최종 예측
```

---

# 주요 하이퍼파라미터

## n_estimators

생성할 Decision Tree의 개수입니다.

```python
RandomForestClassifier(
    n_estimators=100
)
```

의미:

```text
Decision Tree 100개 생성
```

---

## max_depth

각 Decision Tree가 가질 수 있는 최대 깊이입니다.

```python
RandomForestClassifier(
    max_depth=10
)
```

흐름:

```text
max_depth 증가
      ↓
모델 복잡도 증가
      ↓
과적합 가능성 증가
```

반대로 너무 작으면 데이터를 충분히 학습하지 못할 수 있습니다.

---

# 2️⃣0️⃣ mean_AP

여러 Cross Validation Fold에서 나온 AP의 평균입니다.

```text
Fold 1 → AP

Fold 2 → AP

Fold 3 → AP

Fold 4 → AP

      ↓

평균

=

mean_AP
```

mean_AP가 높을수록 평균적인 성능이 좋은 후보라고 볼 수 있습니다.

---

# 2️⃣1️⃣ std_AP

각 Fold의 성능이 얼마나 변하는지를 나타냅니다.

```text
std_AP 작음

=

Fold별 성능 변화가 작음
```

하지만:

```text
std_AP가 작다

=

무조건 좋은 모델 ❌
```

평균 성능인 `mean_AP`도 함께 확인해야 합니다.

---

# 2️⃣2️⃣ 최종 후보 선택

여러 하이퍼파라미터 후보 중 하나를 최종적으로 선택해야 합니다.

학습한 내용의 선택 흐름은 다음과 같습니다.

```text
① 가장 높은 mean_AP 확인
        ↓
② 최고 AP에서 허용 범위 안의 후보 선택
        ↓
③ 성능 변동이 작은 후보 확인
        ↓
④ 미리 정한 기준으로 최종 선택
```

예시:

```text
최고 mean_AP

0.657
```

허용 범위:

```text
0.657 - 0.01

=

0.647
```

따라서:

```text
mean_AP >= 0.647
```

인 후보들이 선택 대상이 됩니다.

## 주의

```text
0.01

=

1% ❌

AP의 절대 차이 0.01 ⭕
```

---

# 2️⃣3️⃣ selected의 의미

`selected`는 여러 하이퍼파라미터 후보 중 최종적으로 선택된 후보입니다.

예제:

```python
best_mean = candidates["mean_AP"].max()

eligible = candidates[
    candidates["mean_AP"] >= best_mean - mean_tolerance
]
```

먼저 가장 높은 성능을 찾습니다.

```text
best_mean
```

그다음 허용 범위 안에 있는 후보를 선택합니다.

```text
eligible
```

그리고 최종 후보를 선택합니다.

```python
selected = eligible.sort_values(
    SELECTION_ORDER,
    kind="mergesort"
).iloc[0]
```

전체 흐름:

```text
전체 후보
   ↓
best_mean 확인
   ↓
조건에 맞는 후보 선택
   ↓
eligible
   ↓
정렬
   ↓
첫 번째 후보
   ↓
selected
```

즉:

```text
selected

=

최종 하이퍼파라미터 후보 1개
```

---

# 2️⃣4️⃣ 최종 모델 학습

Cross Validation을 통해 최종 하이퍼파라미터를 선택한 뒤 모델을 학습합니다.

```text
Cross Validation
        ↓
최적 하이퍼파라미터 선택
        ↓
전체 Train 데이터로 재학습
        ↓
최종 모델 생성
```

---

# 2️⃣5️⃣ Sealed Test

Test 데이터는 마지막까지 사용하지 않습니다.

```text
Train
   ↓
모델 학습

Validation / CV
   ↓
하이퍼파라미터 선택

Test
   ↓
최종 성능 확인
```

핵심:

```text
Test 데이터는

모델 선택 과정에 사용하지 않는다.
```

---

# 🔗 전체 머신러닝 연결

지금까지 학습한 내용을 하나의 흐름으로 연결하면 다음과 같습니다.

```text
데이터
   ↓
Train / Test 분리
   ↓
Train 데이터 기준 전처리
   ↓
필요하면 불균형 데이터 처리
   ↓
Pipeline 구성
   ↓
Cross Validation
   ↓
하이퍼파라미터 탐색
   ↓
mean_AP 비교
   ↓
최종 후보 선택
   ↓
전체 Train 데이터로 최종 학습
   ↓
Sealed Test
   ↓
최종 성능 확인
```

---


# 💡 핵심 정리

| 개념 | 핵심 내용 |
|---|---|
| Train | 모델 학습 |
| Validation | 모델 선택 |
| Test | 최종 성능 평가 |
| Data Leakage | 미래 정보가 학습 과정에 들어가는 것 |
| StandardScaler | Train에서 fit |
| Pipeline | 전처리와 모델 연결 |
| SMOTE | 소수 클래스 데이터 증가 |
| Accuracy | 전체 정확도 |
| Precision | Positive 예측의 정확도 |
| Recall | 실제 Positive를 찾아낸 비율 |
| F1 Score | Precision과 Recall을 함께 고려 |
| PR-AUC | 불균형 데이터 평가에 활용 |
| Cross Validation | 여러 Fold로 모델 평가 |
| Grid Search | 모든 조합 탐색 |
| Random Search | 일부 후보 랜덤 탐색 |
| Optuna | 하이퍼파라미터 자동 탐색 |
| TPE | 이전 결과를 참고해 다음 후보 탐색 |
| Random Forest | 여러 Decision Tree를 결합 |
| mean_AP | Fold 성능의 평균 |
| std_AP | Fold 성능의 변동 정도 |
| selected | 최종 선택된 후보 |
| Sealed Test | 마지막에만 사용하는 Test 데이터 |

---

# 🎯 최종 정리

```text
데이터 누수를 막은 상태에서

Train 데이터를 이용해 전처리와 모델을 학습하고,

Cross Validation으로 여러 하이퍼파라미터 후보를 비교한 뒤,

가장 적절한 모델을 선택하고,

마지막에 Test 데이터로

최종 성능을 확인한다.
```

---

# 🚀 학습 목표

- Train / Validation / Test의 역할 이해
- 데이터 누수 방지 방법 이해
- `fit()`과 `transform()` 차이 이해
- Pipeline 구조 이해
- 불균형 데이터 처리 방법 이해
- SMOTE 적용 위치 이해
- Accuracy 이해
- Precision 이해
- Recall 이해
- F1 Score 이해
- ROC-AUC와 PR-AUC 이해
- Cross Validation 이해
- CV Fit 수 계산
- Grid Search와 Random Search 비교
- Random Forest 주요 하이퍼파라미터 이해
- `mean_AP`와 `std_AP` 이해
- `selected`의 의미 이해
- 최종 모델 선택 과정 이해
- Sealed Test의 역할 이해
