# 401. Binary Watch

## Problem Summary
A binary watch represents hours (0-11) with 4 LEDs and minutes (0-59) with 6 LEDs. Given the number of LEDs currently turned on, return all possible times the watch could be displaying.

## Approach: Brute Force Enumeration
Because the total number of valid times is very small (720), the simplest and most readable way to solve this is to check every possible time.

1. **Double Loop:** Iterate through hours `0-11` and minutes `0-59`.
2. **Bit Counting:** Use the popcount operation (counting the number of `1`s in a binary number) to see how many LEDs are required for the current hour and minute.
3. **Condition:** If `popcount(hour) + popcount(minute) == turnedOn`, the time is valid.
4. **Formatting:** Ensure minutes are formatted with a leading zero (e.g., `:05` instead of `:5`).

## Complexity
- **Time:** $O(1)$ — The input size doesn't change the number of iterations (720 fixed).
- **Space:** $O(1)$ — Only the output list is stored.

---

## C++ Code
```cpp
class Solution {
public:
    vector<string> readBinaryWatch(int turnedOn) {
        vector<string> result;
        for (int h = 0; h < 12; ++h) {
            for (int m = 0; m < 60; ++m) {
                if (__builtin_popcount(h) + __builtin_popcount(m) == turnedOn) {
                    result.push_back(to_string(h) + (m < 10 ? ":0" : ":") + to_string(m));
                }
            }
        }
        return result;
    }
};
