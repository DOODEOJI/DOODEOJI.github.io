---
layout: post
title:  "[Algorithm][GCD] 최대공약수 (Greatest Common Divisor (GCD)) (유클리드 호제법)"
date:   2026-03-08
categories: [백준, Algorithm]
tags: [GCD, BOJ-2609]
---

## 최대공약수 구하기 (유클리드 호제법)

### 핵심 개념

- 큰 수를 작은 수로 나눈 나머지로 이전의 작은 수를 나누는 과정을 반복하여, 나머지가 0이 될 때까지 진행했을 때 나누는 수(나머지가 0이 되기 바로 전의 나머지)가 최대공약수다.

### 예시

1071과 1029의 최대공약수를 구하면,

1. 1071은 1029로 나누어 떨어지지 않기 때문에, 1071을 1029로 나눈 나머지를 구한다. → 42
2. 1029는 42로 나누어 떨어지지 않기 때문에, 1029를 42로 나눈 나머지를 구한다. → 21
3. 42는 21로 나누어 떨어진다.
4. 따라서, 최대공약수는 **21**이다.

## 링크
- [위키백과 - 유클리드 호제법](https://ko.wikipedia.org/wiki/%EC%9C%A0%ED%81%B4%EB%A6%AC%EB%93%9C_%ED%98%B8%EC%A0%9C%EB%B2%95)


---

## 백준 문제: 최대공약수와 최소공배수 (bronze I / 2609)

```python
import sys

a, b = map(int, sys.stdin.readline().rstrip().split(" "))

if a > b:
    x = a
    y = b

else:
    x = b
    y = a

remain = 1

while remain:
    remain = x % y
    x = y
    y = remain

print(x)
print(int((a/x * b/x)*x))

```