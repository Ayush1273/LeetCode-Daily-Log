# 3315. Construct the Minimum Bitwise Array II

 
**Topic:** Bit Manipulation  
 

---

##  Problem Understanding

I was given an array `nums` consisting of prime numbers.  
For each element `nums[i]`, I needed to find the **smallest possible value** `ans[i]` such that:

ans[i] OR (ans[i] + 1) = nums[i]

If no such value exists, I had to return `-1` for that index.

The key challenge here was to **avoid brute force** because `nums[i]` can be as large as `10^9`.

---

## Key Observations

- The expression `x OR (x + 1)` has a very specific binary behavior.
- Adding `1` flips the **rightmost sequence of 1s** into 0s and turns the first `0` before them into `1`.
- Because of this, `x OR (x + 1)` always produces an **odd number**.
- The only even prime is `2`, so:
  - If `nums[i] == 2`, the answer is **always `-1`**.
- For all other primes (which are odd), the binary representation ends with **one or more trailing 1s**.

---

## Approach (How I Solved It)

1. If the number is `2`, I directly return `-1`.
2. Otherwise:
   - I count the number of **trailing 1s** in the binary representation.
   - To minimize `ans[i]`, I flip the **most significant bit of the trailing 1s block** to `0`.
3. This guarantees:
   - `(ans[i] | (ans[i] + 1)) == nums[i]`
   - `ans[i]` is the **minimum possible value**.

This avoids brute force and works efficiently even for large numbers.

---

## Python Implementation

```python
class Solution(object):
    def minBitwiseArray(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        ans = []
        
        for n in nums:
            if n == 2:
                ans.append(-1)
            else:
                temp = n
                count = 0
                
                # Count trailing ones
                while (temp & 1):
                    temp >>= 1
                    count += 1
                
                # Flip the most significant bit of trailing ones
                res = n ^ (1 << (count - 1))
                ans.append(res)
                
        return ans
