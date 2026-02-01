# 3010. Divide an Array Into Subarrays With Minimum Cost I

## Problem Definition
You are given an array of integers `nums` of length `n`. The **cost** of an array is defined as the value of its **first** element.

You need to divide `nums` into **3 disjoint contiguous subarrays**. Return the **minimum** possible sum of the costs of these subarrays.

### Constraints
* `3 <= n <= 50`
* `1 <= nums[i] <= 50`

---

## Approach: Greedy Selection

To minimize the total cost, we need to understand the structure of the 3 subarrays:
1.  **Subarray 1:** Must start at index `0`. Therefore, `nums[0]` is **always** part of the total cost.
2.  **Subarray 2 & 3:** These can start at any indices `i` and `j` such that `1 <= i < j < n`. 

Since the cost of any subarray is simply its first element, our goal is to pick the two smallest elements available in the remainder of the array (`nums[1:]`) to serve as the starting points for the second and third subarrays.



### Steps:
1.  Store the value of `nums[0]`.
2.  Identify all elements from index `1` to `n-1`.
3.  Find the two smallest values among those elements.
4.  Sum `nums[0]` with these two smallest values and return the result.

---

## Complexity Analysis
* **Time Complexity:** $O(n \log n)$ 
    
* **Space Complexity:** $O(n)$ 

---

## Code Implementation

```python
class Solution(object):
    def minimumCost(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        # The first element is always the cost of the first subarray
        fixed_cost = nums[0]
        
        # The remaining elements are candidates for the starts of the next two subarrays
        candidates = nums[1:]
        
        # Sort to find the two smallest values
        candidates.sort()
        
        # Minimum cost = first element + smallest + second smallest
        return fixed_cost + candidates[0] + candidates[1]
