# 260. Single Number III

##  Problem Overview
We are given an array where every number appears twice, except for **two** unique numbers. We need to find those two numbers in **O(n) time** and **O(1) space**.

---

## The Logic: Bit Manipulation
We use the **XOR** operator properties:
1. $x \oplus x = 0$ (Identical numbers cancel out)
2. $x \oplus 0 = x$ (XOR with zero stays the same)

**The Challenge:** If we XOR everything, we get `A ^ B`. We need to separate them. We do this by finding a bit where `A` and `B` differ and using it as a "filter" to split the array into two piles.

---

##  Step-by-Step Approach
1. **Full XOR:** XOR every number in the array. Pairs disappear, leaving `xor_all = A ^ B`.
2. **Find Diff Bit:** Use `xor_all & -xor_all` to find the rightmost bit that is set to `1`. This is a bit where `A` and `B` are guaranteed to be different.
3. **Group & Solve:** Iterate through the array again. If a number has that bit set, XOR it into `Group 1`. Otherwise, XOR it into `Group 2`.
4. **Final Result:** Each group will collapse into one of the unique numbers.

---

## Complexity Analysis
* **TC (Time Complexity):** $O(n)$  

* **SC (Space Complexity):** $O(1)$  
  

---

##  Example Walkthrough
**Input:** `[1, 2, 1, 3, 2, 5]`

1. **XOR all:** `(1^1) ^ (2^2) ^ 3 ^ 5` → `3 ^ 5` = `binary 011 ^ 101` = `110` (6).
2. **Diff Bit:** Rightmost set bit of `110` is `010` (2).
3. **Partition:**
   - **Bit at pos 2 is 1:** `[2, 3, 2]` → XORing these gives `3`.
   - **Bit at pos 2 is 0:** `[1, 1, 5]` → XORing these gives `5`.
4. **Result:** `[3, 5]`



## Implementation (Python)

```python
class Solution(object):
    def singleNumber(self, nums):
        # 1. Get the XOR result of the two unique numbers (A ^ B)
        xor_all = 0
        for n in nums:
            xor_all ^= n
            
        # 2. Find the rightmost set bit (the "differentiating bit")
        # This bit exists because A and B are different
        diff_bit = xor_all & -xor_all
        
        # 3. Partition and XOR into two separate buckets
        res = [0, 0]
        for n in nums:
            if n & diff_bit:
                # Group where the bit is 1
                res[0] ^= n
            else:
                # Group where the bit is 0
                res[1] ^= n
                
        return res
