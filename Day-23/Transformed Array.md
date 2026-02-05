# 3379. Transformed Array

## Problem Description
You are given an integer array `nums` that represents a **circular array**. Your task is to create a new array `result` of the same size based on these rules:

- **If `nums[i] > 0`**: Start at index `i` and move `nums[i]` steps to the **right**.
- **If `nums[i] < 0`**: Start at index `i` and move `abs(nums[i])` steps to the **left**.
- **If `nums[i] == 0`**: The value remains the same.

Since the array is circular, moving past the last element wraps around to the beginning, and moving before the first element wraps back to the end.

---

##  Approach: Unified Circular Indexing

The challenge in circular array problems is handling negative movement (moving left) without landing on a negative index, which would cause a segmentation fault in C++. 

### The Mathematical Secret
Instead of using multiple `if-else` conditions, we use a robust modulo formula that handles both directions:
$$\text{targetIndex} = ((i + \text{jump}) \pmod n + n) \pmod n$$



**Breakdown of the formula:**
1. **`(i + jump) % n`**: Maps the jump to a value within the array bounds. In C++, this can still be negative if the jump is negative.
2. **`+ n`**: By adding the array size, we ensure a negative result becomes positive.
3. **`final % n`**: This final step ensures that if the number was already positive, adding `n` doesn't push it out of the valid range $[0, n-1]$.

---

## Complexity
* **Time Complexity:** $O(n)$ 
* **Space Complexity:** $O(n)$
---
  
##  Code (C++)

```cpp
class Solution {
public:
    vector<int> constructTransformedArray(vector<int>& nums) {
        // Optimization: Untie C++ and C standard streams for faster I/O
        ios_base::sync_with_stdio(false);
        cin.tie(NULL);
        
        int n = nums.size();
        vector<int> result(n);

        for (int i = 0; i < n; ++i) {
            // Apply the unified circular formula.
            // (nums[i] % n) handles jumps larger than the array length.
            int targetIndex = ((i + nums[i]) % n + n) % n;
            result[i] = nums[targetIndex];
        }

        return result;
    }
};
