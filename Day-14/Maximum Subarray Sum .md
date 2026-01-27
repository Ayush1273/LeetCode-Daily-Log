# Maximum Subarray Sum (Kadane’s Algorithm)

## Problem Statement

Given an integer array `nums`, find the **contiguous subarray** (containing at least one number) which has the **largest sum**, and return that sum.

### Constraints
- The subarray must be **contiguous**
- The order of elements **cannot be changed**
- The array contains **at least one element**

---

## Approach (Kadane’s Algorithm)

This problem can be solved efficiently using **Kadane’s Algorithm**.

### Core Idea
At each index, decide:
- Whether to **start a new subarray** from the current element
- Or **extend the existing subarray** by adding the current element

We track two values:
- `current_sum`: Maximum subarray sum ending at the current index
- `max_sum`: Maximum subarray sum found so far

### Key Insight
If the running sum becomes negative, it will reduce future sums.  
So, it is better to **discard it and start fresh** from the next element.

---

## Algorithm

1. Initialize `current_sum` and `max_sum` with the first element of the array.
2. Traverse the array from the second element onward.
3. At each step:
   - Update `current_sum` as the maximum of:
     - Current element
     - Current element + previous `current_sum`
   - Update `max_sum` with the maximum value seen so far.
4. Return `max_sum`.

---

##  Code 

```python
class Solution(object):
  def maxSubArray(self, nums):
      """
      :type nums: List[int]
      :rtype: int
      """
      max_sum = nums[0]
      current_sum = nums[0]

      for i in range(1, len(nums)):
          current_sum = max(nums[i], current_sum + nums[i])
          max_sum = max(max_sum, current_sum)

      return max_sum



