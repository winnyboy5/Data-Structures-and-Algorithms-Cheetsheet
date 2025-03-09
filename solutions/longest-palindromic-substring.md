# Longest Palindromic Substring

## Problem Statement

Given a string `s`, return the longest palindromic substring in `s`.

A **palindrome** is a string that reads the same backward as forward, e.g., "madam" or "racecar".

**Example 1:**
```
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
```

**Example 2:**
```
Input: s = "cbbd"
Output: "bb"
```

**Example 3:**
```
Input: s = "a"
Output: "a"
```

**Example 4:**
```
Input: s = "ac"
Output: "a"
Explanation: "a" and "c" are both valid answers.
```

## Intuition

The key insight for solving this problem is to recognize that a palindrome mirrors around its center. For a palindrome, there are two cases to consider:
1. Odd-length palindromes (like "racecar") have a single character at the center.
2. Even-length palindromes (like "abba") have two characters at the center.

We can use this property to expand around each potential center in the string and find the longest palindrome. There are 2n-1 potential centers: n single characters (for odd-length palindromes) and n-1 pairs of adjacent characters (for even-length palindromes).

## Visual Explanation

Let's visualize the approach with the example `s = "babad"`:

```
String: "babad"
         01234  (indices)

Step 1: Expand around each center

Center at index 0 (character 'b'):
  - Expand: "b" (palindrome of length 1)

Center between indices 0 and 1 (between 'b' and 'a'):
  - No palindrome (characters don't match)

Center at index 1 (character 'a'):
  - Expand: "a" (palindrome of length 1)
  - Expand further: "bab" (palindrome of length 3)

Center between indices 1 and 2 (between 'a' and 'b'):
  - No palindrome (characters don't match)

Center at index 2 (character 'b'):
  - Expand: "b" (palindrome of length 1)
  - Expand further: "aba" (palindrome of length 3)

Center between indices 2 and 3 (between 'b' and 'a'):
  - No palindrome (characters don't match)

Center at index 3 (character 'a'):
  - Expand: "a" (palindrome of length 1)

Center between indices 3 and 4 (between 'a' and 'd'):
  - No palindrome (characters don't match)

Center at index 4 (character 'd'):
  - Expand: "d" (palindrome of length 1)

Step 2: Find the longest palindrome
  - "bab" and "aba" are both of length 3, which is the maximum.
  - Return either one (let's say "bab").
```

