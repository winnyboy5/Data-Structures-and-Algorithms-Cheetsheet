# Missing Number

## Problem Statement

Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return the only number in the range that is missing from the array.

**Example 1:**
```
Input: nums = [3,0,1]
Output: 2
Explanation: n = 3 since there are 3 numbers, so all numbers are in the range [0,3]. 2 is the missing number in the range since it does not appear in nums.
```

**Example 2:**
```
Input: nums = [0,1]
Output: 2
Explanation: n = 2 since there are 2 numbers, so all numbers are in the range [0,2]. 2 is the missing number in the range since it does not appear in nums.
```

**Example 3:**
```
Input: nums = [9,6,4,2,3,5,7,0,1]
Output: 8
Explanation: n = 9 since there are 9 numbers, so all numbers are in the range [0,9]. 8 is the missing number in the range since it does not appear in nums.
```

## Intuition

There are several approaches to solve this problem:

1. **Sum Method**: Calculate the expected sum of numbers from 0 to n using the formula `n * (n + 1) / 2`, then subtract the actual sum of the array. The difference is the missing number.

2. **XOR Method**: XOR all numbers from 0 to n with all numbers in the array. Since XOR of a number with itself is 0, and XOR is commutative and associative, the result will be the missing number.

3. **Sorting Method**: Sort the array and find the first position where the number doesn't match its index.

4. **Hash Set Method**: Put all numbers in a hash set, then check which number from 0 to n is not in the set.

The XOR method is particularly elegant because it doesn't require extra space (beyond the input and output) and has a linear time complexity.

## Visual Explanation

Let's visualize the XOR method with Example 1: `nums = [3,0,1]`

```
Expected numbers: 0, 1, 2, 3
Actual numbers in array: 3, 0, 1

XOR all numbers from 0 to n:
result = 0 ⊕ 1 ⊕ 2 ⊕ 3 = 0

XOR all numbers in the array:
result = result ⊕ 3 ⊕ 0 ⊕ 1
       = 0 ⊕ 3 ⊕ 0 ⊕ 1
       = 3 ⊕ 0 ⊕ 1

Since XOR is commutative and associative:
result = (0 ⊕ 0) ⊕ (1 ⊕ 1) ⊕ (2) ⊕ (3 ⊕ 3)
       = 0 ⊕ 0 ⊕ 2 ⊕ 0
       = 2

The missing number is 2.
```

Let's also visualize the Sum method with the same example:

```
Expected sum of numbers from 0 to 3:
expectedSum = 3 * (3 + 1) / 2 = 6

Actual sum of the array:
actualSum = 3 + 0 + 1 = 4

Missing number = expectedSum - actualSum = 6 - 4 = 2
```

![Missing Number Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

### XOR Method:
1. Initialize a variable `result` to `n` (the length of the array).
2. For each index `i` and value `nums[i]` in the array:
   - XOR `result` with both `i` and `nums[i]`.
3. Return `result`.

### Sum Method:
1. Calculate the expected sum of numbers from 0 to n using the formula `n * (n + 1) / 2`.
2. Calculate the actual sum of the array.
3. Return the difference between the expected sum and the actual sum.

## Code Implementation

### Python (XOR Method)

```python
def missingNumber(nums):
    result = len(nums)
    
    for i, num in enumerate(nums):
        result ^= i ^ num
    
    return result
```

### Python (Sum Method)

```python
def missingNumber(nums):
    n = len(nums)
    expected_sum = n * (n + 1) // 2
    actual_sum = sum(nums)
    
    return expected_sum - actual_sum
```

### JavaScript (XOR Method)

```javascript
function missingNumber(nums) {
    let result = nums.length;
    
    for (let i = 0; i < nums.length; i++) {
        result ^= i ^ nums[i];
    }
    
    return result;
}
```

### JavaScript (Sum Method)

```javascript
function missingNumber(nums) {
    const n = nums.length;
    const expectedSum = n * (n + 1) / 2;
    const actualSum = nums.reduce((sum, num) => sum + num, 0);
    
    return expectedSum - actualSum;
}
```

## Time and Space Complexity

### XOR Method:
- **Time Complexity**: O(n), where n is the length of the array. We iterate through the array once.
- **Space Complexity**: O(1), as we only use a single variable regardless of the input size.

### Sum Method:
- **Time Complexity**: O(n), where n is the length of the array. We iterate through the array once to calculate the sum.
- **Space Complexity**: O(1), as we only use a few variables regardless of the input size.

## Detailed Walkthrough with Example

Let's trace through the XOR method with Example 3: `nums = [9,6,4,2,3,5,7,0,1]`

```
Initialize result = 9 (length of the array)

i = 0, nums[0] = 9:
  result = result ^ i ^ nums[0] = 9 ^ 0 ^ 9 = 0

i = 1, nums[1] = 6:
  result = result ^ i ^ nums[1] = 0 ^ 1 ^ 6 = 7

i = 2, nums[2] = 4:
  result = result ^ i ^ nums[2] = 7 ^ 2 ^ 4 = 1

i = 3, nums[3] = 2:
  result = result ^ i ^ nums[3] = 1 ^ 3 ^ 2 = 0

i = 4, nums[4] = 3:
  result = result ^ i ^ nums[4] = 0 ^ 4 ^ 3 = 7

i = 5, nums[5] = 5:
  result = result ^ i ^ nums[5] = 7 ^ 5 ^ 5 = 7

i = 6, nums[6] = 7:
  result = result ^ i ^ nums[6] = 7 ^ 6 ^ 7 = 6

i = 7, nums[7] = 0:
  result = result ^ i ^ nums[7] = 6 ^ 7 ^ 0 = 1

i = 8, nums[8] = 1:
  result = result ^ i ^ nums[8] = 1 ^ 8 ^ 1 = 8

Return result = 8
```

Let's also trace through the Sum method with the same example:

```
n = 9
Expected sum = 9 * (9 + 1) / 2 = 45
Actual sum = 9 + 6 + 4 + 2 + 3 + 5 + 7 + 0 + 1 = 37
Missing number = 45 - 37 = 8
```

## Edge Cases and Optimizations

- **Empty Array**: The problem states that the array contains n distinct numbers in the range [0, n], so the array will have at least one element.
- **Single Element**: If the array has only one element, it can either be 0 or 1. If it's 0, the missing number is 1. If it's 1, the missing number is 0.
- **Large Arrays**: Both the XOR and Sum methods handle large arrays efficiently with O(n) time complexity.

**Optimization**: The XOR method is particularly efficient because it doesn't require calculating the sum of the array, which could potentially lead to integer overflow for very large arrays. However, the Sum method can be optimized to avoid overflow by calculating the difference iteratively.

## Common Mistakes

1. **Off-by-one errors**: Be careful with the range. The problem states that the range is [0, n], where n is the length of the array.
2. **Integer overflow**: When using the Sum method, be aware of potential integer overflow for large arrays.
3. **Not handling edge cases**: Make sure your solution works for all valid inputs, including arrays with a single element.

## Related Problems

1. **Find the Duplicate Number**: Find the only duplicate in an array containing n+1 integers where each integer is between 1 and n.
2. **Single Number**: Find the only number that appears once in an array where all other numbers appear twice.
3. **First Missing Positive**: Find the smallest missing positive integer in an array.
4. **Find All Numbers Disappeared in an Array**: Find all numbers in the range [1, n] that do not appear in an array of length n. 