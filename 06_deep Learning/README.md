# 🧠 Deep Learning 5장

딥러닝에서 **손실 함수(Loss Function)**의 역할부터 **문제 유형별 Loss 선택**, **Optimizer**, **Learning Rate**, 그리고 실제 **Parameter 업데이트 과정**까지 정리합니다.

---

# 📚 전체 학습 흐름

```text
입력 데이터
   ↓
모델 예측
   ↓
Loss 계산
   ↓
Gradient 계산
   ↓
Optimizer
   ↓
Parameter 업데이트
   ↓
다음 Step
```

---

# 1️⃣ 손실 함수 (Loss Function)

## 손실 함수란?

손실 함수는 모델의 **예측값(prediction)**과 실제 **정답(target)**의 차이를 숫자로 표현합니다.

```text
예측값
   ↓
정답과 비교
   ↓
Loss
```

쉽게 생각하면 다음과 같습니다.

```text
loss가 큼
→ 많이 틀림

loss가 작음
→ 정답에 가까움

loss가 0에 가까움
→ 거의 정답
```

딥러닝의 학습 목표는 이 **Loss를 줄이는 방향으로 모델의 Parameter를 업데이트하는 것**입니다.

---

## 1-1️⃣ Prediction과 Target

손실 함수에는 보통 두 가지 값이 들어갑니다.

```python
import torch

# 모델 예측값
pred = torch.tensor([
    [2.5],
    [3.0],
    [4.0]
])

# 실제 정답값
target = torch.tensor([
    [3.0],
    [2.0],
    [5.0]
])
```

| 이름 | 의미 |
|---|---|
| `prediction` | 모델의 예측값 |
| `target` | 실제 정답값 |
| `loss` | 예측값과 정답값의 차이 |

Loss를 계산하기 전에 shape를 확인하는 것이 중요합니다.

```python
assert pred.shape == target.shape
```

현재 shape는 다음과 같습니다.

```text
pred
→ torch.Size([3, 1])

target
→ torch.Size([3, 1])
```

---

# 2️⃣ MSELoss

회귀 문제에서 대표적으로 사용하는 손실 함수입니다.

MSE는 **Mean Squared Error**의 약자입니다.

계산 과정은 다음과 같습니다.

```text
예측값 - 정답값
      ↓
차이를 제곱
      ↓
전체 평균
```

수식으로 표현하면:

```text
MSE = ((prediction - target)²).mean()
```

---

## 2-1️⃣ MSE 직접 계산하기

```python
# 예측값과 정답값의 차이를 제곱한 뒤 평균 계산
manual_loss = ((pred - target) ** 2).mean()
```

계산 과정:

```text
pred - target

[2.5] - [3.0] = -0.5
[3.0] - [2.0] =  1.0
[4.0] - [5.0] = -1.0
```

차이를 제곱합니다.

```text
(-0.5)² = 0.25
(1.0)²  = 1.00
(-1.0)² = 1.00
```

평균:

```text
(0.25 + 1.00 + 1.00) / 3

= 0.75
```

---

## 2-2️⃣ PyTorch의 MSELoss 사용

PyTorch에서는 `nn.MSELoss()`를 사용할 수 있습니다.

```python
import torch.nn as nn

# PyTorch의 MSELoss로 평균 제곱 오차 계산
torch_loss = nn.MSELoss()(pred, target)
```

직접 계산한 값과 비교합니다.

```python
manual_loss, torch_loss
```

두 결과는 동일합니다.

```python
print(torch.allclose(manual_loss, torch_loss))
```

출력:

```text
True
```

---

# 3️⃣ Reduction

여러 개의 Loss를 어떻게 하나의 결과로 만들지 결정합니다.

| reduction | 의미 |
|---|---|
| `'none'` | 각각의 Loss를 그대로 반환 |
| `'sum'` | 모든 Loss를 더함 |
| `'mean'` | 모든 Loss의 평균 |

예를 들어:

```text
각 샘플의 Loss

[0.25, 1.00, 1.00]
```

### `reduction='none'`

```text
[0.25, 1.00, 1.00]
```

### `reduction='sum'`

```text
0.25 + 1.00 + 1.00

= 2.25
```

