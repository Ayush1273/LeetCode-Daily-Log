# 761. Special Binary String

## Problem Summary
A **Special Binary String** follows two rules:
1. Equal number of `0`s and `1`s.
2. Every prefix has at least as many `1`s as `0`s.

This is exactly like **Valid Parentheses** (`1` = `(`, `0` = `)`). We want to swap consecutive special substrings to make the string lexicographically largest.

## Approach: Recursive Sorting
The problem is equivalent to rearranging nested parentheses to get the "heaviest" structure at the front.

1. **Decomposition:** Iterate through the string and track the balance of `1`s and `0`s. When the balance hits 0, you've identified an independent "atomic" special string.
2. **Nesting:** Every atomic string is of the form `1` + `inner_special_string` + `0`.
3. **Recursion:** Recursively call the function on the `inner_special_string` to ensure all nested levels are also as large as possible.
4. **Greedy Sorting:** Once all top-level components are processed, sort them in descending order. This ensures the largest binary values appear first, maximizing the total string value.

## Complexity
- **Time:** $O(N^2)$.
- **Space:** $O(N)$.

---

## Code (C++)
```cpp
class Solution {
public:
    string makeLargestSpecial(string s) {
        vector<string> components;
        int balance = 0, start = 0;
        for (int i = 0; i < s.size(); ++i) {
            balance += (s[i] == '1' ? 1 : -1);
            if (balance == 0) {
                // Recurse on the middle part, wrap with 1 and 0
                components.push_back("1" + makeLargestSpecial(s.substr(start + 1, i - start - 1)) + "0");
                start = i + 1;
            }
        }
        sort(components.begin(), components.end(), greater<string>());
        string res = "";
        for (const string& sub : components) res += sub;
        return res;
    }
};
