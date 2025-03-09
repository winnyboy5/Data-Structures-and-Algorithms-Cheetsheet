# Searching Algorithms

Searching algorithms are designed to retrieve information stored within some data structure. These algorithms are fundamental to computer science and are used in various applications.

## Table of Contents
- [Linear Search](#linear-search)
- [Binary Search](#binary-search)
- [Jump Search](#jump-search)
- [Interpolation Search](#interpolation-search)
- [Exponential Search](#exponential-search)
- [Ternary Search](#ternary-search)
- [Related Blind 75 & Grind 75 Problems](#related-blind-75--grind-75-problems)
- [Tips and Tricks](#tips-and-tricks)

## Linear Search

### Visual Explanation
```
Array: [5, 8, 2, 10, 3, 9]
Target: 10

Step 1: Check if 5 == 10? No, move to next element.
Step 2: Check if 8 == 10? No, move to next element.
Step 3: Check if 2 == 10? No, move to next element.
Step 4: Check if 10 == 10? Yes, return index 3.
```

### Implementation

#### Python
```python
def linear_search(arr, target):
    for i in range(len(arr)):
        if arr[i] == target:
            return i  # Return the index of the found element
    return -1  # Return -1 if the element is not found
```

#### JavaScript
```javascript
function linearSearch(arr, target) {
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] === target) {
            return i;  // Return the index of the found element
        }
    }
    return -1;  // Return -1 if the element is not found
}
```

### Time & Space Complexity
- **Time Complexity**: O(n) - In the worst case, we need to scan the entire array
- **Space Complexity**: O(1) - No extra space is needed

## Binary Search

### Visual Explanation
```
Sorted Array: [2, 3, 5, 8, 9, 10]
Target: 9

Step 1: Calculate mid = (0 + 5) / 2 = 2
        arr[mid] = 5, 5 < 9, so search in right half
        
Step 2: Calculate mid = (3 + 5) / 2 = 4
        arr[mid] = 9, 9 == 9, return index 4
```

### Implementation

#### Python
```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = left + (right - left) // 2  # Avoid potential overflow
        
        # Check if target is present at mid
        if arr[mid] == target:
            return mid
        
        # If target is greater, ignore left half
        elif arr[mid] < target:
            left = mid + 1
        
        # If target is smaller, ignore right half
        else:
            right = mid - 1
    
    # Element is not present in array
    return -1

# Recursive implementation
def binary_search_recursive(arr, target, left=0, right=None):
    if right is None:
        right = len(arr) - 1
    
    if left > right:
        return -1
    
    mid = left + (right - left) // 2
    
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        return binary_search_recursive(arr, target, mid + 1, right)
    else:
        return binary_search_recursive(arr, target, left, mid - 1)
```

#### JavaScript
```javascript
function binarySearch(arr, target) {
    let left = 0;
    let right = arr.length - 1;
    
    while (left <= right) {
        const mid = left + Math.floor((right - left) / 2);  // Avoid potential overflow
        
        // Check if target is present at mid
        if (arr[mid] === target) {
            return mid;
        }
        
        // If target is greater, ignore left half
        if (arr[mid] < target) {
            left = mid + 1;
        }
        // If target is smaller, ignore right half
        else {
            right = mid - 1;
        }
    }
    
    // Element is not present in array
    return -1;
}

// Recursive implementation
function binarySearchRecursive(arr, target, left = 0, right = arr.length - 1) {
    if (left > right) {
        return -1;
    }
    
    const mid = left + Math.floor((right - left) / 2);
    
    if (arr[mid] === target) {
        return mid;
    } else if (arr[mid] < target) {
        return binarySearchRecursive(arr, target, mid + 1, right);
    } else {
        return binarySearchRecursive(arr, target, left, mid - 1);
    }
}
```

### Time & Space Complexity
- **Time Complexity**: O(log n) - The search space is halved in each step
- **Space Complexity**: O(1) for iterative, O(log n) for recursive due to call stack

## Jump Search

### Visual Explanation
```
Sorted Array: [2, 3, 5, 8, 9, 10, 12, 15, 18, 20]
Target: 15
Block size (jump): √10 ≈ 3

Step 1: Check arr[0] = 2, 2 < 15, jump to index 3
Step 2: Check arr[3] = 8, 8 < 15, jump to index 6
Step 3: Check arr[6] = 12, 12 < 15, jump to index 9
Step 4: Check arr[9] = 20, 20 > 15, perform linear search from index 6 to 9
Step 5: Linear search finds 15 at index 7
```

### Implementation

#### Python
```python
import math

def jump_search(arr, target):
    n = len(arr)
    
    # Finding block size to be jumped
    step = int(math.sqrt(n))
    
    # Finding the block where element is present (if it is present)
    prev = 0
    while arr[min(step, n) - 1] < target:
        prev = step
        step += int(math.sqrt(n))
        if prev >= n:
            return -1
    
    # Doing a linear search for target in block beginning with prev
    while arr[prev] < target:
        prev += 1
        
        # If we reached next block or end of array, element is not present
        if prev == min(step, n):
            return -1
    
    # If element is found
    if arr[prev] == target:
        return prev
    
    return -1
```

#### JavaScript
```javascript
function jumpSearch(arr, target) {
    const n = arr.length;
    
    // Finding block size to be jumped
    const step = Math.floor(Math.sqrt(n));
    
    // Finding the block where element is present (if it is present)
    let prev = 0;
    while (arr[Math.min(step, n) - 1] < target) {
        prev = step;
        step += Math.floor(Math.sqrt(n));
        if (prev >= n) {
            return -1;
        }
    }
    
    // Doing a linear search for target in block beginning with prev
    while (arr[prev] < target) {
        prev++;
        
        // If we reached next block or end of array, element is not present
        if (prev === Math.min(step, n)) {
            return -1;
        }
    }
    
    // If element is found
    if (arr[prev] === target) {
        return prev;
    }
    
    return -1;
}
```

### Time & Space Complexity
- **Time Complexity**: O(√n) - We jump √n steps and then perform at most √n steps of linear search
- **Space Complexity**: O(1) - No extra space is needed

## Interpolation Search

### Visual Explanation
```
Sorted Array: [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]
Target: 10

Step 1: Calculate pos using the formula:
        pos = low + ((target - arr[low]) * (high - low)) / (arr[high] - arr[low])
        pos = 0 + ((10 - 2) * (9 - 0)) / (20 - 2)
        pos = 0 + (8 * 9) / 18
        pos = 0 + 4 = 4
        
        arr[pos] = 10, 10 == 10, return index 4
```

### Implementation

#### Python
```python
def interpolation_search(arr, target):
    low = 0
    high = len(arr) - 1
    
    while low <= high and target >= arr[low] and target <= arr[high]:
        if low == high:
            if arr[low] == target:
                return low
            return -1
        
        # Calculate the position using the formula
        pos = low + int(((float(high - low) / (arr[high] - arr[low])) * (target - arr[low])))
        
        if arr[pos] == target:
            return pos
        
        if arr[pos] < target:
            low = pos + 1
        else:
            high = pos - 1
    
    return -1
```

#### JavaScript
```javascript
function interpolationSearch(arr, target) {
    let low = 0;
    let high = arr.length - 1;
    
    while (low <= high && target >= arr[low] && target <= arr[high]) {
        if (low === high) {
            if (arr[low] === target) {
                return low;
            }
            return -1;
        }
        
        // Calculate the position using the formula
        const pos = low + Math.floor(((target - arr[low]) * (high - low)) / (arr[high] - arr[low]));
        
        if (arr[pos] === target) {
            return pos;
        }
        
        if (arr[pos] < target) {
            low = pos + 1;
        } else {
            high = pos - 1;
        }
    }
    
    return -1;
}
```

### Time & Space Complexity
- **Time Complexity**: O(log log n) for uniformly distributed data, O(n) in worst case
- **Space Complexity**: O(1) - No extra space is needed

## Exponential Search

### Visual Explanation
```
Sorted Array: [2, 3, 5, 8, 9, 10, 12, 15, 18, 20]
Target: 15

Step 1: Start with i = 1, check if arr[0] == 15? No
Step 2: Double i to 2, check if arr[1] == 15? No
Step 3: Double i to 4, check if arr[3] == 15? No
Step 4: Double i to 8, check if arr[7] == 15? Yes, found at index 7
```

### Implementation

#### Python
```python
def exponential_search(arr, target):
    n = len(arr)
    
    # If target is the first element
    if arr[0] == target:
        return 0
    
    # Find range for binary search by repeated doubling
    i = 1
    while i < n and arr[i] <= target:
        i = i * 2
    
    # Call binary search for the found range
    return binary_search(arr, target, i // 2, min(i, n - 1))

def binary_search(arr, target, left, right):
    while left <= right:
        mid = left + (right - left) // 2
        
        if arr[mid] == target:
            return mid
        
        if arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1
```

#### JavaScript
```javascript
function exponentialSearch(arr, target) {
    const n = arr.length;
    
    // If target is the first element
    if (arr[0] === target) {
        return 0;
    }
    
    // Find range for binary search by repeated doubling
    let i = 1;
    while (i < n && arr[i] <= target) {
        i *= 2;
    }
    
    // Call binary search for the found range
    return binarySearch(arr, target, Math.floor(i / 2), Math.min(i, n - 1));
}

function binarySearch(arr, target, left, right) {
    while (left <= right) {
        const mid = left + Math.floor((right - left) / 2);
        
        if (arr[mid] === target) {
            return mid;
        }
        
        if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return -1;
}
```

### Time & Space Complexity
- **Time Complexity**: O(log n) - The binary search is performed on at most 2*i elements
- **Space Complexity**: O(1) - No extra space is needed

## Ternary Search

### Visual Explanation
```
Sorted Array: [2, 3, 5, 8, 9, 10, 12, 15, 18, 20]
Target: 12

Step 1: Calculate mid1 = left + (right - left) / 3 = 0 + (9 - 0) / 3 = 3
        Calculate mid2 = right - (right - left) / 3 = 9 - (9 - 0) / 3 = 6
        
        arr[mid1] = 8, arr[mid2] = 12
        
        Since target == arr[mid2], return index 6
```

### Implementation

#### Python
```python
def ternary_search(arr, target):
    left, right = 0, len(arr) - 1
    
    while left <= right:
        # Find the mid1 and mid2
        mid1 = left + (right - left) // 3
        mid2 = right - (right - left) // 3
        
        # Check if target is present at any mid
        if arr[mid1] == target:
            return mid1
        
        if arr[mid2] == target:
            return mid2
        
        # Since target is not present at mid, check in which region it is present
        if target < arr[mid1]:
            # The target lies in between left and mid1
            right = mid1 - 1
        elif target > arr[mid2]:
            # The target lies in between mid2 and right
            left = mid2 + 1
        else:
            # The target lies in between mid1 and mid2
            left = mid1 + 1
            right = mid2 - 1
    
    # Element is not present in array
    return -1
```

#### JavaScript
```javascript
function ternarySearch(arr, target) {
    let left = 0;
    let right = arr.length - 1;
    
    while (left <= right) {
        // Find the mid1 and mid2
        const mid1 = left + Math.floor((right - left) / 3);
        const mid2 = right - Math.floor((right - left) / 3);
        
        // Check if target is present at any mid
        if (arr[mid1] === target) {
            return mid1;
        }
        
        if (arr[mid2] === target) {
            return mid2;
        }
        
        // Since target is not present at mid, check in which region it is present
        if (target < arr[mid1]) {
            // The target lies in between left and mid1
            right = mid1 - 1;
        } else if (target > arr[mid2]) {
            // The target lies in between mid2 and right
            left = mid2 + 1;
        } else {
            // The target lies in between mid1 and mid2
            left = mid1 + 1;
            right = mid2 - 1;
        }
    }
    
    // Element is not present in array
    return -1;
}
```

### Time & Space Complexity
- **Time Complexity**: O(log₃ n) - The search space is divided into three parts in each step
- **Space Complexity**: O(1) for iterative, O(log₃ n) for recursive due to call stack

## Related Blind 75 & Grind 75 Problems

1. **Binary Search** (Blind 75 #34)
   - Problem: Implement binary search to find a target value in a sorted array.
   - Solution: Use the binary search algorithm as shown above.
   - [LeetCode #704](https://leetcode.com/problems/binary-search/)

2. **Search in Rotated Sorted Array** (Blind 75 #33)
   - Problem: Search for a target value in a rotated sorted array.
   - Solution: Modified binary search that handles the rotation.
   - [LeetCode #33](https://leetcode.com/problems/search-in-rotated-sorted-array/)

3. **Find Minimum in Rotated Sorted Array** (Blind 75 #35)
   - Problem: Find the minimum element in a rotated sorted array.
   - Solution: Modified binary search to find the pivot point.
   - [LeetCode #153](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

4. **Search a 2D Matrix** (Grind 75)
   - Problem: Search for a target value in a sorted 2D matrix.
   - Solution: Treat the 2D matrix as a 1D sorted array and apply binary search.
   - [LeetCode #74](https://leetcode.com/problems/search-a-2d-matrix/)

5. **First Bad Version** (Grind 75)
   - Problem: Find the first bad version in a sequence of versions.
   - Solution: Use binary search to find the first occurrence.
   - [LeetCode #278](https://leetcode.com/problems/first-bad-version/)

6. **Median of Two Sorted Arrays** (Blind 75 #4)
   - Problem: Find the median of two sorted arrays.
   - Solution: Use a modified binary search approach.
   - [LeetCode #4](https://leetcode.com/problems/median-of-two-sorted-arrays/)

## Tips and Tricks

1. **Choosing the Right Searching Algorithm**:
   - For unsorted data: Linear Search
   - For sorted data: Binary Search
   - For sorted data with uniform distribution: Interpolation Search
   - For unbounded/infinite arrays: Exponential Search

2. **Binary Search Variations**:
   - Finding the first occurrence of an element
   - Finding the last occurrence of an element
   - Finding the floor of an element (largest element smaller than or equal to target)
   - Finding the ceiling of an element (smallest element greater than or equal to target)
   - Finding the peak element in a bitonic array

3. **Common Pitfalls**:
   - Off-by-one errors (incorrect boundary conditions)
   - Integer overflow when calculating mid = (left + right) / 2 (use mid = left + (right - left) / 2 instead)
   - Not handling duplicates properly
   - Applying binary search on unsorted data

4. **Interview Tips**:
   - Always clarify if the array is sorted before applying binary search
   - Consider edge cases: empty array, single element, target not in array
   - Be careful with boundary conditions (left <= right vs. left < right)
   - Practice implementing binary search from memory
   - Know how to modify binary search for different problems (e.g., rotated arrays, finding first/last occurrence) 