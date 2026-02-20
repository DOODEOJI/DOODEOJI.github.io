---
layout: post
title:  "[Algorithm][Dynamic Programming] 다이나믹 프로그래밍"
date:   2025-02-20
categories: [백준, Algorithm]
tags: [DP, BOJ-9465]
---

## Dynamic Programming (다이나믹 프로그래밍)

### 고려해야할 점

- 내가 할 수 있는 행동이 무엇이 있는지 (단위화할 수 있는 행동, 반복되는 행동) 정의하기
  - => 아래 문제에서는
    - 아무것도 선택하지 않음
    - 윗 열을 선택함
    - 아래 열을 선택함

  세가지의 경우의 수 존재

- 상태값을 어떤 방향으로 전이할 것인지
  - => 양옆 위아래 스티커가 비활성화 되므로 자연스럽게 행동 선택은 열 방향으로 전개됨

---

## 백준 문제: 스티커 (silver I / 9465)

```python
import sys

n = int(sys.stdin.readline().rstrip())

for _ in range(n):
    column = int(sys.stdin.readline().rstrip())
    sticker = []
    for i in range(2):
        l = list(map(int, sys.stdin.readline().rstrip().split(" ")))
        sticker.append(l)

    process = [[0, sticker[0][0], sticker[1][0]]]

    for j in range(1, column):
        idx0 = max(process[j-1]) # 아무것도 선택 안함
        idx1 = sticker[0][j] + max([process[j-1][0], process[j-1][2]]) # 윗 열 선택
        idx2 = sticker[1][j] + max([process[j-1][0], process[j-1][1]]) # 아래 열 선택

        process.append([idx0, idx1, idx2])

    print(max(process[-1]))
```
