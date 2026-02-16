# 190. Reverse Bits 

## Problem Summary
Reverse the bits of a given 32-bit unsigned integer. For example, if the input is `00...0011`, the output should be `1100...00`.

## Approach: Bitwise Simulation
Since we are dealing with a fixed 32-bit integer, we can process the bits one by one.

1. **Initialization:** Start with `result = 0`.
2. **The Loop:** Run a loop exactly 32 times.
3. **Extraction:** Use `(n & 1)` to grab the rightmost bit of the input.
4. **Placement:** Shift the `result` to the left and append the extracted bit.
5. **Shift Input:** Shift the input `n` to the right to move to the next bit.

This effectively "picks" bits from the end of the input and "drops" them into the front of the result.

## Complexity
- **Time:** $O(1)$ — The loop always runs for 32 iterations.
- **Space:** $O(1)$ — Constant space used.

---

## C++ Code
```cpp
class Solution {
public:
    uint32_t reverseBits(uint32_t n) {
        uint32_t res = 0;
        for (int i = 0; i < 32; i++) {
            res = (res << 1) | (n & 1);
            n >>= 1;
        }
        return res;
    }
};
