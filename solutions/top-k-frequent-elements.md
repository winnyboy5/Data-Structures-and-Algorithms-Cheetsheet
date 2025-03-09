# Top K Frequent Elements

## Problem Statement

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.

**Example 1:**
```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

**Example 2:**
```
Input: nums = [1], k = 1
Output: [1]
```

**Constraints:**
- 1 <= nums.length <= 10^5
- -10^4 <= nums[i] <= 10^4
- k is in the range [1, the number of unique elements in the array]
- It is guaranteed that the answer is unique

**Follow up:** Your algorithm's time complexity must be better than O(n log n), where n is the array's size.

## Intuition

The key insight for this problem is to count the frequency of each element in the array and then find the k elements with the highest frequencies. There are several approaches to solve this problem:

1. **Hash Map + Heap**: Count frequencies using a hash map, then use a min-heap of size k to keep track of the k most frequent elements.
2. **Hash Map + Bucket Sort**: Count frequencies using a hash map, then use bucket sort to find the k most frequent elements.
3. **Hash Map + Quickselect**: Count frequencies using a hash map, then use quickselect to find the k most frequent elements.

The heap approach is intuitive and efficient, while bucket sort can achieve linear time complexity in the best case.

## Visual Explanation

Let's visualize the solution using the heap approach with Example 1: `nums = [1,1,1,2,2,3], k = 2`

```
Step 1: Count the frequency of each element using a hash map.
        nums = [1,1,1,2,2,3]
        frequency map = {1: 3, 2: 2, 3: 1}

Step 2: Create a min-heap of size k to track the k most frequent elements.
        Initially, the heap is empty.

Step 3: Process each element in the frequency map:
        - Add (1, 3) to the heap: heap = [(1, 3)]
        - Add (2, 2) to the heap: heap = [(2, 2), (1, 3)]
        - Add (3, 1) to the heap: heap = [(3, 1), (2, 2), (1, 3)]
          Since the heap size exceeds k (2), remove the minimum: heap = [(2, 2), (1, 3)]

Step 4: Extract the elements from the heap to get the result.
        Result = [1, 2]
```

Now let's visualize the bucket sort approach:

```
Step 1: Count the frequency of each element using a hash map.
        nums = [1,1,1,2,2,3]
        frequency map = {1: 3, 2: 2, 3: 1}

Step 2: Create buckets where the index represents the frequency.
        buckets = [[], [3], [2], [1], [], [], ...]
                     ^    ^    ^
                     |    |    |
                     |    |    elements with frequency 3 (i.e., 1)
                     |    elements with frequency 2 (i.e., 2)
                     elements with frequency 1 (i.e., 3)

Step 3: Iterate through the buckets from highest frequency to lowest,
        collecting elements until we have k elements.
        Start from index n = 6 (array length): buckets[6] is empty
        buckets[5] is empty
        buckets[4] is empty
        buckets[3] = [1] -> collect 1, count = 1
        buckets[2] = [2] -> collect 2, count = 2 (reached k)
        Stop as we've collected k elements.

Step 4: Return the collected elements.
        Result = [1, 2]
```

![Top K Frequent Elements Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

### Approach 1: Hash Map + Heap

1. Create a hash map to count the frequency of each element in the array.
2. Create a min-heap of size k to keep track of the k most frequent elements.
3. Iterate through the frequency map:
   - Add each element and its frequency to the heap.
   - If the heap size exceeds k, remove the element with the minimum frequency.
4. Extract the elements from the heap to get the result.

### Approach 2: Hash Map + Bucket Sort

1. Create a hash map to count the frequency of each element in the array.
2. Create an array of buckets where the index represents the frequency.
3. Place each element in its corresponding bucket based on its frequency.
4. Iterate through the buckets from highest frequency to lowest, collecting elements until we have k elements.
5. Return the collected elements.

## Code Implementation

### Python (Heap Approach)

```python
import heapq
from collections import Counter

def topKFrequent(nums, k):
    # Count the frequency of each element
    count = Counter(nums)
    
    # Create a min-heap of size k
    heap = []
    
    # Process each element in the frequency map
    for num, freq in count.items():
        # Add the element and its frequency to the heap
        heapq.heappush(heap, (freq, num))
        
        # If the heap size exceeds k, remove the element with the minimum frequency
        if len(heap) > k:
            heapq.heappop(heap)
    
    # Extract the elements from the heap
    result = [num for freq, num in heap]
    
    return result
```

### Python (Bucket Sort Approach)

```python
from collections import Counter

def topKFrequent(nums, k):
    # Count the frequency of each element
    count = Counter(nums)
    
    # Create buckets where the index represents the frequency
    buckets = [[] for _ in range(len(nums) + 1)]
    
    # Place each element in its corresponding bucket
    for num, freq in count.items():
        buckets[freq].append(num)
    
    # Collect the k most frequent elements
    result = []
    for i in range(len(buckets) - 1, 0, -1):
        result.extend(buckets[i])
        if len(result) >= k:
            return result[:k]
    
    return result
