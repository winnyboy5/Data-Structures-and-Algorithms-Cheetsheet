# Maximum Product Subarray

## Problem Statement

Given an integer array `nums`, find a contiguous non-empty subarray within the array that has the largest product, and return the product.

The test cases are generated so that the answer will fit in a 32-bit integer.

**Example 1:**
```
Input: nums = [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```

**Example 2:**
```
Input: nums = [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```

## Intuition

This problem is similar to the "Maximum Subarray" problem, but with a key difference: multiplication can turn negative numbers into positive ones when multiplied by another negative number. This means that a very negative number could potentially contribute to the maximum product if we encounter another negative number later.

The key insight is to keep track of both the maximum and minimum products ending at the current position. The minimum product is important because if we encounter a negative number, multiplying it with the minimum product (which could be negative) might give us a new maximum product.

## Visual Explanation

Let's visualize the solution with Example 1: `nums = [2,3,-2,4]`

```
Initialize:
- max_so_far = nums[0] = 2
- min_so_far = nums[0] = 2
- result = nums[0] = 2

Iteration 1 (i = 1, nums[i] = 3):
- Calculate potential new max and min:
  - new_max = max(3, 2*3, 2*3) = max(3, 6, 6) = 6
  - new_min = min(3, 2*3, 2*3) = min(3, 6, 6) = 3
- Update max_so_far = 6, min_so_far = 3
- Update result = max(2, 6) = 6

Iteration 2 (i = 2, nums[i] = -2):
- Calculate potential new max and min:
  - new_max = max(-2, 6*(-2), 3*(-2)) = max(-2, -12, -6) = -2
  - new_min = min(-2, 6*(-2), 3*(-2)) = min(-2, -12, -6) = -12
- Update max_so_far = -2, min_so_far = -12
- Update result = max(6, -2) = 6

Iteration 3 (i = 3, nums[i] = 4):
- Calculate potential new max and min:
  - new_max = max(4, -2*4, -12*4) = max(4, -8, -48) = 4
  - new_min = min(4, -2*4, -12*4) = min(4, -8, -48) = -48
- Update max_so_far = 4, min_so_far = -48
- Update result = max(6, 4) = 6

Result: 6
```

![Maximum Product Subarray Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize three variables:
   - `max_so_far`: Maximum product ending at the current position
   - `min_so_far`: Minimum product ending at the current position
   - `result`: Maximum product found so far
   
2. Set all three variables to the first element of the array.

3. Iterate through the array starting from the second element:
   - For each element, calculate the potential new maximum and minimum products:
     - `new_max = max(nums[i], max_so_far * nums[i], min_so_far * nums[i])`
     - `new_min = min(nums[i], max_so_far * nums[i], min_so_far * nums[i])`
   - Update `max_so_far` and `min_so_far` with the new values.
   - Update `result` if `max_so_far` is greater than the current `result`.

4. Return `result`.

## Code Implementation

### Python

```python
def maxProduct(nums):
    if not nums:
        return 0
    
    max_so_far = min_so_far = result = nums[0]
    
    for i in range(1, len(nums)):
        # Store current values before updating
        curr = nums[i]
        temp_max = max(curr, max_so_far * curr, min_so_far * curr)
        min_so_far = min(curr, max_so_far * curr, min_so_far * curr)
        
        # Update max_so_far after using its old value to calculate min_so_far
        max_so_far = temp_max
        
        # Update result if we found a larger product
        result = max(result, max_so_far)
    
    return result
```

### JavaScript

```javascript
function maxProduct(nums) {
    if (!nums.length) {
        return 0;
    }
    
    let maxSoFar = nums[0];
    let minSoFar = nums[0];
    let result = nums[0];
    
    for (let i = 1; i < nums.length; i++) {
        // Store current values before updating
        const curr = nums[i];
        const tempMax = Math.max(curr, maxSoFar * curr, minSoFar * curr);
        minSoFar = Math.min(curr, maxSoFar * curr, minSoFar * curr);
        
        // Update maxSoFar after using its old value to calculate minSoFar
        maxSoFar = tempMax;
        
        // Update result if we found a larger product
        result = Math.max(result, maxSoFar);
    }
    
    return result;
}
```

## Time and Space Complexity

- **Time Complexity**: O(n), where n is the length of the array. We iterate through the array once.
- **Space Complexity**: O(1), as we only use a constant amount of extra space regardless of the input size.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2: `nums = [-2,0,-1]`

```
Initialize:
- max_so_far = nums[0] = -2
- min_so_far = nums[0] = -2
- result = nums[0] = -2

Iteration 1 (i = 1, nums[i] = 0):
- Calculate potential new max and min:
  - new_max = max(0, -2*0, -2*0) = max(0, 0, 0) = 0
  - new_min = min(0, -2*0, -2*0) = min(0, 0, 0) = 0
- Update max_so_far = 0, min_so_far = 0
- Update result = max(-2, 0) = 0

Iteration 2 (i = 2, nums[i] = -1):
- Calculate potential new max and min:
  - new_max = max(-1, 0*(-1), 0*(-1)) = max(-1, 0, 0) = 0
  - new_min = min(-1, 0*(-1), 0*(-1)) = min(-1, 0, 0) = -1
- Update max_so_far = 0, min_so_far = -1
- Update result = max(0, 0) = 0

Result: 0
```

## Edge Cases and Optimizations

- **Empty Array**: If the array is empty, return 0.
- **Single Element**: If the array has only one element, return that element.
- **All Negative Numbers**: If all numbers are negative, the maximum product will be the largest negative number (if the array has an odd number of elements) or the product of the two smallest negative numbers (if the array has an even number of elements).
- **Contains Zero**: If the array contains zeros, the maximum product might be 0 if no positive product can be formed.

## Common Mistakes

1. **Not considering negative numbers**: A common mistake is to only track the maximum product and not the minimum product, which fails when negative numbers are present.
2. **Resetting on zero**: Some implementations reset the maximum and minimum products to 1 when encountering a zero, but this might not be optimal if there are better subarrays after the zero.
3. **Not initializing result properly**: The result should be initialized to the first element, not to 0 or 1, to handle cases where the array contains only negative numbers.

## Related Problems

1. **Maximum Subarray**: Find the contiguous subarray with the largest sum.
2. **Best Time to Buy and Sell Stock**: Find the maximum profit by buying and selling a stock once.
3. **Maximum Product of Three Numbers**: Find three numbers in an array that maximize the product.
4. **Maximum Subarray Sum Circular**: Find the maximum possible sum of a non-empty subarray in a circular array. 