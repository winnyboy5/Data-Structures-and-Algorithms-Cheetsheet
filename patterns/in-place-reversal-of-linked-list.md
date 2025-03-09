# In-place Reversal of a Linked List Pattern

The In-place Reversal of a Linked List pattern is a technique used to reverse a linked list or parts of it without using extra space. This pattern is particularly useful for problems that require modifying the structure of a linked list.

## Visual Representation

```
Original Linked List: 1 → 2 → 3 → 4 → 5 → NULL

Step 1: Initialize prev=NULL, current=head, next=NULL
        NULL  1 → 2 → 3 → 4 → 5 → NULL
        prev  current

Step 2: Save next, reverse current's pointer, move prev and current
        NULL ← 1  2 → 3 → 4 → 5 → NULL
        prev  current  next

Step 3: Continue the process
        NULL ← 1 ← 2  3 → 4 → 5 → NULL
               prev  current  next

Step 4: Continue the process
        NULL ← 1 ← 2 ← 3  4 → 5 → NULL
                    prev  current  next

Step 5: Continue the process
        NULL ← 1 ← 2 ← 3 ← 4  5 → NULL
                         prev  current  next

Step 6: Final step
        NULL ← 1 ← 2 ← 3 ← 4 ← 5  NULL
                              prev  current  next

Reversed Linked List: 5 → 4 → 3 → 2 → 1 → NULL
```

## When to Use In-place Reversal of a Linked List

- When you need to reverse a linked list or a part of it
- When you need to reorder a linked list
- When you need to modify the structure of a linked list without using extra space
- When the problem involves rotating a linked list

## Common Problems and Solutions

### 1. Reverse a Linked List

**Problem:** Reverse a singly linked list.

**Python Solution:**
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverse_linked_list(head):
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
    
    # Return the new head of the reversed list
    return prev

# Example usage
# Create a linked list: 1 → 2 → 3 → 4 → 5
head = ListNode(1)
head.next = ListNode(2)
head.next.next = ListNode(3)
head.next.next.next = ListNode(4)
head.next.next.next.next = ListNode(5)

# Reverse the linked list
reversed_head = reverse_linked_list(head)

# Print the reversed linked list: 5 → 4 → 3 → 2 → 1
result = []
current = reversed_head
while current:
    result.append(current.val)
    current = current.next
print(result)  # Output: [5, 4, 3, 2, 1]
```

**JavaScript Solution:**
```javascript
class ListNode {
    constructor(val = 0, next = null) {
        this.val = val;
        this.next = next;
    }
}

function reverseLinkedList(head) {
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
    
    // Return the new head of the reversed list
    return prev;
}

// Example usage
// Create a linked list: 1 → 2 → 3 → 4 → 5
const head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
head.next.next.next = new ListNode(4);
head.next.next.next.next = new ListNode(5);

// Reverse the linked list
const reversedHead = reverseLinkedList(head);

// Print the reversed linked list: 5 → 4 → 3 → 2 → 1
const result = [];
let current = reversedHead;
while (current) {
    result.push(current.val);
    current = current.next;
}
console.log(result);  // Output: [5, 4, 3, 2, 1]
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

### 2. Reverse a Sublist

**Problem:** Reverse a linked list from position m to position n.

**Python Solution:**
```python
def reverse_sublist(head, m, n):
    if not head or m == n:
        return head
    
    # Use a dummy node to handle edge cases
    dummy = ListNode(0)
    dummy.next = head
    prev = dummy
    
    # Move prev to the node just before the sublist
    for _ in range(m - 1):
        prev = prev.next
    
    # Start of the sublist to be reversed
    current = prev.next
    
    # Reverse the sublist
    for _ in range(n - m):
        temp = current.next
        current.next = temp.next
        temp.next = prev.next
        prev.next = temp
    
    return dummy.next

# Example usage
# Create a linked list: 1 → 2 → 3 → 4 → 5
head = ListNode(1)
head.next = ListNode(2)
head.next.next = ListNode(3)
head.next.next.next = ListNode(4)
head.next.next.next.next = ListNode(5)

# Reverse the sublist from position 2 to 4
reversed_head = reverse_sublist(head, 2, 4)

# Print the modified linked list: 1 → 4 → 3 → 2 → 5
result = []
current = reversed_head
while current:
    result.append(current.val)
    current = current.next
print(result)  # Output: [1, 4, 3, 2, 5]
```

