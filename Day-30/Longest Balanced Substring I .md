# 3713. Longest Balanced Substring I 

## Problem Summary
Find the length of the longest substring where **all distinct characters** appear the **same number of times**.

- **Example:** `s = "abbac"`
  - Substring `"abba"` contains 'a' twice and 'b' twice. Both appear 2 times.
  - Result: 4.

##  Approach: 
Given the constraints ($N \le 1000$), we can iterate through all possible substrings.

1. **Outer Loop:** Fix the starting index `i`.
2. **Inner Loop:** Expand the end index `j`.
3. **Frequency Map:** Use an array of size 26 to count occurrences.
4. **Balanced Check:** - A substring of length `L` with `D` distinct characters is balanced if every distinct character appears exactly `L / D` times.
   - We only perform this check if `L % D == 0`.

## Complexity
- **Time:** $O(N^2 \cdot 26)$ — acceptable for $N=1000$.
- **Space:** $O(1)$ — frequency array size is constant.

---

## Code (C++)

```cpp
class Solution {
public:
    int longestBalanced(string s) {
        int n = s.length(), maxLen = 0;
        for (int i = 0; i < n; ++i) {
            int freq[26] = {0}, distinct = 0;
            for (int j = i; j < n; ++j) {
                if (freq[s[j] - 'a']++ == 0) distinct++;
                
                int len = j - i + 1;
                if (len % distinct == 0) {
                    int target = len / distinct;
                    bool ok = true;
                    for (int k = 0; k < 26; ++k) {
                        if (freq[k] > 0 && freq[k] != target) {
                            ok = false; break;
                        }
                    }
                    if (ok) maxLen = max(maxLen, len);
                }
            }
        }
        return maxLen;
    }
};
