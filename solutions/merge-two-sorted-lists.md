# Merge Two Sorted Lists

## Problem Statement

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

**Example 1:**
```
Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
```

**Example 2:**
```
Input: list1 = [], list2 = []
Output: []
```

**Example 3:**
```
Input: list1 = [], list2 = [0]
Output: [0]
```

## Intuition

The key insight for this problem is to leverage the fact that both input lists are already sorted. We can create a new merged list by comparing the current nodes of both lists and selecting the smaller one to add to our result.

We'll use a dummy head node to simplify the edge case of inserting into an empty list, and a current pointer to keep track of where to append the next node. By iterating through both lists simultaneously and always choosing the smaller value, we ensure the merged list remains sorted.

## Visual Explanation

Let's visualize the solution with Example 1: `list1 = [1,2,4], list2 = [1,3,4]`

```
Initial state:
list1: 1 -> 2 -> 4 -> NULL
list2: 1 -> 3 -> 4 -> NULL
dummy: 0
curr: dummy

Step 1: Compare list1.val (1) and list2.val (1)
        They're equal, so we can pick either one. Let's pick from list1.
        Append list1 node to curr.next and move list1 pointer forward.
        
        list1: 2 -> 4 -> NULL
        list2: 1 -> 3 -> 4 -> NULL
        dummy: 0 -> 1
        curr: points to the 1 node from list1

Step 2: Compare list1.val (2) and list2.val (1)
        list2.val is smaller, so append list2 node to curr.next and move list2 pointer forward.
        
        list1: 2 -> 4 -> NULL
        list2: 3 -> 4 -> NULL
        dummy: 0 -> 1 -> 1
        curr: points to the 1 node from list2

Step 3: Compare list1.val (2) and list2.val (3)
        list1.val is smaller, so append list1 node to curr.next and move list1 pointer forward.
        
        list1: 4 -> NULL
        list2: 3 -> 4 -> NULL
        dummy: 0 -> 1 -> 1 -> 2
        curr: points to the 2 node

Step 4: Compare list1.val (4) and list2.val (3)
        list2.val is smaller, so append list2 node to curr.next and move list2 pointer forward.
        
        list1: 4 -> NULL
        list2: 4 -> NULL
        dummy: 0 -> 1 -> 1 -> 2 -> 3
        curr: points to the 3 node

Step 5: Compare list1.val (4) and list2.val (4)
        They're equal, so we can pick either one. Let's pick from list1.
        Append list1 node to curr.next and move list1 pointer forward.
        
        list1: NULL
        list2: 4 -> NULL
        dummy: 0 -> 1 -> 1 -> 2 -> 3 -> 4
        curr: points to the 4 node from list1

Step 6: list1 is now empty, so append the remaining nodes from list2.
        
        list1: NULL
        list2: NULL
        dummy: 0 -> 1 -> 1 -> 2 -> 3 -> 4 -> 4
        curr: points to the 4 node from list2

Result: Return dummy.next, which is the head of the merged list: 1 -> 1 -> 2 -> 3 -> 4 -> 4 -> NULL
```

