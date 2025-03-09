# Find Minimum in Rotated Sorted Array

## Problem Statement

Suppose an array of length `n` sorted in ascending order is rotated between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:
- `[4,5,6,7,0,1,2]` if it was rotated 4 times.
- `[0,1,2,4,5,6,7]` if it was rotated 7 times.

Notice that rotating an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums` of unique elements, return the minimum element of this array.

You must write an algorithm that runs in O(log n) time.

**Example 1:**
```
Input: nums = [3,4,5,1,2]
Output: 1
Explanation: The original array was [1,2,3,4,5] rotated 3 times.
```

**Example 2:**
```
Input: nums = [4,5,6,7,0,1,2]
Output: 0
Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.
```

**Example 3:**
```
Input: nums = [11,13,15,17]
Output: 11
Explanation: The original array was [11,13,15,17] and it was rotated 4 times (identical to not being rotated).
```

## Intuition

The key insight for this problem is that a rotated sorted array can be divided into two sorted subarrays. The minimum element is the pivot point where the second subarray begins.

Since we need an O(log n) solution, binary search is the natural approach. However, we need to modify the standard binary search to find the pivot point rather than a specific value.

In a rotated sorted array, if we compare the middle element with the rightmost element:
1. If the middle element is greater than the rightmost element, the minimum must be in the right half.
2. If the middle element is less than the rightmost element, the minimum must be in the left half (including the middle element).

## Visual Explanation

Let's visualize the solution with Example 2: `nums = [4,5,6,7,0,1,2]`

```
Initial state:
nums = [4,5,6,7,0,1,2]
       ^     ^     ^
      left  mid  right
      (0)   (3)   (6)

Compare nums[mid] = 7 with nums[right] = 2:
7 > 2, so the minimum is in the right half.

Update left = mid + 1:
nums = [4,5,6,7,0,1,2]
               ^   ^
              left right
              (4)  (6)
              mid
              (5)

Compare nums[mid] = 1 with nums[right] = 2:
1 < 2, so the minimum is in the left half (including mid).

Update right = mid:
nums = [4,5,6,7,0,1,2]
               ^ ^
              left right
              (4)  (5)
               mid
               (4)

Compare nums[mid] = 0 with nums[right] = 1:
0 < 1, so the minimum is in the left half (including mid).

Update right = mid:
nums = [4,5,6,7,0,1,2]
               ^
            left,right,mid
               (4)

Since left == right, we've found the minimum: nums[left] = 0.
```

![Find Minimum in Rotated Sorted Array Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize two pointers, `left` and `right`, pointing to the start and end of the array.
2. While `left < right`:
   - Calculate the middle index as `mid = left + (right - left) // 2`.
   - Compare the middle element with the rightmost element:
     - If `nums[mid] > nums[right]`, the minimum is in the right half, so update `left = mid + 1`.
     - If `nums[mid] <= nums[right]`, the minimum is in the left half (including the middle element), so update `right = mid`.
3. When the loop terminates, `left` will be pointing to the minimum element.
4. Return `nums[left]`.

## Code Implementation

### Python

```python
def findMin(nums):
    left, right = 0, len(nums) - 1
    
    while left < right:
        mid = left + (right - left) // 2
        
        # If the middle element is greater than the rightmost element,
        # the minimum is in the right half
        if nums[mid] > nums[right]:
            left = mid + 1
        # If the middle element is less than or equal to the rightmost element,
        # the minimum is in the left half (including the middle element)
        else:
            right = mid
    
    # When left == right, we've found the minimum
    return nums[left]
```

### JavaScript

```javascript
function findMin(nums) {
    let left = 0;
    let right = nums.length - 1;
    
    while (left < right) {
        const mid = Math.floor(left + (right - left) / 2);
        
        // If the middle element is greater than the rightmost element,
        // the minimum is in the right half
        if (nums[mid] > nums[right]) {
            left = mid + 1;
        }
        // If the middle element is less than or equal to the rightmost element,
        // the minimum is in the left half (including the middle element)
        else {
            right = mid;
        }
    }
    
    // When left == right, we've found the minimum
    return nums[left];
}
```

## Time and Space Complexity

- **Time Complexity**: O(log n), where n is the length of the array. We're using binary search, which divides the search space in half at each step.
- **Space Complexity**: O(1), as we only use a constant amount of extra space regardless of the input size.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 1: `nums = [3,4,5,1,2]`

```
Initial state:
nums = [3,4,5,1,2]
       ^   ^   ^
      left mid right
      (0)  (2)  (4)

Compare nums[mid] = 5 with nums[right] = 2:
5 > 2, so the minimum is in the right half.

Update left = mid + 1:
nums = [3,4,5,1,2]
             ^ ^
            left right
            (3)  (4)
             mid
             (3)

Compare nums[mid] = 1 with nums[right] = 2:
1 < 2, so the minimum is in the left half (including mid).

Update right = mid:
nums = [3,4,5,1,2]
             ^
          left,right,mid
             (3)

Since left == right, we've found the minimum: nums[left] = 1.
```

## Edge Cases and Optimizations

- **Single Element**: If the array has only one element, that element is the minimum.
- **Not Rotated**: If the array is not rotated (or rotated n times), the first element is the minimum.
- **Fully Rotated**: If the array is rotated n-1 times, the last element is the minimum.

## Common Mistakes

1. **Using the wrong comparison**: A common mistake is to compare the middle element with the leftmost element instead of the rightmost element, which can lead to incorrect results.
2. **Not handling the case where the middle element equals the rightmost element**: In this problem, we're guaranteed that all elements are unique, so this case won't occur. However, in a more general version of the problem where duplicates are allowed, this case would need special handling.
3. **Incorrectly updating the pointers**: Make sure to update the pointers correctly based on the comparison result. If `nums[mid] > nums[right]`, the minimum is in the right half, so `left = mid + 1`. If `nums[mid] <= nums[right]`, the minimum is in the left half (including the middle element), so `right = mid`.

## Related Problems

1. **Search in Rotated Sorted Array**: Find a target value in a rotated sorted array.
2. **Find Minimum in Rotated Sorted Array II**: Find the minimum element in a rotated sorted array that may contain duplicates.
3. **Rotation Count in Rotated Sorted Array**: Find the number of times a sorted array has been rotated.
4. **Find the Rotation Point**: Find the index where the rotation occurs in a rotated sorted array. 