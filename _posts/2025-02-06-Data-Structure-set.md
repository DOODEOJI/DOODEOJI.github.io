---
layout: post
title:  "[Data Structure][Set] 중복 없는 ordered list"
date:   2025-02-06
categories: [백준, Data-Structure]
tags: [set, BOJ-15663]
---

## 중복 없이 ordered list 만들기

- Set 자료형은 중복은 없애되, hash 방식을 사용하여 순서를 보장하지 않음

## Seen, Ordered 두 객체 활용
- `Set 객체(Seen)`를 통해 중복 확인 후 중복되지 않으면 `List 객체(Ordered)`에 `append`
- 또한, 중복 체크하려는 객체가 복수일 때는 `tuple `형으로 넣어야 함 (list는 hashable X)

---

## 백준 문제: N과 M (9) (silver II / 15663)

```python
import sys

N, M = map(int, sys.stdin.readline().rstrip().split(" "))
n_list = sorted(list(map(int, sys.stdin.readline().rstrip().split(" "))))

arr = []
seen = set()
ordered = []

visited = [False] * (N+1)

def backtracking():
    if len(arr) == M:
        t = tuple(arr)
        if t not in seen:
            seen.add(t)
            ordered.append(arr.copy())
        return
    
    for i in range(1, N+1):
        if not visited[i]:
            visited[i] = True
            arr.append(n_list[i-1])
            backtracking()
            arr.pop()
            visited[i] = False

backtracking()
for i in ordered:
    print(*i)
```