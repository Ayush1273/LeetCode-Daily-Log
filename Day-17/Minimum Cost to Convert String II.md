# 2977. Minimum Cost to Convert String II

## Problem Description
You are given two strings `source` and `target` of length $n$. You are also given arrays `original`, `changed`, and `cost`, which define rules for converting a substring $x$ to $y$ at a cost $z$. 

Rules:
1. Substrings converted must be **disjoint** (no overlapping indices).
2. Multiple operations can be performed on the **identical** range (e.g., $A \to B \to C$ for the same substring).
3. Goal: Find the minimum cost to convert `source` to `target`.

## Approach
The problem can be broken down into two main parts: finding the shortest path between string transformations and solving for the optimal partitioning of the source string.

### 1. Pre-calculating Transformation Costs
Since we can chain transformations (e.g., if "abc" $\to$ "def" costs 5 and "def" $\to$ "ghi" costs 3, then "abc" $\to$ "ghi" costs 8), we treat the strings as nodes in a graph.
- **Mapping:** Map every unique string in `original` and `changed` to an integer ID.
- **Shortest Path:** Use the **Floyd-Warshall algorithm** to compute the all-pairs shortest path between all string IDs. This handles the "identical indices" rule.

### 2. Dynamic Programming
To handle the "disjoint indices" rule, we use DP. Let $dp[i]$ be the minimum cost to convert the prefix `source[0...i-1]` to `target[0...i-1]`.

- **Base Case:** $dp[0] = 0$.
- **Transition 1 (No change):** If `source[i] == target[i]`, then $dp[i+1] = \min(dp[i+1], dp[i])$.
- **Transition 2 (Substring change):** For every possible length $L$ that exists in the `original` array, check if `source[i:i+L]` can be converted to `target[i:i+L]`.
  - If a path exists in our graph: $dp[i+L] = \min(dp[i+L], dp[i] + \text{dist}[source\_id][target\_id])$.

## Complexity Analysis
- **Time Complexity:** $O(M^3 + N \cdot L)$
   
- **Space Complexity:** $O(M^2 + N)$ to store the distance matrix and the DP array.

## Code Implementation

```python
class Solution(object):
    def minimumCost(self, source, target, original, changed, cost):
        # 1. Map all unique strings to IDs for the graph
        nodes = list(set(original) | set(changed))
        node_id = {s: i for i, s in enumerate(nodes)}
        m = len(nodes)
        
        # 2. Initialize distance matrix for Floyd-Warshall
        dist = [[float('inf')] * m for _ in range(m)]
        for i in range(m):
            dist[i][i] = 0
            
        for o, c, z in zip(original, changed, cost):
            u, v = node_id[o], node_id[c]
            dist[u][v] = min(dist[u][v], z)
            
        # 3. All-pairs shortest paths
        for k in range(m):
            for i in range(m):
                if dist[i][k] == float('inf'): continue
                for j in range(m):
                    if dist[i][j] > dist[i][k] + dist[k][j]:
                        dist[i][j] = dist[i][k] + dist[k][j]
        
        # 4. DP for total string conversion
        n = len(source)
        dp = [float('inf')] * (n + 1)
        dp[0] = 0
        
        # Optimization: only check lengths that exist in rules
        lengths = sorted(list(set(len(s) for s in original)))
        
        for i in range(n):
            if dp[i] == float('inf'):
                continue
            
            # Option 1: Characters are already the same, move forward with 0 cost
            if source[i] == target[i]:
                dp[i+1] = min(dp[i+1], dp[i])
            
            # Option 2: Convert a substring starting at index i
            for l in lengths:
                if i + l > n:
                    break
                
                sub_s = source[i:i+l]
                sub_t = target[i:i+l]
                
                if sub_s in node_id and sub_t in node_id:
                    u, v = node_id[sub_s], node_id[sub_t]
                    if dist[u][v] != float('inf'):
                        dp[i+l] = min(dp[i+l], dp[i] + dist[u][v])
                        
        return dp[n] if dp[n] != float('inf') else -1
