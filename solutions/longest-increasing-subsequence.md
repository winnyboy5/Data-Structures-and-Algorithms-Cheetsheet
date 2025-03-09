# Longest Increasing Subsequence

## Problem Statement

Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A subsequence is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**
```
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```

**Example 2:**
```
Input: nums = [0,1,0,3,2,3]
Output: 4
Explanation: The longest increasing subsequence is [0,1,2,3], therefore the length is 4.
```

**Example 3:**
```
Input: nums = [7,7,7,7,7,7,7]
Output: 1
Explanation: The longest increasing subsequence is [7], therefore the length is 1.
```

## Intuition

The key insight for this problem is to use dynamic programming to build up the solution. For each element in the array, we need to determine the length of the longest increasing subsequence ending at that element.

For any element at index `i`, we look at all previous elements at indices `j < i`. If `nums[i] > nums[j]`, then we can extend the subsequence ending at `j` by including the element at `i`. We choose the maximum length among all such possibilities.

## Visual Explanation

Let's visualize the solution with Example 1: `nums = [10,9,2,5,3,7,101,18]`

```
Initialize dp array where dp[i] = length of LIS ending at index i
dp = [1, 1, 1, 1, 1, 1, 1, 1] (initially, each element forms a subsequence of length 1)

For i = 0 (nums[0] = 10):
  - No previous elements to check
  - dp[0] = 1

For i = 1 (nums[1] = 9):
  - Check j = 0: nums[1] = 9 < nums[0] = 10, can't extend
  - dp[1] = 1

For i = 2 (nums[2] = 2):
  - Check j = 0: nums[2] = 2 < nums[0] = 10, can't extend
  - Check j = 1: nums[2] = 2 < nums[1] = 9, can't extend
  - dp[2] = 1

For i = 3 (nums[3] = 5):
  - Check j = 0: nums[3] = 5 < nums[0] = 10, can't extend
  - Check j = 1: nums[3] = 5 < nums[1] = 9, can't extend
  - Check j = 2: nums[3] = 5 > nums[2] = 2, can extend: dp[3] = max(dp[3], dp[2] + 1) = max(1, 1 + 1) = 2
  - dp[3] = 2

For i = 4 (nums[4] = 3):
  - Check j = 0: nums[4] = 3 < nums[0] = 10, can't extend
  - Check j = 1: nums[4] = 3 < nums[1] = 9, can't extend
  - Check j = 2: nums[4] = 3 > nums[2] = 2, can extend: dp[4] = max(dp[4], dp[2] + 1) = max(1, 1 + 1) = 2
  - Check j = 3: nums[4] = 3 < nums[3] = 5, can't extend
  - dp[4] = 2

For i = 5 (nums[5] = 7):
  - Check j = 0: nums[5] = 7 < nums[0] = 10, can't extend
  - Check j = 1: nums[5] = 7 < nums[1] = 9, can't extend
  - Check j = 2: nums[5] = 7 > nums[2] = 2, can extend: dp[5] = max(dp[5], dp[2] + 1) = max(1, 1 + 1) = 2
  - Check j = 3: nums[5] = 7 > nums[3] = 5, can extend: dp[5] = max(dp[5], dp[3] + 1) = max(2, 2 + 1) = 3
  - Check j = 4: nums[5] = 7 > nums[4] = 3, can extend: dp[5] = max(dp[5], dp[4] + 1) = max(3, 2 + 1) = 3
  - dp[5] = 3

For i = 6 (nums[6] = 101):
  - Check all previous j: nums[6] = 101 > all previous elements
  - dp[6] = max(dp[j] + 1) for all j < i = max(1+1, 1+1, 1+1, 2+1, 2+1, 3+1) = 4
  - dp[6] = 4

For i = 7 (nums[7] = 18):
  - Check j = 0 to j = 5: nums[7] = 18 > nums[j], can extend from each
  - Check j = 6: nums[7] = 18 < nums[6] = 101, can't extend
  - dp[7] = max(dp[j] + 1) for valid j < i = max(1+1, 1+1, 1+1, 2+1, 2+1, 3+1) = 4
  - dp[7] = 4

Final dp array: [1, 1, 1, 2, 2, 3, 4, 4]
The maximum value in dp is 4, which is the length of the longest increasing subsequence.
```