![Longest Palindromic Substring Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

### Expand Around Center Approach:

1. Initialize variables to track the start index and length of the longest palindrome found so far.
2. Iterate through each potential center in the string (2n-1 centers):
   - For each center, expand outward as long as the characters on both sides match.
   - Keep track of the longest palindrome found.
3. Return the longest palindrome substring.

### Dynamic Programming Approach (Alternative):

1. Create a 2D boolean table `dp` where `dp[i][j]` is `true` if the substring from index `i` to `j` is a palindrome.
2. Initialize all single characters as palindromes (`dp[i][i] = true`).
3. Check for palindromes of length 2 (`dp[i][i+1] = (s[i] == s[i+1])`).
4. For longer palindromes, use the recurrence relation: `dp[i][j] = (s[i] == s[j]) && dp[i+1][j-1]`.
5. Keep track of the longest palindrome found.
6. Return the longest palindrome substring.

## Code Implementation

### Python (Expand Around Center)

```python
def longestPalindrome(s):
    if not s:
        return ""
    
    start = 0
    max_length = 1
    
    def expand_around_center(left, right):
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1
            right += 1
        return right - left - 1
    
    for i in range(len(s)):
        # Expand around center for odd-length palindromes
        len1 = expand_around_center(i, i)
        # Expand around center for even-length palindromes
        len2 = expand_around_center(i, i + 1)
        
        # Find the maximum length
        length = max(len1, len2)
        
        # Update the longest palindrome if needed
        if length > max_length:
            max_length = length
            # Calculate the starting index of the palindrome
            start = i - (length - 1) // 2
    
    return s[start:start + max_length]
```

### Python (Dynamic Programming)

```python
def longestPalindrome(s):
    if not s:
        return ""
    
    n = len(s)
    # Initialize a table to store palindrome information
    dp = [[False for _ in range(n)] for _ in range(n)]
    
    # All substrings of length 1 are palindromes
    for i in range(n):
        dp[i][i] = True
    
    start = 0
    max_length = 1
    
    # Check for substrings of length 2
    for i in range(n - 1):
        if s[i] == s[i + 1]:
            dp[i][i + 1] = True
            start = i
            max_length = 2
    
    # Check for substrings of length 3 or more
    for length in range(3, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1  # ending index
            
            # Check if the substring from i to j is a palindrome
            if s[i] == s[j] and dp[i + 1][j - 1]:
                dp[i][j] = True
                
                if length > max_length:
                    start = i
                    max_length = length
    
    return s[start:start + max_length]
```

### JavaScript (Expand Around Center)

```javascript
function longestPalindrome(s) {
    if (!s || s.length === 0) {
        return "";
    }
    
    let start = 0;
    let maxLength = 1;
    
    function expandAroundCenter(left, right) {
        while (left >= 0 && right < s.length && s[left] === s[right]) {
            left--;
            right++;
        }
        return right - left - 1;
    }
    
    for (let i = 0; i < s.length; i++) {
        // Expand around center for odd-length palindromes
        const len1 = expandAroundCenter(i, i);
        // Expand around center for even-length palindromes
        const len2 = expandAroundCenter(i, i + 1);
        
        // Find the maximum length
        const length = Math.max(len1, len2);
        
        // Update the longest palindrome if needed
        if (length > maxLength) {
            maxLength = length;
            // Calculate the starting index of the palindrome
            start = i - Math.floor((length - 1) / 2);
        }
    }
    
    return s.substring(start, start + maxLength);
}
```

### JavaScript (Dynamic Programming)

```javascript
function longestPalindrome(s) {
    if (!s || s.length === 0) {
        return "";
    }
    
    const n = s.length;
    // Initialize a table to store palindrome information
    const dp = Array(n).fill().map(() => Array(n).fill(false));
    
    // All substrings of length 1 are palindromes
    for (let i = 0; i < n; i++) {
        dp[i][i] = true;
    }
    
    let start = 0;
    let maxLength = 1;
    
    // Check for substrings of length 2
    for (let i = 0; i < n - 1; i++) {
        if (s[i] === s[i + 1]) {
            dp[i][i + 1] = true;
            start = i;
            maxLength = 2;
        }
    }
    
    // Check for substrings of length 3 or more
    for (let length = 3; length <= n; length++) {
        for (let i = 0; i <= n - length; i++) {
            const j = i + length - 1;  // ending index
            
            // Check if the substring from i to j is a palindrome
            if (s[i] === s[j] && dp[i + 1][j - 1]) {
                dp[i][j] = true;
                
                if (length > maxLength) {
                    start = i;
                    maxLength = length;
                }
            }
        }
    }
    
    return s.substring(start, start + maxLength);
}
```

## Time and Space Complexity

### Expand Around Center Approach:
- **Time Complexity**: O(n²), where n is the length of the string. For each of the 2n-1 centers, we might expand up to n times.
- **Space Complexity**: O(1), as we only use a constant amount of extra space.

### Dynamic Programming Approach:
- **Time Complexity**: O(n²), where n is the length of the string. We fill an n×n table.
- **Space Complexity**: O(n²), for the dp table.

## Detailed Walkthrough with Example

Let's trace through the "Expand Around Center" algorithm with the example `s = "cbbd"`:

```
String: "cbbd"
         0123  (indices)

Initialize: start = 0, max_length = 1

Iteration 1 (i = 0, character 'c'):
  - Expand around 'c' (odd-length): "c" (length 1)
  - Expand around 'c' and 'b' (even-length): No palindrome (characters don't match)
  - Current max: "c" (length 1)

Iteration 2 (i = 1, character 'b'):
  - Expand around 'b' (odd-length): "b" (length 1)
  - Expand around 'b' and 'b' (even-length): "bb" (length 2)
  - Current max: "bb" (length 2, start = 1)

Iteration 3 (i = 2, character 'b'):
  - Expand around 'b' (odd-length): "b" (length 1)
  - Expand around 'b' and 'd' (even-length): No palindrome (characters don't match)
  - Current max: "bb" (length 2, start = 1)

Iteration 4 (i = 3, character 'd'):
  - Expand around 'd' (odd-length): "d" (length 1)
  - Expand around 'd' and beyond (even-length): Out of bounds
  - Current max: "bb" (length 2, start = 1)

Result: s[1:3] = "bb"
```

## Manacher's Algorithm (Advanced)

For an O(n) solution, we can use Manacher's algorithm, which is specifically designed for finding the longest palindromic substring. The algorithm uses the symmetry property of palindromes to avoid redundant comparisons.

```python
def longestPalindrome(s):
    # Transform the string to handle both odd and even length palindromes
    t = '#' + '#'.join(s) + '#'
    n = len(t)
    p = [0] * n  # p[i] = radius of palindrome centered at i
    
    center = right = 0
    for i in range(n):
        # Mirror of i with respect to center
        mirror = 2 * center - i
        
        # If i is within the right boundary, use the mirror value
        if i < right:
            p[i] = min(right - i, p[mirror])
        
        # Expand around center i
        while i + p[i] + 1 < n and i - p[i] - 1 >= 0 and t[i + p[i] + 1] == t[i - p[i] - 1]:
            p[i] += 1
        
        # Update center and right boundary if needed
        if i + p[i] > right:
            center = i
            right = i + p[i]
    
    # Find the maximum palindrome length and its center
    max_len, center_idx = max((p[i], i) for i in range(n))
    
    # Extract the palindrome from the transformed string
    start = (center_idx - max_len) // 2
    return s[start:start + max_len]
```

Manacher's algorithm has a time complexity of O(n) and a space complexity of O(n).

## Edge Cases and Optimizations

- **Empty String**: Return an empty string.
- **Single Character**: Return the character itself (it's a palindrome).
- **No Palindrome Longer Than 1**: Return any single character.
- **All Characters Same**: The entire string is a palindrome.

**Optimization**: For the "Expand Around Center" approach, we can stop expanding once we find a palindrome longer than half the string length, as no longer palindrome can exist.

## Common Mistakes

1. **Not handling both odd and even length palindromes**: Make sure to check for both cases.
2. **Incorrect boundary checks**: Be careful with the boundary checks when expanding around centers.
3. **Off-by-one errors**: Be precise with the indices when calculating the start and length of palindromes.
4. **Inefficient implementation**: The brute force approach (checking all substrings) is O(n³) and should be avoided.

## Related Problems

1. **Palindromic Substrings**: Count the number of palindromic substrings in a string.
2. **Longest Palindromic Subsequence**: Find the longest subsequence that is a palindrome.
3. **Shortest Palindrome**: Find the shortest palindrome that can be formed by adding characters to the beginning of a string.
4. **Palindrome Partitioning**: Partition a string such that every substring is a palindrome.
5. **Manacher's Algorithm**: Study this algorithm for an O(n) solution to find the longest palindromic substring. 