# 3666. Minimum Operations to Equalize Binary String 

## Problem Summary

You are given a binary string `s` and an integer `k`.

In one operation, you must flip **exactly `k` different indices** (change `'0'` to `'1'` or `'1'` to `'0'`).

Your goal is to determine the **minimum number of operations** required to make the entire string consist only of `'1'` characters (i.e., no `'0'` remains).

---

## Strategy: "Zero-Count" BFS

Instead of tracking every possible binary string configuration (which would be `2^N`), we track only:

> The **number of zeros** in the string.

Since we can choose **any `k` indices**, the exact positions do not matter — only the **count of zeros** matters.

---

##  State Definition

- **State:** Current number of zeros `Z`
- `0 ≤ Z ≤ n`
- **Goal:** Reach `Z = 0`

---

##  Transition Logic (Range Transitions)

Suppose we currently have `Z` zeros.

We flip:
- `x` zeros → become ones
- `k - x` ones → become zeros  
  (because we must flip exactly `k` indices)

### New Zero Count

\[
Z_{new} = Z - x + (k - x)
\]

\[
Z_{new} = Z + k - 2x
\]

---

### Constraints on `x`

1. `x ≤ Z`  
   (cannot flip more zeros than available)

2. `k - x ≤ n - Z`  
   (cannot flip more ones than available)

---

### Important Parity Observation

\[
Z_{new} = Z + k - 2x
\]

Since `2x` is always even:

- All reachable states from `Z` in one move share the same parity.
- Either all are **even** or all are **odd**.

---

##  Optimization to O(N log N)

With `N ≤ 10^5`, checking all possible `x` for every state would cause `O(N^2)` time.

### Optimization Idea

- Use two `TreeSet`s:
  - One for **even** zero counts
  - One for **odd** zero counts
- Store only **unvisited states**
- Once visited → remove from the set

This guarantees:
- Each state is processed exactly once
- Each operation is `O(log N)`

---

##  Implementation

```java
import java.util.*;

class Solution {
    public int minOperations(String s, int k) {
        int n = s.length();
        int z0 = 0;
        
        // Count initial zeros
        for (int i = 0; i < n; i++) {
            if (s.charAt(i) == '0') z0++;
        }
        
        if (z0 == 0) return 0;

        // Separate unvisited counts by parity
        TreeSet<Integer> evenSet = new TreeSet<>();
        TreeSet<Integer> oddSet = new TreeSet<>();
        
        for (int i = 0; i <= n; i++) {
            if (i == z0) continue;
            if (i % 2 == 0) evenSet.add(i);
            else oddSet.add(i);
        }

        // BFS queue: {current_zero_count, steps}
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{z0, 0});

        while (!queue.isEmpty()) {
            int[] curr = queue.poll();
            int z = curr[0];
            int d = curr[1];

            // Determine how many zeros we can flip
            int minX = Math.max(0, k - (n - z));
            int maxX = Math.min(z, k);

            // Reachable range of new zero counts
            int L = z + k - 2 * maxX;
            int R = z + k - 2 * minX;
            
            // Select correct parity set
            TreeSet<Integer> targetSet = (L % 2 == 0) ? evenSet : oddSet;
            
            // Process all unvisited states in [L, R]
            Integer nextZ = targetSet.ceiling(L);
            while (nextZ != null && nextZ <= R) {
                if (nextZ == 0) return d + 1;
                
                queue.add(new int[]{nextZ, d + 1});
                targetSet.remove(nextZ);
                
                nextZ = targetSet.ceiling(L);
            }
        }

        return -1; // If 0 zeros is unreachable
    }
}
