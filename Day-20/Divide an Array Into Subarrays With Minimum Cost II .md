# 3013. Divide an Array Into Subarrays With Minimum Cost II

## Problem Summary

Given:
- An integer array `nums` of length `n`
- Two integers `k` and `dist`

You need to divide the array into **k contiguous subarrays**.  

### Cost Rule
The cost of a subarray is **its first element**.  
Total cost = sum of first elements of all subarrays.

### Constraint
The difference between the starting index of the **2nd subarray** and **k-th subarray** must satisfy: ik-1 - i1 <= dist


**Goal:** Return the **minimum possible total cost**.

---

## Key Idea (Approach)

1. **Fix the first subarray** at index 0. Its cost is always `nums[0]`.
2. For the remaining `k-1` subarrays:
   - The start positions must lie in the range `[i, i+dist]`
   - To minimize cost, pick the **smallest `k-1` elements** in this window.
3. Slide the window along the array and track the minimum sum.
4. Efficiently maintain the smallest `k-1` elements using **two multisets** (or sets):
   - `low` → smallest `k-1` elements
   - `high` → remaining elements
   - Maintain `sumLow` = sum of elements in `low`.
5. Whenever the window moves:
   - **Add** new element to the appropriate set
   - **Remove** the outgoing element
   - **Rebalance** to ensure `low` always contains exactly `k-1` smallest elements
6. Result = `nums[0] + min(sumLow across all windows)`

---

## Data Structure

- **multiset `low`**: stores smallest `k-1` elements in the current window  
- **multiset `high`**: stores remaining elements in the window  
- **sumLow**: sum of all elements in `low`  

**Operations:** `add(x)`, `remove(x)`, `rebalance()` → all **O(log(dist))**

---

## Complexity Analysis

| Metric | Value |
|--------|-------|
| Time   | O(n log dist) |
| Space  | O(dist) |


---

## C++ Implementation

```cpp
class Solution {
public:
    struct SmartWindow {
        int K;
        multiset<int> low, high;
        long long sumLow = 0;

        SmartWindow(int k) : K(k) {}

        int windowSize() const {
            return (int)low.size() + (int)high.size();
        }

        void rebalance() {
            int need = min(K, windowSize());

            while ((int)low.size() > need) {
                auto it = prev(low.end());
                int x = *it;
                low.erase(it);
                sumLow -= x;
                high.insert(x);
            }

            while ((int)low.size() < need && !high.empty()) {
                auto it = high.begin();
                int x = *it;
                high.erase(it);
                low.insert(x);
                sumLow += x;
            }
        }

        void add(int x) {
            if (low.empty() || x <= *prev(low.end())) {
                low.insert(x);
                sumLow += x;
            } else {
                high.insert(x);
            }
            rebalance();
        }

        void remove(int x) {
            auto itLow = low.find(x);
            if (itLow != low.end()) {
                low.erase(itLow);
                sumLow -= x;
            } else {
                auto itHigh = high.find(x);
                if (itHigh != high.end()) {
                    high.erase(itHigh);
                }
            }
            rebalance();
        }

        long long query() const {
            return sumLow;
        }
    };

    long long minimumCost(vector<int>& nums, int k, int dist) {
        int n = nums.size();
        k -= 1; // exclude first subarray

        SmartWindow window(k);

        for (int i = 1; i <= 1 + dist; i++) {
            window.add(nums[i]);
        }

        long long ans = window.query();

        for (int i = 2; i + dist < n; i++) {
            window.remove(nums[i - 1]);
            window.add(nums[i + dist]);
            ans = min(ans, window.query());
        }

        return ans + nums[0];
    }
};
