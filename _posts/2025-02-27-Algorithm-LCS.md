---
layout: post
title:  "[Algorithm][LCS] 가장 긴 부분 수열 찾기"
date:   2025-02-27
categories: [백준, Algorithm]
tags: [LCS, BOJ-9251]
---

## LCS와 DP 핵심 정리

- LCS는 두 문자열에서 **공통으로 등장하는 부분수열** 중 가장 긴 것을 찾는 문제
- 연속일 필요 없이 순서만 유지되면 됨 (부분문자열과 다름)
- 브루트포스로 모든 부분수열을 비교하면 지수 시간 복잡도 → **2D DP로 해결**

## DP 정의
- `dp[i][j]` = `s1`의 첫 `i`글자, `s2`의 첫 `j`글자 사이의 LCS 길이

## 점화식 아이디어
- `s1[i] == s2[j]`이면: `dp[i][j] = dp[i-1][j-1] + 1`
  - 두 문자가 같으므로 직전 대각선 값에 1을 더함
- `s1[i] != s2[j]`이면: `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`
  - 각각 한 글자씩 제외했을 때의 최적값 중 큰 것을 선택
- 예시: `s1 = "ACAYKP"`, `s2 = "CAPCAK"`
  - LCS = `"ACAK"` → 길이 4

```
     ""  C  A  P  C  A  K
""  [ 0  0  0  0  0  0  0 ]
A   [ 0  0  1  1  1  1  1 ]
C   [ 0  1  1  1  2  2  2 ]
A   [ 0  1  2  2  2  3  3 ]
Y   [ 0  1  2  2  2  3  3 ]
K   [ 0  1  2  2  2  3  4 ]
P   [ 0  1  2  3  3  3  4 ]
```

## 참고 블로그 (그림으로 설명되어 있음)
- [dong1님의 블로그 - [알고리즘] The Longest Common Subsequence(LCS) (Dynamic Programming 활용)](https://ddongwon.tistory.com/106)

---

## 백준 문제: LCS (gold IV / 9251)

```python
import sys

s1 = sys.stdin.readline().rstrip()
s2 = sys.stdin.readline().rstrip()

dp = [[0 for _ in range(len(s2)+1)] for _ in range(len(s1)+1)]

for i in range(len(s1)):
    for j in range(len(s2)):
        if s1[i] == s2[j]:
            dp[i+1][j+1] = dp[i][j] + 1

        else:
            dp[i+1][j+1] = max(dp[i][j+1], dp[i+1][j])

print(dp[-1][-1])
```
