# 1292. Maximum Side Length of a Square with Sum ≤ Threshold

## Problem Summary
Given a matrix and a number `threshold`, we need to find the **largest square sub-matrix** whose total sum is **less than or equal to the threshold**.  
If no such square exists, return `0`.

---

## Intuition
The main challenge is calculating the sum of many square sub-matrices efficiently.  
Recomputing sums every time would be too slow.

To solve this, I used a **2D Prefix Sum**, which allows calculating the sum of any square in constant time.

---

## Approach
1. Build a 2D prefix sum matrix `P`, where each cell stores the sum of elements above and to the left.
2. Iterate through the matrix treating each cell as a possible **bottom-right corner** of a square.
3. Try to increase the square size greedily:
   - If a square of size `k` works, try `k + 1`
4. Use the prefix sum formula to check if the square sum is within the threshold.

---

## Time Complexity
- Building prefix sum: `O(m × n)`
- Checking all positions: `O(m × n)`

**Overall:** `O(m × n)`

---

## Space Complexity
- Prefix sum matrix storage

**Overall:** `O(m × n)`

---

## Code

```python
class Solution(object):
    def maxSideLength(self, mat, threshold):
        m, n = len(mat), len(mat[0])
        
        P = [[0] * (n + 1) for _ in range(m + 1)]
        
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                P[i][j] = (
                    P[i-1][j] + P[i][j-1]
                    - P[i-1][j-1] + mat[i-1][j-1]
                )
        
        max_side = 0
        
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                side = max_side + 1
                if i >= side and j >= side:
                    square_sum = (
                        P[i][j]
                        - P[i-side][j]
                        - P[i][j-side]
                        + P[i-side][j-side]
                    )
                    if square_sum <= threshold:
                        max_side = side
                        
        return max_side
