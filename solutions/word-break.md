# Word Break

## Problem Statement

Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

Note that the same word in the dictionary may be reused multiple times in the segmentation.

**Example 1:**
```
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```

**Example 2:**
```
Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
Note that you are allowed to reuse a dictionary word.
```

**Example 3:**
```
Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```

## Intuition

This problem can be solved using dynamic programming. The key insight is to break down the problem into smaller subproblems:

For each position in the string, we want to determine if the substring from the beginning up to that position can be segmented into dictionary words. If we can reach a position where the entire string has been segmented, then the answer is true.

To solve this, we can use a boolean array `dp` where `dp[i]` represents whether the substring `s[0...i-1]` can be segmented into dictionary words. We initialize `dp[0] = true` because an empty string can always be segmented (it's an empty sequence of words).

Then, for each position `i` in the string, we check if there's a valid word ending at position `i`. If there is, and if the substring before that word can also be segmented (i.e., `dp[j] = true` where `j` is the starting position of the word), then `dp[i] = true`.

## Visual Explanation

Let's visualize the solution with Example 1: `s = "leetcode", wordDict = ["leet", "code"]`

```
Initialize dp[0] = true (empty string can be segmented)
Initialize dp[1...n] = false

For i = 1 (considering substring "l"):
  - Check if any word in wordDict matches the substring ending at position i:
    - "l" is not in wordDict
  dp[1] = false

For i = 2 (considering substring "le"):
  - Check if any word in wordDict matches the substring ending at position i:
    - "le" is not in wordDict
    - "e" is not in wordDict
  dp[2] = false

For i = 3 (considering substring "lee"):
  - Check if any word in wordDict matches the substring ending at position i:
    - "lee" is not in wordDict
    - "ee" is not in wordDict
    - "e" is not in wordDict
  dp[3] = false

For i = 4 (considering substring "leet"):
  - Check if any word in wordDict matches the substring ending at position i:
    - "leet" matches wordDict[0]
    - Check if dp[0] = true (it is)
    - dp[4] = true
  dp[4] = true

For i = 5 (considering substring "leetc"):
  - Check if any word in wordDict matches the substring ending at position i:
    - "leetc" is not in wordDict
    - "eetc" is not in wordDict
    - "etc" is not in wordDict
    - "tc" is not in wordDict
    - "c" is not in wordDict
  dp[5] = false

For i = 6 (considering substring "leetco"):
  - Check if any word in wordDict matches the substring ending at position i:
    - No matches
  dp[6] = false

For i = 7 (considering substring "leetcod"):
  - Check if any word in wordDict matches the substring ending at position i:
    - No matches
  dp[7] = false

For i = 8 (considering substring "leetcode"):
  - Check if any word in wordDict matches the substring ending at position i:
    - "code" matches wordDict[1]
    - Check if dp[4] = true (it is)
    - dp[8] = true
  dp[8] = true

Result: dp[8] = true
```

Here's a visualization of the dp array after each step:

```
dp[0] = true (base case)
dp[1] = false
dp[2] = false
dp[3] = false
dp[4] = true (can segment "leet")
dp[5] = false
dp[6] = false
dp[7] = false
dp[8] = true (can segment "leetcode" as "leet" + "code")
```

![Word Break Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Create a boolean array `dp` of size `n+1`, where `n` is the length of the string `s`.
2. Initialize `dp[0] = true` (an empty string can always be segmented).
3. Initialize `dp[1...n] = false`.
4. For each position `i` from 1 to `n`:
   - For each position `j` from 0 to `i-1`:
     - If `dp[j] = true` and the substring `s[j...i-1]` is in the dictionary:
       - Set `dp[i] = true` and break the inner loop.
5. Return `dp[n]`.

## Code Implementation

### Python

```python
def wordBreak(s, wordDict):
    n = len(s)
    # Convert wordDict to a set for O(1) lookups
    word_set = set(wordDict)
    
    # Initialize dp array
    dp = [False] * (n + 1)
    dp[0] = True  # Empty string can be segmented
    
    # Fill the dp array
    for i in range(1, n + 1):
        for j in range(i):
            if dp[j] and s[j:i] in word_set:
                dp[i] = True
                break
    
    return dp[n]
```

### JavaScript

```javascript
function wordBreak(s, wordDict) {
    const n = s.length;
    // Convert wordDict to a Set for O(1) lookups
    const wordSet = new Set(wordDict);
    
    // Initialize dp array
    const dp = new Array(n + 1).fill(false);
    dp[0] = true;  // Empty string can be segmented
    
    // Fill the dp array
    for (let i = 1; i <= n; i++) {
        for (let j = 0; j < i; j++) {
            if (dp[j] && wordSet.has(s.substring(j, i))) {
                dp[i] = true;
                break;
            }
        }
    }
    
    return dp[n];
}
```

## Optimized Approach

We can optimize the inner loop by only considering substrings that have a length less than or equal to the maximum word length in the dictionary. This can significantly reduce the number of substring checks for large inputs.

### Python (Optimized)

```python
def wordBreak(s, wordDict):
    n = len(s)
    word_set = set(wordDict)
    
    # Find the maximum word length in the dictionary
    max_word_length = max(len(word) for word in wordDict) if wordDict else 0
    
    # Initialize dp array
    dp = [False] * (n + 1)
    dp[0] = True
    
    # Fill the dp array
    for i in range(1, n + 1):
        # Only consider substrings with length <= max_word_length
        for j in range(max(0, i - max_word_length), i):
            if dp[j] and s[j:i] in word_set:
                dp[i] = True
                break
    
    return dp[n]
```

### JavaScript (Optimized)

```javascript
function wordBreak(s, wordDict) {
    const n = s.length;
    const wordSet = new Set(wordDict);
    
    // Find the maximum word length in the dictionary
    const maxWordLength = wordDict.reduce((max, word) => Math.max(max, word.length), 0);
    
    // Initialize dp array
    const dp = new Array(n + 1).fill(false);
    dp[0] = true;
    
    // Fill the dp array
    for (let i = 1; i <= n; i++) {
        // Only consider substrings with length <= maxWordLength
        for (let j = Math.max(0, i - maxWordLength); j < i; j++) {
            if (dp[j] && wordSet.has(s.substring(j, i))) {
                dp[i] = true;
                break;
            }
        }
    }
    
    return dp[n];
}
```

## Time and Space Complexity

### Basic Approach:
- **Time Complexity**: O(n³), where n is the length of the string. We have two nested loops (O(n²)), and in each iteration, we create a substring which takes O(n) time.
- **Space Complexity**: O(n) for the dp array and the word set.

### Optimized Approach:
- **Time Complexity**: O(n * m * k), where n is the length of the string, m is the maximum word length, and k is the average word length. The outer loop runs n times, the inner loop runs at most m times, and creating a substring takes O(k) time.
- **Space Complexity**: O(n) for the dp array and the word set.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 3: `s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]`

```
Initialize dp[0] = true, dp[1...9] = false

For i = 1 (considering substring "c"):
  - Check if any word in wordDict matches the substring ending at position i:
    - "c" is not in wordDict
  dp[1] = false

For i = 2 (considering substring "ca"):
  - Check if any word in wordDict matches the substring ending at position i:
    - "ca" is not in wordDict
  dp[2] = false

For i = 3 (considering substring "cat"):
  - Check if any word in wordDict matches the substring ending at position i:
    - "cat" matches wordDict[4]
    - Check if dp[0] = true (it is)
    - dp[3] = true
  dp[3] = true

For i = 4 (considering substring "cats"):
  - Check if any word in wordDict matches the substring ending at position i:
    - "cats" matches wordDict[0]
    - Check if dp[0] = true (it is)
    - dp[4] = true
  dp[4] = true

For i = 5 (considering substring "catsa"):
  - Check if any word in wordDict matches the substring ending at position i:
    - No matches
  dp[5] = false

For i = 6 (considering substring "catsan"):
  - Check if any word in wordDict matches the substring ending at position i:
    - No matches
  dp[6] = false

For i = 7 (considering substring "catsand"):
  - Check if any word in wordDict matches the substring ending at position i:
    - "and" matches wordDict[3]
    - Check if dp[4] = true (it is)
    - dp[7] = true
  dp[7] = true

For i = 8 (considering substring "catsando"):
  - Check if any word in wordDict matches the substring ending at position i:
    - No matches
  dp[8] = false

For i = 9 (considering substring "catsandog"):
  - Check if any word in wordDict matches the substring ending at position i:
    - "dog" matches wordDict[1]
    - Check if dp[6] = false (it is)
    - dp[9] remains false
  dp[9] = false

Result: dp[9] = false
```

## Edge Cases and Optimizations

- **Empty String**: An empty string can always be segmented (base case).
- **No Dictionary Words**: If the dictionary is empty, the result is false unless the input string is also empty.
- **Single Character Words**: If the dictionary contains single character words, the problem becomes simpler as we can potentially segment the string character by character.
- **Repeated Subproblems**: The dynamic programming approach avoids recalculating the same subproblems, which is a significant optimization over a naive recursive approach.

**Optimization**: As mentioned earlier, we can optimize the inner loop by only considering substrings with length less than or equal to the maximum word length in the dictionary.

## Common Mistakes

1. **Not handling the base case correctly**: Make sure to initialize `dp[0] = true` to handle the case of an empty string.
2. **Inefficient substring checks**: Using a set for the dictionary words allows for O(1) lookups, which is much more efficient than linear search.
3. **Not considering all possible segmentations**: The dynamic programming approach ensures that we consider all possible ways to segment the string.

## Related Problems

1. **Word Break II**: Return all possible word break results.
2. **Palindrome Partitioning**: Partition a string such that every substring is a palindrome.
3. **Concatenated Words**: Find all words that are concatenated from other words in the array.
4. **Maximum Product of Word Lengths**: Find the maximum value of length(word[i]) * length(word[j]) where word[i] and word[j] do not share common letters. 