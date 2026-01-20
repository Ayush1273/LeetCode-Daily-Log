# 3314. Construct the Minimum Bitwise Array I

**Difficulty:** Easy  
**Topic:** Bit Manipulation  
  

---

##  Problem Summary

You are given an array `nums` consisting of prime numbers.

For each `nums[i]`, find the **smallest non-negative integer** `ans[i]` such that:

ans[i] OR (ans[i] + 1) = nums[i]

If no such value exists, set `ans[i] = -1`.

---

##  Key Insight

- Adding `1` to a number flips the **rightmost sequence of 1s** and turns the first `0` before it into `1`.
- The OR operation keeps all `1` bits from both numbers.
- As a result, `x OR (x + 1)` always produces an **odd number**.
- Since `nums[i]` are primes, the only even prime (`2`) can never be formed.

---

##  Approach

1. For each value in `nums`, try all `x` from `0` to `nums[i] - 1`.
2. Check whether `(x | (x + 1)) == nums[i]`.
3. The **first valid `x`** is the minimum answer.
4. If no value satisfies the condition, return `-1`.

This works efficiently due to the small constraints.

---

##  Python Implementation

```python
class Solution(object):
    def minBitwiseArray(self, nums):
        ans = []
        for target in nums:
            found = False
            for x in range(target):
                if (x | (x + 1)) == target:
                    ans.append(x)
                    found = True
                    break
            if not found:
                ans.append(-1)
        return ans
