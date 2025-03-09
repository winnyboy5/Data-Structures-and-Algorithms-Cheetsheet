# Linked List Cycle

## Problem Statement

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.

Return `true` if there is a cycle in the linked list. Otherwise, return `false`.

**Example 1:**
```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
```

**Example 2:**
```
Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.
```

**Example 3:**
```
Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```

## Intuition

The key insight for this problem is to use Floyd's Cycle-Finding Algorithm (also known as the "tortoise and hare" algorithm). The idea is to have two pointers moving through the linked list at different speeds:

1. A slow pointer (tortoise) that moves one step at a time.
2. A fast pointer (hare) that moves two steps at a time.

If there is a cycle in the linked list, the fast pointer will eventually catch up to the slow pointer, indicating the presence of a cycle. If there is no cycle, the fast pointer will reach the end of the list.

This approach works because if there is a cycle, the fast pointer will eventually lap the slow pointer, and they will meet at some point within the cycle.

## Visual Explanation

Let's visualize the solution with Example 1: `head = [3,2,0,-4], pos = 1`

```
Linked List with Cycle:
3 -> 2 -> 0 -> -4
     ^         |
     |_________|

Step 1: Initialize slow = head, fast = head
        slow = 3, fast = 3

Step 2: Move slow one step, fast two steps
        slow = 2, fast = 0

Step 3: Move slow one step, fast two steps
        slow = 0, fast = 2 (fast moves to -4, then to 2)

Step 4: Move slow one step, fast two steps
        slow = -4, fast = 0

Step 5: Move slow one step, fast two steps
        slow = 2, fast = -4

Step 6: Move slow one step, fast two steps
        slow = 0, fast = 2

Step 7: Move slow one step, fast two steps
        slow = -4, fast = 0

Step 8: Move slow one step, fast two steps
        slow = 2, fast = -4

Step 9: Move slow one step, fast two steps
        slow = 0, fast = 2

Eventually, slow and fast will meet, indicating a cycle.
```

![Linked List Cycle Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize two pointers, `slow` and `fast`, both pointing to the head of the linked list.
2. Traverse the linked list:
   - Move the slow pointer one step at a time (`slow = slow.next`).
   - Move the fast pointer two steps at a time (`fast = fast.next.next`).
   - If the fast pointer reaches the end of the list (i.e., `fast` or `fast.next` is `null`), return `false` (no cycle).
   - If the slow pointer meets the fast pointer (i.e., `slow === fast`), return `true` (cycle detected).
3. Continue this process until a cycle is detected or the end of the list is reached.

## Code Implementation

### Python

```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

def hasCycle(head):
    if not head or not head.next:
        return False
    
    # Initialize slow and fast pointers
    slow = head
    fast = head
    
    # Traverse the linked list
    while fast and fast.next:
        # Move slow pointer one step
        slow = slow.next
        # Move fast pointer two steps
        fast = fast.next.next
        
        # If slow meets fast, there is a cycle
        if slow == fast:
            return True
    
    # If fast reaches the end, there is no cycle
    return False
```

### JavaScript

```javascript
function hasCycle(head) {
    if (!head || !head.next) {
        return false;
    }
    
    // Initialize slow and fast pointers
    let slow = head;
    let fast = head;
    
    // Traverse the linked list
    while (fast && fast.next) {
        // Move slow pointer one step
        slow = slow.next;
        // Move fast pointer two steps
        fast = fast.next.next;
        
        // If slow meets fast, there is a cycle
        if (slow === fast) {
            return true;
        }
    }
    
    // If fast reaches the end, there is no cycle
    return false;
}
```

## Time and Space Complexity

- **Time Complexity**: O(n), where n is the number of nodes in the linked list. In the worst case, the slow pointer will traverse the entire list once.
- **Space Complexity**: O(1), as we only use two pointers regardless of the size of the linked list.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 1: `head = [3,2,0,-4], pos = 1`

```
Initialize slow = head = 3, fast = head = 3

Iteration 1:
  slow = slow.next = 2
  fast = fast.next.next = 0
  slow != fast, continue

Iteration 2:
  slow = slow.next = 0
  fast = fast.next.next = 2 (fast moves to -4, then to 2)
  slow != fast, continue

Iteration 3:
  slow = slow.next = -4
  fast = fast.next.next = 0 (fast moves to 2, then to 0)
  slow != fast, continue

Iteration 4:
  slow = slow.next = 2
  fast = fast.next.next = -4 (fast moves to 0, then to -4)
  slow != fast, continue

Iteration 5:
  slow = slow.next = 0
  fast = fast.next.next = 2 (fast moves to -4, then to 2)
  slow != fast, continue

Iteration 6:
  slow = slow.next = -4
  fast = fast.next.next = 0 (fast moves to 2, then to 0)
  slow != fast, continue

Iteration 7:
  slow = slow.next = 2
  fast = fast.next.next = -4 (fast moves to 0, then to -4)
  slow != fast, continue

Eventually, slow and fast will meet at some node in the cycle, and we return true.
```

## Finding the Start of the Cycle

While not required for this problem, it's worth noting that we can also find the start of the cycle using Floyd's algorithm:

1. Once the slow and fast pointers meet, reset the slow pointer to the head of the linked list.
2. Keep the fast pointer at the meeting point.
3. Move both pointers one step at a time.
4. The point where they meet again is the start of the cycle.

```python
def detectCycle(head):
    if not head or not head.next:
        return None
    
    // Initialize slow and fast pointers
    slow = head
    fast = head
    
    // Detect cycle
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
        if slow == fast:
            // Cycle detected, find the start
            slow = head
            while slow != fast:
                slow = slow.next
                fast = fast.next
            return slow
    
    // No cycle
    return None
```

## Edge Cases and Optimizations

- **Empty List**: If the list is empty (`head` is `null`), return `false`.
- **Single Node**: If the list has only one node and it doesn't point to itself, return `false`.
- **Single Node with Cycle**: If the list has only one node and it points to itself, return `true`.
- **No Cycle**: If the list doesn't have a cycle, the fast pointer will reach the end of the list.

## Common Mistakes

1. **Not checking for null pointers**: Always check if `fast` and `fast.next` are not `null` before moving the fast pointer.
2. **Incorrect initialization**: Both slow and fast pointers should start at the head of the linked list.
3. **Incorrect termination condition**: The loop should continue as long as `fast` and `fast.next` are not `null`.

## Related Problems

1. **Linked List Cycle II**: Find the node where the cycle begins.
2. **Happy Number**: Determine if a number is "happy" using the cycle detection algorithm.
3. **Find the Duplicate Number**: Find the duplicate number in an array using the cycle detection algorithm.
4. **Middle of the Linked List**: Find the middle node of a linked list using the slow and fast pointer technique. 