# 🔢 NumPy 학습 정리

## 1️⃣ 배열 생성

### 1차원 배열

```python
import numpy as np

arr = np.array([1, 2, 3, 4])

print(arr)
```

출력:

```text
[1 2 3 4]
```

---

### 2차원 배열

```python
arr = np.array([
    [1, 2, 3],
    [4, 5, 6]
])

print(arr)
```

출력:

```text
[[1 2 3]
 [4 5 6]]
```

---

# 📐 배열의 차원

```python
arr = np.array([
    [1, 2, 3],
    [4, 5, 6]
])

print(arr.shape)
```

출력:

```text
(2, 3)
```

의미:

```text
2개의 행
3개의 열
```

즉,

```text
[
 [1, 2, 3],
 [4, 5, 6]
]
```

`shape`가 `(2, 3)`이라는 것은 **2개의 행과 3개의 열**을 의미합니다.

---

# ➕ 배열 연산

```python
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

print(a + b)
```

출력:

```text
[5 7 9]
```

같은 위치의 원소끼리 계산합니다.

```text
1 + 4 = 5
2 + 5 = 7
3 + 6 = 9
```

---

# ✖️ 스칼라 연산

```python
arr = np.array([1, 2, 3])

print(arr * 2)
```

출력:

```text
[2 4 6]
```

배열의 모든 값에 `2`를 곱합니다.

---

# 📊 평균 계산

```python
arr = np.array([1, 2, 3, 4, 5])

print(np.mean(arr))
```

출력:

```text
3.0
```

`np.mean()`은 배열의 평균을 계산합니다.

---

# 🔄 행렬 생성

```python
matrix = np.array([
    [1, 2],
    [3, 4]
])

print(matrix)
```

출력:

```text
[[1 2]
 [3 4]]
```

---

# ✖️ 행렬 곱

```python
A = np.array([
    [1, 2],
    [3, 4]
])

B = np.array([
    [5, 6],
    [7, 8]
])

result = A @ B

print(result)
```

출력:

```text
[[19 22]
 [43 50]]
```

`@`는 **행렬 곱(Matrix Multiplication)** 을 의미합니다.

---

# 📐 행렬 변환

벡터 또는 데이터에 행렬을 적용할 수 있습니다.

```python
import numpy as np

def apply_T(M, X):
    return X @ M.T
```

여기서:

- `M` → 변환 행렬
- `X` → 변환할 데이터
- `M.T` → 행렬 `M`의 전치 행렬
- `@` → 행렬 곱

예시:

```python
M = np.array([
    [2.0, 0.0],
    [0.0, 0.8]
])

X = np.array([
    [1.0, 1.0],
    [2.0, 2.0]
])

result = apply_T(M, X)

print(result)
```

---

# 🔍 Scaling

행렬을 이용하여 데이터의 크기를 변경할 수 있습니다.

```python
S = np.array([
    [2.0, 0.0],
    [0.0, 0.8]
])
```

의미:

```text
x축 → 2배 확대
y축 → 0.8배 축소
```

예를 들어:

```text
(1, 1)
```

이라는 점에 적용하면:

```text
(2, 0.8)
```

처럼 변환됩니다.

---

# 🪞 Reflection

행렬을 이용하여 좌우 또는 상하 반사를 할 수 있습니다.

예시:

```python
F = np.array([
    [-1, 0],
    [0, 1]
])
```

의미:

```text
x축 값 반전
```

예를 들어:

```text
(2, 3)
```

에 적용하면:

```text
(-2, 3)
```

으로 변환됩니다.

---

# 📉 Projection

벡터를 특정 방향으로 투영할 수 있습니다.

```python
P = np.array([
    [1, 0],
    [0, 0]
])
```

의미:

```text
y축 값을 제거하고
x축 방향으로 투영
```

예를 들어:

```text
(3, 5)
```

에 적용하면:

```text
(3, 0)
```

이 됩니다.

---

# 🧩 NumPy와 AI

```text
데이터
   ↓
NumPy 배열
   ↓
수치 계산
   ↓
머신러닝
   ↓
딥러닝
```

NumPy에서 사용하는 **배열과 행렬 개념**은 이후 PyTorch와 딥러닝에서도 계속 사용됩니다.

특히 다음 개념들이 중요합니다.

- 배열(Array)
- 벡터(Vector)
- 행렬(Matrix)
- 차원(Dimension)
- `shape`
- 행렬 곱(Matrix Multiplication)

---

# 💡 핵심 정리

| 개념 | 설명 |
|---|---|
| `np.array()` | 배열 생성 |
| `shape` | 배열 구조 확인 |
| `ndim` | 배열 차원 확인 |
| `np.mean()` | 평균 계산 |
| `@` | 행렬 곱 |
| Scaling | 크기 변경 |
| Reflection | 반사 |
| Projection | 투영 |

---

# 🚀 학습 목표

- [x] NumPy 배열 이해
- [x] 배열의 차원과 `shape` 이해
- [x] 벡터와 행렬 연산 이해
- [x] 행렬 변환 이해
- [x] Scaling 이해
- [x] Reflection 이해
- [x] Projection 이해
- [x] 머신러닝과 딥러닝 데이터 처리 기초 학습

---

# 🔗 학습 흐름

```text
NumPy
   ↓
기초 수학
   ↓
머신러닝
   ↓
딥러닝
   ↓
PyTorch
   ↓
신경망
   ↓
LLM / 생성형 AI
```
