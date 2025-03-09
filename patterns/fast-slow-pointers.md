# Fast & Slow Pointers Pattern

The Fast & Slow Pointers pattern (also known as the Hare & Tortoise algorithm) uses two pointers that move through a data structure at different speeds. This approach is particularly useful for cycle detection, finding the middle of a linked list, or determining if a linked list has a cycle.

## Visual Representation

```
Linked List with a cycle:

1 → 2 → 3 → 4 → 5 → 6
        ↑         ↓
        9 ← 8 ← 7

Initial state:
1 → 2 → 3 → 4 → 5 → 6
↑       ↑         ↓
S,F     F         ↓
        9 ← 8 ← 7

After some iterations:
1 → 2 → 3 → 4 → 5 → 6
        ↑       ↑ ↓
        S       F ↓
        9 ← 8 ← 7

Eventually:
1 → 2 → 3 → 4 → 5 → 6
        ↑         ↓
        S,F       ↓
        9 ← 8 ← 7
```

## When to Use Fast & Slow Pointers

- Detecting cycles in a linked list
- Finding the middle element of a linked list
- Finding the kth element from the end of a linked list
- Determining if a linked list is a palindrome
- Finding the start of a cycle in a linked list
- Finding the happy number

## Common Problems and Solutions

### 1. Detect Cycle in a Linked List

**Problem:** Given a linked list, determine if it has a cycle in it.

**Python Solution:**
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def has_cycle(head):
    if not head or not head.next:
        return False
    
    slow = head
    fast = head
    
    while fast and fast.next:
        slow = slow.next          # Move slow pointer by 1
        fast = fast.next.next     # Move fast pointer by 2
        
        if slow == fast:          # If they meet, there's a cycle
            return True
    
    return False  # If fast reaches the end, no cycle

# Example usage
# Create a linked list with a cycle: 1 -> 2 -> 3 -> 4 -> 5 -> 3 (points back to 3)
head = ListNode(1)
head.next = ListNode(2)
head.next.next = ListNode(3)
head.next.next.next = ListNode(4)
head.next.next.next.next = ListNode(5)
head.next.next.next.next.next = head.next.next  # Create cycle

print(has_cycle(head))  # Output: True
```

**JavaScript Solution:**
```javascript
class ListNode {
    constructor(val = 0, next = null) {
        this.val = val;
        this.next = next;
    }
}

function hasCycle(head) {
    if (!head || !head.next) {
        return false;
    }
    
    let slow = head;
    let fast = head;
    
    while (fast && fast.next) {
        slow = slow.next;         // Move slow pointer by 1
        fast = fast.next.next;    // Move fast pointer by 2
        
        if (slow === fast) {      // If they meet, there's a cycle
            return true;
        }
    }
    
    return false;  // If fast reaches the end, no cycle
}

// Example usage
// Create a linked list with a cycle: 1 -> 2 -> 3 -> 4 -> 5 -> 3 (points back to 3)
const head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
head.next.next.next = new ListNode(4);
head.next.next.next.next = new ListNode(5);
head.next.next.next.next.next = head.next.next;  // Create cycle

console.log(hasCycle(head));  // Output: true
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

### 2. Find the Middle of a Linked List

**Problem:** Given a non-empty, singly linked list, return the middle node of the linked list. If there are two middle nodes, return the second middle node.

**Python Solution:**
```python
def middle_node(head):
    if not head:
        return None
    
    slow = head
    fast = head
    
    while fast and fast.next:
        slow = slow.next          # Move slow pointer by 1
        fast = fast.next.next     # Move fast pointer by 2
    
    return slow  # When fast reaches the end, slow is at the middle

# Example usage
# Create a linked list: 1 -> 2 -> 3 -> 4 -> 5
head = ListNode(1)
head.next = ListNode(2)
head.next.next = ListNode(3)
head.next.next.next = ListNode(4)
head.next.next.next.next = ListNode(5)

middle = middle_node(head)
print(middle.val)  # Output: 3
```

