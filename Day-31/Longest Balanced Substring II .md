# 3714. Longest Balanced Substring II 

## Problem Description
Given a string `s` consisting only of characters `'a'`, `'b'`, and `'c'`, find the length of the **longest balanced substring**.

A substring is **balanced** if all distinct characters in it appear the **same number of times**. This includes:
1. **One distinct character:** (e.g., `"aaa"`, `"bb"`)
2. **Two distinct characters:** (e.g., `"aabb"`, `"ccaa"`) - the third character must be absent.
3. **Three distinct characters:** (e.g., `"abcabc"`, `"aabbcc"`)

---

##  The Strategy: Category Separation ($O(N)$)
Trying to solve all cases in one pass is complex. Instead, we split the problem into three independent, linear sub-problems and take the maximum result.

### 1. Single Letter Streaks
We find the longest continuous run of the same character.
- **Logic:** Iterate through the string and increment a counter if the current character matches the previous one.

### 2. Two-Letter Balance (The "Wall" Technique)
To find the balance between exactly two letters (e.g., 'a' and 'b'):
- Treat the third letter ('c') as a **wall**. If we hit 'c', the current window is invalid and must reset.
- Use a **Prefix Difference Array**: Track `count(a) - count(b)`. If the same difference appears twice within a segment, the substring between those indices is balanced.

### 3. Three-Letter Balance (The Hashing Trick)
To find when 'a', 'b', and 'c' are all equal:
- We use a **Weighted State**: Assign weights $W_a = 1,000,001$, $W_b = -1,000,000$, and $W_c = -1$.
- Because $W_a + W_b + W_c = 0$, the "state" only repeats when each character has been added an equal number of times. We track these states using a Hash Map.

---

## Complexity Analysis
| Category | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Overall** | $O(N)$ | $O(N)$ |

- **Time:** We perform 5 linear passes (1 for single runs, 3 for pairs, 1 for triplets).
- **Space:** $O(N)$ to store the prefix difference array and the state hash map.

---

##  C++ Implementation

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
#include <unordered_map>

using namespace std;

class Solution {
public:
    int longestBalanced(string s) {
        int n = s.length();
        if (n == 0) return 0;

        // --- STEP 1: Single letter runs ---
        int cur_run = 0, res = 0;
        for (int i = 0; i < n; i++) {
            cur_run = (i > 0 && s[i] == s[i-1]) ? cur_run + 1 : 1;
            res = max(res, cur_run);
        }

        // --- STEP 2: Exactly two letters ---
        res = max({res, find2(s, 'a', 'b'), find2(s, 'a', 'c'), find2(s, 'b', 'c')});

        // --- STEP 3: All three letters ---
        res = max(res, find3(s));

        return res;
    }

private:
    // Helper to find balance between exactly two characters
    int find2(const string& s, char x, char y) {
        int n = s.length(), max_len = 0;
        vector<int> first(2 * n + 1, -2); // Map diffs to array indices
        
        int clear_idx = -1, diff = n; // Offset n to handle negative diffs
        first[diff] = -1; 

        for (int i = 0; i < n; i++) {
            if (s[i] != x && s[i] != y) {
                clear_idx = i; // Reset segment on forbidden character
                diff = n;
                first[diff] = clear_idx;
            } else {
                if (s[i] == x) diff++; else diff--;

                if (first[diff] < clear_idx) {
                    first[diff] = i; // New occurrence of this diff in current segment
                } else {
                    max_len = max(max_len, i - first[diff]);
                }
            }
        }
        return max_len;
    }

    // Helper to find balance between all three characters
    int find3(const string& s) {
        long long state = 0; 
        unordered_map<long long, int> first;
        first[0] = -1;
        int max_len = 0;

        for (int i = 0; i < s.length(); i++) {
            // Weights sum to zero to detect equal growth
            if (s[i] == 'a') state += 1000001LL;
            else if (s[i] == 'b') state -= 1000000LL;
            else if (s[i] == 'c') state -= 1LL;

            if (first.count(state)) {
                max_len = max(max_len, i - first[state]);
            } else {
                first[state] = i;
            }
        }
        return max_len;
    }
};
