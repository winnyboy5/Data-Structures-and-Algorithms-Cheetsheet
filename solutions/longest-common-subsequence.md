# Longest Common Subsequence

## Problem Statement

Given two strings `text1` and `text2`, return the length of their longest common subsequence. If there is no common subsequence, return 0.

A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

A common subsequence of two strings is a subsequence that is common to both strings.

**Example 1:**
```
Input: text1 = "abcde", text2 = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.
```

**Example 2:**
```
Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.
```

**Example 3:**
```
Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.
```

## Intuition

The key insight for this problem is to use dynamic programming to build up the solution. We can define a 2D array `dp` where `dp[i][j]` represents the length of the longest common subsequence (LCS) of the substrings `text1[0...i-1]` and `text2[0...j-1]`.

For each pair of characters `text1[i-1]` and `text2[j-1]`, we have two cases:
1. If the characters match, we can extend the LCS found for the substrings `text1[0...i-2]` and `text2[0...j-2]` by 1.
2. If the characters don't match, we take the maximum of the LCS found for either dropping the last character of `text1` or dropping the last character of `text2`.

## Visual Explanation

Let's visualize the solution with Example 1: `text1 = "abcde", text2 = "ace"`

We'll create a 2D dp table where rows represent characters of text1 and columns represent characters of text2:

```
    |   | a | c | e |
----|---|---|---|---|
    | 0 | 0 | 0 | 0 |
----|---|---|---|---|
a   | 0 | 1 | 1 | 1 |
----|---|---|---|---|
b   | 0 | 1 | 1 | 1 |
----|---|---|---|---|
c   | 0 | 1 | 2 | 2 |
----|---|---|---|---|
d   | 0 | 1 | 2 | 2 |
----|---|---|---|---|
e   | 0 | 1 | 2 | 3 |
----|---|---|---|---|
```

Let's walk through how we fill this table:

1. Initialize the first row and column with 0s (representing empty strings).
2. For each cell (i, j):
   - If text1[i-1] == text2[j-1], then dp[i][j] = dp[i-1][j-1] + 1
   - Otherwise, dp[i][j] = max(dp[i-1][j], dp[i][j-1])

For example:
- At cell (1, 1), text1[0] = 'a' and text2[0] = 'a', they match, so dp[1][1] = dp[0][0] + 1 = 1
- At cell (2, 1), text1[1] = 'b' and text2[0] = 'a', they don't match, so dp[2][1] = max(dp[1][1], dp[2][0]) = max(1, 0) = 1
- At cell (3, 2), text1[2] = 'c' and text2[1] = 'c', they match, so dp[3][2] = dp[2][1] + 1 = 2
- And so on...

The final answer is in the bottom-right cell: dp[5][3] = 3, which is the length of the LCS "ace".

