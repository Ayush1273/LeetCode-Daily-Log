# 3510. Minimum Pair Removal to Sort Array II

## Problem Description
Given an array `nums`, you can perform the following operation any number of times:
1. Select the **adjacent pair** with the **minimum sum** in `nums`.
2. If multiple such pairs exist, choose the **leftmost** one.
3. Replace the pair with their sum.

Return the **minimum number of operations** needed to make the array **non-decreasing**.

**Constraints:**
- $N = 10^5$
- Values up to $10^9$

---

## My Approach: Efficient Simulation

Because the array size is now $10^5$, the $O(N^2)$ approach will fail. We need to find the minimum pair and update the neighbors in logarithmic time.

### 1. The "Linked List" Strategy
When we merge two numbers, their neighbors change. I used a **Doubly Linked List** structure (using a `Node` class) so that when a pair is removed, we can bridge the remaining neighbors in $O(1)$ time.

### 2. The Min-Heap (Priority Queue)
To quickly find the leftmost minimum sum, I used a **Min-Heap**. 
- It stores: `(sum, left_node_index)`.
- If two sums are equal, the `left_node_index` ensures we pick the leftmost one first.

### 3. Inversion Tracking
Instead of checking if the whole array is sorted ($O(N)$) after every single merge, I track the number of **inversions** (where `nums[i] > nums[i+1]`).
- When `inversions == 0`, we know the array is sorted and can stop immediately.

### 4. Handling Stale Data
Since nodes change values or get deleted, the Heap will contain "old" sums. I implemented a validation check: when popping from the heap, we verify the nodes still exist and the sum is still accurate before processing.

---

## Complexity Analysis
* **Time Complexity**: $O(N \log N)$
  - Each merge reduces the number of nodes by one. 
  - Each Heap push/pop operation is $O(\log N)$.
* **Space Complexity**: $O(N)$
  - We store $N$ nodes and up to $O(N)$ entries in the Priority Queue.

---

## Final Code Implementation

```python
import heapq

class Node:
    def __init__(self, val, idx):
        self.val = val
        self.idx = idx
        self.prev = None
        self.next = None
        self.removed = False

class Solution(object):
    def minimumPairRemoval(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        n = len(nums)
        if n < 2:
            return 0
            
        nodes = [Node(nums[i], i) for i in range(n)]
        for i in range(n):
            if i > 0: nodes[i].prev = nodes[i-1]
            if i < n - 1: nodes[i].next = nodes[i+1]
            
        pq = []
        for i in range(n - 1):
            heapq.heappush(pq, (nodes[i].val + nodes[i+1].val, i))
            
        # Efficiently track if the array is sorted
        inversions = 0
        for i in range(n - 1):
            if nums[i] > nums[i+1]:
                inversions += 1
        
        ops = 0
        while inversions > 0 and pq:
            s, idx = heapq.heappop(pq)
            left = nodes[idx]
            right = left.next
            
            # Validation: Skip if nodes are removed or the sum is outdated
            if left.removed or right is None or right.removed or (left.val + right.val != s):
                continue
            
            # Remove inversions related to these two nodes before merging
            if left.prev and left.prev.val > left.val: inversions -= 1
            if left.val > right.val: inversions -= 1
            if right.next and right.val > right.next.val: inversions -= 1
            
            # Perform the merge
            left.val = s
            right.removed = True
            
            # Link the neighbors around the removed node
            new_next = right.next
            left.next = new_next
            if new_next:
                new_next.prev = left
            
            # Add back new inversions created by the new sum
            if left.prev and left.prev.val > left.val: inversions += 1
            if left.next and left.val > left.next.val: inversions += 1
            
            # Push new possible pairs to the heap
            if left.prev:
                heapq.heappush(pq, (left.prev.val + left.val, left.prev.idx))
            if left.next:
                heapq.heappush(pq, (left.val + left.next.val, left.idx))
                
            ops += 1
            
        return ops
