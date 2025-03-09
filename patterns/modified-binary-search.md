# Modified Binary Search Pattern

The Modified Binary Search pattern is a variation of the traditional binary search algorithm that can be applied to more complex scenarios beyond just searching for an element in a sorted array. This pattern is particularly useful for problems where the search space can be divided into two parts, with one part containing the desired result.

## Visual Representation

```
Traditional Binary Search:
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
Search for 7:

Step 1: mid = (0 + 9) / 2 = 4, arr[4] = 5 < 7, so search in right half
[_, _, _, _, _, 6, 7, 8, 9, 10]

Step 2: mid = (5 + 9) / 2 = 7, arr[7] = 8 > 7, so search in left half
[_, _, _, _, _, 6, 7, _, _, _]

Step 3: mid = (5 + 6) / 2 = 5, arr[5] = 6 < 7, so search in right half
[_, _, _, _, _, _, 7, _, _, _]

Step 4: mid = (6 + 6) / 2 = 6, arr[6] = 7 == 7, found!
```

## When to Use Modified Binary Search

- When searching in a sorted array with a twist (e.g., rotated, bitonic)
- When finding a specific element that satisfies certain conditions
- When determining the boundary between two distinct regions in a sorted array
- When the problem can be reduced to finding a specific point in a monotonic function
- When the search space can be halved in each step based on some property

## Basic Modified Binary Search Implementation

### Python Implementation

```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = left + (right - left) // 2
        
        if arr[mid] == target:
            return mid
        
        if arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1  # Target not found

# Example usage
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
target = 7
print(binary_search(arr, target))  # Output: 6
```

### JavaScript Implementation

```javascript
function binarySearch(arr, target) {
    let left = 0;
    let right = arr.length - 1;
    
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
    
    return -1;  // Target not found
}

// Example usage
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const target = 7;
console.log(binarySearch(arr, target));  // Output: 6
```

## Common Problems and Solutions

### 1. Search in Rotated Sorted Array

**Problem:** Given a rotated sorted array, find the index of a target value. The array was originally sorted in ascending order but was rotated at some pivot point.

**Python Solution:**
```python
def search_in_rotated_sorted_array(nums, target):
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = left + (right - left) // 2
        
        if nums[mid] == target:
            return mid
        
        # Check if the left half is sorted
        if nums[left] <= nums[mid]:
            # Check if target is in the left half
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        # Right half is sorted
        else:
            # Check if target is in the right half
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
    
    return -1  # Target not found

# Example usage
nums = [4, 5, 6, 7, 0, 1, 2]
target = 0
print(search_in_rotated_sorted_array(nums, target))  # Output: 4
```

**JavaScript Solution:**
```javascript
function searchInRotatedSortedArray(nums, target) {
    let left = 0;
    let right = nums.length - 1;
    
    while (left <= right) {
        const mid = left + Math.floor((right - left) / 2);
        
        if (nums[mid] === target) {
            return mid;
        }
        
        // Check if the left half is sorted
        if (nums[left] <= nums[mid]) {
            // Check if target is in the left half
            if (nums[left] <= target && target < nums[mid]) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        // Right half is sorted
        else {
            // Check if target is in the right half
            if (nums[mid] < target && target <= nums[right]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
    }
    
    return -1;  // Target not found
}

// Example usage
const nums = [4, 5, 6, 7, 0, 1, 2];
const target = 0;
console.log(searchInRotatedSortedArray(nums, target));  // Output: 4
```

**Time Complexity:** O(log n)  
**Space Complexity:** O(1)

### 2. Find First and Last Position of Element in Sorted Array

**Problem:** Given a sorted array of integers, find the starting and ending position of a given target value.

**Python Solution:**
```python
def search_range(nums, target):
    def find_first_occurrence():
        left, right = 0, len(nums) - 1
        first = -1
        
        while left <= right:
            mid = left + (right - left) // 2
            
            if nums[mid] == target:
                first = mid
                right = mid - 1  # Continue searching in the left half
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        
        return first
    
    def find_last_occurrence():
        left, right = 0, len(nums) - 1
        last = -1
        
        while left <= right:
            mid = left + (right - left) // 2
            
            if nums[mid] == target:
                last = mid
                left = mid + 1  # Continue searching in the right half
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        
        return last
    
    first = find_first_occurrence()
    
    # If target is not found, return [-1, -1]
    if first == -1:
        return [-1, -1]
    
    last = find_last_occurrence()
    
    return [first, last]

# Example usage
nums = [5, 7, 7, 8, 8, 10]
target = 8
print(search_range(nums, target))  # Output: [3, 4]
```