```

### JavaScript (Heap Approach)

```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
function topKFrequent(nums, k) {
    // Count the frequency of each element
    const count = new Map();
    for (const num of nums) {
        count.set(num, (count.get(num) || 0) + 1);
    }
    
    // Create a min-heap of size k
    const heap = [];
    
    // Helper functions for the min-heap
    function parent(i) { return Math.floor((i - 1) / 2); }
    function left(i) { return 2 * i + 1; }
    function right(i) { return 2 * i + 2; }
    
    function swap(i, j) {
        [heap[i], heap[j]] = [heap[j], heap[i]];
    }
    
    function siftDown(i) {
        const n = heap.length;
        let smallest = i;
        const l = left(i);
        const r = right(i);
        
        if (l < n && heap[l][0] < heap[smallest][0]) {
            smallest = l;
        }
        
        if (r < n && heap[r][0] < heap[smallest][0]) {
            smallest = r;
        }
        
        if (smallest !== i) {
            swap(i, smallest);
            siftDown(smallest);
        }
    }
    
    function siftUp(i) {
        while (i > 0 && heap[parent(i)][0] > heap[i][0]) {
            swap(i, parent(i));
            i = parent(i);
        }
    }
    
    function push(val) {
        heap.push(val);
        siftUp(heap.length - 1);
    }
    
    function pop() {
        const result = heap[0];
        heap[0] = heap[heap.length - 1];
        heap.pop();
        if (heap.length > 0) {
            siftDown(0);
        }
        return result;
    }
    
    // Process each element in the frequency map
    for (const [num, freq] of count.entries()) {
        push([freq, num]);
        
        // If the heap size exceeds k, remove the element with the minimum frequency
        if (heap.length > k) {
            pop();
        }
    }
    
    // Extract the elements from the heap
    const result = [];
    while (heap.length > 0) {
        result.push(pop()[1]);
    }
    
    return result;
}
```

### JavaScript (Bucket Sort Approach)

```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
function topKFrequent(nums, k) {
    // Count the frequency of each element
    const count = new Map();
    for (const num of nums) {
        count.set(num, (count.get(num) || 0) + 1);
    }
    
    // Create buckets where the index represents the frequency
    const buckets = Array.from({ length: nums.length + 1 }, () => []);
    
    // Place each element in its corresponding bucket
    for (const [num, freq] of count.entries()) {
        buckets[freq].push(num);
    }
    
    // Collect the k most frequent elements
    const result = [];
    for (let i = buckets.length - 1; i >= 0; i--) {
        for (const num of buckets[i]) {
            result.push(num);
            if (result.length === k) {
                return result;
            }
        }
    }
    
    return result;
}
```

## Time and Space Complexity

### Heap Approach:
- **Time Complexity**: O(n + k log n), where n is the number of elements in the array. We need O(n) time to build the frequency map and O(n log k) time to process the heap operations.
- **Space Complexity**: O(n), where n is the number of elements in the array. We need O(n) space for the frequency map and O(k) space for the heap.

### Bucket Sort Approach:
- **Time Complexity**: O(n), where n is the number of elements in the array. We need O(n) time to build the frequency map, O(n) time to place elements in buckets, and O(n) time to collect the result.
- **Space Complexity**: O(n), where n is the number of elements in the array. We need O(n) space for the frequency map and O(n) space for the buckets.

## Detailed Walkthrough with Example

Let's trace through the bucket sort approach with Example 1: `nums = [1,1,1,2,2,3], k = 2`

```
Step 1: Count the frequency of each element.
        nums = [1,1,1,2,2,3]
        count = {1: 3, 2: 2, 3: 1}

Step 2: Create buckets where the index represents the frequency.
        buckets = [[], [3], [2], [1], [], [], []]
                     ^    ^    ^
                     |    |    |
                     |    |    elements with frequency 3 (i.e., 1)
                     |    elements with frequency 2 (i.e., 2)
                     elements with frequency 1 (i.e., 3)

Step 3: Iterate through the buckets from highest frequency to lowest,
        collecting elements until we have k elements.
        i = 6: buckets[6] is empty
        i = 5: buckets[5] is empty
        i = 4: buckets[4] is empty
        i = 3: buckets[3] = [1] -> result = [1], count = 1
        i = 2: buckets[2] = [2] -> result = [1, 2], count = 2 (reached k)
        Stop as we've collected k elements.

Step 4: Return the collected elements.
        Result = [1, 2]
```

## Edge Cases and Optimizations

- **Single Element**: If the array has only one element, return that element.
- **All Elements Have the Same Frequency**: In this case, any k elements can be returned.
- **k Equals the Number of Unique Elements**: Return all unique elements.

**Optimization**: For the heap approach, we can optimize by only adding elements to the heap if the frequency is greater than the minimum frequency in the heap when the heap is full. This can reduce the number of heap operations.

## Common Mistakes

1. **Not handling the case where k equals the number of unique elements**: Make sure to handle this case correctly.
2. **Using a max-heap instead of a min-heap**: A min-heap of size k is more efficient for finding the k largest elements.
3. **Not considering the follow-up requirement**: The follow-up requires a time complexity better than O(n log n), which rules out sorting the entire frequency map.

## Related Problems

1. **Kth Largest Element in an Array**: Find the kth largest element in an unsorted array.
2. **Sort Characters By Frequency**: Sort the characters in a string by decreasing frequency.
3. **K Closest Points to Origin**: Find the k closest points to the origin in a 2D plane.
4. **Top K Frequent Words**: Find the k most frequent words in a list of words.
5. **Find K Pairs with Smallest Sums**: Find k pairs with the smallest sums from two arrays. 