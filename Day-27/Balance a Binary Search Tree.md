# 1382. Balance a Binary Search Tree

## Problem Description
Given the `root` of a binary search tree (BST), return a **balanced** binary search tree with the same node values. 

A binary search tree is balanced if the depth of the two subtrees of every node never differs by more than 1. If there are multiple valid answers, any of them is acceptable.

---

## Logic & Approach
Since the input is already a **Binary Search Tree**, we can take advantage of its properties to balance it in two main steps:

1.  **In-Order Traversal (Flattening):** * Perform an in-order traversal (`Left -> Root -> Right`) of the original tree.
    * Store the values (or node pointers) in a list. Because it's a BST, an in-order traversal will give us the values in **sorted order**.
    
2.  **Recursive Reconstruction (Building):**
    * To build a balanced tree from a sorted list, we use a technique similar to **Binary Search**.
    * Find the **middle element** of the current list and make it the `root`.
    * Recursively repeat the process for the left half of the list to build the `root->left`.
    * Recursively repeat the process for the right half of the list to build the `root->right`.



---

## Complexity Analysis
| Metric | Complexity | Why? |
| :--- | :--- | :--- |
| **Time Complexity** | $O(N)$ | $O(N)$ to traverse the tree and $O(N)$ to rebuild it. |
| **Space Complexity** | $O(N)$ | We store all $N$ node values in an array/vector. |

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
    vector<TreeNode*> nodes;

    // Step 1: In-order traversal to get nodes in sorted order
    void inOrder(TreeNode* root) {
        if (!root) return;
        inOrder(root->left);
        nodes.push_back(root);
        inOrder(root->right);
    }

    // Step 2: Build a balanced BST from the sorted nodes
    TreeNode* buildTree(int start, int end) {
        if (start > end) return nullptr;

        int mid = start + (end - start) / 2;
        TreeNode* root = nodes[mid];

        // Recursively build left and right subtrees
        root->left = buildTree(start, mid - 1);
        root->right = buildTree(mid + 1, end);

        return root;
    }

    TreeNode* balanceBST(TreeNode* root) {
        nodes.clear();
        inOrder(root);
        return buildTree(0, nodes.size() - 1);
    }
};
