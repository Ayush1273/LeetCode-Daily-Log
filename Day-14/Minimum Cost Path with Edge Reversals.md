# Minimum Cost Path with Edge Reversals

## Problem Overview

Find the minimum cost to travel from node `0` to node `n - 1`.

### Allowed Moves

- **Standard Move**  
  Traverse a directed edge `u → v` with cost `w`.

- **Reversal Move**  
  At any node `u`, you can use a **one-time switch** to reverse an incoming edge  
  `v → u` and traverse it immediately as `u → v` with a cost of `2 × w`.

---

## Approach: State-Augmented Dijkstra

Since the cost of reversing an edge (`2w`) is always higher than the original cost (`w`) and all weights are positive, we can safely apply **Dijkstra’s Algorithm** on an augmented graph.

---

## Graph Construction

For every edge `[u, v, w]`:

- Add a directed edge `u → v` with weight `w`
- Add a directed edge `v → u` with weight `2w`  
  (represents reversing the edge using the switch at node `v`)

---

## Shortest Path Logic

Run Dijkstra’s algorithm starting from node `0`.

### Constraint Handling

Although each node’s switch can only be used once:

- All edge weights are positive
- Any attempt to reuse a switch via a cycle would increase the total cost
- Dijkstra naturally avoids suboptimal cycles

Therefore, a **standard distance array `dist[n]` is sufficient**, and no extra state is required.

---

## Complexity Analysis

- **Time Complexity:**  
  `O((E + V) log V)` using a priority queue

- **Space Complexity:**  
  `O(E + V)` for the adjacency list and distance array

---

## Implementation (Python)

```python
import heapq

class Solution(object):
    def minCost(self, n, edges):
        # Build adjacency list with original and reversed options
        adj = [[] for _ in range(n)]
        for u, v, w in edges:
            adj[u].append((v, w))      # Normal edge
            adj[v].append((u, 2 * w))  # Reversed edge (using switch at v)
            
        # Dijkstra's Algorithm
        dist = [float('inf')] * n
        dist[0] = 0
        pq = [(0, 0)]  # (cost, node)
        
        while pq:
            curr_d, u = heapq.heappop(pq)
            
            if curr_d > dist[u]:
                continue
            if u == n - 1:
                return curr_d
                
            for v, weight in adj[u]:
                if curr_d + weight < dist[v]:
                    dist[v] = curr_d + weight
                    heapq.heappush(pq, (dist[v], v))
                    
        return dist[n - 1] if dist[n - 1] != float('inf') else -1
