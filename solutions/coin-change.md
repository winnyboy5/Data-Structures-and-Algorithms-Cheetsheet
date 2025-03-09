# Coin Change

## Problem Statement

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

**Example 1:**
```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```

**Example 2:**
```
Input: coins = [2], amount = 3
Output: -1
```

**Example 3:**
```
Input: coins = [1], amount = 0
Output: 0
```

## Intuition

This is a classic dynamic programming problem. The key insight is to break down the problem into smaller subproblems:

For each amount from 1 to the target amount, we want to find the minimum number of coins needed to make that amount. To do this, we consider each coin denomination and see if using that coin would lead to a better (smaller) solution.

For example, if we're trying to make amount `a` and we have a coin of value `c`, we can use this coin if `a ≥ c`. The number of coins needed would be `1 + dp[a-c]`, where `dp[a-c]` is the minimum number of coins needed to make amount `a-c`.

By considering all possible coins for each amount, we can build up the solution to the original problem.

## Visual Explanation

Let's visualize the solution with Example 1: `coins = [1,2,5], amount = 11`

We'll use a table `dp` where `dp[i]` represents the minimum number of coins needed to make amount `i`.

```
Initialize dp[0] = 0 (it takes 0 coins to make amount 0)
Initialize dp[1...amount] = amount + 1 (a value larger than any possible solution)

For amount = 1:
  - Using coin 1: dp[1] = min(dp[1], dp[1-1] + 1) = min(12, 0+1) = 1
  - Using coin 2: Can't use (2 > 1)
  - Using coin 5: Can't use (5 > 1)
  dp[1] = 1

For amount = 2:
  - Using coin 1: dp[2] = min(dp[2], dp[2-1] + 1) = min(12, 1+1) = 2
  - Using coin 2: dp[2] = min(dp[2], dp[2-2] + 1) = min(2, 0+1) = 1
  - Using coin 5: Can't use (5 > 2)
  dp[2] = 1

For amount = 3:
  - Using coin 1: dp[3] = min(dp[3], dp[3-1] + 1) = min(12, 1+1) = 2
  - Using coin 2: dp[3] = min(dp[3], dp[3-2] + 1) = min(2, 1+1) = 2
  - Using coin 5: Can't use (5 > 3)
  dp[3] = 2

For amount = 4:
  - Using coin 1: dp[4] = min(dp[4], dp[4-1] + 1) = min(12, 2+1) = 3
  - Using coin 2: dp[4] = min(dp[4], dp[4-2] + 1) = min(3, 1+1) = 2
  - Using coin 5: Can't use (5 > 4)
  dp[4] = 2

For amount = 5:
  - Using coin 1: dp[5] = min(dp[5], dp[5-1] + 1) = min(12, 2+1) = 3
  - Using coin 2: dp[5] = min(dp[5], dp[5-2] + 1) = min(3, 2+1) = 3
  - Using coin 5: dp[5] = min(dp[5], dp[5-5] + 1) = min(3, 0+1) = 1
  dp[5] = 1

For amount = 6:
  - Using coin 1: dp[6] = min(dp[6], dp[6-1] + 1) = min(12, 3+1) = 4
  - Using coin 2: dp[6] = min(dp[6], dp[6-2] + 1) = min(4, 1+1) = 2
  - Using coin 5: dp[6] = min(dp[6], dp[6-5] + 1) = min(2, 1+1) = 2
  dp[6] = 2

... (continuing the process)

For amount = 11:
  - Using coin 1: dp[11] = min(dp[11], dp[11-1] + 1) = min(12, 3+1) = 4
  - Using coin 2: dp[11] = min(dp[11], dp[11-2] + 1) = min(4, 3+1) = 4
  - Using coin 5: dp[11] = min(dp[11], dp[11-5] + 1) = min(4, 2+1) = 3
  dp[11] = 3

Result: dp[11] = 3
```

Here's a visualization of the dp array after each step:

```
dp[0] = 0 (base case)
dp[1] = 1 (1)
dp[2] = 1 (2)
dp[3] = 2 (1+2)
dp[4] = 2 (2+2)
dp[5] = 1 (5)
dp[6] = 2 (1+5 or 2+2+2)
dp[7] = 2 (2+5)
dp[8] = 3 (1+2+5)
dp[9] = 3 (2+2+5)
dp[10] = 2 (5+5)
dp[11] = 3 (1+5+5)
```

