# 3047. Find the Largest Area of Square Inside Two Rectangles

## Problem Summary
Given multiple axis-aligned rectangles, find the **maximum area of a square** that can fit inside the **intersection of at least two rectangles**.  
If no such square exists, return `0`.

---

## Key Idea
- The intersection of two rectangles is also a rectangle.
- The **largest square** that fits inside an intersection is limited by the **smaller of its width and height**.
- So, for every pair of rectangles:
  - Compute their intersection
  - Find the largest possible square inside it
  - Track the maximum area

---

## Approach
1. Iterate through all unique pairs of rectangles.
2. Compute the intersection rectangle using max of bottom-left and min of top-right coordinates.
3. If the intersection is valid:
   - Square side = `min(width, height)`
4. Keep updating the maximum square side found.
5. Return the square of the maximum side.

---

## Time Complexity
Let `n` be the number of rectangles.

- We check all pairs: `O(n²)`

**Overall:** `O(n²)`

---

## Space Complexity
- Uses only a few variables

**Overall:** `O(1)`

---

## Code

```python
class Solution(object):
    def largestSquareArea(self, bottomLeft, topRight):
        max_side = 0
        n = len(bottomLeft)
        
        for i in range(n):
            for j in range(i + 1, n):
                # Intersection boundaries
                x1 = max(bottomLeft[i][0], bottomLeft[j][0])
                y1 = max(bottomLeft[i][1], bottomLeft[j][1])
                x2 = min(topRight[i][0], topRight[j][0])
                y2 = min(topRight[i][1], topRight[j][1])
                
                width = x2 - x1
                height = y2 - y1
                
                if width > 0 and height > 0:
                    max_side = max(max_side, min(width, height))
        
        return max_side * max_side
