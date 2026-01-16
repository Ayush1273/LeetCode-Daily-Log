# 83. Remove Duplicates from Sorted List

## Idea
Since the linked list is already **sorted**, duplicate values always appear **next to each other**.

We traverse the list once:
- If the current node and next node have the same value, skip the next node
- Otherwise, move forward

This ensures each value appears only once.

---

## Complexity
**Time:** `O(n)`  
**Space:** `O(1)`

---

## Code
```python
class Solution(object):
    def deleteDuplicates(self, head):
        curr = head

        while curr and curr.next:
            if curr.val == curr.next.val:
                curr.next = curr.next.next
            else:
                curr = curr.next

        return head
