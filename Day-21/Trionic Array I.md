# 3637. Trionic Array I

## Problem Description
An array `nums` of length $n$ is **trionic** if there exist indices $0 < p < q < n - 1$ such that:
1.  **Segment 1:** `nums[0...p]` is **strictly increasing**.
2.  **Segment 2:** `nums[p...q]` is **strictly decreasing**.
3.  **Segment 3:** `nums[q...n-1]` is **strictly increasing**.



## Approach: Linear Scan (Greedy)
Since the segments must be **strictly** monotonic, we can solve this by simulating the traversal of the array in three distinct phases. If we cannot complete a phase or if a phase has no elements (i.e., we don't move), the array is not trionic.

### Steps:
1.  **First Increase:** Start at the beginning and move as far as the values strictly increase. This identifies the first peak $p$.
2.  **The Drop:** From $p$, move as far as the values strictly decrease. This identifies the valley $q$.
3.  **Final Increase:** From $q$, move as far as the values strictly increase until the end of the array.
4.  **Validation:** Ensure each phase actually occurred (the index moved) and that we reached the final element of the array.

## Complexity Analysis
* **Time Complexity:** $O(n)$.
* **Space Complexity:** $O(1)$.

## Implementation (C++)

```cpp
class Solution {
public:
    bool isTrionic(vector<int>& nums) {
        int n = nums.size();
        if (n < 3) return false;
        
        int i = 0;

        // Phase 1: Strictly Increasing (0 to p)
        if (i + 1 < n && nums[i] < nums[i+1]) {
            while (i + 1 < n && nums[i] < nums[i+1]) i++;
        } else return false;

        // Phase 2: Strictly Decreasing (p to q)
        if (i + 1 < n && nums[i] > nums[i+1]) {
            while (i + 1 < n && nums[i] > nums[i+1]) i++;
        } else return false;

        // Phase 3: Strictly Increasing (q to n-1)
        if (i + 1 < n && nums[i] < nums[i+1]) {
            while (i + 1 < n && nums[i] < nums[i+1]) i++;
        } else return false;

        // Check if we reached the last element
        return i == n - 1;
    }
};
