# 1356. Sort Integers by The Number of 1 Bits

## Problem Summary
Given an array `arr`, sort it based on:
1. The number of set bits (1s) in binary representation (ascending).
2. The actual integer value if the bit counts are equal (ascending).

## Approach: Custom Comparator
The most efficient way to handle this is to use the standard library's sorting algorithm with a custom comparison rule.

1. **Counting Bits:** We use `__builtin_popcount(n)` in C++, which is a built-in compiler function that uses a single CPU instruction (on most modern hardware) to count set bits.
2. **Comparison Logic:**
   - Let `cntA` be set bits of `a` and `cntB` be set bits of `b`.
   - If `cntA != cntB`, return `cntA < cntB`.
   - Otherwise, return `a < b`.
3. **Sorting:** The `std::sort` function handles the rearranging of elements based on these rules.

## Complexity
- **Time Complexity:** $O(N \log N)$.
- **Space Complexity:** $O(1)$.

---

## Code (C++)
```cpp
class Solution {
public:
    vector<int> sortByBits(vector<int>& arr) {
        sort(arr.begin(), arr.end(), [](int a, int b) {
            int ca = __builtin_popcount(a), cb = __builtin_popcount(b);
            return ca == cb ? a < b : ca < cb;
        });
        return arr;
    }
};
