# Heaps

A heap is a specialized tree-based data structure that satisfies the heap property. In a max heap, for any given node, the value of the node is greater than or equal to the values of its children. In a min heap, the value of the node is less than or equal to the values of its children.

## Visual Representation

```
Max Heap:
        ┌───┐
        │ 9 │
        └─┬─┘
     ┌────┴────┐
  ┌──┴──┐   ┌──┴──┐
  │  7  │   │  8  │
  └──┬──┘   └──┬──┘
  ┌──┴──┐   ┌──┴──┐
  │  5  │   │  6  │
  └─────┘   └─────┘

Min Heap:
        ┌───┐
        │ 1 │
        └─┬─┘
     ┌────┴────┐
  ┌──┴──┐   ┌──┴──┐
  │  2  │   │  3  │
  └──┬──┘   └──┬──┘
  ┌──┴──┐   ┌──┴──┐
  │  4  │   │  5  │
  └─────┘   └─────┘
```

## Key Operations

1. **Insert**: Add an element to the heap
2. **Extract Min/Max**: Remove and return the minimum (min heap) or maximum (max heap) element
3. **Peek**: View the minimum (min heap) or maximum (max heap) element without removing it
4. **Heapify**: Convert an array into a heap

## Implementation in Python and JavaScript

### Python

#### Using the heapq Module (Min Heap)

```python
import heapq

# Create a min heap
min_heap = []
heapq.heapify(min_heap)  # Convert list to a heap (not necessary for empty list)

# Insert elements
heapq.heappush(min_heap, 5)
heapq.heappush(min_heap, 3)
heapq.heappush(min_heap, 8)
heapq.heappush(min_heap, 1)
heapq.heappush(min_heap, 10)

print(min_heap)  # Output: [1, 3, 8, 5, 10]

# Extract the minimum element
min_element = heapq.heappop(min_heap)
print(min_element)  # Output: 1
print(min_heap)     # Output: [3, 5, 8, 10]

# Peek at the minimum element
print(min_heap[0])  # Output: 3

# Create a heap from an existing list
nums = [5, 3, 8, 1, 10]
heapq.heapify(nums)
print(nums)  # Output: [1, 3, 8, 5, 10]

# For max heap, negate the values
max_heap = []
heapq.heappush(max_heap, -5)
heapq.heappush(max_heap, -3)
heapq.heappush(max_heap, -8)
heapq.heappush(max_heap, -1)
heapq.heappush(max_heap, -10)

# Extract the maximum element
max_element = -heapq.heappop(max_heap)
print(max_element)  # Output: 10
```

#### Custom Implementation (Min Heap)

```python
class MinHeap:
    def __init__(self):
        self.heap = []
    
    def parent(self, i):
        return (i - 1) // 2
    
    def left_child(self, i):
        return 2 * i + 1
    
    def right_child(self, i):
        return 2 * i + 2
    
    def get_min(self):
        if not self.heap:
            return None
        return self.heap[0]
    
    def insert(self, key):
        self.heap.append(key)
        i = len(self.heap) - 1
        self._sift_up(i)
    
    def extract_min(self):
        if not self.heap:
            return None
        
        min_val = self.heap[0]
        last_val = self.heap.pop()
        
        if self.heap:
            self.heap[0] = last_val
            self._sift_down(0)
        
        return min_val
    
    def _sift_up(self, i):
        parent = self.parent(i)
        if i > 0 and self.heap[parent] > self.heap[i]:
            self.heap[parent], self.heap[i] = self.heap[i], self.heap[parent]
            self._sift_up(parent)
    
    def _sift_down(self, i):
        min_index = i
        left = self.left_child(i)
        right = self.right_child(i)
        
        if left < len(self.heap) and self.heap[left] < self.heap[min_index]:
            min_index = left
        
        if right < len(self.heap) and self.heap[right] < self.heap[min_index]:
            min_index = right
        
        if i != min_index:
            self.heap[i], self.heap[min_index] = self.heap[min_index], self.heap[i]
            self._sift_down(min_index)
    
    def size(self):
        return len(self.heap)
    
    def is_empty(self):
        return len(self.heap) == 0

# Example usage
min_heap = MinHeap()
min_heap.insert(5)
min_heap.insert(3)
min_heap.insert(8)
min_heap.insert(1)
min_heap.insert(10)

print(min_heap.heap)  # Output: [1, 3, 8, 5, 10]

min_element = min_heap.extract_min()
print(min_element)    # Output: 1
print(min_heap.heap)  # Output: [3, 5, 8, 10]
```

#### Custom Implementation (Max Heap)

