# K-way Merge Pattern

The K-way Merge pattern is used to efficiently merge K sorted arrays or lists into a single sorted list. This pattern leverages data structures like heaps (priority queues) to optimize the merging process.

## Table of Contents
- [Understanding the Pattern](#understanding-the-pattern)
- [Using Heaps for K-way Merge](#using-heaps-for-k-way-merge)
- [Common Problem Variations](#common-problem-variations)
- [Implementation Techniques](#implementation-techniques)
- [Related Blind 75 & Grind 75 Problems](#related-blind-75--grind-75-problems)
- [Tips and Tricks](#tips-and-tricks)

## Understanding the Pattern

The K-way Merge pattern is about combining multiple sorted arrays or lists into a single sorted result. Instead of merging arrays one by one (which would be inefficient), we use a min-heap to keep track of the smallest element across all arrays.

### Key Concepts
1. **Sorted Input**: All input arrays or lists must be sorted.
2. **Min Heap**: Used to efficiently find the next smallest element across all arrays.
3. **Pointers**: Track the current position in each array.
4. **Merge Process**: Repeatedly extract the minimum element and advance the corresponding pointer.

### Visual Explanation

Merging 3 sorted arrays: [2, 6, 8], [3, 6, 7], [1, 3, 4]

```
Step 1: Initialize a min heap with the first element from each array
        Min Heap: [(1, 0, 2), (2, 0, 0), (3, 0, 1)]
        Format: (value, array_index, element_index)
        
Step 2: Extract the minimum element (1) and add to result
        Result: [1]
        Advance pointer for array 2: [1, 3, 4] -> [3, 4]
        Add next element from array 2 to heap: (3, 1, 0)
        Min Heap: [(2, 0, 0), (3, 0, 1), (3, 1, 0)]
        
Step 3: Extract the minimum element (2) and add to result
        Result: [1, 2]
        Advance pointer for array 0: [2, 6, 8] -> [6, 8]
        Add next element from array 0 to heap: (6, 0, 0)
        Min Heap: [(3, 0, 1), (3, 1, 0), (6, 0, 0)]
        
Step 4: Extract the minimum element (3) and add to result
        Result: [1, 2, 3]
        Advance pointer for array 0: [3, 6, 7] -> [6, 7]
        Add next element from array 0 to heap: (6, 0, 1)
        Min Heap: [(3, 1, 0), (6, 0, 0), (6, 0, 1)]
        
Step 5: Extract the minimum element (3) and add to result
        Result: [1, 2, 3, 3]
        Advance pointer for array 2: [3, 4] -> [4]
        Add next element from array 2 to heap: (4, 1, 0)
        Min Heap: [(4, 1, 0), (6, 0, 0), (6, 0, 1)]
        
... continue until all elements are processed ...

Final Result: [1, 2, 3, 3, 4, 6, 6, 7, 8]
```

## Using Heaps for K-way Merge

### Basic K-way Merge Algorithm

#### Implementation

##### Python
```python
import heapq

def k_way_merge(arrays):
    # Initialize a min heap with the first element from each array
    min_heap = []
    result = []
    
    # Add the first element from each array to the min heap
    for i, arr in enumerate(arrays):
        if arr:  # Check if the array is not empty
            heapq.heappush(min_heap, (arr[0], i, 0))  # (value, array_index, element_index)
    
    # Process the min heap
    while min_heap:
        val, array_idx, element_idx = heapq.heappop(min_heap)
        result.append(val)
        
        # If there are more elements in the array, add the next one to the heap
        if element_idx + 1 < len(arrays[array_idx]):
            next_element = arrays[array_idx][element_idx + 1]
            heapq.heappush(min_heap, (next_element, array_idx, element_idx + 1))
    
    return result
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
    
    isEmpty() {
        return this.heap.length === 0;
    }
    
    bubbleUp(index) {
        const parent = Math.floor((index - 1) / 2);
        
        if (parent >= 0 && this.heap[parent][0] > this.heap[index][0]) {
            [this.heap[parent], this.heap[index]] = [this.heap[index], this.heap[parent]];
            this.bubbleUp(parent);
        }
    }
    
    bubbleDown(index) {
        const left = 2 * index + 1;
        const right = 2 * index + 2;
        let smallest = index;
        
        if (left < this.heap.length && this.heap[left][0] < this.heap[smallest][0]) {
            smallest = left;
        }
        
        if (right < this.heap.length && this.heap[right][0] < this.heap[smallest][0]) {
            smallest = right;
        }
        
        if (smallest !== index) {
            [this.heap[index], this.heap[smallest]] = [this.heap[smallest], this.heap[index]];
            this.bubbleDown(smallest);
        }
    }
}

function kWayMerge(arrays) {
    // Initialize a min heap with the first element from each array
    const minHeap = new MinHeap();
    const result = [];
    
    // Add the first element from each array to the min heap
    for (let i = 0; i < arrays.length; i++) {
        if (arrays[i].length > 0) {
            minHeap.push([arrays[i][0], i, 0]);  // [value, array_index, element_index]
        }
    }
    
    // Process the min heap
    while (!minHeap.isEmpty()) {
        const [val, arrayIdx, elementIdx] = minHeap.pop();
        result.push(val);
        
        // If there are more elements in the array, add the next one to the heap
        if (elementIdx + 1 < arrays[arrayIdx].length) {
            const nextElement = arrays[arrayIdx][elementIdx + 1];
            minHeap.push([nextElement, arrayIdx, elementIdx + 1]);
        }
    }
    
    return result;
}
```

### Time & Space Complexity
- **Time Complexity**: O(N log K) where N is the total number of elements across all arrays and K is the number of arrays
- **Space Complexity**: O(K) for the heap and O(N) for the result

## Common Problem Variations

### 1. Merge K Sorted Lists

Merge k sorted linked lists into one sorted linked list.

#### Visual Explanation
```
Input:
List 1: 1 -> 4 -> 5
List 2: 1 -> 3 -> 4
List 3: 2 -> 6

Step 1: Initialize min heap with the first node from each list
        Min Heap: [(1, 0), (1, 1), (2, 2)]
        Format: (value, list_index)
        
Step 2: Extract min (1) and add to result
        Result: 1 ->
        Advance pointer for list 0: 4 -> 5
        Add next node to heap: (4, 0)
        Min Heap: [(1, 1), (2, 2), (4, 0)]
        
Step 3: Extract min (1) and add to result
        Result: 1 -> 1 ->
        Advance pointer for list 1: 3 -> 4
        Add next node to heap: (3, 1)
        Min Heap: [(2, 2), (3, 1), (4, 0)]
        
... continue until all nodes are processed ...

Final Result: 1 -> 1 -> 2 -> 3 -> 4 -> 4 -> 5 -> 6
```

#### Implementation

##### Python
```python
import heapq

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def merge_k_lists(lists):
    # Custom wrapper to make ListNode comparable
    ListNodeWrapper = type('ListNodeWrapper', (object,), {
        '__lt__': lambda self, other: self.node.val < other.node.val
    })
    
    # Initialize the min heap
    min_heap = []
    
    # Add the first node from each list to the heap
    for i, lst in enumerate(lists):
        if lst:
            wrapper = ListNodeWrapper()
            wrapper.node = lst
            wrapper.index = i
            heapq.heappush(min_heap, wrapper)
    
    # Create a dummy head for the result list
    dummy = ListNode(0)
    current = dummy
    
    # Process the min heap
    while min_heap:
        # Get the node with the smallest value
        wrapper = heapq.heappop(min_heap)
        node = wrapper.node
        list_index = wrapper.index
        
        # Add the node to the result list
        current.next = node
        current = current.next
        
        # If there are more nodes in the list, add the next one to the heap
        if node.next:
            wrapper.node = node.next
            heapq.heappush(min_heap, wrapper)
    
    return dummy.next
```

### 2. Find K Pairs with Smallest Sums

Find k pairs (u, v) with the smallest sums from two arrays.

#### Visual Explanation
```
Input:
nums1 = [1, 7, 11]
nums2 = [2, 4, 6]
k = 3

Step 1: Initialize min heap with the first pair from each combination
        Min Heap: [(1+2, 0, 0)]
        Format: (sum, index1, index2)
        
Step 2: Extract min (3) and add to result
        Result: [(1, 2)]
        Add next pairs: (1+4, 0, 1) and (7+2, 1, 0)
        Min Heap: [(5, 0, 1), (9, 1, 0)]
        
Step 3: Extract min (5) and add to result
        Result: [(1, 2), (1, 4)]
        Add next pair: (1+6, 0, 2)
        Min Heap: [(7, 0, 2), (9, 1, 0)]
        
Step 4: Extract min (7) and add to result
        Result: [(1, 2), (1, 4), (1, 6)]
        
Final Result: [(1, 2), (1, 4), (1, 6)]
```

#### Implementation

##### Python
```python
import heapq

def k_smallest_pairs(nums1, nums2, k):
    if not nums1 or not nums2:
        return []
    
    # Initialize the min heap with the first pair from each combination
    min_heap = [(nums1[0] + nums2[0], 0, 0)]
    result = []
    visited = {(0, 0)}  # To avoid duplicates
    
    # Process the min heap
    while min_heap and len(result) < k:
        _, i, j = heapq.heappop(min_heap)
        result.append([nums1[i], nums2[j]])
        
        # Add the next pair in the first array
        if i + 1 < len(nums1) and (i + 1, j) not in visited:
            heapq.heappush(min_heap, (nums1[i + 1] + nums2[j], i + 1, j))
            visited.add((i + 1, j))
        
        # Add the next pair in the second array
        if j + 1 < len(nums2) and (i, j + 1) not in visited:
            heapq.heappush(min_heap, (nums1[i] + nums2[j + 1], i, j + 1))
            visited.add((i, j + 1))
    
    return result
```

### 3. Merge K Sorted Arrays

Merge k sorted arrays into one sorted array.

#### Implementation

##### Python
```python
import heapq

def merge_k_sorted_arrays(arrays):
    # This is the same as the basic k-way merge algorithm
    return k_way_merge(arrays)
```

### 4. Smallest Range Covering Elements from K Lists

Find the smallest range that includes at least one number from each of the k lists.

#### Visual Explanation
```
Input:
nums = [[4, 10, 15, 24], [0, 9, 12, 20], [5, 18, 22, 30]]

Step 1: Initialize min heap with the first element from each list
        Min Heap: [(0, 1, 0), (4, 0, 0), (5, 2, 0)]
        Current Range: [0, 5] (min=0, max=5)
        
Step 2: Extract min (0) and update range
        Advance pointer for list 1: [0, 9, 12, 20] -> [9, 12, 20]
        Add next element to heap: (9, 1, 1)
        Min Heap: [(4, 0, 0), (5, 2, 0), (9, 1, 1)]
        Current Range: [4, 9] (min=4, max=9)
        
Step 3: Extract min (4) and update range
        Advance pointer for list 0: [4, 10, 15, 24] -> [10, 15, 24]
        Add next element to heap: (10, 0, 1)
        Min Heap: [(5, 2, 0), (9, 1, 1), (10, 0, 1)]
        Current Range: [5, 10] (min=5, max=10)
        
... continue until one list is exhausted ...

Final Result: The smallest range is [20, 24]
```

#### Implementation

##### Python
```python
import heapq

def smallest_range(nums):
    # Initialize the min heap and track the current maximum value
    min_heap = []
    current_max = float('-inf')
    
    # Add the first element from each list to the heap
    for i, arr in enumerate(nums):
        if arr:  # Check if the array is not empty
            heapq.heappush(min_heap, (arr[0], i, 0))  # (value, list_index, element_index)
            current_max = max(current_max, arr[0])
    
    # Initialize the result range with a large value
    result = [float('-inf'), float('inf')]
    
    # Process the min heap
    while len(min_heap) == len(nums):  # Ensure we have elements from all lists
        val, list_idx, element_idx = heapq.heappop(min_heap)
        
        # Update the result if the current range is smaller
        if current_max - val < result[1] - result[0]:
            result = [val, current_max]
        
        # If there are more elements in the list, add the next one to the heap
        if element_idx + 1 < len(nums[list_idx]):
            next_element = nums[list_idx][element_idx + 1]
            heapq.heappush(min_heap, (next_element, list_idx, element_idx + 1))
            current_max = max(current_max, next_element)
    
    return result
```

## Implementation Techniques

### 1. Using Built-in Heap/Priority Queue

Most programming languages provide built-in heap implementations:

#### Python
```python
import heapq

# Min heap operations
min_heap = []
heapq.heappush(min_heap, (5, 'data'))  # Push tuple with value and additional data
min_value, data = heapq.heappop(min_heap)  # Pop the smallest element

# Heapify an existing list
arr = [(3, 'a'), (1, 'b'), (4, 'c')]
heapq.heapify(arr)  # Converts to min heap in-place
```

#### JavaScript (using custom implementation)
```javascript
class MinHeap {
    // Implementation as shown earlier
}
```

### 2. Divide and Conquer Approach

An alternative to using heaps is a divide and conquer approach, which recursively merges pairs of arrays.

#### Implementation

##### Python
```python
def merge_k_sorted_arrays_dc(arrays):
    if not arrays:
        return []
    
    # Base case: if there's only one array, return it
    if len(arrays) == 1:
        return arrays[0]
    
    # Divide the arrays into two halves
    mid = len(arrays) // 2
    left = merge_k_sorted_arrays_dc(arrays[:mid])
    right = merge_k_sorted_arrays_dc(arrays[mid:])
    
    # Merge the two halves
    return merge_two_sorted_arrays(left, right)

def merge_two_sorted_arrays(arr1, arr2):
    result = []
    i = j = 0
    
    # Merge the two arrays
    while i < len(arr1) and j < len(arr2):
        if arr1[i] <= arr2[j]:
            result.append(arr1[i])
            i += 1
        else:
            result.append(arr2[j])
            j += 1
    
    # Add remaining elements
    result.extend(arr1[i:])
    result.extend(arr2[j:])
    
    return result
```

### Time & Space Complexity
- **Time Complexity**: O(N log K) where N is the total number of elements and K is the number of arrays
- **Space Complexity**: O(N) for the result

## Related Blind 75 & Grind 75 Problems

1. **Merge K Sorted Lists** (Blind 75 #23)
   - Problem: Merge k sorted linked lists into one sorted linked list.
   - Solution: Use a min heap to track the smallest node across all lists.
   - [LeetCode #23](https://leetcode.com/problems/merge-k-sorted-lists/)

2. **Find K Pairs with Smallest Sums** (Grind 75)
   - Problem: Find k pairs with the smallest sums from two arrays.
   - Solution: Use a min heap to track pairs with the smallest sums.
   - [LeetCode #373](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)

3. **Kth Smallest Element in a Sorted Matrix** (Grind 75)
   - Problem: Find the kth smallest element in a sorted matrix.
   - Solution: Use a min heap to track the smallest elements.
   - [LeetCode #378](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)

4. **Smallest Range Covering Elements from K Lists** (Grind 75)
   - Problem: Find the smallest range that includes at least one number from each of the k lists.
   - Solution: Use a min heap to track the smallest element from each list.
   - [LeetCode #632](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/)

5. **Median of Two Sorted Arrays** (Blind 75 #4)
   - Problem: Find the median of two sorted arrays.
   - Solution: Use a modified binary search or merge approach.
   - [LeetCode #4](https://leetcode.com/problems/median-of-two-sorted-arrays/)

6. **Merge Sorted Array** (Grind 75)
   - Problem: Merge two sorted arrays into one sorted array.
   - Solution: Use a two-pointer approach to merge the arrays.
   - [LeetCode #88](https://leetcode.com/problems/merge-sorted-array/)

## Tips and Tricks

1. **Choosing the Right Approach**:
   - For a small number of arrays (K ≤ 2), use a simple merge approach
   - For a moderate number of arrays (2 < K < 10), use a divide and conquer approach
   - For a large number of arrays (K ≥ 10), use a heap-based approach

2. **Optimizing Space Complexity**:
   - If possible, merge in-place to reduce space complexity
   - For linked lists, reuse existing nodes instead of creating new ones
   - Consider using a fixed-size heap if K is large

3. **Time Complexity Analysis**:
   - Building a heap: O(K)
   - Inserting into a heap: O(log K)
   - Extracting from a heap: O(log K)
   - Overall time complexity: O(N log K) where N is the total number of elements

4. **Common Patterns**:
   - Use a tuple in the heap to store additional information (e.g., array index, element index)
   - Track the current maximum value when finding ranges
   - Use a visited set to avoid duplicates in certain problems
   - Consider early termination conditions to optimize performance

5. **Alternative Approaches**:
   - Divide and Conquer: O(N log K) time complexity
   - Sequential Merging: O(NK) time complexity (not recommended for large K)
   - Binary Search: Useful for certain variations (e.g., finding kth element)
   - Two Pointers: Efficient for merging two sorted arrays 