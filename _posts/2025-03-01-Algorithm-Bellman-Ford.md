---
layout: post
title:  "[Algorithm][Bellman Ford] 최단 거리 찾기 (벨만 포드)"
date:   2025-03-01
categories: [백준, Algorithm]
tags: [Bellman-Ford, BOJ-1865]
---

## 벨만 포드 핵심 정리

- 최단 경로 찾기 알고리즘
- 다익스트라와 다르게 **음수 간선이 포함되어도** 최단 거리를 찾을 수 있음
- 정점이 `N`개일 때 최단 경로는 최대 `N-1`개의 간선으로 이루어짐 → 간선 가중치 갱신을 `N-1`번 반복
- `N`번째 반복에서도 값이 갱신된다면 **음수 사이클** 존재

## 알고리즘 동작

1. 시작 정점의 dist는 0, 나머지는 INF로 초기화
2. 모든 간선에 대해 relaxation 반복 (`N-1`번)
   - `dist[next] > dist[current] + weight`이면 갱신
   - `dist[current] != INF` 조건 필요 (아직 방문 안 된 정점의 이웃은 갱신하지 않음)
3. `N`번째 반복에서 갱신되면 음수 사이클 존재 → 최단 거리 정의 불가

## 음수 사이클 탐지 변형 (아래 문제에서 활용)

- **시작점 없이 음수 사이클 유무만 확인**할 경우 dist를 전부 `0`으로 초기화
  - 모든 정점에서 동시에 출발하는 것과 동일한 효과
  - 음수 사이클이 있으면 `N`번째 가중치 갱신에서 반드시 값이 갱신됨
  - `dist[current] != INF` 조건 불필요 (모두 0에서 시작하므로)
  - 모든 정점 간 연결되어 있지 않은 경우 (비연결 그래프) 활용할 수 있음


## 참고 영상 (코드 참조)
- [[동빈나] 코딩 테스트를 위한 벨만 포드 알고리즘 7분 핵심 요약](https://www.youtube.com/watch?v=Ppimbaxm8d8)

---

## 백준 문제: 웜홀 (gold III / 1865)

```python
import sys

TC = int(sys.stdin.readline().rstrip())

def bf():
    dist = [0 for _ in range(N)]

    for i in range(N):
        for j in edges:
            current = j[0]
            next_node = j[1]
            weight = j[2]

            if dist[next_node] > dist[current] + weight:
                dist[next_node] = dist[current] + weight
                if i == N-1:
                    return True
    return False


for _ in range(TC):
    N, M, W = map(int, sys.stdin.readline().rstrip().split(" "))
    edges = []

    for _ in range(M):
        s, e, t = map(int, sys.stdin.readline().rstrip().split(" "))
        edges.append((s-1, e-1,t))
        edges.append((e-1, s-1,t))

    for _ in range(W):
        s, e, t = map(int, sys.stdin.readline().rstrip().split(" "))
        edges.append((s-1, e-1, -1*t))

    negative_cycle = bf()

    if negative_cycle:
        print("YES")

    else:
        print("NO")

```
