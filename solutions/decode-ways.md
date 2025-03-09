# Decode Ways

## Problem Statement

A message containing letters from A-Z can be encoded into numbers using the following mapping:

```
'A' -> "1"
'B' -> "2"
...
'Z' -> "26"
```

To decode an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, "11106" can be mapped into:

- "AAJF" with the grouping (1 1 10 6)
- "KJF" with the grouping (11 10 6)

Note that the grouping (1 11 06) is invalid because "06" cannot be mapped into 'F' since "6" is different from "06".

Given a string `s` containing only digits, return the number of ways to decode it.

**Example 1:**
```
Input: s = "12"
Output: 2
Explanation: "12" could be decoded as "AB" (1 2) or "L" (12).
```

**Example 2:**
```
Input: s = "226"
Output: 3
Explanation: "226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```

**Example 3:**
```
Input: s = "06"
Output: 0
Explanation: "06" cannot be mapped to "F" because of the leading zero ("6" is different from "06").
```

## Intuition

This problem can be solved using dynamic programming. The key insight is to recognize that the number of ways to decode a string up to a certain position depends on the number of ways to decode the string up to the previous positions.

For each position in the string, we have two possible decoding options:
1. Decode the current digit as a single character (if it's not '0')
2. Decode the current digit and the previous digit together as a single character (if they form a valid two-digit number between 10 and 26)

By building up the solution from smaller subproblems, we can find the total number of ways to decode the entire string.

## Visual Explanation

Let's visualize the solution with Example 2: `s = "226"`

```
s = "2 2 6"
    ^ ^ ^
    | | |
    i=0 i=1 i=2

Initialize dp array: dp[0] = 1 (empty string has 1 way to decode)
                    dp[1] = 1 (since s[0] = '2' is valid)

For i = 1 (character '2'):
  - Single digit: '2' is valid, so dp[2] += dp[1] = 1
  - Two digits: "22" is valid (22 <= 26), so dp[2] += dp[0] = 1 + 1 = 2
  - dp[2] = 2

For i = 2 (character '6'):
  - Single digit: '6' is valid, so dp[3] += dp[2] = 2
  - Two digits: "26" is valid (26 <= 26), so dp[3] += dp[1] = 2 + 1 = 3
  - dp[3] = 3

Result: dp[3] = 3
```

Let's also visualize Example 3: `s = "06"`

```
s = "0 6"
    ^ ^
    | |
    i=0 i=1

Initialize dp array: dp[0] = 1 (empty string has 1 way to decode)
                    dp[1] = 0 (since s[0] = '0' is not valid)

For i = 1 (character '6'):
  - Single digit: '6' is valid, but dp[1] = 0, so dp[2] += 0 = 0
  - Two digits: "06" is not valid (leading zero), so dp[2] += 0 = 0
  - dp[2] = 0

Result: dp[2] = 0
```

![Decode Ways Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize a dynamic programming array `dp` where `dp[i]` represents the number of ways to decode the substring `s[0...i-1]`.
2. Set `dp[0] = 1` (the empty string has one way to decode - as an empty string).
3. For `i = 1` to `n`:
   - If the current digit `s[i-1]` is not '0', add `dp[i-1]` to `dp[i]`.
   - If the last two digits form a valid number (10-26), add `dp[i-2]` to `dp[i]`.
4. Return `dp[n]`, which represents the number of ways to decode the entire string.

## Code Implementation

### Python

```python
def numDecodings(s):
    if not s or s[0] == '0':
        return 0
    
    n = len(s)
    dp = [0] * (n + 1)
    dp[0] = 1  # Empty string has 1 way to decode
    
    for i in range(1, n + 1):
        # Check if the current digit is valid (not '0')
        if s[i-1] != '0':
            dp[i] += dp[i-1]
        
        # Check if the last two digits form a valid number (10-26)
        if i > 1 and '10' <= s[i-2:i] <= '26':
            dp[i] += dp[i-2]
    
    return dp[n]
```

### Python (Space-Optimized)

```python
def numDecodings(s):
    if not s or s[0] == '0':
        return 0
    
    n = len(s)
    # Initialize variables to track the previous two states
    prev1 = 1  # dp[i-1]
    prev2 = 1  # dp[i-2]
    
    for i in range(1, n + 1):
        current = 0
        
        # Check if the current digit is valid (not '0')
        if s[i-1] != '0':
            current += prev1
        
        # Check if the last two digits form a valid number (10-26)
        if i > 1 and '10' <= s[i-2:i] <= '26':
            current += prev2
        
        # Update previous states
        prev2 = prev1
        prev1 = current
    
    return prev1
```

### JavaScript

```javascript
function numDecodings(s) {
    if (!s || s.length === 0 || s[0] === '0') {
        return 0;
    }
    
    const n = s.length;
    const dp = new Array(n + 1).fill(0);
    dp[0] = 1;  // Empty string has 1 way to decode
    
    for (let i = 1; i <= n; i++) {
        // Check if the current digit is valid (not '0')
        if (s[i-1] !== '0') {
            dp[i] += dp[i-1];
        }
        
        // Check if the last two digits form a valid number (10-26)
        if (i > 1) {
            const twoDigit = parseInt(s.substring(i-2, i));
            if (twoDigit >= 10 && twoDigit <= 26) {
                dp[i] += dp[i-2];
            }
        }
    }
    
    return dp[n];
}
```

### JavaScript (Space-Optimized)

```javascript
function numDecodings(s) {
    if (!s || s.length === 0 || s[0] === '0') {
        return 0;
    }
    
    const n = s.length;
    // Initialize variables to track the previous two states
    let prev1 = 1;  // dp[i-1]
    let prev2 = 1;  // dp[i-2]
    
    for (let i = 1; i <= n; i++) {
        let current = 0;
        
        // Check if the current digit is valid (not '0')
        if (s[i-1] !== '0') {
            current += prev1;
        }
        
        // Check if the last two digits form a valid number (10-26)
        if (i > 1) {
            const twoDigit = parseInt(s.substring(i-2, i));
            if (twoDigit >= 10 && twoDigit <= 26) {
                current += prev2;
            }
        }
        
        // Update previous states
        prev2 = prev1;
        prev1 = current;
    }
    
    return prev1;
}
```

## Time and Space Complexity

### Time Complexity
- **O(n)**, where n is the length of the string. We iterate through the string once.

### Space Complexity
- **O(n)** for the dynamic programming approach, as we use an array of size n+1 to store intermediate results.
- **O(1)** for the space-optimized approach, as we only use a constant amount of extra space.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2: `s = "226"`

```
Initialize:
- dp[0] = 1 (empty string has 1 way to decode)
- dp[1], dp[2], dp[3] = 0, 0, 0

For i = 1 (character '2'):
- s[0] = '2' is not '0', so dp[1] += dp[0] = 1
- No two digits to check yet
- dp[1] = 1

For i = 2 (character '2'):
- s[1] = '2' is not '0', so dp[2] += dp[1] = 1
- Last two digits "22" is valid (10 <= 22 <= 26), so dp[2] += dp[0] = 1 + 1 = 2
- dp[2] = 2

For i = 3 (character '6'):
- s[2] = '6' is not '0', so dp[3] += dp[2] = 2
- Last two digits "26" is valid (10 <= 26 <= 26), so dp[3] += dp[1] = 2 + 1 = 3
- dp[3] = 3

Result: dp[3] = 3
```

## Edge Cases and Optimizations

- **Empty String**: If the string is empty, return 0.
- **Leading Zero**: If the string starts with '0', return 0 as it cannot be decoded.
- **Consecutive Zeros**: If there are consecutive zeros or a zero that doesn't form a valid two-digit number with the previous digit, return 0.
- **Space Optimization**: Instead of using an array to store all intermediate results, we can use two variables to track the previous two results, reducing the space complexity to O(1).

## Common Mistakes

1. **Not handling zeros correctly**: A digit '0' cannot be decoded on its own, and it can only be part of a two-digit number if the first digit is '1' or '2'.
2. **Incorrect range for two-digit numbers**: Two-digit numbers must be between 10 and 26 (inclusive) to be valid.
3. **Not initializing dp[0] correctly**: The empty string has one way to decode (as an empty string), so dp[0] should be 1.

## Related Problems

1. **Climbing Stairs**: Similar dynamic programming problem where you can take 1 or 2 steps at a time.
2. **Fibonacci Number**: The solution follows a pattern similar to the Fibonacci sequence.
3. **Unique Paths**: Another dynamic programming problem with multiple ways to reach a destination.
4. **Word Break**: Determine if a string can be segmented into words from a dictionary. 