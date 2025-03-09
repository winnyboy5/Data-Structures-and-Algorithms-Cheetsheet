# Unique Paths

## Problem Statement

A robot is located at the top-left corner of a `m x n` grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

**Example 1:**
```
Input: m = 3, n = 7
Output: 28
```

**Example 2:**
```
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Right -> Down
3. Down -> Down -> Right
```

**Example 3:**
```
Input: m = 7, n = 3
Output: 28
```

**Example 4:**
```
Input: m = 3, n = 3
Output: 6
```

## Intuition

This problem can be solved using dynamic programming. The key insight is that to reach any cell (i, j), the robot can only come from the cell above it (i-1, j) or the cell to its left (i, j-1). Therefore, the number of unique paths to reach cell (i, j) is the sum of the number of unique paths to reach cell (i-1, j) and cell (i, j-1).

For the cells in the first row or first column, there is only one way to reach them (by moving only right or only down, respectively).

## Visual Explanation

Let's visualize the solution with a 3x3 grid:

```
Start -> (0,0) (0,1) (0,2)
         (1,0) (1,1) (1,2)
         (2,0) (2,1) (2,2) <- Finish
```

We'll use a 2D array `dp` where `dp[i][j]` represents the number of unique paths to reach cell (i, j).

```
Initialize:
dp[0][0] = 1 (there's only one way to reach the starting cell)
dp[0][j] = 1 for all j (there's only one way to reach cells in the first row - by moving right)
dp[i][0] = 1 for all i (there's only one way to reach cells in the first column - by moving down)

For cell (1,1):
dp[1][1] = dp[0][1] + dp[1][0] = 1 + 1 = 2

For cell (1,2):
dp[1][2] = dp[0][2] + dp[1][1] = 1 + 2 = 3

For cell (2,1):
dp[2][1] = dp[1][1] + dp[2][0] = 2 + 1 = 3

For cell (2,2):
dp[2][2] = dp[1][2] + dp[2][1] = 3 + 3 = 6

Result: dp[2][2] = 6
```

Here's the filled dp table:

```
dp = [
  [1, 1, 1],
  [1, 2, 3],
  [1, 3, 6]
]
```

![Unique Paths Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Create a 2D array `dp` of size `m x n` where `dp[i][j]` represents the number of unique paths to reach cell (i, j).
2. Initialize the first row and first column of `dp` to 1, as there's only one way to reach any cell in the first row or first column.
3. For each cell (i, j) where i > 0 and j > 0, calculate `dp[i][j] = dp[i-1][j] + dp[i][j-1]`.
4. Return `dp[m-1][n-1]`, which represents the number of unique paths to reach the bottom-right corner.

## Code Implementation

### Python

```python
def uniquePaths(m, n):
    # Create a 2D array filled with 1s
    dp = [[1 for _ in range(n)] for _ in range(m)]
    
    # Fill the dp table
    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = dp[i-1][j] + dp[i][j-1]
    
    return dp[m-1][n-1]
```

### Python (Space-Optimized)

```python
def uniquePaths(m, n):
    # Create a 1D array filled with 1s
    dp = [1] * n
    
    # Fill the dp table
    for i in range(1, m):
        for j in range(1, n):
            dp[j] += dp[j-1]
    
    return dp[n-1]
```

### JavaScript

```javascript
function uniquePaths(m, n) {
    // Create a 2D array filled with 1s
    const dp = Array(m).fill().map(() => Array(n).fill(1));
    
    // Fill the dp table
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            dp[i][j] = dp[i-1][j] + dp[i][j-1];
        }
    }
    
    return dp[m-1][n-1];
}
```

### JavaScript (Space-Optimized)

```javascript
function uniquePaths(m, n) {
    // Create a 1D array filled with 1s
    const dp = Array(n).fill(1);
    
    // Fill the dp table
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            dp[j] += dp[j-1];
        }
    }
    
    return dp[n-1];
}
```

## Mathematical Solution

This problem can also be solved using a mathematical approach. The robot needs to make exactly `m-1` moves down and `n-1` moves right to reach the bottom-right corner. So, the total number of moves is `(m-1) + (n-1) = m+n-2`.

The number of unique paths is the number of ways to choose which of these `m+n-2` moves are down moves (or equivalently, right moves). This is a combination problem, and the answer is:

```
C(m+n-2, m-1) = (m+n-2)! / ((m-1)! * (n-1)!)
```

### Python (Mathematical Solution)

```python
def uniquePaths(m, n):
    from math import comb
    return comb(m+n-2, m-1)
```

### JavaScript (Mathematical Solution)

```javascript
function uniquePaths(m, n) {
    // Helper function to calculate combinations
    function combination(n, k) {
        if (k < 0 || k > n) return 0;
        if (k === 0 || k === n) return 1;
        
        let result = 1;
        for (let i = 1; i <= k; i++) {
            result *= (n - (k - i));
            result /= i;
        }
        
        return result;
    }
    
    return combination(m+n-2, m-1);
}
```

## Time and Space Complexity

### Dynamic Programming Approach:
- **Time Complexity**: O(m * n), as we need to fill a 2D array of size m x n.
- **Space Complexity**: O(m * n) for the 2D array approach, and O(n) for the space-optimized approach.

### Mathematical Approach:
- **Time Complexity**: O(min(m, n)), as we need to calculate the combination.
- **Space Complexity**: O(1), as we only use a constant amount of extra space.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2: `m = 3, n = 2`

```
Initialize dp:
dp = [
  [1, 1],
  [1, ?],
  [1, ?]
]

For cell (1,1):
dp[1][1] = dp[0][1] + dp[1][0] = 1 + 1 = 2

For cell (2,1):
dp[2][1] = dp[1][1] + dp[2][0] = 2 + 1 = 3

Final dp:
dp = [
  [1, 1],
  [1, 2],
  [1, 3]
]

Result: dp[2][1] = 3
```

## Edge Cases and Optimizations

- **Single Row or Column**: If either m or n is 1, there's only one way to reach the bottom-right corner.
- **Space Optimization**: Instead of using a 2D array, we can use a 1D array to store the number of paths for the current row, updating it as we go.
- **Mathematical Solution**: For large values of m and n, the mathematical solution might be more efficient, but be careful about potential overflow issues.

## Common Mistakes

1. **Not initializing the first row and column correctly**: The first row and column should be initialized to 1, as there's only one way to reach any cell in the first row or first column.
2. **Off-by-one errors**: Be careful with the indices, especially when accessing the dp array.
3. **Overflow issues with the mathematical solution**: For large values of m and n, the factorial calculations might overflow. Consider using a more robust approach or libraries that handle large numbers.

## Related Problems

1. **Unique Paths II**: Similar problem but with obstacles in the grid.
2. **Minimum Path Sum**: Find the path with the minimum sum from top-left to bottom-right.
3. **Dungeon Game**: Find the minimum initial health needed to reach the bottom-right corner.
4. **Cherry Pickup**: Collect maximum cherries by making two passes through the grid. 