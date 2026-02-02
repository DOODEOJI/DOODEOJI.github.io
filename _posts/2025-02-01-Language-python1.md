---
layout: post
title:  "[Language][Python] 아스키 코드 변환"
date:   2025-02-01
categories: [백준, Language]
tags: [python, chr, ord]
---

## Python 아스키 코드 변환

### 핵심 함수
- **`ord(문자)`**: 문자 → 아스키 코드 (숫자)
- **`chr(숫자)`**: 아스키 코드 → 문자

### 자주 쓰는 아스키 값
| 문자 | 아스키 코드 |
|:---:|:---:|
| `'a'` | 97 |
| `'z'` | 122 |
| `'A'` | 65 |
| `'Z'` | 90 |
| `'0'` | 48 |

---

## 백준 문제: 알파벳 찾기 (Bronze II / 10809)

```python
import sys

word = sys.stdin.readline().rstrip()
alphabet = [-1 for _ in range(26)]

for i in range(len(word)):
    if alphabet[ord(word[i]) - 97] == -1:
        alphabet[ord(word[i]) - 97] = i

print(*alphabet)
```
