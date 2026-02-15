# 67. Add Binary 

## Problem Summary
Given two binary strings `a` and `b`, return their sum as a new binary string.

## Approach: Manual Simulation
Since binary strings can be very long, we simulate addition bit-by-bit from right to left, just like decimal addition.

1. **Pointers:** Use two pointers starting at the ends of `a` and `b`.
2. **Carry:** Maintain a `carry` variable. In binary:
   - $1 + 1 = 0$ with carry $1$
   - $1 + 1 + 1 = 1$ with carry $1$
3. **Building the String:** Append the result of `(sum % 2)` to our result string and update the carry with `(sum / 2)`.
4. **Final Step:** Reverse the result string since we built it from the least significant bit to the most significant bit.

## Complexity
- **Time:** $O(\max(N, M))$ — Linear traversal of the longer string.
- **Space:** $O(\max(N, M))$ — Space for the output string.

---

## C++ Code
```cpp
class Solution {
public:
    string addBinary(string a, string b) {
        string res;
        int i = a.size() - 1, j = b.size() - 1, carry = 0;
        while(i >= 0 || j >= 0 || carry) {
            int sum = carry;
            if(i >= 0) sum += a[i--] - '0';
            if(j >= 0) sum += b[j--] - '0';
            res += to_string(sum % 2);
            carry = sum / 2;
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
