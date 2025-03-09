# House Robber II

## Problem Statement

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an array `nums` representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

**Example 1:**
```
Input: nums = [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
```

**Example 2:**
```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

**Example 3:**
```
Input: nums = [1,2,3]
Output: 3
```

## Intuition

This problem is an extension of the original House Robber problem, with the additional constraint that the houses are arranged in a circle, meaning the first and last houses are adjacent.

The key insight is to break this circular arrangement into two linear problems:
1. Rob houses from index 0 to n-2 (excluding the last house)
2. Rob houses from index 1 to n-1 (excluding the first house)

By solving these two linear problems and taking the maximum result, we can find the optimal solution for the circular arrangement. This works because we can't rob both the first and last houses (they're adjacent in a circle), so at least one of them must be excluded.

## Visual Explanation

Let's visualize the solution with Example 1: `nums = [2,3,2]`

```
Houses arranged in a circle:
    2
   / \
  2   3

Case 1: Exclude the last house [2,3]
- Rob house 1 (2) or house 2 (3)? Choose house 2 (3)
- Result = 3

Case 2: Exclude the first house [3,2]
- Rob house 2 (3) or house 3 (2)? Choose house 2 (3)
- Result = 3

Maximum of both cases = 3
```

Let's visualize Example 2: `nums = [1,2,3,1]`

```
Houses arranged in a circle:
    1
   / \
  1   2
   \ /
    3

Case 1: Exclude the last house [1,2,3]
- Using the House Robber algorithm:
  - dp[0] = 1
  - dp[1] = max(1, 2) = 2
  - dp[2] = max(2, 1+3) = 4
- Result = 4

Case 2: Exclude the first house [2,3,1]
- Using the House Robber algorithm:
  - dp[0] = 2
  - dp[1] = max(2, 3) = 3
  - dp[2] = max(3, 2+1) = 3
- Result = 3

Maximum of both cases = 4
```

![House Robber II Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Handle edge cases:
   - If the array is empty, return 0
   - If the array has only one element, return that element
   - If the array has only two elements, return the maximum of the two

2. Define a helper function to solve the original House Robber problem (linear arrangement)

3. For the circular arrangement:
   - Call the helper function for houses 0 to n-2 (excluding the last house)
   - Call the helper function for houses 1 to n-1 (excluding the first house)
   - Return the maximum of these two results

## Code Implementation

### Python

```python
def rob(nums):
    # Handle edge cases
    if not nums:
        return 0
    if len(nums) == 1:
        return nums[0]
    if len(nums) == 2:
        return max(nums[0], nums[1])
    
    # Helper function to solve the original House Robber problem
    def rob_linear(arr):
        if not arr:
            return 0
        if len(arr) == 1:
            return arr[0]
        
        # Dynamic programming approach
        dp = [0] * len(arr)
        dp[0] = arr[0]
        dp[1] = max(arr[0], arr[1])
        
        for i in range(2, len(arr)):
            dp[i] = max(dp[i-1], dp[i-2] + arr[i])
        
        return dp[-1]
    
    # Case 1: Rob houses 0 to n-2 (exclude the last house)
    result1 = rob_linear(nums[:-1])
    
    # Case 2: Rob houses 1 to n-1 (exclude the first house)
    result2 = rob_linear(nums[1:])
    
    # Return the maximum result
    return max(result1, result2)
```

### Python (Space-Optimized)

```python
def rob(nums):
    # Handle edge cases
    if not nums:
        return 0
    if len(nums) == 1:
        return nums[0]
    if len(nums) == 2:
        return max(nums[0], nums[1])
    
    # Helper function to solve the original House Robber problem
    def rob_linear(arr):
        prev1 = 0  # dp[i-1]
        prev2 = 0  # dp[i-2]
        
        for num in arr:
            curr = max(prev1, prev2 + num)
            prev2 = prev1
            prev1 = curr
        
        return prev1
    
    # Case 1: Rob houses 0 to n-2 (exclude the last house)
    result1 = rob_linear(nums[:-1])
    
    # Case 2: Rob houses 1 to n-1 (exclude the first house)
    result2 = rob_linear(nums[1:])
    
    # Return the maximum result
    return max(result1, result2)