**JavaScript Solution:**
```javascript
function reverseSublist(head, m, n) {
    if (!head || m === n) {
        return head;
    }
    
    // Use a dummy node to handle edge cases
    const dummy = new ListNode(0);
    dummy.next = head;
    let prev = dummy;
    
    // Move prev to the node just before the sublist
    for (let i = 1; i < m; i++) {
        prev = prev.next;
    }
    
    // Start of the sublist to be reversed
    const current = prev.next;
    
    // Reverse the sublist
    for (let i = 0; i < n - m; i++) {
        const temp = current.next;
        current.next = temp.next;
        temp.next = prev.next;
        prev.next = temp;
    }
    
    return dummy.next;
}

// Example usage
// Create a linked list: 1 → 2 → 3 → 4 → 5
const head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
head.next.next.next = new ListNode(4);
head.next.next.next.next = new ListNode(5);

// Reverse the sublist from position 2 to 4
const reversedHead = reverseSublist(head, 2, 4);

// Print the modified linked list: 1 → 4 → 3 → 2 → 5
const result = [];
let current = reversedHead;
while (current) {
    result.push(current.val);
    current = current.next;
}
console.log(result);  // Output: [1, 4, 3, 2, 5]
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

### 3. Reverse Nodes in k-Group

**Problem:** Reverse the nodes of a linked list k at a time and return its modified list.

**Python Solution:**
```python
def reverse_k_group(head, k):
    if not head or k == 1:
        return head
    
    # Use a dummy node to handle edge cases
    dummy = ListNode(0)
    dummy.next = head
    prev_group_end = dummy
    
    while True:
        # Check if there are at least k nodes left
        kth_node = get_kth_node(prev_group_end, k)
        if not kth_node:
            break
        
        next_group_start = kth_node.next
        
        # Reverse the current group
        current = prev_group_end.next
        for _ in range(k - 1):
            temp = current.next
            current.next = temp.next
            temp.next = prev_group_end.next
            prev_group_end.next = temp
        
        # Update prev_group_end for the next group
        prev_group_end = current
        prev_group_end.next = next_group_start
    
    return dummy.next

def get_kth_node(current, k):
    while current and k > 0:
        current = current.next
        k -= 1
    return current

# Example usage
# Create a linked list: 1 → 2 → 3 → 4 → 5
head = ListNode(1)
head.next = ListNode(2)
head.next.next = ListNode(3)
head.next.next.next = ListNode(4)
head.next.next.next.next = ListNode(5)

# Reverse nodes in groups of 2
reversed_head = reverse_k_group(head, 2)

# Print the modified linked list: 2 → 1 → 4 → 3 → 5
result = []
current = reversed_head
while current:
    result.append(current.val)
    current = current.next
print(result)  # Output: [2, 1, 4, 3, 5]
```

**JavaScript Solution:**
```javascript
function reverseKGroup(head, k) {
    if (!head || k === 1) {
        return head;
    }
    
    // Use a dummy node to handle edge cases
    const dummy = new ListNode(0);
    dummy.next = head;
    let prevGroupEnd = dummy;
    
    while (true) {
        // Check if there are at least k nodes left
        const kthNode = getKthNode(prevGroupEnd, k);
        if (!kthNode) {
            break;
        }
        
        const nextGroupStart = kthNode.next;
        
        // Reverse the current group
        let current = prevGroupEnd.next;
        for (let i = 0; i < k - 1; i++) {
            const temp = current.next;
            current.next = temp.next;
            temp.next = prevGroupEnd.next;
            prevGroupEnd.next = temp;
        }
        
        // Update prevGroupEnd for the next group
        prevGroupEnd = current;
        prevGroupEnd.next = nextGroupStart;
    }
    
    return dummy.next;
}

function getKthNode(current, k) {
    while (current && k > 0) {
        current = current.next;
        k--;
    }
    return current;
}

// Example usage
// Create a linked list: 1 → 2 → 3 → 4 → 5
const head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
head.next.next.next = new ListNode(4);
head.next.next.next.next = new ListNode(5);

// Reverse nodes in groups of 2
const reversedHead = reverseKGroup(head, 2);

// Print the modified linked list: 2 → 1 → 4 → 3 → 5
const result = [];
let current = reversedHead;
while (current) {
    result.push(current.val);
    current = current.next;
}
console.log(result);  // Output: [2, 1, 4, 3, 5]
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

