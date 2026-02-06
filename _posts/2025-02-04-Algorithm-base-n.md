---
layout: post
title:  "[Algorithm][Base-n] n진수 자릿수 이어붙이기"
date:   2025-02-04
categories: [백준, Algorithm]
tags: [base-n, recursive function]
---

## 재귀 호출
- 재귀 호출 시 레벨 내려갈 때마다 각 레벨이 n번째 비트를 결정함
- 따라서 누적값을 만들 때는 
**ans = ans * n + quadrant** 처럼 n진수 이어붙이기로 생각하면 깔끔함

## 백준 문제: Z (gold IV / 1074)

```python
import sys

n, fi, fj = map(int, sys.stdin.readline().rstrip().split(" "))

track = []
row_start = 0
row_end = 2 ** n
column_start = 0
column_end = 2 ** n

ans = 0

while n > 0:
    if row_start <= fi < (row_start+row_end)//2 and column_start <= fj < (column_start+column_end)//2:
        ans = ans * 4 + 0
        row_end = (row_start+row_end)//2
        column_end = (column_start+column_end)//2

    elif row_start <= fi < (row_start+row_end)//2 and (column_start+column_end)//2 <= fj < column_end:
        ans = ans * 4 + 1
        row_end = (row_start+row_end)//2
        column_start = (column_start+column_end)//2

    elif (row_start+row_end)//2 <= fi < row_end and column_start <= fj < (column_start+column_end)//2:
        ans = ans * 4 + 2
        row_start = (row_start+row_end)//2
        column_end = (column_start+column_end)//2

    elif (row_start+row_end)//2 <= fi < row_end and (column_start+column_end)//2 <= fj < column_end:
        ans = ans * 4 + 3
        row_start = (row_start+row_end)//2
        column_start = (column_start+column_end)//2
    n -= 1

print(ans)
```