```

### JavaScript

```javascript
function rob(nums) {
    // Handle edge cases
    if (!nums || nums.length === 0) {
        return 0;
    }
    if (nums.length === 1) {
        return nums[0];
    }
    if (nums.length === 2) {
        return Math.max(nums[0], nums[1]);
    }
    
    // Helper function to solve the original House Robber problem
    function robLinear(arr) {
        if (!arr || arr.length === 0) {
            return 0;
        }
        if (arr.length === 1) {
            return arr[0];
        }
        
        // Dynamic programming approach
        const dp = new Array(arr.length).fill(0);
        dp[0] = arr[0];
        dp[1] = Math.max(arr[0], arr[1]);
        
        for (let i = 2; i < arr.length; i++) {
            dp[i] = Math.max(dp[i-1], dp[i-2] + arr[i]);
        }
        
        return dp[dp.length - 1];
    }
    
    // Case 1: Rob houses 0 to n-2 (exclude the last house)
    const result1 = robLinear(nums.slice(0, nums.length - 1));
    
    // Case 2: Rob houses 1 to n-1 (exclude the first house)
    const result2 = robLinear(nums.slice(1));
    
    // Return the maximum result
    return Math.max(result1, result2);
}
```

### JavaScript (Space-Optimized)

```javascript
function rob(nums) {
    // Handle edge cases
    if (!nums || nums.length === 0) {
        return 0;
    }
    if (nums.length === 1) {
        return nums[0];
    }
    if (nums.length === 2) {
        return Math.max(nums[0], nums[1]);
    }
    
    // Helper function to solve the original House Robber problem
    function robLinear(arr) {
        let prev1 = 0;  // dp[i-1]
        let prev2 = 0;  // dp[i-2]
        
        for (const num of arr) {
            const curr = Math.max(prev1, prev2 + num);
            prev2 = prev1;
            prev1 = curr;
        }
        
        return prev1;
    }
    
    // Case 1: Rob houses 0 to n-2 (exclude the last house)
    const result1 = robLinear(nums.slice(0, nums.length - 1));
    
    // Case 2: Rob houses 1 to n-1 (exclude the first house)
    const result2 = robLinear(nums.slice(1));
    
    // Return the maximum result
    return Math.max(result1, result2);
}
```

## Time and Space Complexity

### Time Complexity
- **O(n)**, where n is the number of houses. We solve the linear House Robber problem twice, each taking O(n) time.

### Space Complexity
- **O(n)** for the dynamic programming approach, as we use an array of size n to store intermediate results.
- **O(1)** for the space-optimized approach, as we only use a constant amount of extra space.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2: `nums = [1,2,3,1]`

```
Case 1: Exclude the last house [1,2,3]
- Initialize dp = [0, 0, 0]
- dp[0] = nums[0] = 1
- dp[1] = max(nums[0], nums[1]) = max(1, 2) = 2
- dp[2] = max(dp[1], dp[0] + nums[2]) = max(2, 1 + 3) = max(2, 4) = 4
- Result = dp[2] = 4

Case 2: Exclude the first house [2,3,1]
- Initialize dp = [0, 0, 0]
- dp[0] = nums[0] = 2
- dp[1] = max(nums[0], nums[1]) = max(2, 3) = 3
- dp[2] = max(dp[1], dp[0] + nums[2]) = max(3, 2 + 1) = max(3, 3) = 3
- Result = dp[2] = 3

Maximum of both cases = max(4, 3) = 4
```

## Edge Cases and Optimizations

- **Empty Array**: If the array is empty, return 0.
- **Single House**: If there's only one house, return its value.
- **Two Houses**: If there are only two houses, return the maximum of their values.
- **Space Optimization**: Instead of using an array to store all intermediate results, we can use two variables to track the previous two results, reducing the space complexity to O(1).

## Common Mistakes

1. **Not handling edge cases**: Make sure to handle empty arrays, single houses, and two houses correctly.
2. **Forgetting the circular arrangement**: Remember that the first and last houses are adjacent in a circle, so you can't rob both of them.
3. **Incorrect subarray slicing**: Be careful with the indices when creating the two linear subarrays.

## Related Problems

1. **House Robber**: The original problem without the circular arrangement.
2. **House Robber III**: A variation where the houses are arranged in a binary tree.
3. **Paint House**: Similar dynamic programming problem where you need to paint houses with different colors at different costs.
4. **Delete and Earn**: A problem that can be transformed into a House Robber-like problem. 