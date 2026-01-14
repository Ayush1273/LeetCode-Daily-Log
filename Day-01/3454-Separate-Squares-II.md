# 3454. Separate Squares II (Hard)

## Problem Statement
We are given multiple squares on a 2D plane.  
Each square is represented by:
- `(x, y)` → bottom-left coordinate  
- `l` → side length  

The goal is to find the **minimum y-coordinate of a horizontal line** such that:
- The total area **above** the line is equal to the total area **below** the line
- If squares overlap, the overlapping area should be counted **only once**

---

## Intuition
Instead of thinking in full 2D, I noticed that the total covered area only changes at the **top and bottom edges of squares** (`y` and `y + l`).

So if we cut the plane horizontally at those y-values, the shape of the covered area inside each horizontal strip remains the same.

This allows us to process the problem **strip by strip** from bottom to top.

---

## Approach (Step by Step)

1. **Collect all important y-coordinates**
   - For every square, take `y` and `y + l`
   - Sort them to form horizontal slabs

2. **Process each horizontal slab**
   - For a slab, find all squares that cover it
   - For those squares, collect their x-intervals
   - Merge overlapping x-intervals to avoid double counting
   - The merged width × slab height gives the slab area

3. **Accumulate area from bottom**
   - First calculate the total union area
   - Then move slab by slab from the bottom
   - Stop when cumulative area reaches half of the total
   - Interpolate inside the slab to get the exact y-value

---

## Why This Works
- Converts a difficult 2D geometry problem into simpler 1D interval merging
- Handles overlapping squares correctly
- Avoids brute-force or grid-based approaches
- Efficient even for large inputs

---

## Time and Space Complexity
- **Time Complexity:** `O(N log N)`  
  (mainly due to sorting and interval merging)
- **Space Complexity:** `O(N)`  
  (for storing slabs and intervals)

---

## Python Implementation

```python
class Solution(object):
    def separateSquares(self, squares):
        # Collect all y boundaries
        y_coords = set()
        for x, y, l in squares:
            y_coords.add(y)
            y_coords.add(y + l)
        sorted_y = sorted(y_coords)
        
        # Map y value to index
        y_map = {y: i for i, y in enumerate(sorted_y)}
        
        # Each slab stores x-intervals covering it
        slabs = [[] for _ in range(len(sorted_y) - 1)]
        
        for x, y, l in squares:
            for i in range(y_map[y], y_map[y + l]):
                slabs[i].append((x, x + l))
        
        total_area = 0.0
        widths = [0] * (len(sorted_y) - 1)
        
        # Compute area of each slab
        for i, intervals in enumerate(slabs):
            if not intervals:
                continue
            
            intervals.sort()
            start, end = intervals[0]
            width = 0
            
            for ns, ne in intervals[1:]:
                if ns < end:
                    end = max(end, ne)
                else:
                    width += end - start
                    start, end = ns, ne
            
            width += end - start
            widths[i] = width
            total_area += width * (sorted_y[i + 1] - sorted_y[i])
        
        # Find the y where area is split equally
        half = total_area / 2
        current = 0
        
        for i in range(len(widths)):
            slab_area = widths[i] * (sorted_y[i + 1] - sorted_y[i])
            if current + slab_area >= half:
                return sorted_y[i] + (half - current) / widths[i]
            current += slab_area