**JavaScript Solution:**
```javascript
function middleNode(head) {
    if (!head) {
        return null;
    }
    
    let slow = head;
    let fast = head;
    
    while (fast && fast.next) {
        slow = slow.next;         // Move slow pointer by 1
        fast = fast.next.next;    // Move fast pointer by 2
    }
    
    return slow;  // When fast reaches the end, slow is at the middle
}

// Example usage
// Create a linked list: 1 -> 2 -> 3 -> 4 -> 5
const head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
head.next.next.next = new ListNode(4);
head.next.next.next.next = new ListNode(5);

const middle = middleNode(head);
console.log(middle.val);  // Output: 3
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

### 3. Find the Start of a Cycle in a Linked List

**Problem:** Given a linked list with a cycle, return the node where the cycle begins.

**Python Solution:**
```python
def detect_cycle(head):
    if not head or not head.next:
        return None
    
    slow = head
    fast = head
    
    # Find the meeting point inside the cycle
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
        if slow == fast:  # Meeting point found
            break
    
    # If no cycle, return None
    if not fast or not fast.next:
        return None
    
    # Reset one pointer to the head and move both at the same pace
    slow = head
    while slow != fast:
        slow = slow.next
        fast = fast.next
    
    return slow  # This is the start of the cycle

# Example usage
# Create a linked list with a cycle: 1 -> 2 -> 3 -> 4 -> 5 -> 3 (points back to 3)
head = ListNode(1)
head.next = ListNode(2)
head.next.next = ListNode(3)
head.next.next.next = ListNode(4)
head.next.next.next.next = ListNode(5)
head.next.next.next.next.next = head.next.next  # Create cycle

cycle_start = detect_cycle(head)
print(cycle_start.val)  # Output: 3
```

**JavaScript Solution:**
```javascript
function detectCycle(head) {
    if (!head || !head.next) {
        return null;
    }
    
    let slow = head;
    let fast = head;
    
    // Find the meeting point inside the cycle
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
        
        if (slow === fast) {  // Meeting point found
            break;
        }
    }
    
    // If no cycle, return null
    if (!fast || !fast.next) {
        return null;
    }
    
    // Reset one pointer to the head and move both at the same pace
    slow = head;
    while (slow !== fast) {
        slow = slow.next;
        fast = fast.next;
    }
    
    return slow;  // This is the start of the cycle
}

// Example usage
// Create a linked list with a cycle: 1 -> 2 -> 3 -> 4 -> 5 -> 3 (points back to 3)
const head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
head.next.next.next = new ListNode(4);
head.next.next.next.next = new ListNode(5);
head.next.next.next.next.next = head.next.next;  // Create cycle

const cycleStart = detectCycle(head);
console.log(cycleStart.val);  // Output: 3
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

### 4. Happy Number

**Problem:** A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

**Python Solution:**
```python
def is_happy(n):
    def get_next(number):
        total_sum = 0
        while number > 0:
            digit = number % 10
            total_sum += digit ** 2
            number //= 10
        return total_sum
    
    slow = n
    fast = get_next(n)
    
    while fast != 1 and slow != fast:
        slow = get_next(slow)           # Move slow pointer by 1
        fast = get_next(get_next(fast))  # Move fast pointer by 2
    
    return fast == 1  # If fast reaches 1, it's a happy number

# Example usage
print(is_happy(19))  # Output: True (19 -> 82 -> 68 -> 100 -> 1)
print(is_happy(2))   # Output: False (2 -> 4 -> 16 -> 37 -> 58 -> 89 -> 145 -> 42 -> 20 -> 4 -> ...)
```

**JavaScript Solution:**
```javascript
function isHappy(n) {
    function getNext(number) {
        let totalSum = 0;
        while (number > 0) {
            const digit = number % 10;
            totalSum += digit ** 2;
            number = Math.floor(number / 10);
        }
        return totalSum;
    }
    
    let slow = n;
    let fast = getNext(n);
    
    while (fast !== 1 && slow !== fast) {
        slow = getNext(slow);           // Move slow pointer by 1
        fast = getNext(getNext(fast));  // Move fast pointer by 2
    }
    
    return fast === 1;  // If fast reaches 1, it's a happy number
}

// Example usage
console.log(isHappy(19));  // Output: true (19 -> 82 -> 68 -> 100 -> 1)
console.log(isHappy(2));   // Output: false (2 -> 4 -> 16 -> 37 -> 58 -> 89 -> 145 -> 42 -> 20 -> 4 -> ...)
```

**Time Complexity:** O(log n)  
**Space Complexity:** O(1)

### 5. Palindrome Linked List

**Problem:** Given a singly linked list, determine if it is a palindrome.

