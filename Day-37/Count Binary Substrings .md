# 696. Count Binary Substrings 

## Problem Summary
Count substrings that have an equal number of `0`s and `1`s, where all `0`s and `1`s are grouped consecutively (e.g., `0011` is valid, but `0101` is not).

## Approach: Group Streaks
The key is to realize that valid substrings only occur at the transition points between a streak of `0`s and a streak of `1`s.

1. **Group Lengths:** Identify consecutive groups. For `00011`, the groups are `3` and `2`.
2. **Min Logic:** Between two adjacent groups of size `A` and `B`, the number of valid substrings is `min(A, B)`. 
   - *Example:* For `000` and `11`, we can form `01` and `0011`. Total = `min(3, 2) = 2`.
3. **Linear Pass:** We don't need to store all group lengths in an array. We can just keep track of the `previous_group_length` and the `current_group_length` as we iterate.

## Complexity
- **Time:** $O(N)$.
- **Space:** $O(1)$.

---

## C++ Code
```cpp
class Solution {
public:
    int countBinarySubstrings(string s) {
        int ans = 0, prev = 0, curr = 1;
        for (int i = 1; i < s.size(); i++) {
            if (s[i-1] != s[i]) {
                ans += min(prev, curr);
                prev = curr;
                curr = 1;
            } else {
                curr++;
            }
        }
        return ans + min(prev, curr);
    }
};
