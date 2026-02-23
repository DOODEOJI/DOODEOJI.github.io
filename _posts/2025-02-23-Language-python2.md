---
layout: post
title:  "[Language][Python] 거듭제곱 내장함수"
date:   2025-02-23
categories: [백준, Language]
tags: [python, pow, BOJ-1629]
---

## Python 거듭제곱 내장함수

### pow(base, exp, mod = None)
- 나머지 연산이 기본 파라미터로 존재함

---

## 백준 문제: 곱셈 (silver I / 1629)

```python
import sys

a, b, c = map(int, sys.stdin.readline().rstrip().split(" "))

print(pow(a,b,c))
```