```python
class MaxHeap:
    def __init__(self):
        self.heap = []
    
    def parent(self, i):
        return (i - 1) // 2
    
    def left_child(self, i):
        return 2 * i + 1
    
    def right_child(self, i):
        return 2 * i + 2
    
    def get_max(self):
        if not self.heap:
            return None
        return self.heap[0]
    
    def insert(self, key):
        self.heap.append(key)
        i = len(self.heap) - 1
        self._sift_up(i)
    
    def extract_max(self):
        if not self.heap:
            return None
        
        max_val = self.heap[0]
        last_val = self.heap.pop()
        
        if self.heap:
            self.heap[0] = last_val
            self._sift_down(0)
        
        return max_val
    
    def _sift_up(self, i):
        parent = self.parent(i)
        if i > 0 and self.heap[parent] < self.heap[i]:
            self.heap[parent], self.heap[i] = self.heap[i], self.heap[parent]
            self._sift_up(parent)
    
    def _sift_down(self, i):
        max_index = i
        left = self.left_child(i)
        right = self.right_child(i)
        
        if left < len(self.heap) and self.heap[left] > self.heap[max_index]:
            max_index = left
        
        if right < len(self.heap) and self.heap[right] > self.heap[max_index]:
            max_index = right
        
        if i != max_index:
            self.heap[i], self.heap[max_index] = self.heap[max_index], self.heap[i]
            self._sift_down(max_index)
    
    def size(self):
        return len(self.heap)
    
    def is_empty(self):
        return len(self.heap) == 0

# Example usage
max_heap = MaxHeap()
max_heap.insert(5)
max_heap.insert(3)
max_heap.insert(8)
max_heap.insert(1)
max_heap.insert(10)

print(max_heap.heap)  # Output: [10, 5, 8, 1, 3]

max_element = max_heap.extract_max()
print(max_element)    # Output: 10
print(max_heap.heap)  # Output: [8, 5, 3, 1]
```

### JavaScript

#### Custom Implementation (Min Heap)

```javascript
class MinHeap {
    constructor() {
        this.heap = [];
    }
    
    parent(i) {
        return Math.floor((i - 1) / 2);
    }
    
    leftChild(i) {
        return 2 * i + 1;
    }
    
    rightChild(i) {
        return 2 * i + 2;
    }
    
    getMin() {
        if (this.heap.length === 0) {
            return null;
        }
        return this.heap[0];
    }
    
    insert(key) {
        this.heap.push(key);
        let i = this.heap.length - 1;
        this._siftUp(i);
    }
    
    extractMin() {
        if (this.heap.length === 0) {
            return null;
        }
        
        const minVal = this.heap[0];
        const lastVal = this.heap.pop();
        
        if (this.heap.length > 0) {
            this.heap[0] = lastVal;
            this._siftDown(0);
        }
        
        return minVal;
    }
    
    _siftUp(i) {
        const parent = this.parent(i);
        if (i > 0 && this.heap[parent] > this.heap[i]) {
            [this.heap[parent], this.heap[i]] = [this.heap[i], this.heap[parent]];
            this._siftUp(parent);
        }
    }
    
    _siftDown(i) {
        let minIndex = i;
        const left = this.leftChild(i);
        const right = this.rightChild(i);
        
        if (left < this.heap.length && this.heap[left] < this.heap[minIndex]) {
            minIndex = left;
        }
        
        if (right < this.heap.length && this.heap[right] < this.heap[minIndex]) {
            minIndex = right;
        }
        
        if (i !== minIndex) {
            [this.heap[i], this.heap[minIndex]] = [this.heap[minIndex], this.heap[i]];
            this._siftDown(minIndex);
        }
    }
    
    size() {
        return this.heap.length;
    }
    
    isEmpty() {
        return this.heap.length === 0;
    }
}

// Example usage
const minHeap = new MinHeap();
minHeap.insert(5);
minHeap.insert(3);
minHeap.insert(8);
minHeap.insert(1);
minHeap.insert(10);

console.log(minHeap.heap);  // Output: [1, 3, 8, 5, 10]

const minElement = minHeap.extractMin();
console.log(minElement);    // Output: 1
console.log(minHeap.heap);  // Output: [3, 5, 8, 10]
```

#### Custom Implementation (Max Heap)

