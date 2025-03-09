# Remove Nth Node From End of List

## Problem Statement

Given the head of a linked list, remove the nth node from the end of the list and return its head.

**Example 1:**
```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

**Example 2:**
```
Input: head = [1], n = 1
Output: []
```

**Example 3:**
```
Input: head = [1,2], n = 1
Output: [1]
```

## Intuition

The key insight for this problem is to find the nth node from the end of the list in a single pass. We can use the two-pointer technique:

1. Use two pointers, `fast` and `slow`, initially both pointing to a dummy node that we place before the head.
2. Move the `fast` pointer n nodes ahead.
3. Then move both pointers at the same pace until `fast` reaches the end of the list.
4. At this point, `slow` will be pointing to the node just before the one we want to remove.
5. Skip the nth node by updating the `next` pointer of the `slow` node.

This approach allows us to find the node to remove in a single pass through the list.

## Visual Explanation

Let's visualize the solution with Example 1: `head = [1,2,3,4,5], n = 2`

```
Initial state:
dummy -> 1 -> 2 -> 3 -> 4 -> 5 -> NULL
 ^
 |
slow, fast

Step 1: Move fast pointer n nodes ahead
dummy -> 1 -> 2 -> 3 -> 4 -> 5 -> NULL
 ^                 ^
 |                 |
slow              fast

Step 2: Move both pointers until fast reaches the end
dummy -> 1 -> 2 -> 3 -> 4 -> 5 -> NULL
          ^                 ^
          |                 |
         slow              fast

Step 3: Remove the nth node from the end (slow.next)
dummy -> 1 -> 2 -> 3 -> 5 -> NULL
          ^      |    ^
          |      X    |
         slow         slow.next.next

Result: Return dummy.next, which is the head of the modified list: 1 -> 2 -> 3 -> 5 -> NULL
```

![Remove Nth Node From End Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Create a dummy node and set its `next` pointer to the head of the list.
2. Initialize two pointers, `fast` and `slow`, both pointing to the dummy node.
3. Move the `fast` pointer n nodes ahead.
4. Move both `fast` and `slow` pointers one step at a time until `fast` reaches the end of the list.
5. At this point, `slow` is pointing to the node just before the one we want to remove.
6. Update `slow.next` to skip the nth node from the end: `slow.next = slow.next.next`.
7. Return `dummy.next`, which is the head of the modified list.

## Code Implementation

### Python

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def removeNthFromEnd(head, n):
    # Create a dummy node to handle edge cases
    dummy = ListNode(0)
    dummy.next = head
    
    # Initialize fast and slow pointers
    fast = dummy
    slow = dummy
    
    # Move fast pointer n nodes ahead
    for i in range(n + 1):
        fast = fast.next
    
    # Move both pointers until fast reaches the end
    while fast:
        fast = fast.next
        slow = slow.next
    
    # Remove the nth node from the end
    slow.next = slow.next.next
    
    # Return the head of the modified list
    return dummy.next
```

### JavaScript

```javascript
function removeNthFromEnd(head, n) {
    // Create a dummy node to handle edge cases
    const dummy = { val: 0, next: head };
    
    // Initialize fast and slow pointers
    let fast = dummy;
    let slow = dummy;
    
    // Move fast pointer n nodes ahead
    for (let i = 0; i < n + 1; i++) {
        fast = fast.next;
    }
    
    // Move both pointers until fast reaches the end
    while (fast) {
        fast = fast.next;
        slow = slow.next;
    }
    
    // Remove the nth node from the end
    slow.next = slow.next.next;
    
    // Return the head of the modified list
    return dummy.next;
}
```

## Time and Space Complexity

- **Time Complexity**: O(L), where L is the length of the linked list. We traverse the list once.
- **Space Complexity**: O(1), as we only use a constant amount of extra space regardless of the input size.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 3: `head = [1,2], n = 1`

```
Initialize:
- dummy = ListNode(0)
- dummy.next = head = 1 -> 2 -> NULL
- fast = dummy
- slow = dummy

Step 1: Move fast pointer n+1 (2) nodes ahead
- fast = dummy.next = 1
- fast = fast.next = 2
- fast is now at node 2

Step 2: Move both pointers until fast reaches the end
- fast = fast.next = NULL
- Since fast is NULL, we exit the while loop
- slow is still at dummy

Step 3: Remove the nth node from the end
- slow.next = slow.next.next
- This changes dummy.next from 1 -> 2 -> NULL to 1 -> NULL
- We've effectively removed the last node (2)

Return dummy.next, which is the head of the modified list: 1 -> NULL
```

## Edge Cases and Optimizations

- **Empty List**: If the input list is empty (`head` is `NULL`), the function will return `NULL` (since `dummy.next` will be `NULL`).
- **Removing the Head**: If we need to remove the first node (n equals the length of the list), the function will correctly update `dummy.next` to point to the second node.
- **Single Node List**: If the list has only one node and n = 1, the function will return `NULL` (an empty list).
- **Invalid n**: The problem states that n is valid, so we don't need to handle cases where n is greater than the length of the list.

## Common Mistakes

1. **Not using a dummy node**: Without a dummy node, removing the head of the list becomes a special case that needs to be handled separately.
2. **Off-by-one errors**: Be careful with the number of steps to move the fast pointer. We need to move it n+1 steps ahead to ensure that slow points to the node before the one we want to remove.
3. **Not handling edge cases**: Always consider edge cases like an empty list or removing the head of the list.

## Related Problems

1. **Remove Linked List Elements**: Remove all elements from a linked list that have a specific value.
2. **Delete Node in a Linked List**: Delete a node in a linked list given only access to that node.
3. **Middle of the Linked List**: Find the middle node of a linked list.
4. **Linked List Cycle II**: Find the node where the cycle begins in a linked list. 