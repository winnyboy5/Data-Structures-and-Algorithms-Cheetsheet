# House Robber

## Problem Statement

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array `nums` representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

**Example 1:**
```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

**Example 2:**
```
Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
```

## Intuition

The key insight for this problem is to use dynamic programming. At each house, we have two options:
1. Rob the current house and add its value to the maximum amount we could rob from houses before the previous one (since we can't rob adjacent houses).
2. Skip the current house and take the maximum amount we could rob from all previous houses.

We choose the option that gives us the maximum amount of money.

## Visual Explanation

Let's visualize the solution with Example 1: `nums = [1,2,3,1]`

```
Initialize dp array where dp[i] = maximum amount of money we can rob up to house i
dp = [0, 0, 0, 0] (initially)

For house 0 (money = 1):
  - No previous houses to consider
  - dp[0] = 1

For house 1 (money = 2):
  - Option 1: Rob house 1 = 2
  - Option 2: Rob house 0 = 1
  - dp[1] = max(2, 1) = 2

For house 2 (money = 3):
  - Option 1: Rob house 2 + maximum from houses before house 1 = 3 + dp[0] = 3 + 1 = 4
  - Option 2: Skip house 2 and take maximum from previous houses = dp[1] = 2
  - dp[2] = max(4, 2) = 4

For house 3 (money = 1):
  - Option 1: Rob house 3 + maximum from houses before house 2 = 1 + dp[1] = 1 + 2 = 3
  - Option 2: Skip house 3 and take maximum from previous houses = dp[2] = 4
  - dp[3] = max(3, 4) = 4

Final dp array: [1, 2, 4, 4]
The maximum amount of money we can rob is dp[3] = 4.
```

![House Robber Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize a dp array where dp[i] represents the maximum amount of money we can rob up to house i.
2. Base cases:
   - If there are no houses (nums is empty), return 0.
   - If there is only one house, return the amount of money in that house.
3. Initialize dp[0] = nums[0] and dp[1] = max(nums[0], nums[1]) (if there are at least 2 houses).
4. For each house i from 2 to n-1:
   - Calculate dp[i] = max(nums[i] + dp[i-2], dp[i-1])
   - This represents the maximum of:
     - Robbing the current house and adding it to the maximum amount we could rob from houses before the previous one.
     - Skipping the current house and taking the maximum amount we could rob from all previous houses.
5. Return dp[n-1], which is the maximum amount of money we can rob.

## Code Implementation

### Python

```python
def rob(nums):
    n = len(nums)
    
    # Handle edge cases
    if n == 0:
        return 0
    if n == 1:
        return nums[0]
    
    # Initialize dp array
    dp = [0] * n
    dp[0] = nums[0]
    dp[1] = max(nums[0], nums[1])
    
    # Fill dp array
    for i in range(2, n):
        dp[i] = max(nums[i] + dp[i-2], dp[i-1])
    
    return dp[n-1]
```

### Python (Space-Optimized)

```python
def rob(nums):
    n = len(nums)
    
    # Handle edge cases
    if n == 0:
        return 0
    if n == 1:
        return nums[0]
    
    # Initialize variables to store the maximum amount of money we can rob
    # up to the previous house and the house before that
    prev1 = max(nums[0], nums[1])  # Maximum amount up to the previous house
    prev2 = nums[0]  # Maximum amount up to the house before the previous one
    
    # Iterate through the houses starting from the third house
    for i in range(2, n):
        # Calculate the maximum amount of money we can rob up to the current house
        current = max(nums[i] + prev2, prev1)
        
        # Update prev2 and prev1 for the next iteration
        prev2, prev1 = prev1, current
    
    return prev1