### `reduction='mean'`

```text
(0.25 + 1.00 + 1.00) / 3

= 0.75
```

코드:

```python
loss_none = nn.MSELoss(
    reduction='none'
)(pred, target)

loss_sum = nn.MSELoss(
    reduction='sum'
)(pred, target)

loss_mean = nn.MSELoss(
    reduction='mean'
)(pred, target)
```

일반적인 학습에서는 `mean`을 많이 사용합니다.

---

# 4️⃣ Loss와 Objective Function

기초 단계에서는 `loss`와 `objective`를 비슷한 의미로 사용할 수 있습니다.

조금 더 구분하면:

```text
loss
→ 예측값과 정답값의 차이

objective
→ 학습에서 최종적으로 최소화할 전체 식
```

예를 들어:

```text
objective
=
data_loss
+
λ × regularization
```

현재 학습에서는 우선 **예측값과 정답값의 차이를 계산하는 Loss**에 집중합니다.

---

# 5️⃣ 문제 유형별 Loss 선택

문제 유형에 따라 모델의 출력과 target의 형태가 다르기 때문에 Loss도 다르게 선택해야 합니다.

| 문제 유형 | 모델 출력 | Target | dtype | Loss |
|---|---|---|---|---|
| 회귀 | 실수값 | 실수값 | `float` | `MSELoss` |
| 이진 분류 | Logit 1개 | 0 또는 1 | `float` | `BCEWithLogitsLoss` |
| 다중 분류 | Class별 Logits | Class Index | `long` | `CrossEntropyLoss` |

---

# 6️⃣ 회귀 (Regression)

회귀는 **연속적인 숫자**를 예측하는 문제입니다.

예:

```text
집값 예측
온도 예측
시험 점수 예측
수요량 예측
```

모델은 실수값을 출력합니다.

```python
model = nn.Linear(3, 1)

pred = model(x)
```

Loss는 `MSELoss`를 사용할 수 있습니다.

```python
criterion = nn.MSELoss()

loss = criterion(pred, target)
```

핵심:

```text
출력
→ 실수값

target
→ 실수값

Loss
→ MSELoss
```

---

# 7️⃣ 이진 분류 (Binary Classification)

두 개의 Class 중 하나를 선택하는 문제입니다.

예:

```text
스팸 / 정상

합격 / 불합격

고양이 / 고양이 아님

0 / 1
```

모델은 보통 Logit 하나를 출력합니다.

```python
model = nn.Linear(3, 1)

logits = model(x)
```

Loss는 `BCEWithLogitsLoss`를 사용합니다.

```python
criterion = nn.BCEWithLogitsLoss()

loss = criterion(logits, target)
```

Target 예시:

```python
target = torch.tensor([
    [0.0],
    [1.0],
    [0.0],
    [1.0]
], dtype=torch.float32)
```

핵심:

```text
모델 출력
→ (B, 1)

target
→ (B, 1)

dtype
→ float

Loss
→ BCEWithLogitsLoss
```

---

## 7-1️⃣ Sigmoid 주의

`BCEWithLogitsLoss`는 내부적으로 Sigmoid와 Loss 계산을 함께 처리합니다.

따라서 일반적으로 모델 마지막에 Sigmoid를 직접 넣지 않습니다.

```python
logits = model(x)

loss = nn.BCEWithLogitsLoss()(
    logits,
    target
)
```

확률을 확인하고 싶을 때:

```python
prob = torch.sigmoid(logits)
```

---

# 8️⃣ 다중 분류 (Multiclass Classification)

여러 개의 Class 중 하나를 선택하는 문제입니다.

예:

```text
고양이
강아지
토끼
```

또는:

```text
Class 0
Class 1
Class 2
Class 3
Class 4
```

Class가 5개라면 모델은 5개의 Logit을 출력합니다.

```python
model = nn.Linear(3, 5)

logits = model(x)
```

출력 shape:

```text
(B, 5)
```

Target은 정답 Class의 Index를 사용합니다.

```python
target = torch.tensor(
    [0, 2, 4, 1],
    dtype=torch.long
)
```

Loss:

```python
criterion = nn.CrossEntropyLoss()

loss = criterion(logits, target)
```

