# 2976. Minimum Cost to Convert String I

## Problem
You are given two strings `source` and `target` of equal length.  
You can transform characters using given rules where converting  
`original[i] → changed[i]` costs `cost[i]`.

You may apply **any number of transformations**, including intermediate characters.  
Return the **minimum total cost** to convert `source` into `target`, or `-1` if impossible.

---

## Approach

There are only **26 lowercase letters**, so we model the problem as a **graph**:

- Each character = a node
- Transformation `x → y` = directed edge with weight `cost`
- Multiple transformations may exist between the same characters

We first compute the **minimum cost to convert any character to any other character** using  
**Floyd–Warshall** on a `26 × 26` matrix.

Then:
- For each index `i`
  - If `source[i] == target[i]`, cost = `0`
  - Otherwise add the shortest conversion cost
- If any conversion is impossible, return `-1`

---

## Complexity Analysis

- **Time Complexity**:  `O(n)`

---

## Python Code

```python
class Solution(object):
    def minimumCost(self, source, target, original, changed, cost):
        INF = 10**18
        N = 26

        # dist[u][v] = minimum cost to convert u -> v
        dist = [[INF] * N for _ in range(N)]
        for i in range(N):
            dist[i][i] = 0

        # Build graph
        for o, c, w in zip(original, changed, cost):
            u = ord(o) - ord('a')
            v = ord(c) - ord('a')
            dist[u][v] = min(dist[u][v], w)

        # Floyd-Warshall
        for k in range(N):
            for i in range(N):
                for j in range(N):
                    if dist[i][k] + dist[k][j] < dist[i][j]:
                        dist[i][j] = dist[i][k] + dist[k][j]

        # Calculate total cost
        ans = 0
        for s, t in zip(source, target):
            if s == t:
                continue
            u = ord(s) - ord('a')
            v = ord(t) - ord('a')
            if dist[u][v] == INF:
                return -1
            ans += dist[u][v]

        return ans
