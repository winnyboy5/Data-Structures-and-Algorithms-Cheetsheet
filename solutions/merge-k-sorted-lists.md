# Merge K Sorted Lists

## Problem Statement

You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

**Example 1:**
```
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
Merging them into one sorted list:
1->1->2->3->4->4->5->6
```

**Example 2:**
```
Input: lists = []
Output: []
```

**Example 3:**
```
Input: lists = [[]]
Output: []
```

## Intuition

The key insight for this problem is to efficiently merge multiple sorted lists. There are several approaches we can take:

1. **Brute Force**: Concatenate all lists and sort the result. This is simple but not efficient.
2. **Sequential Merging**: Merge lists one by one. This is better but still not optimal.
3. **Divide and Conquer**: Recursively merge pairs of lists until only one list remains.
4. **Priority Queue (Min Heap)**: Use a min heap to efficiently find the smallest element among all lists.

The priority queue approach is particularly elegant and efficient, as it allows us to always pick the smallest element from all lists in logarithmic time.

## Visual Explanation

Let's visualize the priority queue approach with Example 1: `lists = [[1,4,5],[1,3,4],[2,6]]`

First, we initialize a min heap with the first element from each list:
```
Heap: [(1, list1), (1, list2), (2, list3)]
Result: []
```

Now, we repeatedly extract the minimum element from the heap, add it to our result, and add the next element from the same list to the heap:

1. Extract min (1, list1):
   - Add 1 to result: [1]
   - Add next element from list1 (4) to heap: [(1, list2), (2, list3), (4, list1)]

2. Extract min (1, list2):
   - Add 1 to result: [1, 1]
   - Add next element from list2 (3) to heap: [(2, list3), (3, list2), (4, list1)]

3. Extract min (2, list3):
   - Add 2 to result: [1, 1, 2]
   - Add next element from list3 (6) to heap: [(3, list2), (4, list1), (6, list3)]

4. Extract min (3, list2):
   - Add 3 to result: [1, 1, 2, 3]
   - Add next element from list2 (4) to heap: [(4, list1), (4, list2), (6, list3)]

5. Extract min (4, list1):
   - Add 4 to result: [1, 1, 2, 3, 4]
   - Add next element from list1 (5) to heap: [(4, list2), (5, list1), (6, list3)]

6. Extract min (4, list2):
   - Add 4 to result: [1, 1, 2, 3, 4, 4]
   - No more elements in list2, so heap: [(5, list1), (6, list3)]

7. Extract min (5, list1):
   - Add 5 to result: [1, 1, 2, 3, 4, 4, 5]
   - No more elements in list1, so heap: [(6, list3)]

8. Extract min (6, list3):
   - Add 6 to result: [1, 1, 2, 3, 4, 4, 5, 6]
   - No more elements in list3, so heap is empty

Final result: [1, 1, 2, 3, 4, 4, 5, 6]

