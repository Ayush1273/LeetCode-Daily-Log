# 693. Binary Number with Alternating Bits 

## Problem Summary
Check if a positive integer `n` has alternating bits (e.g., `101010`). Two adjacent bits must always be different.

##  Approach: The XOR Trick
Instead of checking bits one by one, we can use bitwise properties to solve this in constant time.

1. **Shift and XOR:** Let `m = n ^ (n >> 1)`. 
   - If `n` is alternating, every pair of adjacent bits in the XOR operation will be `(0^1)` or `(1^0)`, resulting in `1`.
   - Therefore, `m` will be a sequence of all ones (e.g., `111...1`).
2. **All-Ones Validation:** A quick way to check if a number `m` is all ones is to check `(m & (m + 1)) == 0`.
   - *Example:* If `m = 7` (`111`), then `m + 1 = 8` (`1000`). `111 & 1000` is `0`.

## Complexity
- **Time Complexity:** $O(1)$.
- **Space Complexity:** $O(1)$.

---

##  C++ Code
```cpp
class Solution {
public:
    bool hasAlternatingBits(int n) {
        long m = (long)n ^ (n >> 1);
        return (m & (m + 1)) == 0;
    }
};
