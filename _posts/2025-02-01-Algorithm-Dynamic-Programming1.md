---
layout: post
title:  "[Algorithm][Dynamic Programming] BFS - Visited, DFS - Memoization"
date:   2025-02-01
categories: [백준, Algorithm]
tags: [DP, BFS, DFS]
---

## Dynamic Programming with BFS

### 핵심 개념
- **BFS Memoization**: `visited` 배열을 활용하여 이미 방문한 노드 재탐색 방지
- 불필요한 중복 탐색을 줄여 시간 복잡도 개선
- 최단 경로/최소 연산 횟수 문제에 효과적

---

---

## Dynamic Programming with DFS

### 핵심 개념
- **DFS Memoization**: 딕셔너리/배열에 계산 결과를 저장하여 중복 계산 방지
- 재귀 호출 시 이미 계산한 값이면 바로 반환 → O(2^N) → O(N)으로 개선
- 점화식이 명확한 문제에 효과적 (피보나치, 타일링 등)
- (ex) f(n) = f(n-1) + f(n-2)

---



## 백준 문제: 1로 만들기 (Silver III / 1463)

```python
import sys

class Tree:
    def __init__(self, value):
        self.root = value
        self.level = None
        self.one = None
        self.two = None
        self.three = None

n = int(sys.stdin.readline().rstrip())
t = Tree(n)

process_list = []
process_list.append(t)
t.level = 0
visited = []

while process_list:
    p = process_list.pop(0)
    if p.root in visited:
        continue
    visited.append(p.root)

    if p.root == 1:
        print(p.level)
        break

    if p.root % 2 == 0:
        p.two = Tree(p.root / 2)
        p.two.level = p.level + 1
        process_list.append(p.two)

    if p.root % 3 == 0:
        p.three = Tree(p.root / 3)
        p.three.level = p.level + 1
        process_list.append(p.three)

    p.one = Tree(p.root - 1)
    p.one.level = p.level + 1
    process_list.append(p.one)
```

## 백준 문제: 2xn 타일링 (Silver III / 11726)

```python
import sys

memo = {}

def process(t):
    if t in memo:
        return memo[t]

    if t == 0:
        return 0
    
    elif t == 1:
        return 1
    
    elif t == 2:
        return 2
    
    else:
        memo[t] = process(t-1) + process(t-2)

    return memo[t]

n = int(sys.stdin.readline().rstrip())
k = process(n) % 10007

print(k)
```