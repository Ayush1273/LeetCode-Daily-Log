# LeetCode Daily Progress: Bit Manipulation & Greedy Algorithms

## Summary:
Today I tackled two Medium-level problems focusing on array optimization and bitwise logic. The goal was to achieve maximum efficiency with $O(n)$ time complexity.

---

##  Problem 1: Minimize Maximum Pair Sum
**Goal:** Pair up numbers in an even-sized array to make the largest sum as small as possible.

### What I Learned:
* **The Greedy Strategy:** To minimize a maximum, you must balance the extremes. By pairing the **smallest** number with the **largest** number, you prevent any single pair from "exploding" in value.
* **Two-Pointer Technique:** Sorting the array and using two pointers (one at the start, one at the end) is an efficient way to process pairs.

**TC:** $O(n \log n)$ (due to sorting) 
**SC:** $O(1)$

---

##  Problem 2: [260] Single Number III
**Goal:** In an array where every number appears twice except for two unique ones, find those two numbers using constant space.

###  What I Learned:
* **XOR Magic:** XORing a number with itself results in 0. This is the ultimate tool for finding "lonely" numbers in a sea of pairs.
* **Bit Partitioning:** When you have two unique numbers ($A$ and $B$), XORing everything gives you $A \oplus B$. To separate them, you find the first bit where they differ and use that as a filter to split the array into two groups.
* **The `n & -n` Trick:** I learned this bitwise "magic" to instantly find the rightmost set bit in a number.

**TC:** $O(n)$ 
**SC:** $O(1)$