핵심:

```text
모델 출력
→ (B, Class 개수)

target
→ (B,)

dtype
→ long

Loss
→ CrossEntropyLoss
```

---

## 8-1️⃣ Class Index란?

```python
target = torch.tensor([0, 2, 4, 1])
```

이 숫자는 값을 예측하라는 의미가 아닙니다.

예를 들어:

```text
0
→ Class 0

1
→ Class 1

2
→ Class 2
```

즉:

```text
target = 1
```

은:

```text
두 번째 Class가 정답
```

이라는 의미입니다.

---

# 9️⃣ Optimizer

Loss를 계산하고 `backward()`를 실행했다고 해서 Parameter가 자동으로 변경되는 것은 아닙니다.

실제 Parameter 업데이트는 Optimizer가 수행합니다.

```text
Loss
 ↓
Gradient 계산
 ↓
Optimizer
 ↓
Parameter 업데이트
```

대표적인 Optimizer:

```text
SGD

Adam
```

---

# 🔟 SGD

SGD는 **Stochastic Gradient Descent**입니다.

기본적인 Parameter 업데이트 개념은 다음과 같습니다.

```text
새 Parameter
=
기존 Parameter
-
Learning Rate × Gradient
```

PyTorch 코드:

```python
import torch.optim as optim

optimizer = optim.SGD(
    model.parameters(),
    lr=0.01
)
```

각 부분:

```text
model.parameters()
→ 업데이트할 모델의 Parameter

lr
→ Learning Rate
```

---

## 🔟-1️⃣ Momentum

SGD에 Momentum을 추가할 수 있습니다.

```python
optimizer = optim.SGD(
    model.parameters(),
    lr=0.01,
    momentum=0.9
)
```

Momentum은 이전 Gradient의 방향을 일부 반영하여 업데이트를 돕습니다.

```text
현재 Gradient
+
이전 업데이트 방향
↓
다음 Parameter 업데이트
```

---

# 1️⃣1️⃣ Adam

Adam도 대표적인 Optimizer입니다.

```python
optimizer = optim.Adam(
    model.parameters(),
    lr=0.001
)
```

간단하게 비교하면:

| Optimizer | 특징 |
|---|---|
| SGD | 기본적인 Gradient 기반 업데이트 |
| Adam | Gradient 정보를 이용해 적응적으로 업데이트 |

Adam이 항상 더 좋은 것은 아니며 모델과 데이터에 따라 결과가 달라질 수 있습니다.

---

# 1️⃣2️⃣ Learning Rate

Learning Rate는 Parameter를 한 번에 얼마나 크게 업데이트할지 결정합니다.

```text
Parameter 업데이트 크기
=
Learning Rate × Gradient
```

예:

```text
Learning Rate = 0.1

Gradient = 2.0
```

그러면:

```text
업데이트 크기

= 0.1 × 2.0

= 0.2
```

---

## 1️⃣2️⃣-1️⃣ Learning Rate가 너무 작을 때

```text
Learning Rate가 너무 작음
        ↓
Parameter가 조금씩 이동
        ↓
학습 속도가 느림
```

---

## 1️⃣2️⃣-2️⃣ Learning Rate가 너무 클 때

```text
Learning Rate가 너무 큼
        ↓
Parameter가 너무 크게 이동
        ↓
최적값을 지나칠 수 있음
        ↓
Loss가 진동하거나 발산 가능
```

---

## 1️⃣2️⃣-3️⃣ 정리

```text
Learning Rate가 너무 작음
→ 학습이 느림

Learning Rate가 적절함
→ 안정적으로 학습

Learning Rate가 너무 큼
→ 진동 또는 발산 가능
```

---

# 1️⃣3️⃣ Loss만 계산하면 학습될까?

아닙니다.

```python
loss = criterion(pred, target)
```

이 코드는 단순히 현재 모델이 얼마나 틀렸는지 계산합니다.

Parameter는 변경되지 않습니다.

학습에는 다음 과정이 필요합니다.

```text
Loss 계산
   ↓
backward()
   ↓
Gradient 계산
   ↓
optimizer.step()
   ↓
Parameter 업데이트
```

---

