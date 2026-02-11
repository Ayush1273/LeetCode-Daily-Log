# 3721. Longest Balanced Subarray II 

## Problem Summary
We need to find the **longest subarray** where the number of **distinct** even integers equals the number of **distinct** odd integers.

- **The Challenge:** Standard sliding windows fail because the "distinct" requirement is non-monotonic (adding/removing numbers doesn't change the count linearly).

---

## Key Strategy: Segment Tree ($O(N \log N)$)

Instead of checking every window, we treat the problem as a **dynamic prefix sum** problem.

### 1. The "First Occurrence" Trick
For any subarray starting at index `L`, only the **first** time a number appears to the right of `L` matters. 
- Even number = `+1`
- Odd number = `-1`
- Duplicate = `0`

### 2. Backward Iteration
We move from right to left. If we encounter a number we've seen before, we "move" its contribution:
- Set the old position's value to `0`.
- Set the current (new) position to `+1` or `-1`.

### 3. Smart Searching
We use a **Segment Tree** to track these values. By storing the **minimum** and **maximum** prefix sums in each node, we can search the tree in $O(\log N)$ to find the rightmost index where the total balance is exactly `0`.

---

## Complexity Analysis
| Complexity | Estimation | Why? |
| :--- | :--- | :--- |
| **Time** | $O(N \log N)$ | Single pass over $N$ elements with $\log N$ tree updates. |
| **Space** | $O(N)$ | Storing tree nodes and a hash map for positions. |

---

##  Solution (C++)

```cpp
#include <vector>
#include <unordered_map>
#include <algorithm>

using namespace std;

class SegmentTree {
    int n;
    vector<int> sum, minV, maxV;

    void pull(int node) {
        int l = node * 2, r = node * 2 + 1;
        sum[node] = sum[l] + sum[r];
        minV[node] = min(minV[l], sum[l] + minV[r]);
        maxV[node] = max(maxV[l], sum[l] + maxV[r]);
    }

public:
    SegmentTree(int n) : n(n), sum(4 * n, 0), minV(4 * n, 0), maxV(4 * n, 0) {}

    void update(int idx, int val, int node, int l, int r) {
        if (l == r) {
            sum[node] = minV[node] = maxV[node] = val;
            return;
        }
        int m = l + (r - l) / 2;
        if (idx <= m) update(idx, val, node * 2, l, m);
        else update(idx, val, node * 2 + 1, m + 1, r);
        pull(node);
    }

    int findRight(int node, int l, int r, int curSum, int target) {
        if (l == r) return l;
        int m = l + (r - l) / 2;
        int lChild = node * 2, rChild = node * 2 + 1;
        
        // Greedily check right child first to find the longest subarray
        int rightSum = curSum + sum[lChild];
        if (minV[rChild] <= target - rightSum && target - rightSum <= maxV[rChild])
            return findRight(rChild, m + 1, r, rightSum, target);
        
        return findRight(lChild, l, m, curSum, target);
    }

    int query(int target) {
        if (minV[1] > target || maxV[1] < target) return -1;
        return findRight(1, 0, n - 1, 0, target);
    }
};

class Solution {
public:
    int longestBalanced(vector<int>& nums) {
        int n = nums.size();
