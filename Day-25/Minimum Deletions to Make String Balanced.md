# 1653. Minimum Deletions to Make String Balanced

## Problem Description
You are given a string `s` consisting only of characters `'a'` and `'b'`. You can delete any number of characters to make the string **balanced**.

A string is **balanced** if there is no pair of indices `(i, j)` such that `i < j` and `s[i] = 'b'` and `s[j] = 'a'`. 

**In short:** All `'a'`s must come before all `'b'`s.

---

## Logic & Approach
To solve this in a single pass with optimal space, we use a **Greedy/Dynamic Programming** approach. 

The imbalance only occurs when we see an `'a'` appearing after a `'b'`. At that specific moment, we have two choices to restore balance:
1.  **Delete the current 'a'**: We increment our total deletions by 1.
2.  **Keep the current 'a'**: We must delete **every** `'b'` we have seen so far.

By using the `min()` function at each step, we decide which sacrifice is cheaper: deleting the new `'a'` or deleting all previous `'b'`s.

---

## Complexity Analysis
| Constraint | Complexity | Why? |
| :--- | :--- | :--- |
| **Time Complexity** | $O(N)$ | We traverse the string exactly once. |
| **Space Complexity** | $O(1)$ | We only use two integer variables (`res` and `b_count`). |

---

## Implementation (C++)

```cpp
class Solution {
public:
    int minimumDeletions(string s) {
        int deletions = 0; // The minimum deletions needed so far
        int b_count = 0;   // The number of 'b's encountered so far
        
        for (char c : s) {
            if (c == 'b') {
                // If we see a 'b', increment the counter.
                // It doesn't break balance yet.
                b_count++;
            } else {
                // If we see an 'a', it potentially breaks balance.
                // We take the minimum of:
                // 1. Deleting this 'a' (previous deletions + 1)
                // 2. Keeping this 'a' (must delete all previous 'b's)
                deletions = min(deletions + 1, b_count);
            }
        }
        
        return deletions;
    }
};