# 1️⃣4️⃣ PyTorch Parameter 업데이트 흐름

가장 중요한 학습 순서입니다.

```text
1. zero_grad()
        ↓
2. Forward
        ↓
3. Loss
        ↓
4. Backward
        ↓
5. Step
```

코드:

```python
optimizer.zero_grad()

outputs = model(inputs)

loss = criterion(outputs, targets)

loss.backward()

optimizer.step()
```

---

# 1️⃣5️⃣ `optimizer.zero_grad()`

이전 Step에서 계산된 Gradient를 초기화합니다.

```python
optimizer.zero_grad()
```

PyTorch에서는 Gradient가 누적될 수 있습니다.

예:

```text
첫 번째 backward()

gradient = 2
```

다음에:

```text
두 번째 backward()

gradient = 3
```

초기화하지 않으면:

```text
2 + 3

= 5
```

처럼 Gradient가 누적될 수 있습니다.

따라서 일반적인 학습에서는 Step 시작 전에:

```python
optimizer.zero_grad()
```

를 사용합니다.

---

# 1️⃣6️⃣ Forward

입력 데이터를 모델에 넣어 예측값을 계산합니다.

```python
outputs = model(inputs)
```

흐름:

```text
inputs
   ↓
model
   ↓
outputs
```

이 단계에서는 Parameter가 변경되지 않습니다.

---

# 1️⃣7️⃣ Loss 계산

모델의 예측값과 실제 정답을 비교합니다.

```python
loss = criterion(
    outputs,
    targets
)
```

역할:

```text
현재 모델이
얼마나 틀렸는지 계산
```

이 단계에서도 Parameter는 변경되지 않습니다.

---

# 1️⃣8️⃣ `loss.backward()`

Loss를 기준으로 각 Parameter의 Gradient를 계산합니다.

```python
loss.backward()
```

Gradient는 Parameter의 `.grad`에 저장됩니다.

```text
backward()

→ Gradient 계산

Parameter 변경

→ X
```

즉:

```text
backward()
=
수정 방향 계산
```

이라고 생각하면 됩니다.

---

# 1️⃣9️⃣ `optimizer.step()`

실제로 Parameter를 업데이트하는 단계입니다.

```python
optimizer.step()
```

흐름:

```text
backward()

→ 어떻게 수정할지 계산

step()

→ 실제 Parameter 수정
```

정리:

```text
Loss
→ 얼마나 틀렸는지 계산

backward()
→ 어떻게 수정해야 하는지 계산

optimizer.step()
→ 실제로 Parameter 수정
```

---

# 2️⃣0️⃣ Parameter 업데이트 확인

업데이트 전 Weight를 저장합니다.

```python
weight_before = model.weight.detach().clone()
```

학습을 진행합니다.

```python
optimizer.zero_grad()

outputs = model(inputs)

loss = criterion(outputs, targets)

loss.backward()

optimizer.step()
```

업데이트 후 Weight를 저장합니다.

```python
weight_after = model.weight.detach().clone()
```

변경 여부를 확인합니다.

```python
changed = not torch.equal(
    weight_before,
    weight_after
)

print(changed)
```

출력:

```text
True
```

`True`라면 Parameter가 실제로 변경된 것입니다.

---

# 2️⃣1️⃣ `detach().clone()`

```python
weight_before = model.weight.detach().clone()
```

### `detach()`

Tensor를 계산 그래프에서 분리합니다.

```text
detach()

→ 계산 그래프와 분리
```

### `clone()`

현재 값을 독립적으로 복사합니다.

```text
clone()

→ 값 복사
```

따라서:

```python
model.weight.detach().clone()
```

은 업데이트 전 Weight 값을 독립적으로 저장할 때 사용할 수 있습니다.

---

# 2️⃣2️⃣ 전체 학습 코드

지금까지 배운 내용을 하나의 학습 Step으로 정리하면 다음과 같습니다.

