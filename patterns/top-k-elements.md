# Top K Elements Pattern

The Top K Elements pattern is used to find the k largest or smallest elements in a collection. This pattern leverages data structures like heaps (priority queues) to efficiently solve problems.

## Table of Contents
- [Understanding the Pattern](#understanding-the-pattern)
- [Using Heaps for Top K Elements](#using-heaps-for-top-k-elements)
- [Common Problem Variations](#common-problem-variations)
- [Implementation Techniques](#implementation-techniques)
- [Related Blind 75 & Grind 75 Problems](#related-blind-75--grind-75-problems)
- [Tips and Tricks](#tips-and-tricks)

## Understanding the Pattern

The Top K Elements pattern is about finding the k largest or smallest elements in a given collection. This pattern is useful when you need to find a subset of elements with the highest or lowest values according to some criteria.

### Key Concepts
1. **Heap Data Structure**: A specialized tree-based data structure that satisfies the heap property.
2. **Min Heap**: The key at the root is the minimum among all keys in the heap.
3. **Max Heap**: The key at the root is the maximum among all keys in the heap.
4. **Priority Queue**: An abstract data type that operates similar to a queue but each element has a "priority" associated with it.

### Visual Explanation

Finding the top 3 largest elements in [3, 1, 5, 12, 2, 11]:

```
Step 1: Create a min heap of size k (3)
        Insert first k elements: [1, 3, 5]
        
        Min Heap:
            1
           / \
          3   5

Step 2: For each remaining element (12, 2, 11):
        - If element > heap top, remove top and insert element
        - Otherwise, skip element
        
        Process 12:
        12 > 1, so remove 1 and insert 12
        Min Heap:
            3
           / \
          12  5
        
        Process 2:
        2 < 3, so skip
        
        Process 11:
        11 > 3, so remove 3 and insert 11
        Min Heap:
            5
           / \
          12  11

Step 3: The elements in the heap [5, 12, 11] are the top 3 largest elements
```

## Using Heaps for Top K Elements

### Finding K Largest Elements

To find the k largest elements, we use a min heap of size k. We maintain the k largest elements seen so far in the heap.

#### Implementation

##### Python
```python
import heapq

def find_k_largest(nums, k):
    # Create a min heap
    min_heap = []
    
    # Add first k elements to the heap
    for i in range(k):
        heapq.heappush(min_heap, nums[i])
    
    # For remaining elements, if element > heap top, replace top
    for i in range(k, len(nums)):
        if nums[i] > min_heap[0]:
            heapq.heappop(min_heap)
            heapq.heappush(min_heap, nums[i])
    
    # Return the k largest elements
    return min_heap
```

##### JavaScript
```javascript
class MinHeap {
    constructor() {
        this.heap = [];
    }
    
    push(val) {
        this.heap.push(val);
        this.bubbleUp(this.heap.length - 1);
    }
    
    pop() {
        const min = this.heap[0];
        const last = this.heap.pop();
        
        if (this.heap.length > 0) {
            this.heap[0] = last;
            this.bubbleDown(0);
        }
        
        return min;
    }
    
    peek() {
        return this.heap[0];
    }
    
    size() {
        return this.heap.length;
    }
    
    bubbleUp(index) {
        const parent = Math.floor((index - 1) / 2);
        
        if (parent >= 0 && this.heap[parent] > this.heap[index]) {
            [this.heap[parent], this.heap[index]] = [this.heap[index], this.heap[parent]];
            this.bubbleUp(parent);
        }
    }
    
    bubbleDown(index) {
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
            this.bubbleDown(smallest);
        }
    }
}

function findKLargest(nums, k) {
    // Create a min heap
    const minHeap = new MinHeap();
    
    // Add first k elements to the heap
    for (let i = 0; i < k; i++) {
        minHeap.push(nums[i]);
    }
    
    // For remaining elements, if element > heap top, replace top
    for (let i = k; i < nums.length; i++) {
        if (nums[i] > minHeap.peek()) {
            minHeap.pop();
            minHeap.push(nums[i]);
        }
    }
    
    // Return the k largest elements
    return minHeap.heap;
}
```

### Finding K Smallest Elements

To find the k smallest elements, we use a max heap of size k. We maintain the k smallest elements seen so far in the heap.

#### Implementation

##### Python
```python
import heapq

def find_k_smallest(nums, k):
    # Create a max heap (using negative values for min heap)
    max_heap = []
    
    # Add first k elements to the heap
    for i in range(k):
        heapq.heappush(max_heap, -nums[i])
    
    # For remaining elements, if element < heap top, replace top
    for i in range(k, len(nums)):
        if nums[i] < -max_heap[0]:
            heapq.heappop(max_heap)
            heapq.heappush(max_heap, -nums[i])
    
    # Return the k smallest elements
    return [-x for x in max_heap]
```

##### JavaScript
```javascript
class MaxHeap {
    constructor() {
        this.heap = [];
    }
    
    push(val) {
        this.heap.push(val);
        this.bubbleUp(this.heap.length - 1);
    }
    
    pop() {
        const max = this.heap[0];
        const last = this.heap.pop();
        
        if (this.heap.length > 0) {
            this.heap[0] = last;
            this.bubbleDown(0);
        }
        
        return max;
    }
    
    peek() {
        return this.heap[0];
    }
    
    size() {
        return this.heap.length;
    }
    
    bubbleUp(index) {
        const parent = Math.floor((index - 1) / 2);
        
        if (parent >= 0 && this.heap[parent] < this.heap[index]) {
            [this.heap[parent], this.heap[index]] = [this.heap[index], this.heap[parent]];
            this.bubbleUp(parent);
        }
    }
    
    bubbleDown(index) {
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
            this.bubbleDown(largest);
        }
    }
}

function findKSmallest(nums, k) {
    // Create a max heap
    const maxHeap = new MaxHeap();
    
    // Add first k elements to the heap
    for (let i = 0; i < k; i++) {
        maxHeap.push(nums[i]);
    }
    
    // For remaining elements, if element < heap top, replace top
    for (let i = k; i < nums.length; i++) {
        if (nums[i] < maxHeap.peek()) {
            maxHeap.pop();
            maxHeap.push(nums[i]);
        }
    }
    
    // Return the k smallest elements
    return maxHeap.heap;
}
```

## Common Problem Variations

### 1. Kth Largest Element

Find the kth largest element in an unsorted array.

#### Visual Explanation
```
Array: [3, 1, 5, 12, 2, 11]
k = 3

Using a min heap of size k:
1. Insert first k elements: [1, 3, 5]
2. Process 12: Remove 1, insert 12: [3, 5, 12]
3. Process 2: 2 < 3, skip
4. Process 11: Remove 3, insert 11: [5, 11, 12]

The kth largest element is the smallest in the heap: 5
```

#### Implementation

##### Python
```python
import heapq

def find_kth_largest(nums, k):
    # Create a min heap
    min_heap = []
    
    # Process first k elements
    for i in range(k):
        heapq.heappush(min_heap, nums[i])
    
    # Process remaining elements
    for i in range(k, len(nums)):
        if nums[i] > min_heap[0]:
            heapq.heappop(min_heap)
            heapq.heappush(min_heap, nums[i])
    
    # The kth largest element is at the top of the heap
    return min_heap[0]
```

### 2. Top K Frequent Elements

Find the k most frequent elements in an array.

#### Visual Explanation
```
Array: [1, 1, 1, 2, 2, 3]
k = 2

1. Count frequencies: {1: 3, 2: 2, 3: 1}
2. Use a min heap to track top k frequent elements based on frequency:
   - Insert (1, 3) and (2, 2): [(2, 2), (1, 3)]
   - Process (3, 1): (3, 1) < (2, 2), skip

The top k frequent elements are: [1, 2]
```

#### Implementation

##### Python
```python
import heapq
from collections import Counter

def top_k_frequent(nums, k):
    # Count frequencies
    count = Counter(nums)
    
    # Use a min heap to track top k frequent elements
    min_heap = []
    
    for num, freq in count.items():
        # If heap size < k, add element
        if len(min_heap) < k:
            heapq.heappush(min_heap, (freq, num))
        # If current frequency > min frequency in heap, replace
        elif freq > min_heap[0][0]:
            heapq.heappop(min_heap)
            heapq.heappush(min_heap, (freq, num))
    
    # Extract the elements (not frequencies)
    return [num for freq, num in min_heap]
```

### 3. Sort a Nearly Sorted Array

Sort an array where each element is at most k positions away from its sorted position.

#### Visual Explanation
```
Array: [6, 5, 3, 2, 8, 10, 9]
k = 3

Using a min heap of size k+1:
1. Insert first k+1 elements: [6, 5, 3, 2]
   Min heap: [2, 3, 5, 6]
2. Extract min (2) and insert next element (8):
   Result: [2]
   Min heap: [3, 6, 5, 8]
3. Extract min (3) and insert next element (10):
   Result: [2, 3]
   Min heap: [5, 6, 8, 10]
4. Extract min (5) and insert next element (9):
   Result: [2, 3, 5]
   Min heap: [6, 9, 8, 10]
5. Extract remaining elements:
   Result: [2, 3, 5, 6, 8, 9, 10]
```

#### Implementation

##### Python
```python
import heapq

def sort_k_sorted_array(arr, k):
    n = len(arr)
    result = []
    
    # Create a min heap with first k+1 elements
    min_heap = arr[:min(k+1, n)]
    heapq.heapify(min_heap)
    
    # Process remaining elements
    for i in range(k+1, n):
        # Add the minimum element to result
        result.append(heapq.heappop(min_heap))
        # Add the next element to the heap
        heapq.heappush(min_heap, arr[i])
    
    # Add remaining elements from the heap to result
    while min_heap:
        result.append(heapq.heappop(min_heap))
    
    return result
```

### 4. K Closest Points to Origin

Find the k closest points to the origin (0, 0) in a 2D plane.

#### Visual Explanation
```
Points: [(1, 3), (2, -1), (5, 2), (-2, 4)]
k = 2

1. Calculate distances: [(1, 3) -> 10, (2, -1) -> 5, (5, 2) -> 29, (-2, 4) -> 20]
2. Use a max heap to track k closest points:
   - Insert (1, 3) and (2, -1): [(1, 3, 10), (2, -1, 5)]
   - Process (5, 2): 29 > 10, skip
   - Process (-2, 4): 20 > 10, skip

The k closest points are: [(2, -1), (1, 3)]
```

#### Implementation

##### Python
```python
import heapq

def k_closest_points(points, k):
    # Create a max heap
    max_heap = []
    
    # Process first k points
    for i in range(k):
        x, y = points[i]
        dist = x*x + y*y  # Squared distance
        heapq.heappush(max_heap, (-dist, x, y))  # Negative for max heap
    
    # Process remaining points
    for i in range(k, len(points)):
        x, y = points[i]
        dist = x*x + y*y
        
        # If current distance < max distance in heap, replace
        if dist < -max_heap[0][0]:
            heapq.heappop(max_heap)
            heapq.heappush(max_heap, (-dist, x, y))
    
    # Return the k closest points
    return [(x, y) for _, x, y in max_heap]
```

## Implementation Techniques

### 1. Using Built-in Heap/Priority Queue

Most programming languages provide built-in heap implementations:

#### Python
```python
import heapq

# Min heap operations
min_heap = []
heapq.heappush(min_heap, 5)
min_value = heapq.heappop(min_heap)

# Max heap operations (using negative values)
max_heap = []
heapq.heappush(max_heap, -5)
max_value = -heapq.heappop(max_heap)

# Heapify an existing list
arr = [3, 1, 4, 1, 5]
heapq.heapify(arr)  # Converts to min heap in-place

# Get k smallest elements
k_smallest = heapq.nsmallest(k, arr)

# Get k largest elements
k_largest = heapq.nlargest(k, arr)
```

#### JavaScript (using custom implementation)
```javascript
class MinHeap {
    // Implementation as shown earlier
}

class MaxHeap {
    // Implementation as shown earlier
}
```

### 2. Quick Select Algorithm

An alternative to using heaps for finding the kth largest/smallest element is the Quick Select algorithm, which has an average time complexity of O(n).

#### Implementation

##### Python
```python
def quick_select(arr, k):
    """
    Find the kth smallest element in an unsorted array.
    """
    def partition(left, right, pivot_index):
        pivot = arr[pivot_index]
        # Move pivot to end
        arr[pivot_index], arr[right] = arr[right], arr[pivot_index]
        
        # Move all elements smaller than pivot to the left
        store_index = left
        for i in range(left, right):
            if arr[i] < pivot:
                arr[store_index], arr[i] = arr[i], arr[store_index]
                store_index += 1
        
        # Move pivot to its final place
        arr[store_index], arr[right] = arr[right], arr[store_index]
        
        return store_index
    
    def select(left, right, k_smallest):
        # If the list contains only one element, return that element
        if left == right:
            return arr[left]
        
        # Select a random pivot_index between left and right
        import random
        pivot_index = random.randint(left, right)
        
        # Find the pivot position in a sorted list
        pivot_index = partition(left, right, pivot_index)
        
        # If the pivot is in its final sorted position
        if k_smallest == pivot_index:
            return arr[k_smallest]
        # If there are more elements on the left side of the pivot,
        # then the kth smallest is on the left side
        elif k_smallest < pivot_index:
            return select(left, pivot_index - 1, k_smallest)
        # Otherwise, the kth smallest is on the right side
        else:
            return select(pivot_index + 1, right, k_smallest)
    
    return select(0, len(arr) - 1, k - 1)
```

## Related Blind 75 & Grind 75 Problems

1. **Kth Largest Element in an Array** (Blind 75 #215)
   - Problem: Find the kth largest element in an unsorted array.
   - Solution: Use a min heap of size k or Quick Select algorithm.
   - [LeetCode #215](https://leetcode.com/problems/kth-largest-element-in-an-array/)

2. **Top K Frequent Elements** (Blind 75 #347)
   - Problem: Find the k most frequent elements in an array.
   - Solution: Use a hash map to count frequencies and a min heap to track top k elements.
   - [LeetCode #347](https://leetcode.com/problems/top-k-frequent-elements/)

3. **Find K Closest Elements** (Grind 75)
   - Problem: Find k closest elements to a given value in a sorted array.
   - Solution: Use binary search to find the closest element, then expand outward.
   - [LeetCode #658](https://leetcode.com/problems/find-k-closest-elements/)

4. **K Closest Points to Origin** (Grind 75)
   - Problem: Find the k closest points to the origin in a 2D plane.
   - Solution: Use a max heap of size k to track the closest points.
   - [LeetCode #973](https://leetcode.com/problems/k-closest-points-to-origin/)

5. **Sort Characters By Frequency** (Grind 75)
   - Problem: Sort characters in a string by decreasing frequency.
   - Solution: Count frequencies and use a bucket sort or heap.
   - [LeetCode #451](https://leetcode.com/problems/sort-characters-by-frequency/)

6. **Merge K Sorted Lists** (Blind 75 #23)
   - Problem: Merge k sorted linked lists into one sorted list.
   - Solution: Use a min heap to track the smallest element across all lists.
   - [LeetCode #23](https://leetcode.com/problems/merge-k-sorted-lists/)

## Tips and Tricks

1. **Choosing the Right Heap**:
   - For k largest elements: Use a min heap of size k
   - For k smallest elements: Use a max heap of size k
   - For kth largest element: Use a min heap of size k
   - For kth smallest element: Use a max heap of size k

2. **Optimizing Space Complexity**:
   - If k is small compared to n, using a heap of size k is efficient (O(k) space)
   - If k is close to n, consider sorting the array (O(1) extra space with in-place sort)
   - For finding a single element (like kth largest), consider Quick Select (O(1) extra space)

3. **Time Complexity Analysis**:
   - Building a heap: O(n)
   - Inserting into a heap: O(log k)
   - Extracting from a heap: O(log k)
   - Overall time complexity for top k elements: O(n log k)

4. **Common Patterns**:
   - Count frequencies first for problems involving frequency
   - Calculate distances/differences for proximity problems
   - Use custom comparators for complex ordering requirements
   - Consider the "streaming" version of problems where data comes in real-time

5. **Alternative Approaches**:
   - Quick Select: O(n) average time for finding kth element
   - Bucket Sort: O(n) time for certain distributions
   - Binary Search: Useful for sorted arrays or when k is close to n
   - Two Pointers: Can be used for certain variations of the problem 