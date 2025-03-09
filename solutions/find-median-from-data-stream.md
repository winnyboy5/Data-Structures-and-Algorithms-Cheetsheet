# Find Median from Data Stream

## Problem Statement

Design a data structure that supports the following two operations:

1. `addNum(num)`: Add an integer number to the data structure.
2. `findMedian()`: Find and return the median of all elements so far.

The median is the middle value in an ordered list of numbers. If the size of the list is even, the median is the average of the two middle values.

**Example:**

```
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.findMedian(); // return 1.0
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
medianFinder.addNum(3);    // arr = [1, 2, 3]
medianFinder.findMedian(); // return 2.0
```

## Intuition

The key insight for this problem is to maintain two heaps:
1. A max heap for the smaller half of the numbers
2. A min heap for the larger half of the numbers

By balancing these two heaps, we can efficiently find the median:
- If the total number of elements is odd, the median is the top element of the max heap (the largest element in the smaller half).
- If the total number of elements is even, the median is the average of the top elements of both heaps.

This approach allows us to find the median in O(1) time while adding numbers in O(log n) time.

## Visual Explanation

Let's visualize the solution with the example:

```
Initial state:
Max Heap (smaller half): []
Min Heap (larger half): []

Step 1: Add 1
- Add to max heap: [1]
- Balance heaps
Max Heap: [1]
Min Heap: []
Median = 1.0 (top of max heap)

Step 2: Add 2
- Add to min heap: [2]
- Balance heaps
Max Heap: [1]
Min Heap: [2]
Median = (1 + 2) / 2 = 1.5

Step 3: Add 3
- Add to max heap: [1, 3]
- But 3 > 2 (top of min heap), so we need to swap
- Add 3 to min heap: [2, 3]
- Add 2 to max heap: [2, 1]
- Balance heaps
Max Heap: [2, 1]
Min Heap: [3]
Median = 2.0 (top of max heap)
```

