# 3634. Minimum Removals to Balance Array

## Problem Description
You are given an integer array `nums` and an integer `k`. An array is considered **balanced** if its maximum element is at most `k` times its minimum element:
$$\text{max}(subset) \le \text{min}(subset) \times k$$

Your goal is to find the **minimum number of elements to remove** so that the remaining elements form a balanced array. 

> **Note:** A single element array is always balanced.

---

## Approach: Sorting + Sliding Window

The core idea is that the order of elements in the original array doesn't matter—only their values do. By sorting the array, we can easily identify the minimum and maximum of any contiguous range.

### 1. Sorting
We sort `nums` in non-decreasing order. Once sorted, for any window (subarray) from index `i` to `j`:
* The **minimum** element is `nums[i]`.
* The **maximum** element is `nums[j]`.

### 2. Sliding Window
We use two pointers, `i` (left) and `j` (right), to find the **longest** balanced subarray. 
* We expand the window by moving `j` to the right.
* If the condition `nums[j] > nums[i] * k` is violated, the window is no longer balanced. We then shrink the window from the left by incrementing `i` until the condition is met again.
* The number of elements we **keep** is the length of this window: $(j - i + 1)$.



### 3. Final Answer
The minimum removals required is simply the total number of elements minus the maximum number of elements we managed to keep:
$$\text{Removals} = \text{Total Elements} - \text{Max Kept}$$

---

## Complexity Analysis
* **Time Complexity:** $O(n \log n)$ 
* **Space Complexity:** $O(1)$ 
   
---

## Implementation

```cpp
class Solution {
public:
    int minRemoval(vector<int>& nums, int k) {
        int n = nums.size();
        // Step 1: Sort to make min/max identification easy
        sort(nums.begin(), nums.end());
        
        int max_kept = 0;
        int i = 0;
        
        // Step 2: Slide the window across the array
        for (int j = 0; j < n; ++j) {
            // If the max (nums[j]) is more than k times the min (nums[i])
            // shrink the window from the left
            while (nums[j] > (long long)nums[i] * k) {
                i++;
            }
            
            // Calculate the current window size and update our maximum
            max_kept = max(max_kept, j - i + 1);
        }
        
        // Step 3: Result is total minus what we kept
        return n - max_kept;
    }
};