```javascript
class MaxHeap {
    constructor() {
        this.heap = [];
    }
    
    parent(i) {
        return Math.floor((i - 1) / 2);
    }
    
    leftChild(i) {
        return 2 * i + 1;
    }
    
    rightChild(i) {
        return 2 * i + 2;
    }
    
    getMax() {
        if (this.heap.length === 0) {
            return null;
        }
        return this.heap[0];
    }
    
    insert(key) {
        this.heap.push(key);
        let i = this.heap.length - 1;
        this._siftUp(i);
    }
    
    extractMax() {
        if (this.heap.length === 0) {
            return null;
        }
        
        const maxVal = this.heap[0];
        const lastVal = this.heap.pop();
        
        if (this.heap.length > 0) {
            this.heap[0] = lastVal;
            this._siftDown(0);
        }
        
        return maxVal;
    }
    
    _siftUp(i) {
        const parent = this.parent(i);
        if (i > 0 && this.heap[parent] < this.heap[i]) {
            [this.heap[parent], this.heap[i]] = [this.heap[i], this.heap[parent]];
            this._siftUp(parent);
        }
    }
    
    _siftDown(i) {
        let maxIndex = i;
        const left = this.leftChild(i);
        const right = this.rightChild(i);
        
        if (left < this.heap.length && this.heap[left] > this.heap[maxIndex]) {
            maxIndex = left;
        }
        
        if (right < this.heap.length && this.heap[right] > this.heap[maxIndex]) {
            maxIndex = right;
        }
        
        if (i !== maxIndex) {
            [this.heap[i], this.heap[maxIndex]] = [this.heap[maxIndex], this.heap[i]];
            this._siftDown(maxIndex);
        }
    }
    
    size() {
        return this.heap.length;
    }
    
    isEmpty() {
        return this.heap.length === 0;
    }
}

// Example usage
const maxHeap = new MaxHeap();
maxHeap.insert(5);
maxHeap.insert(3);
maxHeap.insert(8);
maxHeap.insert(1);
maxHeap.insert(10);

console.log(maxHeap.heap);  // Output: [10, 5, 8, 1, 3]

const maxElement = maxHeap.extractMax();
console.log(maxElement);    // Output: 10
console.log(maxHeap.heap);  // Output: [8, 5, 3, 1]
```

## Common Heap Algorithms

### 1. Heap Sort

**Python:**
```python
def heap_sort(arr):
    n = len(arr)
    
    # Build a max heap
    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i)
    
    # Extract elements one by one
    for i in range(n - 1, 0, -1):
        arr[i], arr[0] = arr[0], arr[i]  # Swap
        heapify(arr, i, 0)
    
    return arr

def heapify(arr, n, i):
    largest = i
    left = 2 * i + 1
    right = 2 * i + 2
    
    if left < n and arr[left] > arr[largest]:
        largest = left
    
    if right < n and arr[right] > arr[largest]:
        largest = right
    
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify(arr, n, largest)

# Example usage
arr = [12, 11, 13, 5, 6, 7]
sorted_arr = heap_sort(arr)
print(sorted_arr)  # Output: [5, 6, 7, 11, 12, 13]
```

**JavaScript:**
```javascript
function heapSort(arr) {
    const n = arr.length;
    
    // Build a max heap
    for (let i = Math.floor(n / 2) - 1; i >= 0; i--) {
        heapify(arr, n, i);
    }
    
    // Extract elements one by one
    for (let i = n - 1; i > 0; i--) {
        [arr[0], arr[i]] = [arr[i], arr[0]];  // Swap
        heapify(arr, i, 0);
    }
    
    return arr;
}

function heapify(arr, n, i) {
    let largest = i;
    const left = 2 * i + 1;
    const right = 2 * i + 2;
    
    if (left < n && arr[left] > arr[largest]) {
        largest = left;
    }
    
    if (right < n && arr[right] > arr[largest]) {
        largest = right;
    }
    
    if (largest !== i) {
        [arr[i], arr[largest]] = [arr[largest], arr[i]];
        heapify(arr, n, largest);
    }
}

// Example usage
const arr = [12, 11, 13, 5, 6, 7];
const sortedArr = heapSort(arr);
console.log(sortedArr);  // Output: [5, 6, 7, 11, 12, 13]
```

### 2. Find the Kth Largest Element

**Python:**
```python
import heapq

def find_kth_largest(nums, k):
    # Using a min heap of size k
    min_heap = []
    
    for num in nums:
        if len(min_heap) < k:
            heapq.heappush(min_heap, num)
        elif num > min_heap[0]:
            heapq.heappop(min_heap)
            heapq.heappush(min_heap, num)
    
    return min_heap[0]

# Example usage
nums = [3, 2, 1, 5, 6, 4]
k = 2
print(find_kth_largest(nums, k))  # Output: 5
```

**JavaScript:**
```javascript
function findKthLargest(nums, k) {
    // Using a min heap of size k
    const minHeap = new MinHeap();
    
    for (const num of nums) {
        if (minHeap.size() < k) {
            minHeap.insert(num);
        } else if (num > minHeap.getMin()) {
            minHeap.extractMin();
            minHeap.insert(num);
        }
    }
    
    return minHeap.getMin();
}

// Example usage
const nums = [3, 2, 1, 5, 6, 4];
const k = 2;
console.log(findKthLargest(nums, k));  // Output: 5
```

### 3. Merge K Sorted Lists

