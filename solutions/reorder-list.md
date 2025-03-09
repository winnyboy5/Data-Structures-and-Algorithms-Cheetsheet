# Reorder List

## Problem Statement

You are given the head of a singly linked list. The list can be represented as:

L₀ → L₁ → … → Lₙ₋₁ → Lₙ

Reorder the list to be in the following form:

L₀ → Lₙ → L₁ → Lₙ₋₁ → L₂ → Lₙ₋₂ → …

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

**Example 1:**
```
Input: head = [1,2,3,4]
Output: [1,4,2,3]
```

**Example 2:**
```
Input: head = [1,2,3,4,5]
Output: [1,5,2,4,3]
```

## Intuition

The key insight for this problem is to break it down into three steps:

1. Find the middle of the linked list using the slow and fast pointer technique.
2. Reverse the second half of the linked list.
3. Merge the first half and the reversed second half by alternating nodes.

This approach allows us to reorder the list in-place with O(1) extra space.

## Visual Explanation

Let's visualize the solution with Example 2: `head = [1,2,3,4,5]`

```
Step 1: Find the middle of the linked list
Initial list: 1 -> 2 -> 3 -> 4 -> 5 -> NULL
                     ^
                    middle

First half: 1 -> 2 -> 3 -> NULL
Second half: 4 -> 5 -> NULL

Step 2: Reverse the second half
First half: 1 -> 2 -> 3 -> NULL
Reversed second half: 5 -> 4 -> NULL

Step 3: Merge the two halves by alternating nodes
1 -> 5 -> 2 -> 4 -> 3 -> NULL
```

Let's break down the process for Example 1: `head = [1,2,3,4]`

```
Step 1: Find the middle of the linked list
Initial list: 1 -> 2 -> 3 -> 4 -> NULL
                 ^
                middle

First half: 1 -> 2 -> NULL
Second half: 3 -> 4 -> NULL

Step 2: Reverse the second half
First half: 1 -> 2 -> NULL
Reversed second half: 4 -> 3 -> NULL

Step 3: Merge the two halves by alternating nodes
1 -> 4 -> 2 -> 3 -> NULL
```

![Reorder List Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. **Find the middle of the linked list**:
   - Use the slow and fast pointer technique. The slow pointer moves one step at a time, while the fast pointer moves two steps.
   - When the fast pointer reaches the end, the slow pointer will be at the middle.
   - Split the list into two halves at the middle.

2. **Reverse the second half of the linked list**:
   - Initialize three pointers: `prev` as `NULL`, `current` as the head of the second half, and `next` as `NULL`.
   - Iterate through the second half, reversing the direction of each pointer.

3. **Merge the two halves by alternating nodes**:
   - Initialize two pointers: `p1` for the first half and `p2` for the reversed second half.
   - Alternate between nodes from the first half and the reversed second half.
   - Connect the nodes in the new order.

## Code Implementation

### Python

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reorderList(head):
    if not head or not head.next:
        return
    
    # Step 1: Find the middle of the linked list
    slow = head
    fast = head
    while fast.next and fast.next.next:
        slow = slow.next
        fast = fast.next.next
    
    # Split the list into two halves
    second_half = slow.next
    slow.next = None
    
    # Step 2: Reverse the second half
    prev = None
    current = second_half
    while current:
        next_temp = current.next
        current.next = prev
        prev = current
        current = next_temp
    
    # Now prev is the head of the reversed second half
    
    # Step 3: Merge the two halves
    first = head
    second = prev
    while second:
        temp1 = first.next
        temp2 = second.next
        
        first.next = second
        second.next = temp1
        
        first = temp1
        second = temp2
```

### JavaScript

```javascript
function reorderList(head) {
    if (!head || !head.next) {
        return;
    }
    
    // Step 1: Find the middle of the linked list
    let slow = head;
    let fast = head;
    while (fast.next && fast.next.next) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    // Split the list into two halves
    let secondHalf = slow.next;
    slow.next = null;
    
    // Step 2: Reverse the second half
    let prev = null;
    let current = secondHalf;
    while (current) {
        const nextTemp = current.next;
        current.next = prev;
        prev = current;
        current = nextTemp;
    }
    
    // Now prev is the head of the reversed second half
    
    // Step 3: Merge the two halves
    let first = head;
    let second = prev;
    while (second) {
        const temp1 = first.next;
        const temp2 = second.next;
        
        first.next = second;
        second.next = temp1;
        
        first = temp1;
        second = temp2;
    }
}
```

## Time and Space Complexity

- **Time Complexity**: O(n), where n is the number of nodes in the linked list. We traverse the list multiple times, but each traversal is O(n).
- **Space Complexity**: O(1), as we only use a constant amount of extra space regardless of the input size.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 1: `head = [1,2,3,4]`

```
Initialize:
- head = 1 -> 2 -> 3 -> 4 -> NULL

Step 1: Find the middle of the linked list
- slow = head = 1
- fast = head = 1
- Iteration 1:
  - slow = slow.next = 2
  - fast = fast.next.next = 3
- Since fast.next.next is NULL, we exit the loop
- slow is at node 2
- second_half = slow.next = 3
- slow.next = NULL
- First half: 1 -> 2 -> NULL
- Second half: 3 -> 4 -> NULL

Step 2: Reverse the second half
- prev = NULL
- current = second_half = 3
- Iteration 1:
  - next_temp = current.next = 4
  - current.next = prev = NULL
  - prev = current = 3
  - current = next_temp = 4
- Iteration 2:
  - next_temp = current.next = NULL
  - current.next = prev = 3
  - prev = current = 4
  - current = next_temp = NULL
- Since current is NULL, we exit the loop
- Reversed second half: 4 -> 3 -> NULL

Step 3: Merge the two halves
- first = head = 1
- second = prev = 4
- Iteration 1:
  - temp1 = first.next = 2
  - temp2 = second.next = 3
  - first.next = second = 4
  - second.next = temp1 = 2
  - first = temp1 = 2
  - second = temp2 = 3
- Iteration 2:
  - temp1 = first.next = NULL
  - temp2 = second.next = NULL
  - first.next = second = 3
  - second.next = temp1 = NULL
  - first = temp1 = NULL
  - second = temp2 = NULL
- Since second is NULL, we exit the loop
- Reordered list: 1 -> 4 -> 2 -> 3 -> NULL
```

## Edge Cases and Optimizations

- **Empty List**: If the input list is empty (`head` is `NULL`), the function will return immediately.
- **Single Node**: If the list has only one node, the function will return immediately.
- **Two Nodes**: If the list has two nodes, the function will correctly reorder them.
- **Even vs. Odd Length**: The algorithm handles both even and odd length lists correctly.

## Common Mistakes

1. **Not handling the middle node correctly**: For odd-length lists, the middle node should be part of the first half.
2. **Not terminating the first half**: Make sure to set `slow.next = NULL` to terminate the first half of the list.
3. **Losing track of nodes during merging**: Use temporary variables to store the next nodes before modifying the pointers.

## Related Problems

1. **Palindrome Linked List**: Check if a linked list is a palindrome.
2. **Reverse Linked List**: Reverse a singly linked list.
3. **Middle of the Linked List**: Find the middle node of a linked list.
4. **Rotate List**: Rotate a linked list to the right by k places. 