### 4. Rotate a Linked List

**Problem:** Rotate a linked list to the right by k places.

**Python Solution:**
```python
def rotate_right(head, k):
    if not head or not head.next or k == 0:
        return head
    
    # Find the length of the linked list
    length = 1
    current = head
    while current.next:
        current = current.next
        length += 1
    
    # Connect the last node to the head to form a circle
    current.next = head
    
    # Calculate the actual rotation needed (k could be larger than length)
    k = k % length
    
    # Find the new tail and head
    new_tail_position = length - k
    new_tail = head
    for _ in range(new_tail_position - 1):
        new_tail = new_tail.next
    
    # Break the circle and return the new head
    new_head = new_tail.next
    new_tail.next = None
    
    return new_head

# Example usage
# Create a linked list: 1 → 2 → 3 → 4 → 5
head = ListNode(1)
head.next = ListNode(2)
head.next.next = ListNode(3)
head.next.next.next = ListNode(4)
head.next.next.next.next = ListNode(5)

# Rotate the linked list to the right by 2 places
rotated_head = rotate_right(head, 2)

# Print the rotated linked list: 4 → 5 → 1 → 2 → 3
result = []
current = rotated_head
while current:
    result.append(current.val)
    current = current.next
print(result)  # Output: [4, 5, 1, 2, 3]
```

**JavaScript Solution:**
```javascript
function rotateRight(head, k) {
    if (!head || !head.next || k === 0) {
        return head;
    }
    
    // Find the length of the linked list
    let length = 1;
    let current = head;
    while (current.next) {
        current = current.next;
        length++;
    }
    
    // Connect the last node to the head to form a circle
    current.next = head;
    
    // Calculate the actual rotation needed (k could be larger than length)
    k = k % length;
    
    // Find the new tail and head
    const newTailPosition = length - k;
    let newTail = head;
    for (let i = 1; i < newTailPosition; i++) {
        newTail = newTail.next;
    }
    
    // Break the circle and return the new head
    const newHead = newTail.next;
    newTail.next = null;
    
    return newHead;
}

// Example usage
// Create a linked list: 1 → 2 → 3 → 4 → 5
const head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
head.next.next.next = new ListNode(4);
head.next.next.next.next = new ListNode(5);

// Rotate the linked list to the right by 2 places
const rotatedHead = rotateRight(head, 2);

// Print the rotated linked list: 4 → 5 → 1 → 2 → 3
const result = [];
let current = rotatedHead;
while (current) {
    result.push(current.val);
    current = current.next;
}
console.log(result);  // Output: [4, 5, 1, 2, 3]
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

### 5. Reorder List

**Problem:** Reorder a linked list such that L0 → L1 → ... → Ln-1 → Ln becomes L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...

**Python Solution:**
```python
def reorder_list(head):
    if not head or not head.next:
        return
    
    # Step 1: Find the middle of the linked list
    slow, fast = head, head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    
    # Step 2: Reverse the second half of the linked list
    prev = None
    current = slow
    while current:
        next_temp = current.next
        current.next = prev
        prev = current
        current = next_temp
    
    # Step 3: Merge the first half and the reversed second half
    first = head
    second = prev
    while second.next:
        temp1 = first.next
        temp2 = second.next
        first.next = second
        second.next = temp1
        first = temp1
        second = temp2

# Example usage
# Create a linked list: 1 → 2 → 3 → 4 → 5
head = ListNode(1)
head.next = ListNode(2)
head.next.next = ListNode(3)
head.next.next.next = ListNode(4)
head.next.next.next.next = ListNode(5)

# Reorder the linked list
reorder_list(head)

# Print the reordered linked list: 1 → 5 → 2 → 4 → 3
result = []
current = head
while current:
    result.append(current.val)
    current = current.next
print(result)  # Output: [1, 5, 2, 4, 3]
```

**JavaScript Solution:**
```javascript
function reorderList(head) {
    if (!head || !head.next) {
        return;
    }
    
    // Step 1: Find the middle of the linked list
    let slow = head;
    let fast = head;
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    // Step 2: Reverse the second half of the linked list
    let prev = null;
    let current = slow;
    while (current) {
        const nextTemp = current.next;
        current.next = prev;
        prev = current;
        current = nextTemp;
    }
    
    // Step 3: Merge the first half and the reversed second half
    let first = head;
    let second = prev;
    while (second.next) {
        const temp1 = first.next;
        const temp2 = second.next;
        first.next = second;
        second.next = temp1;
        first = temp1;
        second = temp2;
    }
}

