# 3507. Minimum Pair Removal to Sort Array I

## Problem Overview
The goal is to find how many operations are needed to make an array sorted (non-decreasing). In each operation, you must:
1. Find the pair of adjacent numbers with the **smallest sum**.
2. If there's a tie, pick the one that appears **first** (leftmost).
3. Replace those two numbers with their combined sum.

## My Approach: Simulation
Since the array size is small ($N \le 50$), we can simply "act out" the instructions step-by-step:

1. **The Check**: We look at the array to see if it's already sorted. If it is, we're done!
2. **The Search**: We loop through the array once to find the pair $(i, i+1)$ that gives us the lowest `nums[i] + nums[i+1]`.
3. **The Update**: We add the two numbers together, update the first index with this new value, and remove the second number from the list.
4. **The Counter**: We keep a running tally of how many times we perform these steps.

## Complexity Analysis
* **Time Complexity**: $O(N^2)$
 
* **Space Complexity**: $O(1)$
 

## Final Code Implementation

```python
class Solution(object):
    def minimumPairRemoval(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        def is_sorted(arr):
            # Check if the array is non-decreasing
            for i in range(len(arr) - 1):
                if arr[i] > arr[i+1]:
                    return False
            return True

        op_count = 0
        
        # Keep smashing pairs until the list is sorted
        while not is_sorted(nums):
            min_sum = float('inf')
            target_idx = -1
            
            # Step 1: Find the leftmost pair with the smallest sum
            for i in range(len(nums) - 1):
                current_sum = nums[i] + nums[i+1]
                if current_sum < min_sum:
                    min_sum = current_sum
                    target_idx = i
            
            # Step 2: Replace the pair with their sum
            nums[target_idx] = nums[target_idx] + nums[target_idx + 1]
            nums.pop(target_idx + 1)
            
            # Step 3: Increment our operation counter
            op_count += 1
            
        return op_count
