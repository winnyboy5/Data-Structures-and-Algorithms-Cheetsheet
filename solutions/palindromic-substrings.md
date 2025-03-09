# Palindromic Substrings

## Problem Statement

Given a string `s`, return the number of palindromic substrings in it.

A string is a palindrome when it reads the same backward as forward.

A substring is a contiguous sequence of characters within the string.

**Example 1:**
```
Input: s = "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c"
```

**Example 2:**
```
Input: s = "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa"
```

## Intuition

A palindrome is a string that reads the same backward as forward. To count all palindromic substrings, we need to check each possible substring of the given string.

The key insight is that we can expand around centers. For a palindrome, there are two types of centers:
1. A single character (for odd-length palindromes)
2. The space between two characters (for even-length palindromes)

For a string of length n, there are 2n-1 such centers: n centers for single characters and n-1 centers for spaces between characters.

For each center, we can expand outward and check if the characters on both sides match. If they do, we've found a palindrome. We continue expanding until we reach the boundaries of the string or find characters that don't match.

## Visual Explanation

Let's visualize the solution with Example 1: `s = "abc"`

```
String: "abc"
Length: 3
Number of centers: 2*3-1 = 5

Centers:
1. Character 'a' (index 0)
2. Between 'a' and 'b' (indices 0-1)
3. Character 'b' (index 1)
4. Between 'b' and 'c' (indices 1-2)
5. Character 'c' (index 2)

For center 1 (character 'a'):
  - Palindrome: "a" (length 1)
  - Count: 1

For center 2 (between 'a' and 'b'):
  - No palindromes (characters don't match)
  - Count: 0

For center 3 (character 'b'):
  - Palindrome: "b" (length 1)
  - Count: 1

For center 4 (between 'b' and 'c'):
  - No palindromes (characters don't match)
  - Count: 0

For center 5 (character 'c'):
  - Palindrome: "c" (length 1)
  - Count: 1

Total count: 1 + 0 + 1 + 0 + 1 = 3
```

Now let's visualize Example 2: `s = "aaa"`

```
String: "aaa"
Length: 3
Number of centers: 2*3-1 = 5

Centers:
1. Character 'a' (index 0)
2. Between 'a' and 'a' (indices 0-1)
3. Character 'a' (index 1)
4. Between 'a' and 'a' (indices 1-2)
5. Character 'a' (index 2)

For center 1 (character 'a'):
  - Palindrome: "a" (length 1)
  - Count: 1

For center 2 (between 'a' and 'a'):
  - Palindrome: "aa" (length 2)
  - Count: 1

For center 3 (character 'a'):
  - Palindrome: "a" (length 1)
  - Expand: "aaa" (length 3)
  - Count: 2

For center 4 (between 'a' and 'a'):
  - Palindrome: "aa" (length 2)
  - Count: 1

For center 5 (character 'a'):
  - Palindrome: "a" (length 1)
  - Count: 1

Total count: 1 + 1 + 2 + 1 + 1 = 6
```

