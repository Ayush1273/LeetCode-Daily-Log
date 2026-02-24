# 1022. Sum of Root To Leaf Binary Numbers 

## Problem Summary
Each root-to-leaf path in a binary tree represents a binary number. Given a tree where nodes are `0` or `1`, return the sum of all numbers represented by these paths.

## Approach: Depth First Search (DFS)
We can solve this by traversing the tree and building the binary value as we go.

1. **Building the Value:** As we descend from a parent to a child, we shift the accumulated value to the left by 1 (multiply by 2) and add the child's value. 
   - `new_val = (old_val << 1) | node->val`
2. **Identifying Leaves:** When we reach a node with no children, we have completed a path. This path's value is added to our total sum.
3. **Recursive Summing:** Each internal node returns the sum of values found in its left and right subtrees.

## Complexity
- **Time Complexity:** $O(N)$.
- **Space Complexity:** $O(H)$ — Recursion stack depth is equal to the tree height.

---

## Code (C++)
```cpp
class Solution {
public:
    int sumRootToLeaf(TreeNode* root, int val = 0) {
        if (!root) return 0;
        val = (val << 1) | root->val;
        if (!root->left && !root->right) return val;
        return sumRootToLeaf(root->left, val) + sumRootToLeaf(root->right, val);
    }
};
