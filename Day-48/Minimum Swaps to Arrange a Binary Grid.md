# 1536. Minimum Swaps to Arrange a Binary Grid

## Problem Summary
A grid is valid if all cells above the main diagonal are `0`. You can only swap **adjacent** rows. Find the minimum swaps to make the grid valid.

## Approach: Greedy Bubble Sort
The problem reduces to a 1D array problem:
1. **Represent Rows:** Calculate the number of trailing zeros in each row. Row $i$ needs at least $n - 1 - i$ trailing zeros.
2. **Greedy Search:** Starting from the first row, find the nearest row below it that meets the requirement for that specific position.
3. **Simulate Swaps:** Move that row to the current position. The number of steps to move a row from index $j$ to $i$ (where $j > i$) via adjacent swaps is exactly $j - i$.
4. **Impossible Case:** If no row satisfies the condition for a specific index, return `-1`.

## Complexity
- **Time:** $O(n^2)$ .
- **Space:** $O(n)$.

## Code (C++)
```cpp
class Solution {
public:
    int minSwaps(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<int> trailingZeros;

        // 1. Count trailing zeros for each row
        for (int i = 0; i < n; ++i) {
            int count = 0;
            for (int j = n - 1; j >= 0; --j) {
                if (grid[i][j] == 0) count++;
                else break;
            }
            trailingZeros.push_back(count);
        }

        int totalSwaps = 0;

        // 2. Try to satisfy the requirement for each row position i
        for (int i = 0; i < n; ++i) {
            int required = n - 1 - i;
            int foundIdx = -1;

            // Find the first row that satisfies the condition
            for (int j = i; j < n; ++j) {
                if (trailingZeros[j] >= required) {
                    foundIdx = j;
                    break;
                }
            }

            // If no row satisfies the condition, return -1
            if (foundIdx == -1) return -1;

            // 3. Move the row from foundIdx to i using adjacent swaps
            for (int j = foundIdx; j > i; --j) {
                swap(trailingZeros[j], trailingZeros[j - 1]);
                totalSwaps++;
            }
        }

        return totalSwaps;
    }
};