![Palindromic Substrings Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize a counter to 0.
2. For each center position in the string:
   - Expand around the center for odd-length palindromes (single character center).
   - Expand around the center for even-length palindromes (between two characters).
   - Increment the counter for each palindrome found.
3. Return the counter.

## Code Implementation

### Python

```python
def countSubstrings(s):
    count = 0
    n = len(s)
    
    # Helper function to count palindromes by expanding around center
    def expand_around_center(left, right):
        count = 0
        while left >= 0 and right < n and s[left] == s[right]:
            count += 1
            left -= 1
            right += 1
        return count
    
    # Count palindromes for each center
    for i in range(n):
        # Odd-length palindromes (single character center)
        count += expand_around_center(i, i)
        
        # Even-length palindromes (between two characters)
        count += expand_around_center(i, i + 1)
    
    return count
```

### JavaScript

```javascript
function countSubstrings(s) {
    let count = 0;
    const n = s.length;
    
    // Helper function to count palindromes by expanding around center
    function expandAroundCenter(left, right) {
        let count = 0;
        while (left >= 0 && right < n && s[left] === s[right]) {
            count++;
            left--;
            right++;
        }
        return count;
    }
    
    // Count palindromes for each center
    for (let i = 0; i < n; i++) {
        // Odd-length palindromes (single character center)
        count += expandAroundCenter(i, i);
        
        // Even-length palindromes (between two characters)
        count += expandAroundCenter(i, i + 1);
    }
    
    return count;
}
```

## Alternative Approach: Dynamic Programming

We can also solve this problem using dynamic programming. We'll create a 2D boolean array `dp` where `dp[i][j]` represents whether the substring from index `i` to index `j` is a palindrome.

### Python (DP)

```python
def countSubstrings(s):
    n = len(s)
    count = 0
    
    # Initialize dp array
    dp = [[False] * n for _ in range(n)]
    
    # All single characters are palindromes
    for i in range(n):
        dp[i][i] = True
        count += 1
    
    # Check for palindromes of length 2
    for i in range(n - 1):
        if s[i] == s[i + 1]:
            dp[i][i + 1] = True
            count += 1
    
    # Check for palindromes of length 3 or more
    for length in range(3, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            if s[i] == s[j] and dp[i + 1][j - 1]:
                dp[i][j] = True
                count += 1
    
    return count
```

### JavaScript (DP)

```javascript
function countSubstrings(s) {
    const n = s.length;
    let count = 0;
    
    // Initialize dp array
    const dp = Array(n).fill().map(() => Array(n).fill(false));
    
    // All single characters are palindromes
    for (let i = 0; i < n; i++) {
        dp[i][i] = true;
        count++;
    }
    
    // Check for palindromes of length 2
    for (let i = 0; i < n - 1; i++) {
        if (s[i] === s[i + 1]) {
            dp[i][i + 1] = true;
            count++;
        }
    }
    
    // Check for palindromes of length 3 or more
    for (let length = 3; length <= n; length++) {
        for (let i = 0; i <= n - length; i++) {
            const j = i + length - 1;
            if (s[i] === s[j] && dp[i + 1][j - 1]) {
                dp[i][j] = true;
                count++;
            }
        }
    }
    
    return count;
}
```

## Time and Space Complexity

### Expand Around Center Approach:
- **Time Complexity**: O(n²), where n is the length of the string. We have n centers and for each center, we might expand up to n times.
- **Space Complexity**: O(1), as we only use a constant amount of extra space.

### Dynamic Programming Approach:
- **Time Complexity**: O(n²), where n is the length of the string. We fill a 2D array of size n×n.
- **Space Complexity**: O(n²) for the dp array.

## Detailed Walkthrough with Example

Let's trace through the expand around center approach with Example 2: `s = "aaa"`

```
Initialize count = 0

For center at index 0 (character 'a'):
  - Expand around (0, 0):
    - s[0] = s[0] = 'a', so it's a palindrome
    - count = 1
    - Try to expand to (-1, 1), but -1 is out of bounds
  - Expand around (0, 1):
    - s[0] = s[1] = 'a', so it's a palindrome
    - count = 2
    - Try to expand to (-1, 2), but -1 is out of bounds

For center at index 1 (character 'a'):
  - Expand around (1, 1):
    - s[1] = s[1] = 'a', so it's a palindrome
    - count = 3
    - Try to expand to (0, 2):
      - s[0] = s[2] = 'a', so it's a palindrome
      - count = 4
      - Try to expand to (-1, 3), but -1 and 3 are out of bounds
  - Expand around (1, 2):
    - s[1] = s[2] = 'a', so it's a palindrome
    - count = 5
    - Try to expand to (0, 3), but 3 is out of bounds

For center at index 2 (character 'a'):
  - Expand around (2, 2):
    - s[2] = s[2] = 'a', so it's a palindrome
    - count = 6
    - Try to expand to (1, 3), but 3 is out of bounds
  - Expand around (2, 3), but 3 is out of bounds

Result: count = 6
```

## Edge Cases and Optimizations

- **Empty String**: An empty string has 0 palindromic substrings.
- **Single Character**: A string with a single character has 1 palindromic substring (the character itself).
- **All Same Characters**: A string with all the same characters has n(n+1)/2 palindromic substrings, where n is the length of the string.

**Optimization**: In the expand around center approach, we can break the expansion early if we know that the remaining substring cannot be a palindrome. For example, if s[left] != s[right], we can stop expanding.

## Common Mistakes

1. **Not considering all centers**: Remember that there are 2n-1 centers, not just n.
2. **Off-by-one errors**: Be careful with the indices when expanding around centers, especially for even-length palindromes.
3. **Not handling edge cases**: Make sure to handle empty strings and single-character strings correctly.

## Related Problems

1. **Longest Palindromic Substring**: Find the longest palindromic substring in a given string.
2. **Longest Palindromic Subsequence**: Find the longest subsequence of a string that is also a palindrome.
3. **Palindrome Partitioning**: Partition a string such that every substring is a palindrome.
4. **Shortest Palindrome**: Find the shortest palindrome that can be formed by adding characters to the beginning of a string. 