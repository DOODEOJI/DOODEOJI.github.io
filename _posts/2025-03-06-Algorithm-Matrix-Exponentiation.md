---
layout: post
title:  "[Algorithm][Matrix Exponentiation] 분할정복을 통한 행렬의 거듭제곱 (feat. 2진수 표현)"
date:   2025-03-06
categories: [백준, Algorithm]
tags: [Matrix Exponentiation, BOJ-10830, BOJ-11444]
---

## 분할정복을 통한 행렬의 거듭제곱 핵심 정리

- A^n을 구할 때 단순 반복이면 O(n)이지만, 분할정복을 사용하면 **O(log n)** 으로 줄일 수 있음
- 거듭제곱 지수를 **2진수로 표현**해 각 비트에 해당하는 행렬만 곱함
  - ex: A^13 = A^(8+4+1) = A^8 × A^4 × A^1 (13 = 1101₂)
- 비트 연산자 `&`와 `<<=`를 사용해 현재 비트 포함 여부 확인 및 다음 비트로 이동

## 알고리즘 동작

1. `result`를 **항등행렬**로 초기화 (곱셈의 시작점)
2. `current = 1`부터 시작해 지수 `n`의 각 비트를 확인
3. 해당 비트가 1이면 `result`에 현재 `matrix` 곱하기
4. `matrix`를 제곱하고, `current`를 왼쪽 시프트 (`current <<= 1`)
5. `current > n`이 될 때까지 반복

### 아래 문제에서 BOJ-10830 행렬 제곱은 BOJ-11444 피보나치 수 6의 일반화된 버전

---

## 백준 문제: 행렬 제곱 (gold IV / 10830)

```python
import sys

n, m = map(int, sys.stdin.readline().rstrip().split(" "))
current = 1

matrix = []
for _ in range(n):
    sub = list(map(int, sys.stdin.readline().rstrip().split(" ")))
    matrix.append(sub)

# 항등행렬로 시작
result = [[1 if i == j else 0 for j in range(n)] for i in range(n)]

while current <= m:
    if m & current:
        temp1 = [[] for _ in range(n)]
        for i in range(n):
            for j in range(n):
                s = 0
                for k in range(n):
                    s += (matrix[i][k] * result[k][j])
                temp1[i].append(s%1000)
        result = temp1
    
    current <<= 1
    temp2 = [[] for _ in range(n)]
    for i in range(n):
        for j in range(n):
            s = 0
            for k in range(n):
                s += (matrix[i][k] * matrix[k][j])
            temp2[i].append(s%1000)
    matrix = temp2
    
for r in result:
    print(*r)
```

## 백준 문제: 피보나치 수 6 (gold II / 11444)

```python
import sys

n = int(sys.stdin.readline().rstrip())
current = 1

m = [[1, 1], [1, 0]]
result = [[1, 0], [0, 1]]

while current <= n:
    if n & current:
        temp1 = [[],[]]
        for i in range(2):
            for j in range(2):
                s = 0
                for k in range(2):
                    s += (m[i][k] * result[k][j])
                temp1[i].append(s%1000000007)
        result = temp1
    
    current <<= 1
    temp2 = [[],[]]
    for i in range(2):
        for j in range(2):
            s = 0
            for k in range(2):
                s += (m[i][k] * m[k][j])
            temp2[i].append(s%1000000007)
    m = temp2
    
print(result[0][1])
```