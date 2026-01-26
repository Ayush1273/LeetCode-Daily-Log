# 1200. Minimum Absolute Difference

## Problem Description
Given an array of **distinct** integers `arr`, find all pairs of elements that have the smallest possible absolute difference between them. 

You need to return these pairs in a list, sorted in ascending order. Each pair `[a, b]` must satisfy:
1. `a < b`
2. `b - a == min_difference`

**Example:**
* **Input:** `arr = [4, 2, 1, 3]`
* **Output:** `[[1, 2], [2, 3], [3, 4]]`
* **Explanation:** The smallest difference between any two numbers here is 1. The pairs with a difference of 1 are [1,2], [2,3], and [3,4].

---

## The Approach: 
The "brute force" way would be to compare every single number with every other number, but that's too slow ($O(n^2)$). 

The secret to this problem is **Sorting**. When the numbers are sorted, the two numbers that produce the minimum difference **must** be right next to each other.

1.  **Sort the Array:** This puts the numbers in order ($a < b < c...$).
2.  **Find the Minimum Gap:** Walk through the sorted array once and compare each pair of neighbors (`arr[i]` and `arr[i+1]`) to find the smallest difference.
3.  **Collect the Pairs:** Walk through the array a second time (or do it during the first pass) to collect all neighbor pairs that match that minimum difference.



---

## Complexity Analysis
* **Time Complexity:** $O(n \log n)$
   
* **Space Complexity:** $O(n)$ or $O(\log n)$
    

---

## Implementation

```python
class Solution(object):
    def minimumAbsDifference(self, arr):
        """
        :type arr: List[int]
        :rtype: List[List[int]]
        """
        # 1. Sort the array first
        arr.sort()
        
        min_diff = float('inf')
        result = []
        
        # 2. First pass: find the actual minimum difference
        for i in range(len(arr) - 1):
            current_diff = arr[i+1] - arr[i]
            if current_diff < min_diff:
                min_diff = current_diff
        
        # 3. Second pass: collect all pairs with that min_diff
        for i in range(len(arr) - 1):
            if arr[i+1] - arr[i] == min_diff:
                result.append([arr[i], arr[i+1]])
                
        return result