```python
import torch
import torch.nn as nn
import torch.optim as optim

# 모델 생성
model = nn.Linear(5, 2)

# Loss Function
criterion = nn.CrossEntropyLoss()

# Optimizer
optimizer = optim.Adam(
    model.parameters(),
    lr=0.001
)

# 입력 데이터
inputs = torch.randn(8, 5)

# 정답 Class Index
targets = torch.tensor(
    [0, 1, 0, 1, 1, 0, 1, 0],
    dtype=torch.long
)

# 1. 이전 Gradient 초기화
optimizer.zero_grad()

# 2. 모델 예측
outputs = model(inputs)

# 3. Loss 계산
loss = criterion(
    outputs,
    targets
)

# 4. Gradient 계산
loss.backward()

# 5. Parameter 업데이트
optimizer.step()

print("loss:", loss.item())
```

전체 흐름:

```text
optimizer.zero_grad()
        ↓
이전 Gradient 초기화

outputs = model(inputs)
        ↓
모델 예측

loss = criterion(outputs, targets)
        ↓
오차 계산

loss.backward()
        ↓
Gradient 계산

optimizer.step()
        ↓
Parameter 실제 업데이트
```

---

# 2️⃣3️⃣ 자주 하는 실수

## ❌ 1. `zero_grad()`를 빼먹는 경우

```python
outputs = model(inputs)

loss = criterion(
    outputs,
    targets
)

loss.backward()

optimizer.step()
```

문제:

```text
이전 Gradient가
누적될 수 있음
```

올바른 코드:

```python
optimizer.zero_grad()

outputs = model(inputs)

loss = criterion(
    outputs,
    targets
)

loss.backward()

optimizer.step()
```

---

## ❌ 2. `optimizer.step()`을 먼저 호출하는 경우

잘못된 순서:

```python
optimizer.step()

loss.backward()
```

올바른 순서:

```python
loss.backward()

optimizer.step()
```

먼저 Gradient를 계산해야 Optimizer가 그 Gradient를 사용할 수 있습니다.

---

## ❌ 3. `loss.item().backward()`

잘못된 코드:

```python
loss.item().backward()
```

`loss.item()`은 Tensor를 Python 숫자로 변환합니다.

```text
loss

→ Tensor

→ backward() 가능
```

반면:

```text
loss.item()

→ Python 숫자

→ backward() 불가능
```

따라서:

```python
loss.backward()
```

를 사용합니다.

값을 출력할 때만:

```python
print(loss.item())
```

을 사용합니다.

---

## ❌ 4. Prediction과 Target의 Shape가 다른 경우

예:

```python
pred = torch.randn(8, 1)

target = torch.randn(8)
```

Shape:

```text
pred

→ (8, 1)

target

→ (8,)
```

필요하다면:

```python
target = target.unsqueeze(1)
```

결과:

```text
(8,)
 ↓
unsqueeze(1)
 ↓
(8, 1)
```

Loss를 계산하기 전에 Shape를 확인하는 습관이 중요합니다.

```python
assert pred.shape == target.shape
```

---

# 2️⃣4️⃣ 5장 전체 흐름

```text
문제 정의
    ↓
출력층 설계
    ↓
Target 형태 확인
    ↓
Loss 선택
    ↓
Optimizer 선택
    ↓
Forward
    ↓
Loss 계산
    ↓
Backward
    ↓
Gradient 계산
    ↓
Optimizer.step()
    ↓
Parameter 업데이트
```

---

# ⭐ 5장 핵심 정리

## 문제 유형별 Loss

```text
회귀
→ MSELoss
```

```text
이진 분류
→ BCEWithLogitsLoss
```

```text
다중 분류
→ CrossEntropyLoss
```

---

## 대표적인 Optimizer

```text
SGD

→ 기본적인 Gradient 기반 업데이트
```

```text
Adam

→ Gradient 정보를 이용해 적응적으로 업데이트
```

---

## Learning Rate

```text
Learning Rate

→ Parameter를 얼마나 크게 업데이트할지 결정
```

---

## 가장 중요한 학습 순서

```python
optimizer.zero_grad()

outputs = model(inputs)

loss = criterion(
    outputs,
    targets
)

loss.backward()

optimizer.step()
```

---

# 📝 5장 한 줄 정리

**문제 유형에 맞는 Loss를 선택하고, `zero_grad() → forward → loss → backward() → step()` 순서로 Gradient를 계산하고 Parameter를 업데이트하면서 모델을 학습한다.**
