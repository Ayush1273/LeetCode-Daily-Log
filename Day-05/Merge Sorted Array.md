# 88. Merge Sorted Array

## Problem Summary
We are given two sorted arrays `nums1` and `nums2`.

- `nums1` has extra space at the end to hold all elements
- We must merge `nums2` into `nums1`
- The result should be stored **in-place** inside `nums1`

---

## Intuition
If we try to merge from the beginning, we would need to shift elements in `nums1`.

To avoid this, we merge **from the end**:
- Compare the largest elements
- Place the larger one at the back of `nums1`

This way, no data gets overwritten.

---

## Approach
1. Use three pointers:
   - `p1` → last valid element in `nums1`
   - `p2` → last element in `nums2`
   - `p` → last index of `nums1`
2. Compare elements from the back and place the larger one at position `p`
3. If elements remain in `nums2`, copy them into `nums1`

---

## Time Complexity
- Each element is processed once

**O(m + n)**

---

## Space Complexity
- No extra space used

**O(1)**

---

## Code

```python
class Solution(object):
    def merge(self, nums1, m, nums2, n):
        p1 = m - 1
        p2 = n - 1
        p = m + n - 1
        
        while p1 >= 0 and p2 >= 0:
            if nums1[p1] > nums2[p2]:
                nums1[p] = nums1[p1]
                p1 -= 1
            else:
                nums1[p] = nums2[p2]
                p2 -= 1
            p -= 1
        
        while p2 >= 0:
            nums1[p] = nums2[p2]
            p2 -= 1
            p -= 1
