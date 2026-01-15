# 2943. Maximize Area of Square Hole in Grid

##  Problem Description
You are given a grid with $n + 2$ horizontal and $m + 2$ vertical bars. You can remove specific bars listed in `hBars` and `vBars`. The goal is to find the area of the **largest square-shaped hole** that can be created by removing these bars.

##  Intuition
To create a "hole" in the grid, we must remove **consecutive** bars.
* If we remove 1 bar (e.g., bar 2), we merge the space between 1-2 and 2-3, creating a gap of 2 units.
* If we remove 2 consecutive bars (e.g., bars 2 and 3), we merge the spaces between 1-2, 2-3, and 3-4, creating a gap of 3 units.
* **Formula:** If we remove $k$ consecutive bars, we create a gap of $k + 1$ units.





---

##  Implementation Steps
1. **Sort the Bars:** We sort `hBars` and `vBars` to easily identify consecutive sequences.
2. **Find Max Consecutive Sequence:** - Iterate through the sorted bars.
   - Count how many bars follow each other numerically (e.g., $2, 3, 4$).
   - Keep track of the longest sequence found.
3. **Calculate Max Gaps:** The maximum gap for either dimension is `longest_consecutive_sequence + 1`.
4. **Determine Square Area:** Find the minimum of the two max gaps and square it.

---

##  Complexity Analysis
* **Time Complexity:** $O(H \log H + V \log V)$
  - Sorting `hBars` takes $O(H \log H)$, where $H$ is the length of `hBars`.
  - Sorting `vBars` takes $O(V \log V)$, where $V$ is the length of `vBars`.
  - The linear scan to find consecutive sequences takes $O(H + V)$.
* **Space Complexity:** $O(1)$ or $O(H+V)$ 
  - Python's `sort()` (Timsort) uses $O(N)$ auxiliary space in the worst case.

---

##  Solution

```python
class Solution(object):
    def maximizeSquareHoleArea(self, n, m, hBars, vBars):
        """
        :type n: int
        :type m: int
        :type hBars: List[int]
        :type vBars: List[int]
        :rtype: int
        """
        def get_max_gap(bars):
            # 1. Sort bars to find consecutive sequences
            bars.sort()
            max_consecutive = 1
            current_consecutive = 1
            
            for i in range(1, len(bars)):
                # 2. Check if the current bar is consecutive to the previous one
                if bars[i] == bars[i-1] + 1:
                    current_consecutive += 1
                else:
                    current_consecutive = 1
                max_consecutive = max(max_consecutive, current_consecutive)
            
            # 3. Gap size = number of consecutive bars removed + 1
            return max_consecutive + 1

        # Get the maximum possible gap in both directions
        max_h = get_max_gap(hBars)
        max_v = get_max_gap(vBars)
        
        # 4. The side of the square is limited by the smaller gap
        side = min(max_h, max_v)
        
        return side * side