![Longest Increasing Subsequence Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize a dp array of the same length as the input array, with all values set to 1 (since each element by itself forms an increasing subsequence of length 1).
2. For each element at index i:
   - Iterate through all previous elements at indices j < i.
   - If nums[i] > nums[j], we can extend the subsequence ending at j to include the element at i.
   - Update dp[i] = max(dp[i], dp[j] + 1).
3. Return the maximum value in the dp array, which represents the length of the longest increasing subsequence.

## Code Implementation

### Python

```python
def lengthOfLIS(nums):
    if not nums:
        return 0
    
    n = len(nums)
    dp = [1] * n
    
    for i in range(1, n):
        for j in range(i):
            if nums[i] > nums[j]:
                dp[i] = max(dp[i], dp[j] + 1)
    
    return max(dp)
```

### Python (Optimized - Binary Search)

```python
def lengthOfLIS(nums):
    if not nums:
        return 0
    
    # tails[i] = smallest ending value of all increasing subsequences of length i+1
    tails = []
    
    for num in nums:
        # Binary search to find the position to insert num
        left, right = 0, len(tails)
        while left < right:
            mid = (left + right) // 2
            if tails[mid] < num:
                left = mid + 1
            else:
                right = mid
        
        # If we're at the end, append to tails
        if left == len(tails):
            tails.append(num)
        # Otherwise, replace the element at the found position
        else:
            tails[left] = num
    
    return len(tails)
```

### JavaScript

```javascript
function lengthOfLIS(nums) {
    if (!nums || nums.length === 0) {
        return 0;
    }
    
    const n = nums.length;
    const dp = Array(n).fill(1);
    
    for (let i = 1; i < n; i++) {
        for (let j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
    }
    
    return Math.max(...dp);
}
```

### JavaScript (Optimized - Binary Search)

```javascript
function lengthOfLIS(nums) {
    if (!nums || nums.length === 0) {
        return 0;
    }
    
    // tails[i] = smallest ending value of all increasing subsequences of length i+1
    const tails = [];
    
    for (const num of nums) {
        // Binary search to find the position to insert num
        let left = 0;
        let right = tails.length;
        
        while (left < right) {
            const mid = Math.floor((left + right) / 2);
            if (tails[mid] < num) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        // If we're at the end, append to tails
        if (left === tails.length) {
            tails.push(num);
        } 
        // Otherwise, replace the element at the found position
        else {
            tails[left] = num;
        }
    }
    
    return tails.length;
}
```

## Time and Space Complexity

### Dynamic Programming Approach:
- **Time Complexity**: O(n²), where n is the length of the input array. We have two nested loops, each iterating through the array.
- **Space Complexity**: O(n) for the dp array.

### Binary Search Approach:
- **Time Complexity**: O(n log n), where n is the length of the input array. For each element, we perform a binary search which takes O(log n) time.
- **Space Complexity**: O(n) for the tails array.

## Detailed Walkthrough with Example

Let's trace through the dynamic programming approach with Example 2: `nums = [0,1,0,3,2,3]`

```
Initialize dp = [1, 1, 1, 1, 1, 1]

For i = 0 (nums[0] = 0):
  - No previous elements to check
  - dp[0] = 1

For i = 1 (nums[1] = 1):
  - Check j = 0: nums[1] = 1 > nums[0] = 0, can extend: dp[1] = max(dp[1], dp[0] + 1) = max(1, 1 + 1) = 2
  - dp[1] = 2

For i = 2 (nums[2] = 0):
  - Check j = 0: nums[2] = 0 = nums[0] = 0, can't extend (not strictly increasing)
  - Check j = 1: nums[2] = 0 < nums[1] = 1, can't extend
  - dp[2] = 1

For i = 3 (nums[3] = 3):
  - Check j = 0: nums[3] = 3 > nums[0] = 0, can extend: dp[3] = max(dp[3], dp[0] + 1) = max(1, 1 + 1) = 2
  - Check j = 1: nums[3] = 3 > nums[1] = 1, can extend: dp[3] = max(dp[3], dp[1] + 1) = max(2, 2 + 1) = 3
  - Check j = 2: nums[3] = 3 > nums[2] = 0, can extend: dp[3] = max(dp[3], dp[2] + 1) = max(3, 1 + 1) = 3
  - dp[3] = 3

For i = 4 (nums[4] = 2):
  - Check j = 0: nums[4] = 2 > nums[0] = 0, can extend: dp[4] = max(dp[4], dp[0] + 1) = max(1, 1 + 1) = 2
  - Check j = 1: nums[4] = 2 > nums[1] = 1, can extend: dp[4] = max(dp[4], dp[1] + 1) = max(2, 2 + 1) = 3
  - Check j = 2: nums[4] = 2 > nums[2] = 0, can extend: dp[4] = max(dp[4], dp[2] + 1) = max(3, 1 + 1) = 3
  - Check j = 3: nums[4] = 2 < nums[3] = 3, can't extend
  - dp[4] = 3

For i = 5 (nums[5] = 3):
  - Check j = 0: nums[5] = 3 > nums[0] = 0, can extend: dp[5] = max(dp[5], dp[0] + 1) = max(1, 1 + 1) = 2
  - Check j = 1: nums[5] = 3 > nums[1] = 1, can extend: dp[5] = max(dp[5], dp[1] + 1) = max(2, 2 + 1) = 3
  - Check j = 2: nums[5] = 3 > nums[2] = 0, can extend: dp[5] = max(dp[5], dp[2] + 1) = max(3, 1 + 1) = 3
  - Check j = 3: nums[5] = 3 = nums[3] = 3, can't extend (not strictly increasing)
  - Check j = 4: nums[5] = 3 > nums[4] = 2, can extend: dp[5] = max(dp[5], dp[4] + 1) = max(3, 3 + 1) = 4
  - dp[5] = 4

Final dp array: [1, 2, 1, 3, 3, 4]
The maximum value in dp is 4, which is the length of the longest increasing subsequence.
```

## Edge Cases and Optimizations

- **Empty Array**: If the input array is empty, the function should return 0.
- **Single Element**: If the array has only one element, the function will return 1.
- **All Equal Elements**: If all elements in the array are equal, the function will return 1 (since we need a strictly increasing subsequence).
- **Already Sorted Array**: If the array is already sorted in ascending order, the function will return the length of the array.
- **Reverse Sorted Array**: If the array is sorted in descending order, the function will return 1.

**Optimization**: The binary search approach improves the time complexity from O(n²) to O(n log n) by maintaining an array of the smallest ending values for subsequences of each length.

## Common Mistakes

1. **Not handling duplicates correctly**: Remember that the problem asks for a strictly increasing subsequence, so equal elements cannot be part of the same subsequence.
2. **Confusing subsequence with subarray**: A subsequence doesn't need to be contiguous, while a subarray must be contiguous.
3. **Incorrect initialization**: The dp array should be initialized with 1s, not 0s, since each element by itself forms a subsequence of length 1.

## Related Problems

1. **Longest Common Subsequence**: Find the length of the longest subsequence common to two sequences.
2. **Increasing Triplet Subsequence**: Determine if there exists a triplet of indices (i, j, k) such that i < j < k and nums[i] < nums[j] < nums[k].
3. **Russian Doll Envelopes**: You have a number of envelopes with widths and heights given as a pair of integers. One envelope can fit into another if and only if both the width and height of one envelope are greater than the other envelope's width and height.
4. **Maximum Length of Pair Chain**: You are given pairs of numbers. In every pair, the first number is always smaller than the second number. A pair (c, d) can follow another pair (a, b) if b < c. Chain of pairs can be formed in this fashion. Find the longest chain which can be formed. 