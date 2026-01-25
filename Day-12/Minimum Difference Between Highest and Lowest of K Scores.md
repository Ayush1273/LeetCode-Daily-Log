# 1984. Minimum Difference Between Highest and Lowest of K Scores

## Problem Description
You are given an array `nums` of student scores and an integer `k`. Your goal is to pick **any** `k` scores from the array such that the difference between the **highest** and **lowest** of those `k` scores is as small as possible.

**Example:**
* **Input:** `nums = [9, 4, 1, 7]`, `k = 2`
* **Output:** `2`
* **Explanation:** If we pick `[7, 9]`, the difference is $9 - 7 = 2$. This is the smallest possible gap.

---

## The Approach: 
If we pick scores randomly, it's hard to know if we found the smallest gap. However, if we **sort** the scores first, the numbers that are closest to each other will be right next to each other.

1.  **Sort the Array:** By sorting `nums` in ascending order, any group of $k$ scores with the minimum spread must be a **contiguous subarray** of length $k$.
2.  **Sliding Window:** We move a window of size $k$ across the sorted array.
3.  **Calculate Gap:** For every window, the "High" is at the end of the window ($i + k - 1$) and the "Low" is at the start ($i$).
4.  **Update Minimum:** Compare this gap to our current best result and keep the smaller one.



---

## Complexity Analysis
* **Time Complexity:** $O(n \log n)$

* **Space Complexity:** $O(1)$ or $O(n)$
    

---

## Implementation

```python
class Solution(object):
    def minimumDifference(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        # Edge case: If we only pick one student, the difference is 0
        if k == 1:
            return 0
        
        # 1. Sort to bring similar scores together
        nums.sort()
        
        # 2. Initialize our answer with a large value
        res = float("inf")
        
        # 3. Slide the window of size k through the array
        # Left pointer is 'i', Right pointer is 'i + k - 1'
        for i in range(len(nums) - k + 1):
            diff = nums[i + k - 1] - nums[i]
            res = min(res, diff)
            
        return res
