---
layout: post
title:  "[Algorithm][Grid Graph] 격자 그래프 탐색"
date:   2025-03-05
categories: [백준, Algorithm]
tags: [Grid Graph, BFS, BOJ-2206]
---

## 격자 그래프 탐색 핵심 정리

- **격자 그래프(Grid Graph)**: 0과 1로 이루어진 2차원 배열 형태의 그래프
  - 0: 이동 가능한 칸 / 1: 벽(이동 불가)
- BFS를 사용해 최단 거리를 구하는 문제에 자주 사용됨
- **방향 벡터** `dx, dy`를 사용하면 상하좌우 이동을 간결하게 처리 가능

## 알고리즘 동작

1. 시작점 `(0, 0)`에서 BFS 시작, 도착점 `(n-1, m-1)` 까지 최단 거리 탐색
2. 큐에서 꺼낸 좌표에서 상하좌우 4방향으로 인접 칸 탐색
3. 범위를 벗어나지 않고, 이동 가능한 칸이며, 아직 방문하지 않은 경우 큐에 추가
4. 도착점 도달 시 현재 거리 출력, 큐가 비어도 도달 못하면 `-1` 출력

## 3차원 visited (상태 추가)

- 일반적인 격자 탐색: `visited[x][y]` → 2차원으로 충분
- **이 문제처럼 조건이 추가되면 visited를 3차원으로 확장**: `visited[x][y][상태]`
  - 상태가 다르면 같은 좌표를 다시 방문해야 할 수 있기 때문
  - 이 문제에서는 **벽을 부순 적 있는지(0/1)** 가 상태
  - `visited[x][y][0]`: 벽을 부수지 않은 상태로 방문 여부
  - `visited[x][y][1]`: 벽을 부순 상태로 방문 여부

---

## 백준 문제: 벽 부수고 이동하기 (gold III / 2206)

```python
import sys
from collections import deque

n, m = map(int, sys.stdin.readline().rstrip().split(" "))
matrix = []

for _ in range(n):
    m_list = list(sys.stdin.readline().rstrip())
    temp = []
    for i in range(m):
        temp.append(int(m_list[i]))
    matrix.append(temp)

# if문 사용 버전
process = deque([(0, 0, 0, 1)])
# 이전에 벽이 부서진 상태에서 visit 한건지 구분이 필요하므로
# 3차원으로 구성해야함
visited = [[[0]*2 for _ in range(m)] for _ in range(n)]

while True:
    if not process:
        print(-1)
        break
    p = process.popleft()

    if p[0] == n-1 and p[1] == m-1:
        print(p[3])
        break
    break_wall = p[2]
    
    if visited[p[0]][p[1]][break_wall]:
        continue
    visited[p[0]][p[1]][break_wall] = 1

    dist = p[3]

    if p[0] > 0 and not matrix[p[0]-1][p[1]]:
        process.append((p[0]-1, p[1], break_wall, dist + 1))

    elif p[0] > 0 and matrix[p[0]-1][p[1]] and not break_wall:
        process.append((p[0]-1, p[1], 1, dist + 1))

    if p[0] < n-1 and not matrix[p[0]+1][p[1]]:
        process.append((p[0]+1, p[1], break_wall, dist + 1))

    elif p[0] < n-1 and matrix[p[0]+1][p[1]] and not break_wall:
        process.append((p[0]+1, p[1], 1, dist + 1))

    if p[1] > 0 and not matrix[p[0]][p[1]-1]:
        process.append((p[0], p[1]-1, break_wall, dist + 1))

    elif p[1] > 0 and matrix[p[0]][p[1]-1] and not break_wall:
        process.append((p[0], p[1]-1, 1, dist + 1))

    if p[1] < m-1 and not matrix[p[0]][p[1]+1]:
        process.append((p[0], p[1]+1, break_wall, dist + 1))

    elif p[1] < m-1 and matrix[p[0]][p[1]+1] and not break_wall:
        process.append((p[0], p[1]+1, 1, dist + 1))


# 방향 벡터 사용 버전

dx = [-1, 1, 0, 0] # 상하좌우
dy = [0, 0, -1, 1]

process = deque([(0, 0, 0, 1)])
visited = [[[0]*2 for _ in range(m)] for _ in range(n)]

while True:
    if not process:
        print(-1)
        break

    x, y, wall, dist = process.popleft()

    if x == n-1 and y == m-1:
        print(dist)
        break
    
    for i in range(4):
        nx = x + dx[i]
        ny = y + dy[i]

        if 0 <= nx < n and 0 <= ny < m:
            if not matrix[nx][ny] and not visited[nx][ny][wall]:
                visited[nx][ny][wall] = 1
                process.append((nx, ny, wall, dist+1))

            if matrix[nx][ny] and not wall and not visited[nx][ny][wall]:
                visited[nx][ny][1] = 1
                process.append((nx, ny, 1, dist+1))
```
