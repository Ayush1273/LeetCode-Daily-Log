# 1895. Largest Magic Square

## Problem Summary
A **magic square** is a `k x k` grid where:
- All row sums are equal
- All column sums are equal
- Both diagonal sums are equal

Given a grid, we need to find the **largest possible magic square** inside it and return its side length.

---

## Intuition
Since the grid size is small (`≤ 50`), we can check all possible square sizes.

The main challenge is calculating row and column sums efficiently.  
To solve this, I used **prefix sums**, which allow sum queries in constant time.

---

## Approach
1. Precompute prefix sums for:
   - Rows
   - Columns
2. Start checking square sizes from **largest to smallest**.
3. For each possible `k x k` square:
   - Use prefix sums to verify all rows and columns
   - Directly check both diagonals
4. The first valid square found is the answer.

---

## Time Complexity
Let `m` = rows and `n` = columns.

- Checking all sizes and positions: `O(m · n · min(m, n)²)`

**Overall:** `O(m · n · min(m, n)²)`

---

## Space Complexity
- Prefix sum arrays for rows and columns

**Overall:** `O(m · n)`

---

## Code

```python
class Solution(object):
    def largestMagicSquare(self, grid):
        rows, cols = len(grid), len(grid[0])
        
        rowSum = [[0] * (cols + 1) for _ in range(rows)]
        colSum = [[0] * cols for _ in range(rows + 1)]
        
        for r in range(rows):
            for c in range(cols):
                rowSum[r][c+1] = rowSum[r][c] + grid[r][c]
                colSum[r+1][c] = colSum[r][c] + grid[r][c]
        
        def isMagic(r, c, k):
            target = rowSum[r][c+k] - rowSum[r][c]
            
            for i in range(r, r+k):
                if rowSum[i][c+k] - rowSum[i][c] != target:
                    return False
            
            for j in range(c, c+k):
                if colSum[r+k][j] - colSum[r][j] != target:
                    return False
            
            d1 = d2 = 0
            for i in range(k):
                d1 += grid[r+i][c+i]
                d2 += grid[r+i][c+k-1-i]
            
            return d1 == target and d2 == target
        
        for k in range(min(rows, cols), 1, -1):
            for r in range(rows - k + 1):
                for c in range(cols - k + 1):
                    if isMagic(r, c, k):
                        return k
        
        return 1