**JavaScript Solution:**
```javascript
function searchRange(nums, target) {
    function findFirstOccurrence() {
        let left = 0;
        let right = nums.length - 1;
        let first = -1;
        
        while (left <= right) {
            const mid = left + Math.floor((right - left) / 2);
            
            if (nums[mid] === target) {
                first = mid;
                right = mid - 1;  // Continue searching in the left half
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return first;
    }
    
    function findLastOccurrence() {
        let left = 0;
        let right = nums.length - 1;
        let last = -1;
        
        while (left <= right) {
            const mid = left + Math.floor((right - left) / 2);
            
            if (nums[mid] === target) {
                last = mid;
                left = mid + 1;  // Continue searching in the right half
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return last;
    }
    
    const first = findFirstOccurrence();
    
    // If target is not found, return [-1, -1]
    if (first === -1) {
        return [-1, -1];
    }
    
    const last = findLastOccurrence();
    
    return [first, last];
}

// Example usage
const nums = [5, 7, 7, 8, 8, 10];
const target = 8;
console.log(searchRange(nums, target));  // Output: [3, 4]
```

**Time Complexity:** O(log n)  
**Space Complexity:** O(1)

### 3. Find Minimum in Rotated Sorted Array

**Problem:** Given a rotated sorted array, find the minimum element.

**Python Solution:**
```python
def find_min(nums):
    left, right = 0, len(nums) - 1
    
    # If the array is not rotated or has only one element
    if nums[left] < nums[right] or left == right:
        return nums[left]
    
    while left <= right:
        mid = left + (right - left) // 2
        
        # Check if mid is the minimum element
        if mid > 0 and nums[mid] < nums[mid - 1]:
            return nums[mid]
        
        # Check if mid+1 is the minimum element
        if mid < len(nums) - 1 and nums[mid] > nums[mid + 1]:
            return nums[mid + 1]
        
        # Decide which half to search
        if nums[mid] > nums[left]:
            # Minimum is in the right half
            left = mid + 1
        else:
            # Minimum is in the left half
            right = mid - 1
    
    return -1  # Should not reach here

# Example usage
nums = [4, 5, 6, 7, 0, 1, 2]
print(find_min(nums))  # Output: 0
```

**JavaScript Solution:**
```javascript
function findMin(nums) {
    let left = 0;
    let right = nums.length - 1;
    
    // If the array is not rotated or has only one element
    if (nums[left] < nums[right] || left === right) {
        return nums[left];
    }
    
    while (left <= right) {
        const mid = left + Math.floor((right - left) / 2);
        
        // Check if mid is the minimum element
        if (mid > 0 && nums[mid] < nums[mid - 1]) {
            return nums[mid];
        }
        
        // Check if mid+1 is the minimum element
        if (mid < nums.length - 1 && nums[mid] > nums[mid + 1]) {
            return nums[mid + 1];
        }
        
        // Decide which half to search
        if (nums[mid] > nums[left]) {
            // Minimum is in the right half
            left = mid + 1;
        } else {
            // Minimum is in the left half
            right = mid - 1;
        }
    }
    
    return -1;  // Should not reach here
}

// Example usage
const nums = [4, 5, 6, 7, 0, 1, 2];
console.log(findMin(nums));  // Output: 0
```

**Time Complexity:** O(log n)  
**Space Complexity:** O(1)

### 4. Search in a Sorted Array of Unknown Size

**Problem:** Given a sorted array of unknown size, find the index of a target value. The array is accessible through an API that returns the value at a given index, or -1 if the index is out of bounds.

