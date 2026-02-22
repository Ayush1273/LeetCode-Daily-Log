# 868. Binary Gap

## Problem Summary
Find the longest distance between two adjacent `1`s in the binary representation of a positive integer `n`. If no such pair exists, return `0`.

## Approach: Last Position Tracker
We can solve this by scanning the bits of the number and keeping track of the index where we last saw a `1`.

1. **Scan Bits:** Iterate through the bit positions from 0 to 31.
2. **Detection:** When a bit is `1`:
   - If we have seen a `1` previously, calculate the gap: `current_index - last_index`.
   - Update the `max_gap` if the current gap is larger.
   - Update `last_index` to the `current_index`.
3. **Efficiency:** This avoids converting the number to a string and uses simple bitwise shifting.

## Complexity
- **Time Complexity:** $O(\log N)$.
- **Space Complexity:** $O(1)$.

---

## Code (C++)
```cpp
class Solution {
public:
    int binaryGap(int n) {
        int maxGap = 0, last = -1;
        for (int i = 0; i < 31; ++i) {
            if ((n >> i) & 1) {
                if (last != -1) maxGap = max(maxGap, i - last);
                last = i;
            }
        }
        return maxGap;
    }
};
