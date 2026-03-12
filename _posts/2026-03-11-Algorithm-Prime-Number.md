---
layout: post
title:  "[Algorithm][Prime Number] 소수 구하기 (에라토스테네스의 체)"
date:   2026-03-11
categories: [백준, Algorithm]
tags: [Prime Number]
---

## 소수 구하기 (에라토스테네스의 체)

### 핵심 개념

**소수(Prime Number)** 란 1과 자기 자신만을 약수로 가지는 1보다 큰 자연수다.

소수를 판별하는 가장 기본적인 방법은 2부터 `n-1`까지 나누어 보는 것이지만, 이를 개선하면 **√n까지만 확인**해도 충분하다.

> **증명**: n이 합성수라면 n = A × B (A ≤ B) 로 나타낼 수 있다.
>
> A ≤ B 이므로, A × A ≤ A × B = n
>
> ∴ A² ≤ n → **A ≤ √n**
>
> 즉, 두 약수 중 작은 쪽(A)은 반드시 √n 이하이므로, √n까지만 나누어 보면 충분하다.

### 코드

```kotlin
import kotlin.math.sqrt

fun main() {
    println("Prime number under 100 is ..")
    for (i in 2..100) {
        val num = kotlin.math.sqrt(i.toDouble()).toInt()
        var is_prime = 1
        for (j in 2..num) {
            if (i % j == 0) {
                is_prime = 0
                break
            }
        }
        if (is_prime == 1) {
            println("$i is prime number")
        }
    }
}
```