![Longest Common Subsequence Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Create a 2D dp array of size (m+1) x (n+1), where m and n are the lengths of text1 and text2, respectively.
2. Initialize the first row and column with 0s (representing empty strings).
3. Iterate through each character of both strings:
   - If the characters match, dp[i][j] = dp[i-1][j-1] + 1
   - If the characters don't match, dp[i][j] = max(dp[i-1][j], dp[i][j-1])
4. Return the value in the bottom-right cell of the dp array.

## Code Implementation

### Python

```python
def longestCommonSubsequence(text1, text2):
    m, n = len(text1), len(text2)
    
    # Create a 2D dp array initialized with 0s
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    # Fill the dp array
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i - 1] == text2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    
    return dp[m][n]
```

### Python (Space-Optimized)

```python
def longestCommonSubsequence(text1, text2):
    m, n = len(text1), len(text2)
    
    # Ensure text1 is the shorter string to optimize space
    if m > n:
        text1, text2 = text2, text1
        m, n = n, m
    
    # We only need two rows of the dp array
    prev = [0] * (n + 1)
    curr = [0] * (n + 1)
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i - 1] == text2[j - 1]:
                curr[j] = prev[j - 1] + 1
            else:
                curr[j] = max(prev[j], curr[j - 1])
        
        # Update prev row for the next iteration
        prev, curr = curr, prev
    
    return prev[n]
```

### JavaScript

```javascript
function longestCommonSubsequence(text1, text2) {
    const m = text1.length;
    const n = text2.length;
    
    // Create a 2D dp array initialized with 0s
    const dp = Array(m + 1).fill().map(() => Array(n + 1).fill(0));
    
    // Fill the dp array
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (text1[i - 1] === text2[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }
    
    return dp[m][n];
}
```

### JavaScript (Space-Optimized)

```javascript
function longestCommonSubsequence(text1, text2) {
    let m = text1.length;
    let n = text2.length;
    
    // Ensure text1 is the shorter string to optimize space
    if (m > n) {
        [text1, text2] = [text2, text1];
        [m, n] = [n, m];
    }
    
    // We only need two rows of the dp array
    let prev = Array(n + 1).fill(0);
    let curr = Array(n + 1).fill(0);
    
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (text1[i - 1] === text2[j - 1]) {
                curr[j] = prev[j - 1] + 1;
            } else {
                curr[j] = Math.max(prev[j], curr[j - 1]);
            }
        }
        
        // Update prev row for the next iteration
        [prev, curr] = [curr, prev];
    }
    
    return prev[n];
}
```

## Time and Space Complexity

### Standard Approach:
- **Time Complexity**: O(m × n), where m and n are the lengths of the two strings. We need to fill a 2D dp array of size (m+1) × (n+1).
- **Space Complexity**: O(m × n) for the 2D dp array.

### Space-Optimized Approach:
- **Time Complexity**: O(m × n), same as the standard approach.
- **Space Complexity**: O(min(m, n)), as we only need to store two rows of the dp array, and we ensure that we're using the shorter string for the rows.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 1: `text1 = "abcde", text2 = "ace"`

```
Initialize dp array:
dp[0][0] = 0, dp[0][1] = 0, dp[0][2] = 0, dp[0][3] = 0
dp[1][0] = 0, dp[2][0] = 0, dp[3][0] = 0, dp[4][0] = 0, dp[5][0] = 0

For i = 1, j = 1:
text1[0] = 'a', text2[0] = 'a', they match
dp[1][1] = dp[0][0] + 1 = 1

For i = 1, j = 2:
text1[0] = 'a', text2[1] = 'c', they don't match
dp[1][2] = max(dp[0][2], dp[1][1]) = max(0, 1) = 1

For i = 1, j = 3:
text1[0] = 'a', text2[2] = 'e', they don't match
dp[1][3] = max(dp[0][3], dp[1][2]) = max(0, 1) = 1

For i = 2, j = 1:
text1[1] = 'b', text2[0] = 'a', they don't match
dp[2][1] = max(dp[1][1], dp[2][0]) = max(1, 0) = 1

For i = 2, j = 2:
text1[1] = 'b', text2[1] = 'c', they don't match
dp[2][2] = max(dp[1][2], dp[2][1]) = max(1, 1) = 1

For i = 2, j = 3:
text1[1] = 'b', text2[2] = 'e', they don't match
dp[2][3] = max(dp[1][3], dp[2][2]) = max(1, 1) = 1

For i = 3, j = 1:
text1[2] = 'c', text2[0] = 'a', they don't match
dp[3][1] = max(dp[2][1], dp[3][0]) = max(1, 0) = 1

For i = 3, j = 2:
text1[2] = 'c', text2[1] = 'c', they match
dp[3][2] = dp[2][1] + 1 = 1 + 1 = 2

For i = 3, j = 3:
text1[2] = 'c', text2[2] = 'e', they don't match
dp[3][3] = max(dp[2][3], dp[3][2]) = max(1, 2) = 2

For i = 4, j = 1:
text1[3] = 'd', text2[0] = 'a', they don't match
dp[4][1] = max(dp[3][1], dp[4][0]) = max(1, 0) = 1

For i = 4, j = 2:
text1[3] = 'd', text2[1] = 'c', they don't match
dp[4][2] = max(dp[3][2], dp[4][1]) = max(2, 1) = 2

For i = 4, j = 3:
text1[3] = 'd', text2[2] = 'e', they don't match
dp[4][3] = max(dp[3][3], dp[4][2]) = max(2, 2) = 2

For i = 5, j = 1:
text1[4] = 'e', text2[0] = 'a', they don't match
dp[5][1] = max(dp[4][1], dp[5][0]) = max(1, 0) = 1

For i = 5, j = 2:
text1[4] = 'e', text2[1] = 'c', they don't match
dp[5][2] = max(dp[4][2], dp[5][1]) = max(2, 1) = 2

For i = 5, j = 3:
text1[4] = 'e', text2[2] = 'e', they match
dp[5][3] = dp[4][2] + 1 = 2 + 1 = 3

Final dp array:
    |   | a | c | e |
----|---|---|---|---|
    | 0 | 0 | 0 | 0 |
----|---|---|---|---|
a   | 0 | 1 | 1 | 1 |
----|---|---|---|---|
b   | 0 | 1 | 1 | 1 |
----|---|---|---|---|
c   | 0 | 1 | 2 | 2 |
----|---|---|---|---|
d   | 0 | 1 | 2 | 2 |
----|---|---|---|---|
e   | 0 | 1 | 2 | 3 |
----|---|---|---|---|

The answer is dp[5][3] = 3, which is the length of the LCS "ace".
```

## Edge Cases and Optimizations

- **Empty Strings**: If either of the strings is empty, the LCS is 0.
- **Same Strings**: If both strings are the same, the LCS is the length of the string.
- **No Common Characters**: If there are no common characters between the strings, the LCS is 0.

**Optimization**: The space-optimized approach reduces the space complexity from O(m × n) to O(min(m, n)) by only storing two rows of the dp array at a time.

## Common Mistakes

1. **Incorrect initialization**: The first row and column of the dp array should be initialized with 0s, representing the LCS of an empty string with any other string.
2. **Off-by-one errors**: When accessing characters in the strings, remember that dp[i][j] represents the LCS of text1[0...i-1] and text2[0...j-1], so the characters to compare are at indices i-1 and j-1.
3. **Confusing subsequence with substring**: A subsequence doesn't need to be contiguous, while a substring must be contiguous.

## Related Problems

1. **Longest Increasing Subsequence**: Find the length of the longest strictly increasing subsequence in an array.
2. **Edit Distance**: Find the minimum number of operations required to convert one string to another.
3. **Shortest Common Supersequence**: Find the shortest string that has both strings as subsequences.
4. **Distinct Subsequences**: Count the number of distinct subsequences of one string that equal another string. 