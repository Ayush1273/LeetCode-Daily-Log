# Minimum Cost Path with Teleportations

## Problem Brief
You are given an `m × n` grid. Starting from the top-left cell `(0,0)`, reach the bottom-right cell `(m−1,n−1)` with minimum cost.

### Moves
- **Normal move**: Right or Down  
  Cost = value of the destination cell
- **Teleport**: Jump to any cell `(x, y)` such that  
  `grid[x][y] ≤ grid[i][j]`  
  Cost = `0`, usable at most `k` times

---

## Approach
This is a **shortest path problem with state**.

### State
(i, j, t)

- `(i, j)` → current cell
- `t` → teleports used (`0 ≤ t ≤ k`)

We apply **Dijkstra’s algorithm** on this state space.

### Optimization for Teleport
- Pre-sort all cells by value
- For each teleport layer `t`, use a pointer over sorted cells
- When a cell with value `V` is reached, all cells with value `≤ V` are unlocked
- Each cell is processed **once per teleport layer**

This avoids trying all teleport destinations repeatedly.

---

## Complexity
- **Time**: `O((m × n × k) log (m × n × k))`
- **Space**: `O(m × n × k)`

---

## Python Code

```python
import heapq

class Solution(object):
    def minCost(self, grid, k):
        m, n = len(grid), len(grid[0])
        INF = 10**18

        dist = [[[INF] * (k + 1) for _ in range(n)] for __ in range(m)]
        dist[0][0][0] = 0

        pq = [(0, 0, 0, 0)]

        cells = []
        for i in range(m):
            for j in range(n):
                cells.append((grid[i][j], i, j))
        cells.sort()

        ptr = [0] * (k + 1)

        while pq:
            cost, x, y, t = heapq.heappop(pq)
            if cost != dist[x][y][t]:
                continue

            if x == m - 1 and y == n - 1:
                return cost

            if x + 1 < m:
                nc = cost + grid[x + 1][y]
                if nc < dist[x + 1][y][t]:
                    dist[x + 1][y][t] = nc
                    heapq.heappush(pq, (nc, x + 1, y, t))

            if y + 1 < n:
                nc = cost + grid[x][y + 1]
                if nc < dist[x][y + 1][t]:
                    dist[x][y + 1][t] = nc
                    heapq.heappush(pq, (nc, x, y + 1, t))

            if t < k:
                while ptr[t] < len(cells) and cells[ptr[t]][0] <= grid[x][y]:
                    _, nx, ny = cells[ptr[t]]
                    ptr[t] += 1
                    if cost < dist[nx][ny][t + 1]:
                        dist[nx][ny][t + 1] = cost
                        heapq.heappush(pq, (cost, nx, ny, t + 1))

        return -1
