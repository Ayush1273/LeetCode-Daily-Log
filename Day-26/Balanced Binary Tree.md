# 110. Balanced Binary Tree

## Problem Description
Given a binary tree, determine if it is **height-balanced**.

A height-balanced binary tree is defined as:
> A binary tree in which the left and right subtrees of every node differ in height by no more than 1.

---

## Logic & Approach
To solve this efficiently, we use a **Bottom-Up Depth First Search (DFS)**. 

---

### The Strategy:
1.  **Height Calculation:** We create a helper function that returns the height of a node.
2.  **Base Case:** An empty node (null) has a height of `0`.
3.  **Recursive Step:** * Recursively find the height of the left and right subtrees.
    * If any subtree is already unbalanced, we propagate a special value (like `-1`) all the way up.
    * At each node, check if the absolute difference between the left height and right height is greater than `1`.
    * If it is, the tree is unbalanced; return `-1`.
4.  **Final Result:** If the helper function returns anything other than `-1`, the tree is balanced.



---

## Complexity Analysis
| Metric | Complexity | Why? |
| :--- | :--- | :--- |
| **Time Complexity** | $O(N)$ | We visit each node exactly once. |
| **Space Complexity** | $O(H)$ | Where $H$ is the height of the tree, representing the recursion stack. |

---

## Implementation (C++)

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 * int val;
 * TreeNode *left;
 * TreeNode *right;
 * TreeNode() : val(0), left(nullptr), right(nullptr) {}
 * TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 * TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    // Helper function to calculate height and check balance simultaneously
    int checkHeight(TreeNode* root) {
        if (root == nullptr) return 0;

        // Check left subtree
        int leftHeight = checkHeight(root->left);
        if (leftHeight == -1) return -1; // Already unbalanced

        // Check right subtree
        int rightHeight = checkHeight(root->right);
        if (rightHeight == -1) return -1; // Already unbalanced

        // If the current node is unbalanced, return -1
        if (abs(leftHeight - rightHeight) > 1) return -1;

        // Otherwise, return the actual height
        return max(leftHeight, rightHeight) + 1;
    }

    bool isBalanced(TreeNode* root) {
        // If checkHeight returns -1, the tree is not balanced
        return checkHeight(root) != -1;
    }
};
