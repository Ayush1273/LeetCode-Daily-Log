# 2975. Maximum Square Area by Removing Fences From a Field

## Idea
A square can be formed only when the distance between two **horizontal fences**
matches the distance between two **vertical fences**.

So:
- Add boundary fences
- Compute all possible distances
- Find the largest common distance
- Square it to get the area

Return `-1` if no such distance exists.

---

## Complexity
**Time:** `O(h² + v²)`  
**Space:** `O(h² + v²)`

---

## Code
```python
class Solution(object):
    def maximizeSquareArea(self, m, n, hFences, vFences):
        MOD = 10**9 + 7

        hFences += [1, m]
        vFences += [1, n]

        def get_distances(fences):
            fences.sort()
            return {fences[j] - fences[i]
                    for i in range(len(fences))
                    for j in range(i + 1, len(fences))}

        h_dist = get_distances(hFences)
        v_dist = get_distances(vFences)

        max_side = max((d for d in h_dist if d in v_dist), default=-1)
        return -1 if max_side == -1 else (max_side * max_side) % MOD
