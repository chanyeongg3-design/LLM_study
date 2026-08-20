# 🔢 NumPy 학습 정리

## 1. NumPy?
NumPy는 Python에서 **숫자 데이터를 효율적으로 계산하기 위한 라이브러리**입니다.
배열(Array) 처리, 행렬(Matrix)계산, 벡터(Vector)연산, 통계 계산, 데이터 분석, 머신러닝, 딥러닝과 같은 작업에 많이 사용됩니다.
NumPy는 머신러닝과 딥러닝을 배우기 위한 중요한 기초 도구입니다. 

## 2. NumPy 불러오기
보통 NumPy를 np라는 이름으로 사용합니다

## 3. NumPy 배열 생성
**1차원 배열**
import numpy as np
arr = np.array([1, 2, 3, 4])
print(arr)
출력: [1 2 3 4]
**2차원 배열**
arr = np.array([
    [1, 2, 3],
    [4, 5, 6]
])
print(arr)
출력: [[1 2 3]
       [4 5 6]]

## 4. shape: 배열의 크기와 구조를 확인합니다
arr = np.array([
    [1, 2, 3],
    [4, 5, 6]
])

print(arr.shape)
출력: (2, 3), 의미: 2개의 행 3개의 열
## 5. ndim
