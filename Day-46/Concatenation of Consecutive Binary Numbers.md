# 1680. Concatenation of Consecutive Binary Numbers

## Problem Summary
Form a large binary string by concatenating the binary representations of integers from $1$ to $n$, then return its decimal value modulo $10^9 + 7$.

## Approach: Mathematical Shift
Concatenating a binary number $i$ to a running total $R$ is equivalent to:
$$R = (R \times 2^{\text{bits in } i} + i) \pmod{MOD}$$

Instead of converting to strings, we use bitwise operators:
1. **Bit Length Tracking:** The number of bits in $i$ only increases when $i$ is a power of two ($1, 2, 4, 8...$).
2. **Shift and Combine:** Use `result = ((result << bitLength) + i) % MOD`.

## Complexity
- **Time Complexity:** $O(n)$.
- **Space Complexity:** $O(1)$.

---

## Code (Java)
```java
class Solution {
    public int concatenatedBinary(int n) {
        long res = 0, mod = 1_000_000_007;
        int size = 0;
        for (int i = 1; i <= n; i++) {
            if ((i & (i - 1)) == 0) size++;
            res = ((res << size) | i) % mod;
        }
        return (int) res;
    }
}
