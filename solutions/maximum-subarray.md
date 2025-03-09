# Maximum Subarray

## Problem Statement

Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

A **subarray** is a contiguous part of an array.

**Example 1:**
```
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

**Example 2:**
```
Input: nums = [1]
Output: 1
```

**Example 3:**
```
Input: nums = [5,4,-1,7,8]
Output: 23
Explanation: [5,4,-1,7,8] has the largest sum = 23.
```

## Intuition

This problem is a classic example of Kadane's algorithm, which efficiently solves the maximum subarray problem in linear time. The key insight is that:

1. If the current subarray sum becomes negative, it's better to start a new subarray from the current element.
2. At each step, we have two choices:
   - Continue the current subarray by adding the current element
   - Start a new subarray from the current element

We keep track of the maximum subarray sum seen so far and update it whenever we find a better sum.

## Visual Explanation

Let's visualize Kadane's algorithm with Example 1: `nums = [-2,1,-3,4,-1,2,1,-5,4]`

```
Initialize:
- currentSum = 0 (sum of current subarray)
- maxSum = -infinity (maximum subarray sum seen so far)

Step 1: Element = -2
        currentSum = max(0 + (-2), 0) = 0
        maxSum = max(-infinity, 0) = 0

Step 2: Element = 1
        currentSum = max(0 + 1, 0) = 1
        maxSum = max(0, 1) = 1

Step 3: Element = -3
        currentSum = max(1 + (-3), 0) = 0
        maxSum = max(1, 0) = 1

Step 4: Element = 4
        currentSum = max(0 + 4, 0) = 4
        maxSum = max(1, 4) = 4

Step 5: Element = -1
        currentSum = max(4 + (-1), 0) = 3
        maxSum = max(4, 3) = 4

Step 6: Element = 2
        currentSum = max(3 + 2, 0) = 5
        maxSum = max(4, 5) = 5

Step 7: Element = 1
        currentSum = max(5 + 1, 0) = 6
        maxSum = max(5, 6) = 6

Step 8: Element = -5
        currentSum = max(6 + (-5), 0) = 1
        maxSum = max(6, 1) = 6

Step 9: Element = 4
        currentSum = max(1 + 4, 0) = 5
        maxSum = max(6, 5) = 6

Result: Maximum subarray sum = 6
```

![Maximum Subarray Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach (Kadane's Algorithm)

1. Initialize two variables:
   - `currentSum` to track the sum of the current subarray, initially set to 0.
   - `maxSum` to track the maximum subarray sum seen so far, initially set to the first element (to handle all negative arrays).
2. Iterate through the array starting from the first element:
   - Update `currentSum` by adding the current element.
   - If `currentSum` becomes negative, reset it to 0 (start a new subarray).
   - Update `maxSum` if `currentSum` is greater.
3. Return `maxSum`.

## Code Implementation

### Python

```python
def maxSubArray(nums):
    if not nums:
        return 0
    
    # Initialize maxSum to the first element to handle all negative arrays
    maxSum = nums[0]
    currentSum = 0
    
    for num in nums:
        # Add current element to the current subarray
        currentSum += num
        
        # Update maxSum if currentSum is greater
        maxSum = max(maxSum, currentSum)
        
        # If currentSum becomes negative, reset it to 0
        currentSum = max(currentSum, 0)
    
    return maxSum
```

### JavaScript

```javascript
function maxSubArray(nums) {
    if (!nums || nums.length === 0) {
        return 0;
    }
    
    // Initialize maxSum to the first element to handle all negative arrays
    let maxSum = nums[0];
    let currentSum = 0;
    
    for (let i = 0; i < nums.length; i++) {
        // Add current element to the current subarray
        currentSum += nums[i];
        
        // Update maxSum if currentSum is greater
        maxSum = Math.max(maxSum, currentSum);
        
        // If currentSum becomes negative, reset it to 0
        currentSum = Math.max(currentSum, 0);
    }
    
    return maxSum;
}
```

## Alternative Approach: Divide and Conquer

Another approach to solve this problem is using the divide and conquer technique:

1. Divide the array into two halves.
2. Recursively find the maximum subarray sum in the left half and right half.
3. Find the maximum subarray sum that crosses the middle element.
4. Return the maximum of the three sums.

This approach has a time complexity of O(n log n), which is less efficient than Kadane's algorithm.

## Time and Space Complexity

### Kadane's Algorithm:
- **Time Complexity**: O(n), where n is the length of the array. We traverse the array once.
- **Space Complexity**: O(1), as we only use a constant amount of extra space.

### Divide and Conquer:
- **Time Complexity**: O(n log n), due to the recursive calls.
- **Space Complexity**: O(log n) for the recursion stack.

## Edge Cases and Optimizations

- **Empty Array**: If the array is empty, the function will return 0.
- **All Negative Numbers**: If all numbers in the array are negative, the maximum subarray will consist of the largest (least negative) number.
- **Single Element**: If the array has only one element, that element is the maximum subarray sum.

## Common Mistakes

1. **Initializing maxSum to 0**: This doesn't handle the case where all elements are negative. Instead, initialize maxSum to the first element.
2. **Not resetting currentSum when it becomes negative**: The key insight of Kadane's algorithm is to reset currentSum to 0 when it becomes negative.
3. **Not handling edge cases**: Always consider edge cases like empty arrays or arrays with all negative numbers.

## Related Problems

1. **Maximum Product Subarray**: Similar to this problem, but find the subarray with the largest product.
2. **Maximum Circular Subarray Sum**: Find the maximum subarray sum in a circular array.
3. **Maximum Sum Rectangle in a 2D Matrix**: Find the submatrix with the largest sum.
4. **Maximum Subarray Sum After One Operation**: Find the maximum subarray sum after changing exactly one element. 