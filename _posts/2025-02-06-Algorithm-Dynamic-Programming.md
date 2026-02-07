---
layout: post
title:  "[Algorithm][Dynamic Programming] 다이나믹 프로그래밍"
date:   2025-02-07
categories: [백준, Algorithm]
tags: [DP, BOJ-1149, BOJ-9095, BOJ-1463, BOJ-2579]
---

## Dynamic Programming (다이나믹 프로그래밍)

### 핵심 개념

- 테이블 정의
`arr[i]` 가 의미하는 바를 정의

- 점화식 세우기
초기값 정의 후 `arr[i] = arr[alpha] ? arr[beta]`
i단계의 테이블 값이 이전 값을 어떻게 활용하는지 파악하여 식 세우기 -> 반복문

> 테이블, 점화식만 잘 세우면 반복문 한 번으로 간단하게 해결할 수 있음

---

## 백준 문제: 1로 만들기 (silver III / 1463)

```python
import sys
n = int(sys.stdin.readline().rstrip())

dp = [0]
dp.append(0)

for i in range(2, n+1):
    candidate = []
    if i % 3 == 0:
        candidate.append(dp[i//3] + 1)
    if i % 2 == 0:
        candidate.append(dp[i//2] + 1)
    
    candidate.append(dp[i-1] + 1)

    dp.append(min(candidate))

print(dp[n])
```

---

## 백준 문제: 1, 2, 3 더하기 (silver III / 9095)

```python
import sys
T = int(sys.stdin.readline().rstrip())
dp = [0, 1, 2, 4]

for _ in range(T):
    n = int(sys.stdin.readline().rstrip())
    for i in range(4, n+1):
        if i < len(dp):
            continue
        dp.append(dp[i-3]+dp[i-2]+dp[i-1])

    print(dp)
```

---

## 백준 문제: 계단 오르기 (silver III / 2579)

```python
import sys 

# 밟은 계단 마킹
T = int(sys.stdin.readline().rstrip())
stairs = []

for _ in range(T):
    n = int(sys.stdin.readline().rstrip())
    stairs.append(n)

dp = [[0,0],[stairs[0], stairs[0]]]

for i in range(2, T+1):
    dp0 = max(dp[i-2][0], dp[i-2][1]) + stairs[i-1]
    dp1 = dp[i-1][0] + stairs[i-1]
    dp.append([dp0, dp1])

print(max(dp[T]))

# 밟지 않은 계단 마킹
# 밟지 않는 경우의 수는 이전 계단을 밟고 두 칸 건너 뛰는 경우밖에 없음

stairs.append(0) # 마지막은 무조건 밟아야하므로 마지막 다음 걸 건너뛰게
dp = [0, stairs[0], stairs[1]]

for i in range(3, T+2):
    dp.append(min(dp[i-2], dp[i-3]) + stairs[i-1])

print(sum(stairs) - dp[T+1])

```

---

## 백준 문제: RGB 거리 (silver I / 1149)

```python
import sys

n = int(sys.stdin.readline().rstrip())
house = [[0,0,0]]

for _ in range(n):
    rgb = list(map(int, sys.stdin.readline().rstrip().split(" ")))
    house.append(rgb)

for i in range(1, n+1):
    result = []
    for j in range(3):
        temp = house[i-1].copy()
        temp.pop(j)
        house[i][j] += min(temp)
    
print(min(house[-1]))

```