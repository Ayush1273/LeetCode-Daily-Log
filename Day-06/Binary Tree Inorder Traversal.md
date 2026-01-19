# 94. Binary Tree Inorder Traversal

## Problem Summary
Given the root of a binary tree, return the **inorder traversal** of its nodes’ values.

Inorder traversal follows the order:
**Left → Root → Right**

---

## Intuition
The recursive solution is straightforward, but the iterative approach helps understand how recursion actually works behind the scenes.

By using a **stack**, we can simulate the recursive call stack and control the traversal manually.

---

## Approach (Iterative)
1. Start from the root.
2. Keep moving left and push nodes onto a stack.
3. When no left child exists:
   - Pop from the stack
   - Add the node’s value to the result
4. Move to the right child and repeat.

This ensures nodes are visited in **Left → Root → Right** order.

---

## Time Complexity
- Each node is visited once

**O(n)**

---

## Space Complexity
- Stack stores nodes up to the height of the tree

**O(h)**  
(Worst case `O(n)` for a skewed tree)

---

## Code

```python
class Solution(object):
    def inorderTraversal(self, root):
        res = []
        stack = []
        curr = root
        
        while curr or stack:
            while curr:
                stack.append(curr)
                curr = curr.left
            
            curr = stack.pop()
            res.append(curr.val)
            
            curr = curr.right
        
        return res
