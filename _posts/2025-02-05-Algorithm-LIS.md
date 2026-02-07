---
layout: post
title:  "[Algorithm][Dynamic Programming][LIS] 최장 길이 증가 부분 수열"
date:   2025-02-05
categories: [백준, Algorithm]
tags: [DP, Longest Increasing Subsequence, LIS, BOJ-11053]
---

## LIS와 DP 핵심 정리

- LIS는 연속이 아닌 **부분수열** 문제
- 모든 경우를 branch/재귀로 탐색하면 시간초과 발생
- 해결 핵심은 **각 위치에서 끝나는 최적값을 저장하는 DP**

## DP 정의
- `dp[i]` = `i` 바로 직전까지의 가장 긴 증가 부분수열의 길이 + 1

## 점화식 아이디어
- `i` 앞의 값 중 `arr[j] < arr[i]`인 것만 후보로, 그중 **dp[j] 최대값 + 1**로 갱신
- 만일 더 큰 값이 포함된다면 증가 부분수열이 아님
- 예시: `[1, 2, 9, 4]`에서 4일 경우, `[1, 2]`가 대상
  - 9가 포함되면 `1, 2, 9, 4`와 같이 증가하는 수열이 성립하지 않음

---

## 백준 문제: 가장 긴 증가하는 부분 수열 (silver II / 11053)

```python
import sys

num = int(sys.stdin.readline().rstrip())
n_list = list(map(int, sys.stdin.readline().rstrip().split(" ")))

dp = [1 for _ in range(num)]

for i in range(1, num):
    best = 0
    for j in range(i-1, -1, -1):
        if n_list[i] > n_list[j]:
            if best < dp[j]:
                best = dp[j]

    if best == 0:
        continue

    dp[i] = best + 1

print(max(dp))
```