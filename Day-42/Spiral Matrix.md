# 54. Spiral Matrix 

## Problem Summary
Given an `m x n` matrix, return all elements in spiral order (Right → Down → Left → Up, then repeat inward).

## Approach: Boundary Simulation
We use four pointers to represent the current boundaries of the matrix: `top`, `bottom`, `left`, and `right`.

1. **Move Right:** Iterate through the `top` row, then shift the `top` boundary down (`top++`).
2. **Move Down:** Iterate down the `right` column, then shift the `right` boundary left (`right--`).
3. **Move Left:** (If rows remain) Iterate across the `bottom` row, then shift `bottom` up (`bottom--`).
4. **Move Up:** (If columns remain) Iterate up the `left` column, then shift `left` right (`left++`).

We repeat this cycle until the `top` boundary crosses `bottom` or `left` crosses `right`.

## Complexity
- **Time Complexity:** $O(M \times N)$ — Every cell is visited exactly once.
- **Space Complexity:** $O(1)$ — Only a few pointers are used (output storage not included).

---

## Code (C++)
```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        int top = 0, bottom = m - 1, left = 0, right = n - 1;
        vector<int> res;
        
        while (res.size() < m * n) {
            for (int j = left; j <= right && res.size() < m * n; j++) 
                res.push_back(matrix[top][j]);
            top++;
            for (int i = top; i <= bottom && res.size() < m * n; i++) 
                res.push_back(matrix[i][right]);
            right--;
            for (int j = right; j >= left && res.size() < m * n; j--) 
                res.push_back(matrix[bottom][j]);
            bottom--;
            for (int i = bottom; i >= top && res.size() < m * n; i--) 
                res.push_back(matrix[i][left]);
            left++;
        }
        return res;
    }
};
