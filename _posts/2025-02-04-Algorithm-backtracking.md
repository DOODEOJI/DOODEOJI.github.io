---
layout: post
title:  "[Algorithm][Backtracking] 백트래킹 (되돌아가기)"
date:   2025-02-04
categories: [백준, Algorithm]
tags: [backtracking, recursive function, BOJ-15649]
---

## Backtracking
- 기본 정의는 재귀 호출 트리에서 가볼 필요가 없는 가지는 쳐낸다
- 상태 리스트를 두고 해당 리스트로 방문 필요성을 판단

### 예시: 수열 만들기

`[1, 2, 3]`으로 가능한 조합의 수열을 만든다고 할 때,

```
arr = []
visited = [False] * 4
```

- `[n-m]` 안의 `n`은 호출된 함수 깊이, `m`은 for문에서의 인덱스 `i`

1. `[1-1]` 1을 선택한 후 `visited[1] = True`
2. `[2-2]` 재귀 호출하여 1은 건너뛰고 2를 선택한 후 `visited[2] = True`
3. `[3-3]` 재귀 호출하여 1, 2는 건너뛰고 3을 선택한 후 `visited[3] = True`
4. `[4]` return 조건 `(len == 3)`을 충족하였으므로 return
   - `visited = [F, T, T, T]`
   - `arr = [1, 2, 3]`

5. `[3-3]` 3을 pop 한 후 `visited[3] = False`
6. `[2-2]` 2를 pop 한 후 `visited[2] = False`
7. `[2-3]` `visited[3]`이 False이므로 3을 선택한 후 `visited[3] = True`
8. `[3-2]` 재귀 호출하여 1은 건너뛰고 2를 선택한 후 `visited[2] = True`
   - `visited = [F, T, T, T]`
   - `arr = [1, 3, 2]`

9. `[4]` return 조건 `(len == 3)`을 충족하였으므로 return
10. `[3-2]` 2를 pop 한 후 `visited[2] = False`
11. `[3-3]` pass
12. `[2-3]` 3을 pop 한 후 `visited[3] = False`
13. `[1-1]` 1을 pop 한 후 `visited[1] = False`
    - `visited = [F, F, F, F]`
    - `arr = []`

14. `[1-2]` 2을 선택한 후 `visited[2] = True`
15. `[2-2]` 재귀 호출하여 1을 선택하여 `visited[1] = True`
16. ...

다음과 같이 방금 선택했던 상태를 True로 전환

## 참고 강의
- [바킹독의 실전 알고리즘 0x0C강 - 백트래킹](https://www.youtube.com/watch?v=Enz2csssTCs)

---

## 백준 문제: N과 M (1) (silver III / 15649)

```python
import sys

N, M = map(int, sys.stdin.readline().rstrip().split(" "))

arr = []
visited = [False] * (N+1)

def backtracking():
    if len(arr) == M:
        print(*arr)
        return
    
    for i in range(1, N+1):
        if not visited[i]:
            visited[i] = True
            arr.append(i)
            backtracking()
            arr.pop()
            visited[i] = False

backtracking()
```