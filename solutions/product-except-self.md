# Product of Array Except Self

## Problem Statement

Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer.

You must write an algorithm that runs in O(n) time and without using the division operation.

**Example 1:**
```
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
Explanation: 
- answer[0] = 2 * 3 * 4 = 24
- answer[1] = 1 * 3 * 4 = 12
- answer[2] = 1 * 2 * 4 = 8
- answer[3] = 1 * 2 * 3 = 6
```

**Example 2:**
```
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]
Explanation: 
- answer[0] = 1 * 0 * -3 * 3 = 0
- answer[1] = -1 * 0 * -3 * 3 = 0
- answer[2] = -1 * 1 * -3 * 3 = 9
- answer[3] = -1 * 1 * 0 * 3 = 0
- answer[4] = -1 * 1 * 0 * -3 = 0
```

## Intuition

The key insight for this problem is to recognize that for each element at index `i`, we need to compute the product of all elements to the left of `i` and all elements to the right of `i`.

A naive approach would be to use nested loops, resulting in O(n²) time complexity. However, we can optimize this by:

1. First calculating the product of all elements to the left of each index.
2. Then calculating the product of all elements to the right of each index.
3. Finally, multiplying these two products for each index.

This approach allows us to solve the problem in O(n) time without using division.

## Visual Explanation

Let's visualize the solution with Example 1: `nums = [1,2,3,4]`

```
Step 1: Calculate the product of all elements to the left of each index.
        left[0] = 1 (no elements to the left, so use 1 as the identity for multiplication)
        left[1] = 1 * 1 = 1
        left[2] = 1 * 2 = 2
        left[3] = 2 * 3 = 6
        left = [1, 1, 2, 6]

Step 2: Calculate the product of all elements to the right of each index.
        right[3] = 1 (no elements to the right, so use 1)
        right[2] = 1 * 4 = 4
        right[1] = 4 * 3 = 12
        right[0] = 12 * 2 = 24
        right = [24, 12, 4, 1]

Step 3: Multiply the left and right products for each index.
        answer[0] = left[0] * right[0] = 1 * 24 = 24
        answer[1] = left[1] * right[1] = 1 * 12 = 12
        answer[2] = left[2] * right[2] = 2 * 4 = 8
        answer[3] = left[3] * right[3] = 6 * 1 = 6
        answer = [24, 12, 8, 6]
```

