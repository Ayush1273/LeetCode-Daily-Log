# 1404. Number of Steps to Reduce a Binary Number to One

## Problem Summary
Given a binary string `s`, reduce it to `1` using these rules:
- If even (ends in `0`), divide by 2.
- If odd (ends in `1`), add 1.

## Approach: Right-to-Left Simulation
Since the binary number can be massive, we process it from the Least Significant Bit (LSB) to the Most Significant Bit (MSB) using a `carry` variable.

1. **Iteration:** Traverse the string from `n-1` down to `1`.
2. **Logic:**
   - If `s[i] + carry` is **1** (Odd): We need to add 1 and then divide. This takes **2 steps**. A `carry` is generated for the next position.
   - If `s[i] + carry` is **0 or 2** (Even): We only need to divide. This takes **1 step**.
3. **Termination:** The loop stops before the first character. If a `carry` exists at the end, it means the leading `1` became `10`, requiring one final division step.

## Complexity
- **Time Complexity:** $O(N)$.
- **Space Complexity:** $O(1)$.

---

## Code (C++)
```cpp
class Solution {
public:
    int numSteps(string s) {
        int steps = 0, carry = 0;
        for (int i = s.size() - 1; i > 0; i--) {
            if ((s[i] - '0') + carry == 1) {
                steps += 2;
                carry = 1;
            } else {
                steps++;
            }
        }
        return steps + carry;
    }
};
