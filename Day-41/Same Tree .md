# 100. Same Tree 

## Problem Summary
Check if two binary trees `p` and `q` are identical. Identical trees must have the same structure and the same values at every node.

## Approach: Recursion (DFS)
We can solve this by traversing both trees simultaneously using Depth First Search (DFS).

1. **Base Cases:**
   - Both nodes are `null` → Return `true` (Reached the end of identical branches).
   - One node is `null`, the other is not → Return `false` (Structural mismatch).
   - Node values are different → Return `false` (Value mismatch).
2. **Recursive Step:**
   - If the current nodes match, recursively check:
     - `isSameTree(p->left, q->left)`
     - `isSameTree(p->right, q->right)`
   - Both must be `true` for the trees to be the same.

## Complexity
- **Time Complexity:** $O(N)$.
- **Space Complexity:** $O(H)$.

---

## Code (C++)
```cpp
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (!p && !q) return true;
        if (!p || !q || p->val != q->val) return false;
        return isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
    }
};