![Merge K Sorted Lists Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

### Priority Queue (Min Heap) Approach:

1. Create a min heap to store the first node from each list, along with a reference to which list it came from.
2. Initialize an empty result list and a dummy head node.
3. While the heap is not empty:
   - Extract the minimum element from the heap.
   - Add this element to the result list.
   - If this element has a next node, add the next node to the heap.
4. Return the result list.

### Divide and Conquer Approach:

1. If the array of lists is empty, return null.
2. If there's only one list, return it.
3. Recursively merge pairs of lists until only one list remains:
   - Divide the array of lists into two halves.
   - Recursively merge each half.
   - Merge the two sorted halves using the merge two sorted lists algorithm.
4. Return the final merged list.

## Code Implementation

### Python (Priority Queue)

```python
import heapq

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def mergeKLists(lists):
    # Custom wrapper to make ListNode comparable
    class Wrapper:
        def __init__(self, node):
            self.node = node
        def __lt__(self, other):
            return self.node.val < other.node.val
    
    # Initialize the heap
    heap = []
    for i, lst in enumerate(lists):
        if lst:
            heapq.heappush(heap, Wrapper(lst))
    
    # Create a dummy head for the result list
    dummy = ListNode(0)
    curr = dummy
    
    # Process nodes from the heap
    while heap:
        # Extract the minimum node
        wrapper = heapq.heappop(heap)
        node = wrapper.node
        
        # Add to result list
        curr.next = node
        curr = curr.next
        
        # Add the next node from the same list to the heap
        if node.next:
            heapq.heappush(heap, Wrapper(node.next))
    
    return dummy.next
```

### JavaScript (Priority Queue)

```javascript
class MinHeap {
    constructor() {
        this.heap = [];
    }
    
    push(node) {
        this.heap.push(node);
        this.heapifyUp(this.heap.length - 1);
    }
    
    pop() {
        if (this.heap.length === 0) return null;
        
        const min = this.heap[0];
        const last = this.heap.pop();
        
        if (this.heap.length > 0) {
            this.heap[0] = last;
            this.heapifyDown(0);
        }
        
        return min;
    }
    
    heapifyUp(index) {
        const parent = Math.floor((index - 1) / 2);
        if (index > 0 && this.heap[parent].val > this.heap[index].val) {
            [this.heap[parent], this.heap[index]] = [this.heap[index], this.heap[parent]];
            this.heapifyUp(parent);
        }
    }
    
    heapifyDown(index) {
        const left = 2 * index + 1;
        const right = 2 * index + 2;
        let smallest = index;
        
        if (left < this.heap.length && this.heap[left].val < this.heap[smallest].val) {
            smallest = left;
        }
        
        if (right < this.heap.length && this.heap[right].val < this.heap[smallest].val) {
            smallest = right;
        }
        
        if (smallest !== index) {
            [this.heap[index], this.heap[smallest]] = [this.heap[smallest], this.heap[index]];
            this.heapifyDown(smallest);
        }
    }
    
    size() {
        return this.heap.length;
    }
}

function mergeKLists(lists) {
    // Initialize the heap
    const heap = new MinHeap();
    for (const list of lists) {
        if (list) {
            heap.push(list);
        }
    }
    
    // Create a dummy head for the result list
    const dummy = { val: 0, next: null };
    let curr = dummy;
    
    // Process nodes from the heap
    while (heap.size() > 0) {
        // Extract the minimum node
        const node = heap.pop();
        
        // Add to result list
        curr.next = node;
        curr = curr.next;
        
        // Add the next node from the same list to the heap
        if (node.next) {
            heap.push(node.next);
        }
    }
    
    return dummy.next;
}
```

### Python (Divide and Conquer)

```python
def mergeKLists(lists):
    if not lists:
        return None
    
    # Helper function to merge two sorted lists
    def mergeTwoLists(l1, l2):
        dummy = ListNode(0)
        curr = dummy
        
        while l1 and l2:
            if l1.val < l2.val:
                curr.next = l1
                l1 = l1.next
            else:
                curr.next = l2
                l2 = l2.next
            curr = curr.next
        
        curr.next = l1 if l1 else l2
        return dummy.next
    
    # Recursive function to merge lists using divide and conquer
    def merge(lists, start, end):
        if start == end:
            return lists[start]
        if start > end:
            return None
        
        mid = (start + end) // 2
        left = merge(lists, start, mid)
        right = merge(lists, mid + 1, end)
        
        return mergeTwoLists(left, right)
    
    return merge(lists, 0, len(lists) - 1)
```

### JavaScript (Divide and Conquer)

```javascript
function mergeKLists(lists) {
    if (!lists || lists.length === 0) {
        return null;
    }
    
    // Helper function to merge two sorted lists
    function mergeTwoLists(l1, l2) {
        const dummy = { val: 0, next: null };
        let curr = dummy;
        
        while (l1 && l2) {
            if (l1.val < l2.val) {
                curr.next = l1;
                l1 = l1.next;
            } else {
                curr.next = l2;
                l2 = l2.next;
            }
            curr = curr.next;
        }
        
        curr.next = l1 || l2;
        return dummy.next;
    }
    
    // Recursive function to merge lists using divide and conquer
    function merge(lists, start, end) {
        if (start === end) {
            return lists[start];
        }
        if (start > end) {
            return null;
        }
        
        const mid = Math.floor((start + end) / 2);
        const left = merge(lists, start, mid);
        const right = merge(lists, mid + 1, end);
        
        return mergeTwoLists(left, right);
    }
    
    return merge(lists, 0, lists.length - 1);
}
```

## Time and Space Complexity

### Priority Queue (Min Heap) Approach:
- **Time Complexity**: O(N log k), where N is the total number of nodes across all lists and k is the number of lists. Each insertion and extraction from the heap takes O(log k) time, and we do this for each of the N nodes.
- **Space Complexity**: O(k) for the heap, which stores at most k nodes (one from each list).

### Divide and Conquer Approach:
- **Time Complexity**: O(N log k), where N is the total number of nodes and k is the number of lists. The merge operation is O(N), and we perform log k levels of merging.
- **Space Complexity**: O(log k) for the recursion stack.

## Detailed Walkthrough with Example

Let's trace through the priority queue algorithm with a simpler example: `lists = [[1,3],[2,4],[5]]`

1. Initialize the heap with the first node from each list:
   - Heap: [(1, list1), (2, list2), (5, list3)]
   - Result: []

2. Extract min (1, list1):
   - Add 1 to result: [1]
   - Add next element from list1 (3) to heap: [(2, list2), (3, list1), (5, list3)]

3. Extract min (2, list2):
   - Add 2 to result: [1, 2]
   - Add next element from list2 (4) to heap: [(3, list1), (4, list2), (5, list3)]

4. Extract min (3, list1):
   - Add 3 to result: [1, 2, 3]
   - No more elements in list1, so heap: [(4, list2), (5, list3)]

5. Extract min (4, list2):
   - Add 4 to result: [1, 2, 3, 4]
   - No more elements in list2, so heap: [(5, list3)]

6. Extract min (5, list3):
   - Add 5 to result: [1, 2, 3, 4, 5]
   - No more elements in list3, so heap is empty

Final result: [1, 2, 3, 4, 5]

## Edge Cases and Optimizations

- **Empty Lists**: If the input array is empty or contains only empty lists, the function will return null.
- **Single List**: If there's only one list, we can return it directly without any merging.
- **Lists with Different Lengths**: The algorithm handles lists of different lengths naturally.

**Optimization**: In the divide and conquer approach, we can use an iterative implementation to avoid the overhead of recursion. We can also optimize the priority queue approach by using a custom comparator for the heap.

## Common Mistakes

1. **Not handling empty lists**: Make sure to check if the input array is empty or contains only empty lists.
2. **Incorrect heap implementation**: Be careful with the heap implementation, especially when comparing nodes.
3. **Not maintaining the list structure**: Remember that we're merging linked lists, not just sorting values.

## Related Problems

1. **Merge Two Sorted Lists**: Merge two sorted linked lists into one sorted list.
2. **Sort List**: Sort a linked list in O(n log n) time and O(1) space.
3. **Kth Smallest Element in a Sorted Matrix**: Find the kth smallest element in a sorted matrix.
4. **Find K Pairs with Smallest Sums**: Find k pairs with the smallest sums from two arrays. 