![Merge Two Sorted Lists Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Create a dummy node to serve as the head of the merged list.
2. Initialize a current pointer to the dummy node.
3. Iterate while both lists have nodes:
   - Compare the values of the current nodes in both lists.
   - Append the node with the smaller value to the merged list.
   - Move the pointer of the list that contributed the node forward.
   - Move the current pointer of the merged list forward.
4. If one list is exhausted, append the remaining nodes from the other list.
5. Return the next node of the dummy node (the actual head of the merged list).

## Code Implementation

### Python

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def mergeTwoLists(list1, list2):
    # Create a dummy node to simplify edge cases
    dummy = ListNode(0)
    current = dummy
    
    # Iterate while both lists have nodes
    while list1 and list2:
        if list1.val <= list2.val:
            current.next = list1
            list1 = list1.next
        else:
            current.next = list2
            list2 = list2.next
        current = current.next
    
    # If one list is exhausted, append the remaining nodes from the other list
    if list1:
        current.next = list1
    else:
        current.next = list2
    
    # Return the head of the merged list (next node of the dummy)
    return dummy.next
```

### JavaScript

```javascript
function mergeTwoLists(list1, list2) {
    // Create a dummy node to simplify edge cases
    const dummy = { val: 0, next: null };
    let current = dummy;
    
    // Iterate while both lists have nodes
    while (list1 && list2) {
        if (list1.val <= list2.val) {
            current.next = list1;
            list1 = list1.next;
        } else {
            current.next = list2;
            list2 = list2.next;
        }
        current = current.next;
    }
    
    // If one list is exhausted, append the remaining nodes from the other list
    current.next = list1 || list2;
    
    // Return the head of the merged list (next node of the dummy)
    return dummy.next;
}
```

## Recursive Approach

We can also solve this problem recursively:

### Python (Recursive)

```python
def mergeTwoLists(list1, list2):
    # Base cases
    if not list1:
        return list2
    if not list2:
        return list1
    
    # Recursive case
    if list1.val <= list2.val:
        list1.next = mergeTwoLists(list1.next, list2)
        return list1
    else:
        list2.next = mergeTwoLists(list1, list2.next)
        return list2
```

### JavaScript (Recursive)

```javascript
function mergeTwoLists(list1, list2) {
    // Base cases
    if (!list1) return list2;
    if (!list2) return list1;
    
    // Recursive case
    if (list1.val <= list2.val) {
        list1.next = mergeTwoLists(list1.next, list2);
        return list1;
    } else {
        list2.next = mergeTwoLists(list1, list2.next);
        return list2;
    }
}
```

## Time and Space Complexity

### Iterative Approach:
- **Time Complexity**: O(n + m), where n and m are the lengths of the two input lists. We traverse each node exactly once.
- **Space Complexity**: O(1), as we only use a constant amount of extra space regardless of the input size. Note that we're not creating new nodes, just rearranging the existing ones.

### Recursive Approach:
- **Time Complexity**: O(n + m), where n and m are the lengths of the two input lists.
- **Space Complexity**: O(n + m) due to the recursion stack. In the worst case, the recursion depth is equal to the sum of the lengths of both lists.

## Detailed Walkthrough with Example

Let's trace through the iterative algorithm with Example 3: `list1 = [], list2 = [0]`

```
Initialize:
- dummy = ListNode(0)
- current = dummy
- list1 = NULL
- list2 = 0 -> NULL

Since list1 is NULL, we skip the while loop and go to the if-else statement:
- list1 is NULL, so current.next = list2
- current.next now points to the node with value 0

Return dummy.next, which is the node with value 0.

The merged list is: 0 -> NULL
```

## Edge Cases and Optimizations

- **Both Lists Empty**: If both input lists are empty, the function will return `NULL`.
- **One List Empty**: If one of the lists is empty, the function will return the other list.
- **Lists with Different Lengths**: The algorithm handles this correctly by appending the remaining nodes from the non-empty list.
- **Duplicate Values**: The algorithm handles duplicate values correctly by maintaining the relative order of nodes with the same value.

## Common Mistakes

1. **Not handling empty lists**: Always check if either list is empty before attempting to merge them.
2. **Creating new nodes**: This problem asks us to splice together the existing nodes, not create new ones.
3. **Not using a dummy node**: Using a dummy node simplifies the edge case of inserting into an empty list.

## Related Problems

1. **Merge k Sorted Lists**: Merge k sorted linked lists into one sorted list.
2. **Merge Sorted Array**: Merge two sorted arrays into one sorted array.
3. **Sort List**: Sort a linked list in O(n log n) time and O(1) space.
4. **Intersection of Two Linked Lists**: Find the node at which two linked lists intersect. 