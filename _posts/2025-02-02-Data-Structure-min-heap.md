---
layout: post
title:  "[Data Structure][Heap] 최소 힙"
date:   2025-02-02
categories: [백준, Data-Structure]
tags: [Min Heap, BOJ-1927]
---

## Min Heap (최소 힙)

### 핵심 개념
- **삽입 (add)**: 맨 끝에 추가 후 `sift_up`으로 부모와 비교하며 위로 이동
- **삭제 (pop)**: 루트와 마지막 요소 교환 후 `sift_down`으로 자식과 비교하며 아래로 이동
- Python `heapq` 모듈 사용하면 간편하게 구현 가능

---

## 백준 문제: 최소 힙 (Silver II / 1927)

```python
import sys

num = int(sys.stdin.readline().rstrip())

class Min_Heap:
    def __init__(self):
        self.heap = []

    def add(self, value):
        self.heap.append(value)
        self.sift_up()

    def pop(self):
        self.heap[0], self.heap[-1] = self.heap[-1], self.heap[0]
        temp = self.heap.pop()
        self.sift_down()
        return temp

    def sift_up(self):
        if len(self.heap) == 1:
            return True
        idx = len(self.heap) - 1
        while True:
            if idx == 0:
                break
            if self.heap[idx] < self.heap[(idx - 1) // 2]:
                self.heap[idx], self.heap[(idx - 1) // 2] = self.heap[(idx - 1) // 2], self.heap[idx]
                idx = (idx - 1) // 2
            else:
                break

    def sift_down(self):
        n = len(self.heap)
        idx = 0
        while True:
            left = 2 * idx + 1
            right = 2 * idx + 2
            smallest = idx

            if left < n and self.heap[left] < self.heap[smallest]:
                smallest = left
            if right < n and self.heap[right] < self.heap[smallest]:
                smallest = right

            if smallest == idx:
                break

            self.heap[idx], self.heap[smallest] = self.heap[smallest], self.heap[idx]
            idx = smallest

mh = Min_Heap()

for _ in range(num):
    n = int(sys.stdin.readline().rstrip())
    if n > 0:
        mh.add(n)
    elif n == 0:
        if mh.heap:
            sys.stdout.write(str(mh.pop()) + "\n")
        else:
            sys.stdout.write("0\n")
```
