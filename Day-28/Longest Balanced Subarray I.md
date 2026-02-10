# 3719. Longest Balanced Subarray I

## Problem Description
You are given an integer array `nums`. A subarray is considered **balanced** if the number of **distinct even** integers equals the number of **distinct odd** integers.

Your goal is to return the length of the **longest** such balanced subarray.

---

## Logic & Approach
Given the constraints ($N \le 1500$), an $O(N^2)$ approach is efficient enough to pass. We can check every possible subarray and count the distinct even and odd numbers.

1.  **Iterate through all subarrays:** Use a nested loop where the outer loop marks the `start` and the inner loop marks the `end` of the subarray.
2.  **Track Distinct Numbers:** For each starting point, use two sets (or boolean arrays/hash maps) to keep track of the distinct even and odd numbers encountered as you expand the subarray.
3.  **Check Balance:** At every step of the inner loop:
    * Count how many distinct even numbers are in the set.
    * Count how many distinct odd numbers are in the set.
    * If `even_count == odd_count`, update the `max_length`.

---

## Complexity Analysis
| Metric | Complexity | Why? |
| :--- | :--- | :--- |
| **Time Complexity** | $O(N^2)$ | We use two nested loops to explore all possible subarrays. |
| **Space Complexity** | $O(D)$ | Where $D$ is the number of distinct elements in the subarray, stored in sets. |

---

## Implementation (C++)

```cpp
class Solution {
public:
    int longestBalanced(vector<int>& nums) {
        int n = nums.size();
        int maxLength = 0;

        // Iterate over every possible starting point of a subarray
        for (int i = 0; i < n; ++i) {
            unordered_set<int> distinctEvens;
            unordered_set<int> distinctOdds;

            // Expand the subarray from the starting point i to j
            for (int j = i; j < n; ++j) {
                if (nums[j] % 2 == 0) {
                    distinctEvens.insert(nums[j]);
                } else {
                    distinctOdds.insert(nums[j]);
                }

                // If number of distinct evens equals distinct odds, it's balanced
                if (distinctEvens.size() == distinctOdds.size()) {
                    maxLength = max(maxLength, j - i + 1);
                }
            }
        }

        return maxLength;
    }
};
