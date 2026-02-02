---
layout: post
title:  "[Algorithm][Dynamic Programming] BFS에서 Visited 활용"
date:   2025-02-01
categories: [백준, Algorithm]
tags: [DP, BFS]
---

## Dynamic Programming with BFS

### 핵심 개념
- **BFS Memoization**: `visited` 배열을 활용하여 이미 방문한 노드 재탐색 방지
- 불필요한 중복 탐색을 줄여 시간 복잡도 개선
- 최단 경로/최소 연산 횟수 문제에 효과적

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
