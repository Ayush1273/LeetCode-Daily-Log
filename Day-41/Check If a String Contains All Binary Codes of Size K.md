# 1461. Check If a String Contains All Binary Codes of Size K 

## Problem Summary
Check if every possible binary string of length `k` exists as a substring in the given string `s`. There are $2^k$ such possible strings.

## Approach: Sliding Window + Bit Manipulation
We need to efficiently find all unique substrings of length `k`.

1. **Total Codes:** Calculate $2^k$ using `1 << k`.
2. **Rolling Integer:** Instead of `s.substr()`, treat the window as a sliding integer.
   - Shift the integer left by 1.
   - Apply a **mask** to keep only the lower `k` bits.
   - Add the new bit from the string.
3. **Tracking:** Add each integer code to an `unordered_set`.
4. **Result:** If the set size reaches $2^k$, return `true`.

## Complexity
- **Time Complexity:** $O(N)$ .
- **Space Complexity:** $O(2^k)$.

---

## Code (C++)
```cpp
class Solution {
public:
    bool hasAllCodes(string s, int k) {
        if (s.size() < k) return false;
        unordered_set<int> seen;
        int res = 0, mask = (1 << k) - 1;
        for (int i = 0; i < s.size(); i++) {
            res = ((res << 1) & mask) | (s[i] - '0');
            if (i >= k - 1) seen.insert(res);
        }
        return seen.size() == (1 << k);
    }
};
