# Search in Rotated Sorted Array

## Problem Statement

There is an integer array `nums` sorted in ascending order (with distinct values).

Prior to being passed to your function, `nums` is possibly rotated at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (0-indexed). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index 3 and become `[4,5,6,7,0,1,2]`.

Given the array `nums` after the possible rotation and an integer `target`, return the index of `target` if it is in `nums`, or `-1` if it is not in `nums`.

You must write an algorithm with O(log n) runtime complexity.

**Example 1:**
```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2:**
```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

**Example 3:**
```
Input: nums = [1], target = 0
Output: -1
```

## Intuition

This problem is an extension of the "Find Minimum in Rotated Sorted Array" problem. The key insight is that a rotated sorted array can be divided into two sorted subarrays. We need to first determine which subarray the target is in, and then perform a binary search in that subarray.

Since we need an O(log n) solution, binary search is the natural approach. However, we need to modify the standard binary search to handle the rotation.

The main idea is to compare the middle element with the leftmost element to determine which subarray is sorted. Then, we check if the target is within the range of the sorted subarray. If it is, we search in that subarray; otherwise, we search in the other subarray.

## Visual Explanation

Let's visualize the solution with Example 1: `nums = [4,5,6,7,0,1,2], target = 0`

```
Initial state:
nums = [4,5,6,7,0,1,2], target = 0
        ^     ^     ^
       left  mid  right
       (0)   (3)   (6)

Compare nums[left] = 4 with nums[mid] = 7:
4 < 7, so the left half is sorted.

Check if target is in the sorted left half:
target = 0, range = [4, 7]
0 is not in the range [4, 7], so search in the right half.

Update left = mid + 1:
nums = [4,5,6,7,0,1,2], target = 0
                ^   ^
               left right
               (4)  (6)
               mid
               (5)

Compare nums[left] = 0 with nums[mid] = 1:
0 < 1, so the left half is sorted.

Check if target is in the sorted left half:
target = 0, range = [0, 1]
0 is in the range [0, 1], so search in the left half.

Update right = mid - 1:
nums = [4,5,6,7,0,1,2], target = 0
                ^
             left,right,mid
                (4)

Since left == right and nums[left] = target, we've found the target at index 4.
```

![Search in Rotated Sorted Array Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize two pointers, `left` and `right`, pointing to the start and end of the array.
2. While `left <= right`:
   - Calculate the middle index as `mid = left + (right - left) // 2`.
   - If `nums[mid] == target`, return `mid`.
   - Determine which half of the array is sorted:
     - If `nums[left] <= nums[mid]`, the left half is sorted.
       - If `target` is within the range of the sorted left half (`nums[left] <= target < nums[mid]`), search in the left half by updating `right = mid - 1`.
       - Otherwise, search in the right half by updating `left = mid + 1`.
     - If `nums[mid] <= nums[right]`, the right half is sorted.
       - If `target` is within the range of the sorted right half (`nums[mid] < target <= nums[right]`), search in the right half by updating `left = mid + 1`.
       - Otherwise, search in the left half by updating `right = mid - 1`.
3. If the loop terminates without finding the target, return `-1`.

## Code Implementation

### Python

```python
def search(nums, target):
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = left + (right - left) // 2
        
        # If we found the target, return its index
        if nums[mid] == target:
            return mid
        
        # Determine which half of the array is sorted
        if nums[left] <= nums[mid]:
            # Left half is sorted
            if nums[left] <= target < nums[mid]:
                # Target is in the sorted left half
                right = mid - 1
            else:
                # Target is in the right half
                left = mid + 1
        else:
            # Right half is sorted
            if nums[mid] < target <= nums[right]:
                # Target is in the sorted right half
                left = mid + 1
            else:
                # Target is in the left half
                right = mid - 1
    
    # Target not found
    return -1
```

### JavaScript

```javascript
function search(nums, target) {
    let left = 0;
    let right = nums.length - 1;
    
    while (left <= right) {
        const mid = Math.floor(left + (right - left) / 2);
        
        // If we found the target, return its index
        if (nums[mid] === target) {
            return mid;
        }
        
        // Determine which half of the array is sorted
        if (nums[left] <= nums[mid]) {
            // Left half is sorted
            if (nums[left] <= target && target < nums[mid]) {
                // Target is in the sorted left half
                right = mid - 1;
            } else {
                // Target is in the right half
                left = mid + 1;
            }
        } else {
            // Right half is sorted
            if (nums[mid] < target && target <= nums[right]) {
                // Target is in the sorted right half
                left = mid + 1;
            } else {
                // Target is in the left half
                right = mid - 1;
            }
        }
    }
    
    // Target not found
    return -1;
}
```

## Time and Space Complexity

- **Time Complexity**: O(log n), where n is the length of the array. We're using binary search, which divides the search space in half at each step.
- **Space Complexity**: O(1), as we only use a constant amount of extra space regardless of the input size.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2: `nums = [4,5,6,7,0,1,2], target = 3`

```
Initial state:
nums = [4,5,6,7,0,1,2], target = 3
        ^     ^     ^
       left  mid  right
       (0)   (3)   (6)

Compare nums[mid] = 7 with target = 3:
7 != 3, so we need to determine which half to search in.

Compare nums[left] = 4 with nums[mid] = 7:
4 < 7, so the left half is sorted.

Check if target is in the sorted left half:
target = 3, range = [4, 7)
3 is not in the range [4, 7), so search in the right half.

Update left = mid + 1:
nums = [4,5,6,7,0,1,2], target = 3
                ^   ^
               left right
               (4)  (6)
               mid
               (5)

Compare nums[mid] = 1 with target = 3:
1 != 3, so we need to determine which half to search in.

Compare nums[left] = 0 with nums[mid] = 1:
0 < 1, so the left half is sorted.

Check if target is in the sorted left half:
target = 3, range = [0, 1)
3 is not in the range [0, 1), so search in the right half.

Update left = mid + 1:
nums = [4,5,6,7,0,1,2], target = 3
                    ^
                 left,right,mid
                    (6)

Compare nums[mid] = 2 with target = 3:
2 != 3, so we need to determine which half to search in.

Since left == right, there's only one element left to check.
nums[mid] = 2 != target = 3, so the target is not found.

Return -1.
```

## Edge Cases and Optimizations

- **Single Element**: If the array has only one element, check if it's the target and return accordingly.
- **Not Rotated**: If the array is not rotated (or rotated n times), standard binary search will work.
- **Target Not in Array**: If the target is not in the array, the function will return -1.

## Common Mistakes

1. **Incorrect comparison for determining which half is sorted**: Make sure to use `nums[left] <= nums[mid]` to check if the left half is sorted, not just `nums[left] < nums[mid]`. This is important for handling cases where the array has duplicate elements.
2. **Incorrect range check for determining if the target is in the sorted half**: Be careful with the range checks. For the left half, use `nums[left] <= target < nums[mid]`. For the right half, use `nums[mid] < target <= nums[right]`.
3. **Not handling the case where the middle element is the target**: Always check if `nums[mid] == target` before determining which half to search in.

## Related Problems

1. **Find Minimum in Rotated Sorted Array**: Find the minimum element in a rotated sorted array.
2. **Search in Rotated Sorted Array II**: Search in a rotated sorted array that may contain duplicates.
3. **Find First and Last Position of Element in Sorted Array**: Find the starting and ending position of a given target value in a sorted array.
4. **Search Insert Position**: Find the index where a target would be inserted in a sorted array. 