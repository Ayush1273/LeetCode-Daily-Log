# 744. Find Smallest Letter Greater Than Target

## Problem Description
You are given an array of characters `letters` that is sorted in **non-decreasing order**, and a character `target`. There are at least two different characters in `letters`.

Return the smallest character in `letters` that is lexicographically greater than `target`. If such a character does not exist, return the first character in `letters`.

### Example Cases
| Input `letters` | Input `target` | Output | Explanation |
| :--- | :--- | :--- | :--- |
| `["c","f","j"]` | `"a"` | `"c"` | 'c' is the smallest char > 'a'. |
| `["c","f","j"]` | `"c"` | `"f"` | 'f' is the smallest char > 'c'. |
| `["x","x","y","y"]` | `"z"` | `"x"` | No char > 'z', so wrap around to index 0. |

---

## Approach: Binary Search
Since the array is already sorted, a linear scan would take $O(n)$ time. We can optimize this to $O(\log n)$ using **Binary Search**.

### Logic
1. We seek the **upper bound** of the `target`.
2. Even if we find a character that matches the `target`, we must continue searching to the **right** because we need a character strictly *greater* than the target.
3. **Pointers:** We use `low` and `high` pointers to narrow the search space.
4. **Modulo Wrap-around:** The problem states that if no such character exists, we return `letters[0]`. Since the loop terminates with `low` pointing to the insertion index, `letters[low % len(letters)]` will correctly return the first element if `low` exceeds the array bounds.



---

## Complexity Analysis
* **Time Complexity:** $O(\log n)$ .
* **Space Complexity:** $O(1)$.

---

## Implementation (Python)

```python
class Solution(object):
    def nextGreatestLetter(self, letters, target):
        """
        :type letters: List[str]
        :type target: str
        :rtype: str
        """
        low = 0
        high = len(letters) - 1
        
        # Standard Binary Search for Upper Bound
        while low <= high:
            mid = low + (high - low) // 2
            
            if letters[mid] <= target:
                # If current letter is target or smaller, 
                # we need to look in the right half.
                low = mid + 1
            else:
                # If current letter is greater, 
                # it could be the answer, but check left for smaller options.
                high = mid - 1
        
        # If low is equal to len(letters), the modulo returns letters[0]
        return letters[low % len(letters)]