**Python Solution:**
```python
# Assume we have an API ArrayReader.get(index) that returns the element at index
# or -1 if the index is out of bounds
class ArrayReader:
    def __init__(self, arr):
        self.arr = arr
    
    def get(self, index):
        if index < len(self.arr):
            return self.arr[index]
        return 2**31 - 1  # A large value indicating out of bounds

def search_in_unknown_sized_array(reader, target):
    # First, find the bounds of the array
    left, right = 0, 1
    
    # Double the right bound until we find a value greater than or equal to the target
    while reader.get(right) < target:
        left = right
        right *= 2
    
    # Perform binary search within the bounds
    while left <= right:
        mid = left + (right - left) // 2
        
        mid_value = reader.get(mid)
        
        if mid_value == target:
            return mid
        
        if mid_value < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1  # Target not found

# Example usage
arr = [1, 3, 5, 7, 9, 11, 13, 15, 17, 19]
reader = ArrayReader(arr)
target = 11
print(search_in_unknown_sized_array(reader, target))  # Output: 5
```

**JavaScript Solution:**
```javascript
// Assume we have an API ArrayReader.get(index) that returns the element at index
// or a large value if the index is out of bounds
class ArrayReader {
    constructor(arr) {
        this.arr = arr;
    }
    
    get(index) {
        if (index < this.arr.length) {
            return this.arr[index];
        }
        return 2**31 - 1;  // A large value indicating out of bounds
    }
}

function searchInUnknownSizedArray(reader, target) {
    // First, find the bounds of the array
    let left = 0;
    let right = 1;
    
    // Double the right bound until we find a value greater than or equal to the target
    while (reader.get(right) < target) {
        left = right;
        right *= 2;
    }
    
    // Perform binary search within the bounds
    while (left <= right) {
        const mid = left + Math.floor((right - left) / 2);
        
        const midValue = reader.get(mid);
        
        if (midValue === target) {
            return mid;
        }
        
        if (midValue < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return -1;  // Target not found
}

// Example usage
const arr = [1, 3, 5, 7, 9, 11, 13, 15, 17, 19];
const reader = new ArrayReader(arr);
const target = 11;
console.log(searchInUnknownSizedArray(reader, target));  // Output: 5
```

**Time Complexity:** O(log n)  
**Space Complexity:** O(1)

### 5. Find Peak Element

**Problem:** A peak element is an element that is strictly greater than its neighbors. Given an integer array, find a peak element, and return its index. If the array contains multiple peaks, return the index of any peak.

**Python Solution:**
```python
def find_peak_element(nums):
    left, right = 0, len(nums) - 1
    
    while left < right:
        mid = left + (right - left) // 2
        
        # If mid is a peak
        if mid > 0 and mid < len(nums) - 1 and nums[mid] > nums[mid - 1] and nums[mid] > nums[mid + 1]:
            return mid
        
        # If mid is on an upward slope, peak is to the right
        if nums[mid] < nums[mid + 1]:
            left = mid + 1
        # If mid is on a downward slope, peak is to the left
        else:
            right = mid
    
    # When left == right, we've found a peak
    return left

# Example usage
nums = [1, 2, 3, 1]
print(find_peak_element(nums))  # Output: 2
```

**JavaScript Solution:**
```javascript
function findPeakElement(nums) {
    let left = 0;
    let right = nums.length - 1;
    
    while (left < right) {
        const mid = left + Math.floor((right - left) / 2);
        
        // If mid is a peak
        if (mid > 0 && mid < nums.length - 1 && nums[mid] > nums[mid - 1] && nums[mid] > nums[mid + 1]) {
            return mid;
        }
        
        // If mid is on an upward slope, peak is to the right
        if (nums[mid] < nums[mid + 1]) {
            left = mid + 1;
        }
        // If mid is on a downward slope, peak is to the left
        else {
            right = mid;
        }
    }
    
    // When left == right, we've found a peak
    return left;
}

// Example usage
const nums = [1, 2, 3, 1];
console.log(findPeakElement(nums));  // Output: 2
```

**Time Complexity:** O(log n)  
**Space Complexity:** O(1)

## Time and Space Complexity

| Algorithm | Time Complexity | Space Complexity |
|-----------|----------------|-----------------|
| Binary Search | O(log n) | O(1) |
| Search in Rotated Sorted Array | O(log n) | O(1) |
| Find First and Last Position | O(log n) | O(1) |
| Find Minimum in Rotated Sorted Array | O(log n) | O(1) |
| Search in Unknown Sized Array | O(log n) | O(1) |
| Find Peak Element | O(log n) | O(1) |

