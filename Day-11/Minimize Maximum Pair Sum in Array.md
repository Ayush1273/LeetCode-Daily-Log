# 1877. Minimize Maximum Pair Sum in Array

##  Problem Overview
We are given an array of even length. Our goal is to split the numbers into pairs such that:
1. Every number is used exactly once.
2. We calculate the sum of each pair.
3. We want the **largest** of these sums to be as **minimized** as possible.

---

##  The Strategy: *The Greedy Balance*
To keep the maximum sum small, we must prevent large numbers from pairing with other large numbers.

**The Logic:**
- Pairing the two largest numbers makes the sum unnecessarily large.
- To balance things out, pair the **largest** number with the **smallest** available number.

This greedy pairing ensures no single pair becomes too large.

---

##  Step-by-Step Approach
1. **Sort the Array**  
   Arrange the numbers from smallest to largest.

2. **Two-Pointer Pairing**  
   - Pair the smallest element (`i`) with the largest element (`n - 1 - i`).
   - Move inward until all elements are paired.

3. **Track the Maximum Pair Sum**  
   - Compute each pair’s sum.
   - Keep track of the maximum sum encountered.

---

##  Complexity Analysis
**Time Complexity (TC):** O(n log n)   
**Space Complexity (SC):** O(1) .

---

## Implementation (Python)

```python
class Solution(object):
    def minPairSum(self, nums):
        # 1. Sort the array
        nums.sort()
        
        max_sum = 0
        n = len(nums)
        
        # 2. Pair smallest with largest
        for i in range(n // 2):
            current_pair_sum = nums[i] + nums[n - 1 - i]
            max_sum = max(max_sum, current_pair_sum)
                
        return max_sum
