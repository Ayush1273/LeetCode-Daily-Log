# 70. Climbing Stairs (Easy)

## Problem Statement
You are climbing a staircase with `n` steps.

Each time, you can climb either:
- 1 step, or
- 2 steps

The task is to find the **number of distinct ways** to reach the top.

---

## Intuition
To reach a particular step, you must come from:
- the previous step (1 step move), or
- the step before that (2 step move)

So, the total number of ways to reach step `n` is the sum of:
- ways to reach step `n - 1`
- ways to reach step `n - 2`

This makes the problem very similar to the **Fibonacci sequence**.

---

## Approach (Simple Explanation)

1. If `n = 1`, there is only **1 way**
2. If `n = 2`, there are **2 ways**
3. For all other steps:
   - `ways(n) = ways(n - 1) + ways(n - 2)`
4. Instead of using extra memory, we only keep track of the last two values

---

## Why This Works
- Every step depends only on the previous two steps
- Using iteration avoids recursion overhead
- Optimized to use constant space

---

## Time and Space Complexity
- **Time Complexity:** `O(n)`
- **Space Complexity:** `O(1)`

---

## Python Solution

```python
class Solution(object):
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """
        if n == 1:
            return 1
        if n == 2:
            return 2
        
        prev2 = 1
        prev1 = 2
        
        for i in range(3, n + 1):
            curr = prev1 + prev2
            prev2 = prev1
            prev1 = curr
        
        return prev1
