# Climbing Stairs

## Problem Statement

You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

**Example 1:**
```
Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```

**Example 2:**
```
Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

## Intuition

This problem is a classic example of dynamic programming. The key insight is to recognize that to reach the nth step, we can either:
1. Take a single step from the (n-1)th step, or
2. Take a double step from the (n-2)th step

Therefore, the total number of ways to reach the nth step is the sum of the ways to reach the (n-1)th step and the ways to reach the (n-2)th step.

This forms a recurrence relation:
```
f(n) = f(n-1) + f(n-2)
```

With base cases:
```
f(1) = 1 (only one way to climb 1 step)
f(2) = 2 (two ways to climb 2 steps: 1+1 or 2)
```

If this pattern looks familiar, it's because it's the Fibonacci sequence!

## Visual Explanation

Let's visualize the solution for n = 5:

```
Step 0 (ground): 1 way to be here (starting point)
Step 1: 1 way to reach (only from step 0)
Step 2: 2 ways to reach (1+1 or 2)
Step 3: 3 ways to reach (1+1+1 or 1+2 or 2+1)
Step 4: 5 ways to reach (1+1+1+1 or 1+1+2 or 1+2+1 or 2+1+1 or 2+2)
Step 5: 8 ways to reach (sum of ways to reach step 3 and step 4)
```

![Climbing Stairs Visualization](https://i.imgur.com/JUDTkUC.png)

We can also visualize this as a decision tree:

```
                  Start
                 /     \
                1       2
               / \     / \
              1   2   1   2
             / \ / \ / \
            1  2 1  2 1  2
           ...
```

Each path from the root to a leaf that sums to n represents a valid way to climb the stairs.

## Step-by-Step Approach

### Dynamic Programming Approach:

1. Create an array `dp` where `dp[i]` represents the number of ways to reach the ith step.
2. Initialize the base cases:
   - `dp[1] = 1` (one way to reach the 1st step)
   - `dp[2] = 2` (two ways to reach the 2nd step)
3. For i from 3 to n, calculate `dp[i] = dp[i-1] + dp[i-2]`.
4. Return `dp[n]`.

### Optimized Approach:

Since we only need the previous two values to calculate the current value, we can optimize the space complexity to O(1) by using just two variables instead of an array.

## Code Implementation

### Python (Dynamic Programming)

```python
def climbStairs(n):
    if n <= 2:
        return n
    
    # Initialize dp array
    dp = [0] * (n + 1)
    dp[1] = 1
    dp[2] = 2
    
    # Fill dp array
    for i in range(3, n + 1):
        dp[i] = dp[i - 1] + dp[i - 2]
    
    return dp[n]
```

### Python (Optimized)

```python
def climbStairs(n):
    if n <= 2:
        return n
    
    # Initialize variables for the first two steps
    one_step_before = 2  # Ways to reach step 2
    two_steps_before = 1  # Ways to reach step 1
    current = 0
    
    # Calculate ways for remaining steps
    for i in range(3, n + 1):
        current = one_step_before + two_steps_before
        two_steps_before = one_step_before
        one_step_before = current
    
    return one_step_before
```

### JavaScript (Dynamic Programming)

```javascript
function climbStairs(n) {
    if (n <= 2) {
        return n;
    }
    
    // Initialize dp array
    const dp = new Array(n + 1).fill(0);
    dp[1] = 1;
    dp[2] = 2;
    
    // Fill dp array
    for (let i = 3; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    
    return dp[n];
}
```

### JavaScript (Optimized)

```javascript
function climbStairs(n) {
    if (n <= 2) {
        return n;
    }
    
    // Initialize variables for the first two steps
    let oneStepBefore = 2;  // Ways to reach step 2
    let twoStepsBefore = 1;  // Ways to reach step 1
    let current = 0;
    
    // Calculate ways for remaining steps
    for (let i = 3; i <= n; i++) {
        current = oneStepBefore + twoStepsBefore;
        twoStepsBefore = oneStepBefore;
        oneStepBefore = current;
    }
    
    return oneStepBefore;
}
```

## Time and Space Complexity

### Dynamic Programming Approach:
- **Time Complexity**: O(n), as we iterate from 3 to n once.
- **Space Complexity**: O(n), for the dp array.

### Optimized Approach:
- **Time Complexity**: O(n), as we iterate from 3 to n once.
- **Space Complexity**: O(1), as we only use a constant amount of extra space.

## Detailed Walkthrough with Example

Let's trace through the optimized algorithm for n = 5:

```
Initialize:
- one_step_before = 2 (ways to reach step 2)
- two_steps_before = 1 (ways to reach step 1)

Iteration 1 (i = 3):
- current = one_step_before + two_steps_before = 2 + 1 = 3
- two_steps_before = one_step_before = 2
- one_step_before = current = 3

Iteration 2 (i = 4):
- current = one_step_before + two_steps_before = 3 + 2 = 5
- two_steps_before = one_step_before = 3
- one_step_before = current = 5

Iteration 3 (i = 5):
- current = one_step_before + two_steps_before = 5 + 3 = 8
- two_steps_before = one_step_before = 5
- one_step_before = current = 8

Return one_step_before = 8
```

So there are 8 distinct ways to climb to the top of a 5-step staircase.

## Mathematical Solution

The solution to this problem is actually the (n+1)th Fibonacci number. We can use the closed-form expression for the Fibonacci sequence:

```
F(n) = (1/√5) * (((1 + √5)/2)^n - ((1 - √5)/2)^n)
```

For this problem, the answer would be F(n+1).

```python
def climbStairs(n):
    sqrt5 = 5 ** 0.5
    phi = (1 + sqrt5) / 2
    psi = (1 - sqrt5) / 2
    return int((phi ** (n + 1) - psi ** (n + 1)) / sqrt5)
```

This gives us O(1) time complexity, but be cautious of potential floating-point precision issues for large values of n.

## Edge Cases and Optimizations

- **n = 0**: The problem statement doesn't explicitly define this, but logically there is 1 way (do nothing).
- **n = 1**: There is 1 way to climb 1 step.
- **n = 2**: There are 2 ways to climb 2 steps.
- **Large n**: For very large values of n, the mathematical solution might face precision issues. The iterative approach is more reliable.

## Common Mistakes

1. **Incorrect base cases**: Make sure to handle the base cases correctly (n = 1 and n = 2).
2. **Off-by-one errors**: Be careful with the indexing, especially if you're using a 0-indexed array.
3. **Overlooking the recurrence relation**: Remember that the number of ways to reach step n is the sum of the ways to reach steps n-1 and n-2.

## Related Problems

1. **Fibonacci Number**: Calculate the nth Fibonacci number.
2. **Min Cost Climbing Stairs**: Find the minimum cost to reach the top of the staircase.
3. **Decode Ways**: Count the number of ways to decode a message.
4. **Unique Paths**: Count the number of unique paths from the top-left to the bottom-right of a grid.
5. **Coin Change 2**: Count the number of ways to make a given amount using a set of coins. 