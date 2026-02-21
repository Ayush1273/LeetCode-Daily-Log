# 762. Prime Number of Set Bits in Binary Representation 

## Problem Summary
Given a range `[left, right]`, count how many integers have a **prime number** of set bits (1s) in their binary form.

## Approach: Iterative Bit Counting
Since the constraints go up to $10^6$, we know that $2^{20} \approx 1.04 \times 10^6$. Therefore, the maximum number of set bits we can encounter is **20**.

1. **Pre-identify Primes:** Within the range [0, 20], the primes are: `2, 3, 5, 7, 11, 13, 17, 19`.
2. **Count Set Bits:** For every number in the range, calculate the number of bits set to 1.
   - In C++, we use `__builtin_popcount(i)` for high performance.
3. **Validate:** If the bit count is in our prime set, increment the result.

## Complexity
- **Time Complexity:** $O(N)$. 
- **Space Complexity:** $O(1)$.

---

## Code (C++)
```cpp
class Solution {
public:
    int countPrimeSetBits(int left, int right) {
        int result = 0;
        // A bitmask representing primes: bit 2, 3, 5, 7, 11, 13, 17, 19 are set.
        // 665770 in decimal = 101000101000101010110 in binary
        int primeMask = 665770; 
        
        for (int i = left; i <= right; ++i) {
            if ((primeMask >> __builtin_popcount(i)) & 1) {
                result++;
            }
        }
        return result;
    }
};