![Coin Change Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Create a dp array of size `amount + 1` to store the minimum number of coins needed for each amount.
2. Initialize `dp[0] = 0` (it takes 0 coins to make amount 0).
3. Initialize `dp[1...amount] = amount + 1` (a value larger than any possible solution).
4. For each amount from 1 to the target amount:
   - For each coin denomination:
     - If the coin value is less than or equal to the current amount:
       - Update `dp[amount]` to be the minimum of its current value and `dp[amount - coin] + 1`.
5. If `dp[amount]` is still `amount + 1`, return `-1` (it's not possible to make the amount).
6. Otherwise, return `dp[amount]`.

## Code Implementation

### Python

```python
def coinChange(coins, amount):
    # Initialize dp array with a value larger than any possible solution
    dp = [amount + 1] * (amount + 1)
    
    # Base case: it takes 0 coins to make amount 0
    dp[0] = 0
    
    # Fill the dp array
    for i in range(1, amount + 1):
        for coin in coins:
            if coin <= i:
                dp[i] = min(dp[i], dp[i - coin] + 1)
    
    # If dp[amount] is still amount + 1, it's not possible to make the amount
    return dp[amount] if dp[amount] <= amount else -1
```

### JavaScript

```javascript
function coinChange(coins, amount) {
    // Initialize dp array with a value larger than any possible solution
    const dp = new Array(amount + 1).fill(amount + 1);
    
    // Base case: it takes 0 coins to make amount 0
    dp[0] = 0;
    
    // Fill the dp array
    for (let i = 1; i <= amount; i++) {
        for (const coin of coins) {
            if (coin <= i) {
                dp[i] = Math.min(dp[i], dp[i - coin] + 1);
            }
        }
    }
    
    // If dp[amount] is still amount + 1, it's not possible to make the amount
    return dp[amount] <= amount ? dp[amount] : -1;
}
```

## Time and Space Complexity

- **Time Complexity**: O(amount * n), where n is the number of coin denominations. We have two nested loops: one iterating through amounts from 1 to the target amount, and the other iterating through all coin denominations.
- **Space Complexity**: O(amount), as we use a dp array of size `amount + 1`.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2: `coins = [2], amount = 3`

```
Initialize dp[0] = 0, dp[1...3] = 4

For amount = 1:
  - Using coin 2: Can't use (2 > 1)
  dp[1] = 4

For amount = 2:
  - Using coin 2: dp[2] = min(dp[2], dp[2-2] + 1) = min(4, 0+1) = 1
  dp[2] = 1

For amount = 3:
  - Using coin 2: dp[3] = min(dp[3], dp[3-2] + 1) = min(4, 1+1) = 2
  dp[3] = 2

Wait, this doesn't match the expected output of -1. Let's check our reasoning:

With coins = [2], we can make:
- Amount 0 with 0 coins (base case)
- Amount 2 with 1 coin (using one 2-coin)
- Amount 4 with 2 coins (using two 2-coins)
- Amount 6 with 3 coins (using three 2-coins)
- ...

But we can't make amount 1, 3, 5, etc. with only 2-coins.

The issue is that our algorithm assumes we can always make the amount if dp[amount] <= amount, but this isn't always true. We need to check if dp[amount] was updated from its initial value.

Let's correct our algorithm:
```

### Corrected Python

```python
def coinChange(coins, amount):
    # Initialize dp array with a value larger than any possible solution
    dp = [float('inf')] * (amount + 1)
    
    # Base case: it takes 0 coins to make amount 0
    dp[0] = 0
    
    # Fill the dp array
    for i in range(1, amount + 1):
        for coin in coins:
            if coin <= i:
                dp[i] = min(dp[i], dp[i - coin] + 1)
    
    # If dp[amount] is still infinity, it's not possible to make the amount
    return dp[amount] if dp[amount] != float('inf') else -1
```

### Corrected JavaScript

```javascript
function coinChange(coins, amount) {
    // Initialize dp array with a value larger than any possible solution
    const dp = new Array(amount + 1).fill(Infinity);
    
    // Base case: it takes 0 coins to make amount 0
    dp[0] = 0;
    
    // Fill the dp array
    for (let i = 1; i <= amount; i++) {
        for (const coin of coins) {
            if (coin <= i) {
                dp[i] = Math.min(dp[i], dp[i - coin] + 1);
            }
        }
    }
    
    // If dp[amount] is still Infinity, it's not possible to make the amount
    return dp[amount] !== Infinity ? dp[amount] : -1;
}
```

Now let's trace through the corrected algorithm with Example 2: `coins = [2], amount = 3`

```
Initialize dp[0] = 0, dp[1...3] = Infinity

For amount = 1:
  - Using coin 2: Can't use (2 > 1)
  dp[1] = Infinity

For amount = 2:
  - Using coin 2: dp[2] = min(dp[2], dp[2-2] + 1) = min(Infinity, 0+1) = 1
  dp[2] = 1

For amount = 3:
  - Using coin 2: dp[3] = min(dp[3], dp[3-2] + 1) = min(Infinity, 1+1) = 2
  dp[3] = 2

Final dp array: [0, Infinity, 1, 2]

Result: dp[3] = 2
```

This still doesn't match the expected output of -1. Let's check our reasoning again:

With coins = [2], we can make:
- Amount 0 with 0 coins (base case)
- Amount 2 with 1 coin (using one 2-coin)
- Amount 4 with 2 coins (using two 2-coins)
- ...

But we can't make amount 3 with only 2-coins. The issue is that our algorithm is incorrectly calculating dp[3] based on dp[1], which is Infinity. Let's correct our algorithm once more:

### Corrected Python (Final)

```python
def coinChange(coins, amount):
    # Initialize dp array with a value larger than any possible solution
    dp = [float('inf')] * (amount + 1)
    
    # Base case: it takes 0 coins to make amount 0
    dp[0] = 0
    
    # Fill the dp array
    for i in range(1, amount + 1):
        for coin in coins:
            if coin <= i and dp[i - coin] != float('inf'):
                dp[i] = min(dp[i], dp[i - coin] + 1)
    
    # If dp[amount] is still infinity, it's not possible to make the amount
    return dp[amount] if dp[amount] != float('inf') else -1
```

### Corrected JavaScript (Final)

```javascript
function coinChange(coins, amount) {
    // Initialize dp array with a value larger than any possible solution
    const dp = new Array(amount + 1).fill(Infinity);
    
    // Base case: it takes 0 coins to make amount 0
    dp[0] = 0;
    
    // Fill the dp array
    for (let i = 1; i <= amount; i++) {
        for (const coin of coins) {
            if (coin <= i && dp[i - coin] !== Infinity) {
                dp[i] = Math.min(dp[i], dp[i - coin] + 1);
            }
        }
    }
    
    // If dp[amount] is still Infinity, it's not possible to make the amount
    return dp[amount] !== Infinity ? dp[amount] : -1;
}
```

Actually, the issue is with our tracing, not the algorithm. Let's trace through the original algorithm again with Example 2: `coins = [2], amount = 3`

```
Initialize dp[0] = 0, dp[1...3] = Infinity

For amount = 1:
  - Using coin 2: Can't use (2 > 1)
  dp[1] = Infinity

For amount = 2:
  - Using coin 2: dp[2] = min(dp[2], dp[2-2] + 1) = min(Infinity, 0+1) = 1
  dp[2] = 1

For amount = 3:
  - Using coin 2: Can use, but dp[3-2] = dp[1] = Infinity
  - Since dp[1] = Infinity, dp[3] remains Infinity
  dp[3] = Infinity

Final dp array: [0, Infinity, 1, Infinity]

Result: dp[3] = Infinity, so return -1
```

This matches the expected output of -1. The original algorithm is correct; we just made an error in our tracing.

## Edge Cases and Optimizations

- **Amount = 0**: The minimum number of coins needed is 0.
- **No Solution**: If it's not possible to make the amount with the given coins, return -1.
- **Greedy Approach Doesn't Work**: A common misconception is that a greedy approach (always choosing the largest coin that fits) would work. However, this is not always optimal. For example, with coins = [1, 3, 4] and amount = 6, the greedy approach would give 4+1+1 = 3 coins, but the optimal solution is 3+3 = 2 coins.

**Optimization**: We can optimize the inner loop by only considering coins that are less than or equal to the current amount.

## Common Mistakes

1. **Not handling the case where no solution exists**: Make sure to check if dp[amount] was updated from its initial value.
2. **Using a greedy approach**: As mentioned, a greedy approach doesn't always give the optimal solution for this problem.
3. **Off-by-one errors**: Be careful with the initialization and boundary conditions.

## Related Problems

1. **Coin Change 2**: Count the number of ways to make a given amount using a set of coins.
2. **Minimum Cost For Tickets**: Find the minimum cost to travel on a set of days.
3. **Perfect Squares**: Find the minimum number of perfect square numbers that sum to a given number.
4. **Combination Sum**: Find all unique combinations of candidates that sum to a target. 