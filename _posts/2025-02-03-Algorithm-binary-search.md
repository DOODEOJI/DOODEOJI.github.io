---
layout: post
title:  "[Algorithm][Binary Search] 이진 탐색"
date:   2025-02-03
categories: [백준, Algorithm]
tags: [Binary Search, Binary Jumping, BOJ-2805]
---

## Binary Search (이진 탐색)

### 참고 링크
- [이분 탐색 (Binary Search) 헷갈리지 않게 구현하기](https://www.acmicpc.net/blog/view/109)

---

### Binary Jumping Method

> dohoon 님의 댓글 참고

binary jumping 형식의 이진 탐색도 소개하면 어떨까요? 빠르게 구현할 수 있다는 장점이 있고, 이상과 초과의 경계를 구분하는 것이 쉽다고 생각합니다.

[lo,hi]에 대한 binary jumping 구현입니다:

```c
int x = lo-1;
for (int b = hi; b >= 1; b >>= 1)
    while (x+b <= hi and f(x+b)) x += b;
```

만약 최종 x가 lo-1 라면 범위 내에 true가 존재하지 않는다는 의미가 됩니다.

파라메트릭 서치로 구하는 위치는 4가지 뿐인데, 아래 표처럼 수정해서 사용하면 됩니다.

| Case | 패턴 | 조건 | 최종 위치 |
|:---:|:---:|:---:|:---:|
| 1 | `0...*0* 1...1` | `not f(x+b)` | x |
| 2 | `0...0 *1*...1` | `not f(x+b)` | x+1 |
| 3 | `1...*1* 0...0` | `f(x+b)` | x |
| 4 | `1...1 *0*...0` | `f(x+b)` | x+1 |

**단점**: 범위 내 모든 원소를 구성할 수 있어서 성립하지만 이에 대한 증명은 어렵습니다.

**구현상 주의점**: x+b의 범위를 따져봐야 합니다. 정수 오버플로우가 나면 사태가 심각해집니다.

---

## 백준 문제: 나무 자르기 (Silver II / 2805)

### 초기 구현

```python
import sys

n, tree_h = map(int, sys.stdin.readline().rstrip().split(" "))
tree_list = list(map(int, sys.stdin.readline().rstrip().split(" ")))
tree_list.sort(reverse = True)

cnt = 0
tsum = 0
idx = 0

for i in range(tree_list[0], -1, -1):
    tsum += cnt

    if tsum >= tree_h:
        break

    while (len(tree_list) - idx > 0):
        if i <= tree_list[idx]:
            cnt += 1
            idx += 1
        else:
            break

print(i)
```

### 이진 탐색으로 구현

```python
import sys

n, tree_h = map(int, sys.stdin.readline().rstrip().split(" "))
tree_list = list(map(int, sys.stdin.readline().rstrip().split(" ")))

def check(pivot):
    tsum = 0
    for i in tree_list:
        tsum += max(0, i - pivot)
    return tsum >= tree_h

i = 0
j = max(tree_list)
ans = 0

while (j >= i):
    mid = (i+j) // 2

    if check(mid):
        ans = mid
        i = mid + 1
    else:
        j = mid - 1
print(ans)
```
