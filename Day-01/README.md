# Day 1 – LeetCode Practice

Today I solved two LeetCode problems covering both **advanced geometry** and **basic dynamic programming**.

---

## Problems Solved

### 1. Separate Squares II (Hard)
In this problem, we are given multiple squares on a 2D plane, where some squares may overlap.  
The task is to find the minimum y-coordinate of a horizontal line such that the total area above the line is equal to the total area below it, while counting overlapping regions only once.

To solve this, the plane is divided into horizontal slabs using the top and bottom edges of the squares.  
For each slab, overlapping x-intervals are merged to compute the correct union area.  
By accumulating the area from bottom to top, the exact y-coordinate where the area gets split equally is calculated.

---

### 2. Climbing Stairs (Easy)
This is a classic dynamic programming problem.  
Given `n` steps, at each step you can climb either 1 or 2 steps.

The number of ways to reach the top depends on the number of ways to reach the previous two steps.  
This leads to a Fibonacci-style solution where each step builds upon the results of the last two steps.

---

Both problems helped practice **problem decomposition**, **pattern recognition**, and writing **clean, explainable solutions**.
