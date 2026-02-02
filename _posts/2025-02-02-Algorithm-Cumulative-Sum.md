---
layout: post
title:  "[Algorithm][Cumulative Sum] 누적합 구하기 알고리즘"
date:   2025-02-02
categories: [백준, Algorithm]
tags: [Prefix Sum, Cumulative Sum]
---

## Cumulative Sum (누적합 알고리즘)

### 핵심 개념
- 누적합 배열을 미리 계산해두면 구간 합을 O(1)에 구할 수 있음
- **구간 [A, B]의 합** = `sum[B] - sum[A-1]`
- 매번 구간을 순회하는 O(N) 대신 O(1)로 최적화

### 시간 복잡도
- 전처리: O(N)
- 쿼리당: O(1)

---

## 백준 문제: 구간 합 구하기 4 (Silver III / 11659)

```python
import sys

n, m = map(int, sys.stdin.readline().rstrip().split(" "))
n_list = list(map(int, sys.stdin.readline().rstrip().split(" ")))

# 누적합 배열 생성
sum_list = [0]
nsum = 0
for num in n_list:
    nsum += num
    sum_list.append(nsum)

# 쿼리 처리
for _ in range(m):
    i, j = map(int, sys.stdin.readline().rstrip().split(" "))
    print(sum_list[j] - sum_list[i - 1])
```
