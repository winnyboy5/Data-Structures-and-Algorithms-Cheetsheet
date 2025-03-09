# Reverse Linked List

## Problem Statement

Given the head of a singly linked list, reverse the list, and return the reversed list.

**Example 1:**
```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

**Example 2:**
```
Input: head = [1,2]
Output: [2,1]
```

**Example 3:**
```
Input: head = []
Output: []
```

## Intuition

The key insight for this problem is to reverse the direction of pointers in the linked list. We need to change each node's `next` pointer to point to its previous node instead of its next node.

To accomplish this, we can iterate through the list while keeping track of three pointers:
1. `prev`: The node that will become the next node of the current node after reversal
2. `current`: The node we are currently processing
3. `next`: A temporary pointer to store the next node before we change the current node's pointer

This approach allows us to reverse the list in a single pass with O(1) extra space.

## Visual Explanation

Let's visualize the solution with Example 1: `head = [1,2,3,4,5]`

```
Initial state:
NULL <- prev  current -> next
                1 -> 2 -> 3 -> 4 -> 5 -> NULL

Step 1: Save next, reverse current's pointer, move prev and current
NULL <- 1    current -> next
      prev     2 -> 3 -> 4 -> 5 -> NULL

Step 2: Save next, reverse current's pointer, move prev and current
NULL <- 1 <- 2    current -> next
           prev     3 -> 4 -> 5 -> NULL

Step 3: Save next, reverse current's pointer, move prev and current
NULL <- 1 <- 2 <- 3    current -> next
               prev     4 -> 5 -> NULL

Step 4: Save next, reverse current's pointer, move prev and current
NULL <- 1 <- 2 <- 3 <- 4    current -> next
                   prev     5 -> NULL

Step 5: Save next, reverse current's pointer, move prev and current
NULL <- 1 <- 2 <- 3 <- 4 <- 5    current = NULL
                       prev     next = NULL

Final state:
NULL <- 1 <- 2 <- 3 <- 4 <- 5
                       head
```

![Reverse Linked List Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

### Iterative Approach:

1. Initialize three pointers:
   - `prev` as `NULL` (since the new tail will point to `NULL`)
   - `current` as the `head` of the original list
   - `next` as `NULL` (will be used to temporarily store the next node)
2. Iterate through the list until `current` becomes `NULL`:
   - Save the next node: `next = current.next`
   - Reverse the current node's pointer: `current.next = prev`
   - Move `prev` to the current node: `prev = current`
   - Move `current` to the next node: `current = next`
3. Return `prev` as the new head of the reversed list.

### Recursive Approach:

1. Base case: If the list is empty or has only one node, return the head.
2. Recursively reverse the rest of the list starting from the second node.
3. Fix the pointers:
   - Make the second node point to the first node: `head.next.next = head`
   - Make the first node point to `NULL`: `head.next = NULL`
4. Return the new head of the reversed list.

## Code Implementation

### Python (Iterative)

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverseList(head):
    prev = None
    current = head
    
    while current:
        # Save the next node
        next_temp = current.next
        
        # Reverse the current node's pointer
        current.next = prev
        
        # Move prev and current one step forward
        prev = current
        current = next_temp
    
    # prev is the new head of the reversed list
    return prev
```

### Python (Recursive)

```python
def reverseList(head):
    # Base case: empty list or only one node
    if not head or not head.next:
        return head
    
    # Recursively reverse the rest of the list
    new_head = reverseList(head.next)
    
    # Fix the pointers
    head.next.next = head
    head.next = None
    
    # Return the new head
    return new_head
```

### JavaScript (Iterative)

```javascript
function reverseList(head) {
    let prev = null;
    let current = head;
    
    while (current) {
        // Save the next node
        const nextTemp = current.next;
        
        // Reverse the current node's pointer
        current.next = prev;
        
        // Move prev and current one step forward
        prev = current;
        current = nextTemp;
    }
    
    // prev is the new head of the reversed list
    return prev;
}
```

### JavaScript (Recursive)

```javascript
function reverseList(head) {
    // Base case: empty list or only one node
    if (!head || !head.next) {
        return head;
    }
    
    // Recursively reverse the rest of the list
    const newHead = reverseList(head.next);
    
    // Fix the pointers
    head.next.next = head;
    head.next = null;
    
    // Return the new head
    return newHead;
}
```

## Time and Space Complexity

### Iterative Approach:
- **Time Complexity**: O(n), where n is the number of nodes in the linked list. We traverse the list once.
- **Space Complexity**: O(1), as we only use a constant amount of extra space regardless of the input size.

### Recursive Approach:
- **Time Complexity**: O(n), where n is the number of nodes in the linked list. We make n recursive calls.
- **Space Complexity**: O(n) due to the recursion stack. In the worst case, the recursion depth is equal to the number of nodes in the list.

## Detailed Walkthrough with Example

Let's trace through the iterative algorithm with Example 2: `head = [1,2]`

```
Initialize:
- prev = null
- current = head = 1

Iteration 1:
- next_temp = current.next = 2
- current.next = prev = null
- prev = current = 1
- current = next_temp = 2

Iteration 2:
- next_temp = current.next = null
- current.next = prev = 1
- prev = current = 2
- current = next_temp = null

Since current is now null, we exit the loop.
Return prev = 2 as the new head.

The reversed list is: 2 -> 1 -> null
```

## Edge Cases and Optimizations

- **Empty List**: If the input list is empty (`head` is `NULL`), the function will return `NULL`.
- **Single Node**: If the list has only one node, the function will return the same node (since there's nothing to reverse).
- **Two Nodes**: For a list with two nodes, the function will swap their pointers correctly.

## Common Mistakes

1. **Not handling the null pointer**: Always check if the list is empty or has only one node before attempting to reverse it.
2. **Losing track of the next node**: Make sure to save the next node before changing the current node's pointer.
3. **Not updating the head**: Remember that the head of the reversed list will be the last node of the original list.

## Related Problems

1. **Reverse Linked List II**: Reverse a linked list from position m to n.
2. **Palindrome Linked List**: Check if a linked list is a palindrome.
3. **Swap Nodes in Pairs**: Swap every two adjacent nodes and return the head of the modified list.
4. **Reverse Nodes in k-Group**: Reverse the nodes of the list k at a time and return the modified list. 