**Python Solution:**
```python
def is_palindrome(head):
    if not head or not head.next:
        return True
    
    # Find the middle of the linked list
    slow = head
    fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    
    # Reverse the second half
    prev = None
    current = slow
    
    while current:
        next_temp = current.next
        current.next = prev
        prev = current
        current = next_temp
    
    # Compare the first and second half
    first_half = head
    second_half = prev
    
    while second_half:
        if first_half.val != second_half.val:
            return False
        first_half = first_half.next
        second_half = second_half.next
    
    return True

# Example usage
# Create a palindrome linked list: 1 -> 2 -> 2 -> 1
head = ListNode(1)
head.next = ListNode(2)
head.next.next = ListNode(2)
head.next.next.next = ListNode(1)

print(is_palindrome(head))  # Output: True
```

**JavaScript Solution:**
```javascript
function isPalindrome(head) {
    if (!head || !head.next) {
        return true;
    }
    
    // Find the middle of the linked list
    let slow = head;
    let fast = head;
    
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    // Reverse the second half
    let prev = null;
    let current = slow;
    
    while (current) {
        const nextTemp = current.next;
        current.next = prev;
        prev = current;
        current = nextTemp;
    }
    
    // Compare the first and second half
    let firstHalf = head;
    let secondHalf = prev;
    
    while (secondHalf) {
        if (firstHalf.val !== secondHalf.val) {
            return false;
        }
        firstHalf = firstHalf.next;
        secondHalf = secondHalf.next;
    }
    
    return true;
}

// Example usage
// Create a palindrome linked list: 1 -> 2 -> 2 -> 1
const head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(2);
head.next.next.next = new ListNode(1);

console.log(isPalindrome(head));  // Output: true
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

## How to Identify Fast & Slow Pointers Problems

Look for these clues in the problem statement:

1. The problem involves a linked list or a sequence that might have a cycle
2. You need to find the middle element of a linked list
3. You need to determine if a linked list is a palindrome
4. You need to find the kth element from the end of a linked list
5. The problem involves detecting a cycle or finding the start of a cycle
6. Keywords like "cycle," "loop," "middle," "circular," or "tortoise and hare"

## Mathematical Intuition

The Fast & Slow Pointers pattern works because:

1. **Cycle Detection**: If there's a cycle, the fast pointer will eventually catch up to the slow pointer. This is because the fast pointer moves 1 step closer to the slow pointer in each iteration.

2. **Finding the Middle**: When the fast pointer reaches the end, the slow pointer will be at the middle. This is because the fast pointer travels twice as fast, so the slow pointer will have traveled half the distance.

3. **Finding the Start of a Cycle**: After finding the meeting point, if we reset one pointer to the head and move both at the same pace, they will meet at the start of the cycle. This is based on the mathematical property that if the distance from the head to the cycle start is 'a', and the distance from the cycle start to the meeting point is 'b', then the distance from the meeting point back to the cycle start (going around the cycle) is also 'a'.

## Tips and Tricks

1. **Always check for null pointers**: Before accessing `next` or `next.next`, make sure the current pointer is not null.

2. **Handle edge cases**: Consider empty lists, single-element lists, and lists with no cycles.

3. **Use the pattern for cycle detection first**: If you're not sure if a linked list has a cycle, check for that before trying to find the middle or other operations.

4. **Understand the mathematical intuition**: Knowing why the algorithm works helps in adapting it to different problems.

5. **Consider the stopping condition**: Depending on the problem, you might want to stop when `fast` reaches the end or when `fast` and `slow` meet.

## Common Pitfalls

1. **Not checking for null pointers**: This can lead to null pointer exceptions.

2. **Incorrect initialization**: Both pointers should usually start at the head.

3. **Incorrect movement**: The slow pointer should move one step at a time, and the fast pointer should move two steps.

4. **Not handling edge cases**: Empty lists, single-element lists, and lists with no cycles need special handling.

5. **Infinite loops**: If not implemented correctly, the algorithm might enter an infinite loop.

## Common Fast & Slow Pointers Problems from Blind 75 and Grind 75

1. **Linked List Cycle** (Easy): Determine if a linked list has a cycle.
2. **Linked List Cycle II** (Medium): Find the node where the cycle begins in a linked list.
3. **Middle of the Linked List** (Easy): Find the middle node of a linked list.
4. **Palindrome Linked List** (Easy): Determine if a linked list is a palindrome.
5. **Reorder List** (Medium): Reorder a linked list to L₀ → Lₙ → L₁ → Lₙ₋₁ → L₂ → Lₙ₋₂ → ...
6. **Happy Number** (Easy): Determine if a number is a happy number.
7. **Find the Duplicate Number** (Medium): Find the duplicate number in an array where each integer appears only once except for one integer which appears multiple times. 