**Python:**
```python
import heapq

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def merge_k_lists(lists):
    # Create a min heap
    min_heap = []
    
    # Add the first element of each list to the heap
    for i, lst in enumerate(lists):
        if lst:
            heapq.heappush(min_heap, (lst.val, i, lst))
    
    dummy = ListNode(0)
    current = dummy
    
    # Process the heap
    while min_heap:
        val, i, node = heapq.heappop(min_heap)
        
        # Add the node to the result list
        current.next = node
        current = current.next
        
        # Add the next node from the same list to the heap
        if node.next:
            heapq.heappush(min_heap, (node.next.val, i, node.next))
    
    return dummy.next

# Example usage
# Create three sorted linked lists
list1 = ListNode(1, ListNode(4, ListNode(5)))
list2 = ListNode(1, ListNode(3, ListNode(4)))
list3 = ListNode(2, ListNode(6))

merged = merge_k_lists([list1, list2, list3])

# Print the merged list
result = []
while merged:
    result.append(merged.val)
    merged = merged.next
print(result)  # Output: [1, 1, 2, 3, 4, 4, 5, 6]
```

**JavaScript:**
```javascript
class ListNode {
    constructor(val = 0, next = null) {
        this.val = val;
        this.next = next;
    }
}

function mergeKLists(lists) {
    // Create a min heap
    const minHeap = new MinHeap();
    
    // Add the first element of each list to the heap
    for (let i = 0; i < lists.length; i++) {
        if (lists[i]) {
            minHeap.insert({ val: lists[i].val, i, node: lists[i] });
        }
    }
    
    const dummy = new ListNode(0);
    let current = dummy;
    
    // Process the heap
    while (!minHeap.isEmpty()) {
        const { val, i, node } = minHeap.extractMin();
        
        // Add the node to the result list
        current.next = node;
        current = current.next;
        
        // Add the next node from the same list to the heap
        if (node.next) {
            minHeap.insert({ val: node.next.val, i, node: node.next });
        }
    }
    
    return dummy.next;
}

// Example usage
// Create three sorted linked lists
const list1 = new ListNode(1, new ListNode(4, new ListNode(5)));
const list2 = new ListNode(1, new ListNode(3, new ListNode(4)));
const list3 = new ListNode(2, new ListNode(6));

const merged = mergeKLists([list1, list2, list3]);

// Print the merged list
const result = [];
let current = merged;
while (current) {
    result.push(current.val);
    current = current.next;
}
console.log(result);  // Output: [1, 1, 2, 3, 4, 4, 5, 6]
```

## Time and Space Complexity

| Operation | Time Complexity |
|-----------|----------------|
| Insert    | O(log n)       |
| Extract Min/Max | O(log n) |
| Peek      | O(1)           |
| Heapify   | O(n)           |
| Heap Sort | O(n log n)     |

Space Complexity: O(n) for storing the heap

## Advantages of Heaps

1. **Efficient Priority Queue**: Heaps are ideal for implementing priority queues
2. **Fast Access to Min/Max**: O(1) access to the minimum or maximum element
3. **Efficient Sorting**: Heap sort is an efficient in-place sorting algorithm
4. **Efficient Insertion/Deletion**: O(log n) time for insertions and deletions

## Disadvantages of Heaps

1. **No Random Access**: Cannot efficiently access elements other than the root
2. **Not Suitable for Searching**: O(n) time complexity for searching
3. **Complex Implementation**: More complex to implement than simple arrays or linked lists
4. **Not Cache-Friendly**: Poor locality of reference can lead to cache misses

## When to Use Heaps

- When you need to efficiently find and remove the minimum or maximum element
- When implementing a priority queue
- When you need to perform heap sort
- When you need to find the k largest or smallest elements
- When you need to merge k sorted lists
- When implementing graph algorithms like Dijkstra's or Prim's

## Common Heap Problems from Blind 75 and Grind 75

1. **Kth Largest Element in an Array** (Medium): Find the kth largest element in an unsorted array.
2. **Merge k Sorted Lists** (Hard): Merge k sorted linked lists into one sorted linked list.
3. **Find Median from Data Stream** (Hard): Design a data structure that supports adding integers and finding the median.
4. **Top K Frequent Elements** (Medium): Find the k most frequent elements in an array.
5. **Task Scheduler** (Medium): Schedule tasks with cooldown periods to minimize the total time.
6. **Ugly Number II** (Medium): Find the nth ugly number (prime factors only 2, 3, 5).
7. **K Closest Points to Origin** (Medium): Find the k closest points to the origin.

## How to Identify Heap Problems

Look for these clues in the problem statement:

1. The problem involves finding the k largest or smallest elements.
2. The problem requires maintaining a priority queue.
3. The problem involves merging multiple sorted lists or arrays.
4. The problem requires efficient access to the minimum or maximum element.
5. The problem involves scheduling or resource allocation.
6. Keywords like "priority," "k largest," "k smallest," "median," or "merge sorted." 