```

### JavaScript

```javascript
function rob(nums) {
    const n = nums.length;
    
    // Handle edge cases
    if (n === 0) {
        return 0;
    }
    if (n === 1) {
        return nums[0];
    }
    
    // Initialize dp array
    const dp = new Array(n).fill(0);
    dp[0] = nums[0];
    dp[1] = Math.max(nums[0], nums[1]);
    
    // Fill dp array
    for (let i = 2; i < n; i++) {
        dp[i] = Math.max(nums[i] + dp[i-2], dp[i-1]);
    }
    
    return dp[n-1];
}
```

### JavaScript (Space-Optimized)

```javascript
function rob(nums) {
    const n = nums.length;
    
    // Handle edge cases
    if (n === 0) {
        return 0;
    }
    if (n === 1) {
        return nums[0];
    }
    
    // Initialize variables to store the maximum amount of money we can rob
    // up to the previous house and the house before that
    let prev1 = Math.max(nums[0], nums[1]);  // Maximum amount up to the previous house
    let prev2 = nums[0];  // Maximum amount up to the house before the previous one
    
    // Iterate through the houses starting from the third house
    for (let i = 2; i < n; i++) {
        // Calculate the maximum amount of money we can rob up to the current house
        const current = Math.max(nums[i] + prev2, prev1);
        
        // Update prev2 and prev1 for the next iteration
        prev2 = prev1;
        prev1 = current;
    }
    
    return prev1;
}
```

## Time and Space Complexity

### Standard Approach:
- **Time Complexity**: O(n), where n is the number of houses. We iterate through each house once.
- **Space Complexity**: O(n) for the dp array.

### Space-Optimized Approach:
- **Time Complexity**: O(n), same as the standard approach.
- **Space Complexity**: O(1), as we only use a constant amount of extra space regardless of the input size.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2: `nums = [2,7,9,3,1]`

```
Initialize dp array:
dp = [0, 0, 0, 0, 0] (initially)

For house 0 (money = 2):
  - No previous houses to consider
  - dp[0] = 2

For house 1 (money = 7):
  - Option 1: Rob house 1 = 7
  - Option 2: Rob house 0 = 2
  - dp[1] = max(7, 2) = 7

For house 2 (money = 9):
  - Option 1: Rob house 2 + maximum from houses before house 1 = 9 + dp[0] = 9 + 2 = 11
  - Option 2: Skip house 2 and take maximum from previous houses = dp[1] = 7
  - dp[2] = max(11, 7) = 11

For house 3 (money = 3):
  - Option 1: Rob house 3 + maximum from houses before house 2 = 3 + dp[1] = 3 + 7 = 10
  - Option 2: Skip house 3 and take maximum from previous houses = dp[2] = 11
  - dp[3] = max(10, 11) = 11

For house 4 (money = 1):
  - Option 1: Rob house 4 + maximum from houses before house 3 = 1 + dp[2] = 1 + 11 = 12
  - Option 2: Skip house 4 and take maximum from previous houses = dp[3] = 11
  - dp[4] = max(12, 11) = 12

Final dp array: [2, 7, 11, 11, 12]
The maximum amount of money we can rob is dp[4] = 12.
```

## Edge Cases and Optimizations

- **Empty Array**: If the input array is empty, the function should return 0.
- **Single House**: If there is only one house, the maximum amount of money we can rob is the amount in that house.
- **Two Houses**: If there are only two houses, we can only rob one of them (since they are adjacent), so we choose the one with more money.

**Optimization**: The space-optimized approach reduces the space complexity from O(n) to O(1) by only keeping track of the maximum amount of money we can rob up to the previous two houses, rather than storing the entire dp array.

## Common Mistakes

1. **Not handling edge cases**: Make sure to handle cases where there are 0, 1, or 2 houses.
2. **Incorrect recurrence relation**: The recurrence relation should be dp[i] = max(nums[i] + dp[i-2], dp[i-1]), not dp[i] = max(nums[i] + dp[i-2], nums[i-1] + dp[i-3]).
3. **Off-by-one errors**: Be careful with the indices when accessing the dp array and the nums array.

## Related Problems

1. **House Robber II**: The houses are arranged in a circle, so the first and last houses are adjacent.
2. **House Robber III**: The houses are arranged in a binary tree, and you can't rob adjacent houses.
3. **Delete and Earn**: Given an array of integers, you can perform operations where you choose a value x, delete all occurrences of x, and earn the sum of all x values.
4. **Maximum Alternating Subsequence Sum**: Find the maximum alternating sum of any subsequence of the array. 