![Two Heap Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Create two heaps:
   - A max heap to store the smaller half of the numbers
   - A min heap to store the larger half of the numbers

2. For the `addNum(num)` operation:
   - If the max heap is empty or num ≤ the top of the max heap, add num to the max heap.
   - Otherwise, add num to the min heap.
   - Balance the heaps to ensure that the max heap has either the same number of elements as the min heap or one more element.

3. For the `findMedian()` operation:
   - If the max heap has more elements than the min heap, return the top element of the max heap.
   - Otherwise, return the average of the top elements of both heaps.

## Code Implementation

### Python

```python
import heapq

class MedianFinder:
    def __init__(self):
        # Max heap for the smaller half (negate values for max heap in Python)
        self.max_heap = []
        # Min heap for the larger half
        self.min_heap = []

    def addNum(self, num: int) -> None:
        # Add to the appropriate heap
        if not self.max_heap or num <= -self.max_heap[0]:
            heapq.heappush(self.max_heap, -num)  # Negate for max heap
        else:
            heapq.heappush(self.min_heap, num)
        
        # Balance the heaps
        if len(self.max_heap) > len(self.min_heap) + 1:
            # Move the largest element from max heap to min heap
            heapq.heappush(self.min_heap, -heapq.heappop(self.max_heap))
        elif len(self.max_heap) < len(self.min_heap):
            # Move the smallest element from min heap to max heap
            heapq.heappush(self.max_heap, -heapq.heappop(self.min_heap))

    def findMedian(self) -> float:
        if len(self.max_heap) > len(self.min_heap):
            # Odd number of elements, median is the top of max heap
            return -self.max_heap[0]
        else:
            # Even number of elements, median is the average of the tops
            return (-self.max_heap[0] + self.min_heap[0]) / 2
```

### JavaScript

```javascript
class MaxHeap {
    constructor() {
        this.heap = [];
    }
    
    size() {
        return this.heap.length;
    }
    
    peek() {
        return this.heap[0] || 0;
    }
    
    push(val) {
        this.heap.push(val);
        this._siftUp(this.heap.length - 1);
    }
    
    pop() {
        if (this.heap.length === 0) return 0;
        
        const max = this.heap[0];
        const last = this.heap.pop();
        
        if (this.heap.length > 0) {
            this.heap[0] = last;
            this._siftDown(0);
        }
        
        return max;
    }
    
    _siftUp(index) {
        const parent = Math.floor((index - 1) / 2);
        
        if (parent >= 0 && this.heap[parent] < this.heap[index]) {
            [this.heap[parent], this.heap[index]] = [this.heap[index], this.heap[parent]];
            this._siftUp(parent);
        }
    }
    
    _siftDown(index) {
        const left = 2 * index + 1;
        const right = 2 * index + 2;
        let largest = index;
        
        if (left < this.heap.length && this.heap[left] > this.heap[largest]) {
            largest = left;
        }
        
        if (right < this.heap.length && this.heap[right] > this.heap[largest]) {
            largest = right;
        }
        
        if (largest !== index) {
            [this.heap[index], this.heap[largest]] = [this.heap[largest], this.heap[index]];
            this._siftDown(largest);
        }
    }
}

class MinHeap {
    constructor() {
        this.heap = [];
    }
    
    size() {
        return this.heap.length;
    }
    
    peek() {
        return this.heap[0] || 0;
    }
    
    push(val) {
        this.heap.push(val);
        this._siftUp(this.heap.length - 1);
    }
    
    pop() {
        if (this.heap.length === 0) return 0;
        
        const min = this.heap[0];
        const last = this.heap.pop();
        
        if (this.heap.length > 0) {
            this.heap[0] = last;
            this._siftDown(0);
        }
        
        return min;
    }
    
    _siftUp(index) {
        const parent = Math.floor((index - 1) / 2);
        
        if (parent >= 0 && this.heap[parent] > this.heap[index]) {
            [this.heap[parent], this.heap[index]] = [this.heap[index], this.heap[parent]];
            this._siftUp(parent);
        }
    }
    
    _siftDown(index) {
        const left = 2 * index + 1;
        const right = 2 * index + 2;
        let smallest = index;
        
        if (left < this.heap.length && this.heap[left] < this.heap[smallest]) {
            smallest = left;
        }
        
        if (right < this.heap.length && this.heap[right] < this.heap[smallest]) {
            smallest = right;
        }
        
        if (smallest !== index) {
            [this.heap[index], this.heap[smallest]] = [this.heap[smallest], this.heap[index]];
            this._siftDown(smallest);
        }
    }
}

class MedianFinder {
    constructor() {
        // Max heap for the smaller half
        this.maxHeap = new MaxHeap();
        // Min heap for the larger half
        this.minHeap = new MinHeap();
    }
    
    addNum(num) {
        // Add to the appropriate heap
        if (this.maxHeap.size() === 0 || num <= this.maxHeap.peek()) {
            this.maxHeap.push(num);
        } else {
            this.minHeap.push(num);
        }
        
        // Balance the heaps
        if (this.maxHeap.size() > this.minHeap.size() + 1) {
            // Move the largest element from max heap to min heap
            this.minHeap.push(this.maxHeap.pop());
        } else if (this.maxHeap.size() < this.minHeap.size()) {
            // Move the smallest element from min heap to max heap
            this.maxHeap.push(this.minHeap.pop());
        }
    }
    
    findMedian() {
        if (this.maxHeap.size() > this.minHeap.size()) {
            // Odd number of elements, median is the top of max heap
            return this.maxHeap.peek();
        } else {
            // Even number of elements, median is the average of the tops
            return (this.maxHeap.peek() + this.minHeap.peek()) / 2;
        }
    }
}
```

## Alternative Approach: Using a Sorted Array

Another approach is to maintain a sorted array and insert each new number in its correct position. This makes finding the median easy, but insertion becomes expensive.

### Python (Sorted Array)

```python
import bisect

class MedianFinder:
    def __init__(self):
        self.nums = []

    def addNum(self, num: int) -> None:
        # Insert num in the correct position to maintain sorted order
        bisect.insort(self.nums, num)

    def findMedian(self) -> float:
        n = len(self.nums)
        if n % 2 == 1:
            # Odd number of elements
            return self.nums[n // 2]
        else:
            # Even number of elements
            return (self.nums[n // 2 - 1] + self.nums[n // 2]) / 2
```

## Time and Space Complexity

### Two Heaps Approach:
- **Time Complexity**:
  - `addNum`: O(log n) for heap operations
  - `findMedian`: O(1) to access the top elements of the heaps
- **Space Complexity**: O(n) to store all elements in the heaps

### Sorted Array Approach:
- **Time Complexity**:
  - `addNum`: O(n) for insertion in a sorted array
  - `findMedian`: O(1) to access the middle elements
- **Space Complexity**: O(n) to store all elements in the array

## Detailed Walkthrough with Example

Let's trace through the two heaps approach with a more detailed example:

```
Initial state:
Max Heap: []
Min Heap: []

Add 5:
- Add to max heap: [5]
- Balance heaps
Max Heap: [5]
Min Heap: []
Median = 5

Add 10:
- 10 > 5, so add to min heap: [10]
- Balance heaps
Max Heap: [5]
Min Heap: [10]
Median = (5 + 10) / 2 = 7.5

Add 2:
- 2 < 5, so add to max heap: [5, 2]
- Heapify: [5, 2] -> [5, 2] (already a valid max heap)
- Balance heaps
Max Heap: [5, 2]
Min Heap: [10]
Median = 5

Add 8:
- 8 > 5, so add to min heap: [10, 8]
- Heapify: [10, 8] -> [8, 10]
- Balance heaps
Max Heap: [5, 2]
Min Heap: [8, 10]
Median = (5 + 8) / 2 = 6.5

Add 3:
- 3 < 5, so add to max heap: [5, 2, 3]
- Heapify: [5, 2, 3] -> [5, 2, 3] (already a valid max heap)
- Max heap has 3 elements, min heap has 2 elements
- Need to balance: move 5 from max heap to min heap
- Max Heap: [3, 2]
- Min Heap: [5, 8, 10]
- Balance heaps
Max Heap: [3, 2]
Min Heap: [5, 8, 10]
Median = (3 + 5) / 2 = 4
```

## Edge Cases and Optimizations

- **Empty Data Structure**: If no numbers have been added, the median is undefined. The implementation should handle this case.
- **Single Element**: If only one number has been added, that number is the median.
- **Duplicate Elements**: The solution handles duplicate elements correctly.
- **Large Number of Elements**: The two heaps approach is efficient even for a large number of elements.

**Optimization**: Instead of rebalancing after every insertion, we could rebalance only when necessary (when the size difference between the heaps exceeds 1).

## Common Mistakes

1. **Not balancing the heaps properly**: Ensure that the max heap has either the same number of elements as the min heap or one more element.
2. **Forgetting to negate values for max heap in Python**: Python's heapq module implements a min heap, so we need to negate values to simulate a max heap.
3. **Incorrect median calculation**: Be careful with the median calculation, especially when the number of elements is even.
4. **Not handling empty heaps**: Check if the heaps are empty before accessing their top elements.

## Related Problems

1. **Sliding Window Median**: Find the median of all subarrays of size k in an array.
2. **Kth Largest Element in a Stream**: Design a class to find the kth largest element in a stream of numbers.
3. **Moving Average from Data Stream**: Calculate the moving average of a stream of integers.
4. **Data Stream as Disjoint Intervals**: Design a data structure that efficiently tracks a set of integers as disjoint intervals. 