![Product of Array Except Self Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize an array `answer` of the same length as `nums`, filled with 1s.
2. Calculate the product of all elements to the left of each index:
   - Iterate from left to right, keeping track of the running product of all elements seen so far.
   - For each index `i`, set `answer[i]` to the running product, then update the running product by multiplying it with `nums[i]`.
3. Calculate the product of all elements to the right of each index and multiply it with the left product:
   - Iterate from right to left, keeping track of the running product of all elements seen so far.
   - For each index `i`, multiply `answer[i]` by the running product, then update the running product by multiplying it with `nums[i]`.
4. Return the `answer` array.

## Code Implementation

### Python

```python
def productExceptSelf(nums):
    n = len(nums)
    answer = [1] * n
    
    # Calculate the product of all elements to the left of each index
    left_product = 1
    for i in range(n):
        answer[i] = left_product
        left_product *= nums[i]
    
    # Calculate the product of all elements to the right of each index
    # and multiply it with the left product
    right_product = 1
    for i in range(n - 1, -1, -1):
        answer[i] *= right_product
        right_product *= nums[i]
    
    return answer
```

### JavaScript

```javascript
function productExceptSelf(nums) {
    const n = nums.length;
    const answer = new Array(n).fill(1);
    
    // Calculate the product of all elements to the left of each index
    let leftProduct = 1;
    for (let i = 0; i < n; i++) {
        answer[i] = leftProduct;
        leftProduct *= nums[i];
    }
    
    // Calculate the product of all elements to the right of each index
    // and multiply it with the left product
    let rightProduct = 1;
    for (let i = n - 1; i >= 0; i--) {
        answer[i] *= rightProduct;
        rightProduct *= nums[i];
    }
    
    return answer;
}
```

## Time and Space Complexity

- **Time Complexity**: O(n), where n is the length of the array. We iterate through the array twice.
- **Space Complexity**: O(1), excluding the output array. We only use a constant amount of extra space.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2: `nums = [-1,1,0,-3,3]`

```
Initialize answer = [1, 1, 1, 1, 1]

Step 1: Calculate the product of all elements to the left of each index.
        left_product = 1
        
        i = 0:
        answer[0] = left_product = 1
        left_product *= nums[0] = 1 * (-1) = -1
        
        i = 1:
        answer[1] = left_product = -1
        left_product *= nums[1] = -1 * 1 = -1
        
        i = 2:
        answer[2] = left_product = -1
        left_product *= nums[2] = -1 * 0 = 0
        
        i = 3:
        answer[3] = left_product = 0
        left_product *= nums[3] = 0 * (-3) = 0
        
        i = 4:
        answer[4] = left_product = 0
        left_product *= nums[4] = 0 * 3 = 0
        
        answer = [1, -1, -1, 0, 0]

Step 2: Calculate the product of all elements to the right of each index.
        right_product = 1
        
        i = 4:
        answer[4] *= right_product = 0 * 1 = 0
        right_product *= nums[4] = 1 * 3 = 3
        
        i = 3:
        answer[3] *= right_product = 0 * 3 = 0
        right_product *= nums[3] = 3 * (-3) = -9
        
        i = 2:
        answer[2] *= right_product = -1 * (-9) = 9
        right_product *= nums[2] = -9 * 0 = 0
        
        i = 1:
        answer[1] *= right_product = -1 * 0 = 0
        right_product *= nums[1] = 0 * 1 = 0
        
        i = 0:
        answer[0] *= right_product = 1 * 0 = 0
        right_product *= nums[0] = 0 * (-1) = 0
        
        answer = [0, 0, 9, 0, 0]
```

## Alternative Approaches

### Using Division (Not Allowed by the Problem)

If division were allowed, we could:
1. Calculate the product of all elements in the array.
2. For each index `i`, divide the total product by `nums[i]` to get the product of all elements except `nums[i]`.

However, this approach would fail if there are zeros in the array, and the problem explicitly states that division is not allowed.

### Using Extra Space

We could use two separate arrays to store the left and right products:

```python
def productExceptSelf(nums):
    n = len(nums)
    left_products = [1] * n
    right_products = [1] * n
    answer = [1] * n
    
    # Calculate left products
    for i in range(1, n):
        left_products[i] = left_products[i - 1] * nums[i - 1]
    
    # Calculate right products
    for i in range(n - 2, -1, -1):
        right_products[i] = right_products[i + 1] * nums[i + 1]
    
    # Calculate the final answer
    for i in range(n):
        answer[i] = left_products[i] * right_products[i]
    
    return answer
```

This approach has a time complexity of O(n) but a space complexity of O(n).

## Edge Cases and Optimizations

- **Array with Zeros**: The algorithm handles arrays with zeros correctly. If there's a zero at index `i`, all products except for `answer[i]` will be zero.
- **Negative Numbers**: The algorithm works correctly with negative numbers.
- **Single Element Array**: If the array has only one element, the answer would be an array with a single element 1 (since there are no other elements to multiply).
- **Empty Array**: The problem statement guarantees that the input array is non-empty.

## Common Mistakes

1. **Using Division**: The problem explicitly states that division is not allowed.
2. **Not Handling Zeros**: If using division (which is not allowed), not handling zeros properly would lead to division by zero errors.
3. **Inefficient Space Usage**: Using separate arrays for left and right products increases the space complexity to O(n).

## Related Problems

1. **Maximum Product Subarray**: Find the contiguous subarray within an array that has the largest product.
2. **Subarray Product Less Than K**: Find the number of contiguous subarrays where the product of all the elements in the subarray is less than k.
3. **K Empty Slots**: Given an array of flowers where flowers[i] means the ith flower will bloom on the flowers[i]th day, find the minimum number of days to wait to see exactly k empty slots between any two blooming flowers.
4. **Trapping Rain Water**: Calculate how much water can be trapped after raining. 