// Example usage
// Create a linked list: 1 → 2 → 3 → 4 → 5
const head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
head.next.next.next = new ListNode(4);
head.next.next.next.next = new ListNode(5);

// Reorder the linked list
reorderList(head);

// Print the reordered linked list: 1 → 5 → 2 → 4 → 3
const result = [];
let current = head;
while (current) {
    result.push(current.val);
    current = current.next;
}
console.log(result);  // Output: [1, 5, 2, 4, 3]
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

## Time and Space Complexity

| Algorithm | Time Complexity | Space Complexity |
|-----------|----------------|-----------------|
| Reverse a Linked List | O(n) | O(1) |
| Reverse a Sublist | O(n) | O(1) |
| Reverse Nodes in k-Group | O(n) | O(1) |
| Rotate a Linked List | O(n) | O(1) |
| Reorder List | O(n) | O(1) |

## Tips and Tricks for In-place Reversal of a Linked List

1. **Use Multiple Pointers**: Keep track of the previous, current, and next nodes to perform the reversal.

2. **Dummy Node**: Use a dummy node to handle edge cases, especially when dealing with the head of the list.

3. **Two-Step Process**: For complex operations like reordering, break the problem into smaller steps (e.g., find the middle, reverse the second half, merge).

4. **Careful Pointer Manipulation**: Be careful when updating pointers to avoid creating cycles or losing parts of the list.

5. **Edge Cases**: Always consider edge cases like empty lists, single-node lists, or when the operation doesn't need to be performed (e.g., k = 0 or k = 1 in k-group reversal).

6. **Visualize the Process**: Draw out the linked list and the pointer changes to better understand the algorithm.

## Common Pitfalls

1. **Losing References**: Not saving the next node before changing pointers can lead to losing parts of the list.

2. **Creating Cycles**: Incorrectly updating pointers can create cycles in the linked list.

3. **Off-by-One Errors**: Miscounting positions or nodes can lead to incorrect results.

4. **Not Handling Edge Cases**: Forgetting to handle empty lists, single-node lists, or other edge cases.

5. **Incorrect Termination Conditions**: Using the wrong conditions to terminate loops can lead to infinite loops or incomplete operations.

## How to Identify In-place Reversal of a Linked List Problems

Look for these clues in the problem statement:

1. The problem involves a linked list and requires modifying its structure.
2. The problem asks for reversing a linked list or a part of it.
3. The problem requires reordering or rotating a linked list.
4. The problem specifies that the solution should use O(1) space complexity.
5. Keywords like "reverse," "rotate," "reorder," or "in-place."

## Common In-place Reversal of a Linked List Problems from Blind 75 and Grind 75

1. **Reverse Linked List** (Easy): Reverse a singly linked list.
2. **Reverse Linked List II** (Medium): Reverse a linked list from position m to n.
3. **Reverse Nodes in k-Group** (Hard): Reverse the nodes of a linked list k at a time.
4. **Rotate List** (Medium): Rotate a linked list to the right by k places.
5. **Reorder List** (Medium): Reorder a linked list in a specific pattern.
6. **Swap Nodes in Pairs** (Medium): Swap every two adjacent nodes in a linked list.
7. **Palindrome Linked List** (Easy): Check if a linked list is a palindrome.

## In-place Reversal of a Linked List Template

Here's a general template for solving in-place reversal of a linked list problems:

```python
def reverse_linked_list_template(head, condition_function):
    prev = None
    current = head
    
    while current and condition_function(current):
        # Save the next node
        next_temp = current.next
        
        # Reverse the current node's pointer
        current.next = prev
        
        # Move prev and current one step forward
        prev = current
        current = next_temp
    
    # Additional logic based on the specific problem
    
    return prev  # Or another node based on the problem
```

## Real-World Applications

1. **Undo/Redo Functionality**: Implementing undo/redo operations in text editors or other applications.
2. **Transaction Processing**: Managing and potentially reversing transactions in financial systems.
3. **Browser History**: Implementing forward and backward navigation in web browsers.
4. **Memory Management**: Efficiently managing linked memory blocks.
5. **Network Packet Processing**: Reordering or reversing network packets in certain protocols.
6. **Data Serialization/Deserialization**: Converting between different data formats that use linked structures. 