# 3640. Trionic Array II - Maximum Sum Subarray

## Problem Overview
We need to find a contiguous subarray that follows a specific "Up-Down-Up" pattern:
1.  **Start:** Strictly increasing.
2.  **Middle:** Strictly decreasing.
3.  **End:** Strictly increasing.

The goal is to return the **maximum possible sum** of such a subarray.



## My Approach: "Hinge" Optimization
Since the array can have up to $10^5$ elements, we need a fast $O(n)$ solution. My approach focuses on the middle "decreasing" part and attaches the best possible wings to it.

### 1. Precomputing the "Wings"
* **Left Wing (`maxEndingAt`):** I walk through the array and calculate the best sum of an increasing sequence ending at each index. To keep it "trionic," this wing must have at least two numbers.
* **Right Wing (`maxStartingAt`):** I walk backward and calculate the best sum of an increasing sequence starting at each index. This also must have at least two numbers.

### 2. Finding the "Core"
* I look for every part of the array that is strictly decreasing.
* The start of this decrease is the **Peak ($p$)** and the end is the **Valley ($q$)**.

### 3. Combining for the Max Sum
For every decreasing core found:
* I check if there is a valid "Left Wing" ending at the peak.
* I check if there is a valid "Right Wing" starting at the valley.
* If both exist, I add them all together. (I subtract the peak and valley values once because they are shared by the wings and the core).

## Complexity
* **Time Complexity:** $O(n)$ 
* **Space Complexity:** $O(n)$ 

## Solution Code (C++)

```cpp
class Solution {
public:
    long long maxSumTrionic(vector<int>& nums) {
        int n = nums.size();
        long long INF = 2e18; 
        
        // 1. Calculate best increasing sums ending at each index (Left Wing)
        vector<long long> maxEndingAt(n, -INF);
        for (int i = 1; i < n; i++) {
            if (nums[i] > nums[i-1]) {
                long long startNew = (long long)nums[i] + nums[i-1];
                long long extendOld = (maxEndingAt[i-1] != -INF) ? maxEndingAt[i-1] + nums[i] : -INF;
                maxEndingAt[i] = max(startNew, extendOld);
            }
        }

        // 2. Calculate best increasing sums starting at each index (Right Wing)
        vector<long long> maxStartingAt(n, -INF);
        for (int i = n - 2; i >= 0; i--) {
            if (nums[i] < nums[i+1]) {
                long long startNew = (long long)nums[i] + nums[i+1];
                long long extendOld = (maxStartingAt[i+1] != -INF) ? maxStartingAt[i+1] + nums[i] : -INF;
                maxStartingAt[i] = max(startNew, extendOld);
            }
        }

        long long ans = -INF;

        // 3. Find decreasing segments and attach the best wings
        for (int i = 0; i < n - 1; ) {
            if (nums[i] > nums[i+1]) {
                int p = i;
                long long current_decreasing_sum = nums[p];
                int j = i;
                
                while (j + 1 < n && nums[j] > nums[j+1]) {
                    j++;
                    current_decreasing_sum += nums[j];
                    int q = j;

                    if (maxEndingAt[p] != -INF && maxStartingAt[q] != -INF) {
                        // Formula: Left Wing + (Decreasing Core minus overlapping hinges) + Right Wing
                        long long total = maxEndingAt[p] + (current_decreasing_sum - nums[p] - nums[q]) + maxStartingAt[q];
                        ans = max(ans, total);
                    }
                }
                i = j; 
            } else {
                i++;
            }
        }

        return ans;
    }
};
