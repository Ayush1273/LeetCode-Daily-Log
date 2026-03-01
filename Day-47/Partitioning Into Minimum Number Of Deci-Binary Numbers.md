# 1689. Partitioning Into Minimum Number Of Deci-Binary Numbers

## Problem Summary
A **deci-binary** number consists only of digits `0` and `1`. Given a string `n`, find the minimum number of deci-binary numbers that sum up to `n`.

## Approach: The Maximum Digit Rule
The problem is simpler than it looks. Because each deci-binary number can contribute at most a `1` to any decimal slot:
1. To form a digit `d` at any position, you need at least `d` deci-binary numbers.
2. The total number of deci-binary numbers required is determined by the **largest digit** present in the string `n`.

**Example:** For `n = "82734"`, the largest digit is `8`. We need 8 deci-binary numbers because the first digit `8` requires eight `1`s to be summed.

## Complexity
- **Time Complexity:** $O(L)$ — Linear scan of the string of length $L$.
- **Space Complexity:** $O(1)$ — Only one integer used for tracking the maximum.

---

## Code (Java)
```java
class Solution {
    public int minPartitions(String n) {
        int max = 0;
        for (char c : n.toCharArray()) {
            max = Math.max(max, c - '0');
            if (max == 9) break; // Optimization: Cannot exceed 9
        }
        return max;
    }
}