## Tips and Tricks for Modified Binary Search

1. **Mid-Point Calculation**: Use `mid = left + (right - left) // 2` instead of `mid = (left + right) // 2` to avoid integer overflow.

2. **Boundary Conditions**: Be careful with the boundary conditions, especially when deciding whether to use `left <= right` or `left < right` as the loop condition.

3. **Handling Duplicates**: When dealing with arrays that may contain duplicates, consider how duplicates affect your search logic.

4. **Identifying the Sorted Half**: In rotated arrays, always identify which half is sorted before deciding where to search.

5. **Finding Boundaries**: For problems like finding the first or last occurrence, use separate binary searches with different conditions.

6. **Expanding Search Space**: For arrays of unknown size, start with a small range and double it until you find the bounds.

7. **Monotonicity**: Binary search works when there's a monotonic property that allows you to eliminate half of the search space.

## Common Pitfalls

1. **Off-by-One Errors**: Be careful with the indices, especially when updating `left` and `right`.

2. **Infinite Loops**: Ensure that your loop makes progress in each iteration to avoid infinite loops.

3. **Incorrect Mid Calculation**: Using `(left + right) / 2` can lead to integer overflow for large arrays.

4. **Wrong Boundary Conditions**: Using the wrong loop condition (`left <= right` vs. `left < right`) can lead to incorrect results.

5. **Not Handling Edge Cases**: Forgetting to handle edge cases like empty arrays or arrays with a single element.

## How to Identify Modified Binary Search Problems

Look for these clues in the problem statement:

1. The problem involves searching in a sorted or partially sorted array.
2. The problem requires finding a specific element or a boundary between two regions.
3. The problem mentions O(log n) time complexity as a requirement.
4. The problem involves finding a minimum, maximum, or peak element in a sorted or rotated array.
5. Keywords like "sorted," "rotated," "search," "find," or "binary search."

## Common Modified Binary Search Problems from Blind 75 and Grind 75

1. **Binary Search** (Easy): Search for a target value in a sorted array.
2. **Search in Rotated Sorted Array** (Medium): Find a target value in a rotated sorted array.
3. **Find First and Last Position of Element in Sorted Array** (Medium): Find the starting and ending position of a given target value.
4. **Find Minimum in Rotated Sorted Array** (Medium): Find the minimum element in a rotated sorted array.
5. **Search in a 2D Matrix** (Medium): Search for a target value in a sorted 2D matrix.
6. **Find Peak Element** (Medium): Find a peak element in an array.
7. **Search a 2D Matrix II** (Medium): Search for a target value in a sorted 2D matrix with different sorting rules.
8. **Median of Two Sorted Arrays** (Hard): Find the median of two sorted arrays.
9. **Kth Smallest Element in a Sorted Matrix** (Medium): Find the kth smallest element in a sorted matrix.
10. **Find K Closest Elements** (Medium): Find k closest elements to a given value in a sorted array.

## Modified Binary Search Template

Here's a general template for solving modified binary search problems:

```python
def modified_binary_search(arr, target):
    left, right = 0, len(arr) - 1
    
    while left <= right:  # Or left < right, depending on the problem
        mid = left + (right - left) // 2
        
        if found_target(arr, mid, target):
            return process_result(arr, mid)
        
        if should_search_left(arr, mid, target):
            right = mid - 1  # Or right = mid, depending on the problem
        else:
            left = mid + 1
    
    return handle_not_found()
```

## Real-World Applications

1. **Database Indexing**: Finding records in a database using binary search on indexed columns.
2. **IP Routing**: Finding the best matching prefix in routing tables.
3. **Version Control**: Finding the commit that introduced a bug using binary search (git bisect).
4. **Machine Learning**: Finding optimal hyperparameters using binary search.
5. **Image Processing**: Finding the threshold value for image segmentation.
6. **Computational Geometry**: Finding the closest pair of points in a sorted array.
7. **Game Development**: Finding the optimal difficulty level based on player performance. 