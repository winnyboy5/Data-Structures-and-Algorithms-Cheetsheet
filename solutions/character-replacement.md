# Longest Repeating Character Replacement

## Problem Statement

Given a string `s` and an integer `k`, you can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.

Return the length of the longest substring containing the same letter after performing the above operations.

**Example 1:**
```
Input: s = "ABAB", k = 2
Output: 4
Explanation: Replace the two 'A's with two 'B's or vice versa.
```

**Example 2:**
```
Input: s = "AABABBA", k = 1
Output: 4
Explanation: Replace the one 'A' at index 2 with 'B', resulting in "AABBBBA".
The substring "BBBB" has the longest repeating letters, which is 4.
```

## Intuition

The key insight for this problem is to use a sliding window approach. We want to find the longest substring where the number of characters that are not the most frequent character is at most `k`. This is because we can replace these non-frequent characters to match the most frequent one.

In other words, for a window to be valid, the following condition must be true:
```
(window length) - (count of most frequent character in the window) ≤ k
```

If this condition is not met, we need to shrink the window from the left until it becomes valid again.

## Visual Explanation

Let's visualize the solution with Example 2: `s = "AABABBA", k = 1`

```
Step 1: Initialize a sliding window with left and right pointers at the start of the string.
        s = "AABABBA"
             ^
            l,r
        char_count = {}
        max_length = 0
        max_count = 0 (count of the most frequent character in the current window)

Step 2: Expand the window by moving the right pointer.
        s = "AABABBA"
             ^
             l r
        char_count = {'A': 1}
        max_count = 1
        Window length = 1, max_count = 1, replacements needed = 0 ≤ k, so window is valid.
        max_length = 1

Step 3: Expand the window.
        s = "AABABBA"
             ^
             l  r
        char_count = {'A': 2}
        max_count = 2
        Window length = 2, max_count = 2, replacements needed = 0 ≤ k, so window is valid.
        max_length = 2

Step 4: Expand the window.
        s = "AABABBA"
             ^
             l   r
        char_count = {'A': 2, 'B': 1}
        max_count = 2
        Window length = 3, max_count = 2, replacements needed = 1 ≤ k, so window is valid.
        max_length = 3

Step 5: Expand the window.
        s = "AABABBA"
             ^
             l    r
        char_count = {'A': 3, 'B': 1}
        max_count = 3
        Window length = 4, max_count = 3, replacements needed = 1 ≤ k, so window is valid.
        max_length = 4

Step 6: Expand the window.
        s = "AABABBA"
             ^
             l     r
        char_count = {'A': 3, 'B': 2}
        max_count = 3
        Window length = 5, max_count = 3, replacements needed = 2 > k, so window is invalid.
        Shrink the window from the left.
        
        s = "AABABBA"
              ^
              l    r
        char_count = {'A': 2, 'B': 2}
        max_count = 3 (we don't decrement max_count even if the character count decreases)
        Window length = 4, max_count = 3, replacements needed = 1 ≤ k, so window is valid.
        max_length = 4

Step 7: Expand the window.
        s = "AABABBA"
              ^
              l     r
        char_count = {'A': 2, 'B': 3}
        max_count = 3
        Window length = 5, max_count = 3, replacements needed = 2 > k, so window is invalid.
        Shrink the window from the left.
        
        s = "AABABBA"
               ^
               l    r
        char_count = {'A': 1, 'B': 3}
        max_count = 3
        Window length = 4, max_count = 3, replacements needed = 1 ≤ k, so window is valid.
        max_length = 4

Step 8: Expand the window.
        s = "AABABBA"
               ^
               l     r
        char_count = {'A': 2, 'B': 3}
        max_count = 3
        Window length = 5, max_count = 3, replacements needed = 2 > k, so window is invalid.
        Shrink the window from the left.
        
        s = "AABABBA"
                ^
                l    r
        char_count = {'A': 2, 'B': 2}
        max_count = 3
        Window length = 4, max_count = 3, replacements needed = 1 ≤ k, so window is valid.
        max_length = 4

Result: max_length = 4
```

![Character Replacement Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize a sliding window with left and right pointers at the start of the string.
2. Initialize a dictionary to count the frequency of each character in the current window.
3. Initialize a variable `max_count` to track the count of the most frequent character in the current window.
4. Iterate through the string with the right pointer:
   - Increment the count of the current character in the dictionary.
   - Update `max_count` if the count of the current character is higher.
   - If the window is invalid (i.e., (window length) - max_count > k), shrink the window from the left until it becomes valid again.
   - Update the maximum length of the valid window seen so far.
5. Return the maximum length.

## Code Implementation

### Python

```python
def characterReplacement(s, k):
    # Initialize a dictionary to count the frequency of each character
    char_count = {}
    
    # Initialize pointers and max_length
    left = 0
    max_length = 0
    max_count = 0  # Count of the most frequent character in the current window
    
    # Iterate through the string with the right pointer
    for right in range(len(s)):
        # Increment the count of the current character
        char_count[s[right]] = char_count.get(s[right], 0) + 1
        
        # Update max_count if the count of the current character is higher
        max_count = max(max_count, char_count[s[right]])
        
        # If the window is invalid, shrink it from the left
        # Window is invalid if (window length) - max_count > k
        if (right - left + 1) - max_count > k:
            char_count[s[left]] -= 1
            left += 1
        
        # Update max_length
        max_length = max(max_length, right - left + 1)
    
    return max_length
```

### JavaScript

```javascript
function characterReplacement(s, k) {
    // Initialize a map to count the frequency of each character
    const charCount = new Map();
    
    // Initialize pointers and max_length
    let left = 0;
    let maxLength = 0;
    let maxCount = 0;  // Count of the most frequent character in the current window
    
    // Iterate through the string with the right pointer
    for (let right = 0; right < s.length; right++) {
        // Increment the count of the current character
        charCount.set(s[right], (charCount.get(s[right]) || 0) + 1);
        
        // Update maxCount if the count of the current character is higher
        maxCount = Math.max(maxCount, charCount.get(s[right]));
        
        // If the window is invalid, shrink it from the left
        // Window is invalid if (window length) - maxCount > k
        if ((right - left + 1) - maxCount > k) {
            charCount.set(s[left], charCount.get(s[left]) - 1);
            left++;
        }
        
        // Update maxLength
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}
```

## Time and Space Complexity

- **Time Complexity**: O(n), where n is the length of the string. We iterate through the string once with the right pointer, and the left pointer also moves at most n times.
- **Space Complexity**: O(1), as we only need to store the frequency of uppercase English letters, which is at most 26 characters.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 1: `s = "ABAB", k = 2`

```
Initialize:
- char_count = {}
- left = 0
- max_length = 0
- max_count = 0

Iteration 1 (right = 0, s[right] = 'A'):
- char_count = {'A': 1}
- max_count = 1
- Window length = 1, max_count = 1, replacements needed = 0 ≤ k, so window is valid.
- max_length = 1

Iteration 2 (right = 1, s[right] = 'B'):
- char_count = {'A': 1, 'B': 1}
- max_count = 1
- Window length = 2, max_count = 1, replacements needed = 1 ≤ k, so window is valid.
- max_length = 2

Iteration 3 (right = 2, s[right] = 'A'):
- char_count = {'A': 2, 'B': 1}
- max_count = 2
- Window length = 3, max_count = 2, replacements needed = 1 ≤ k, so window is valid.
- max_length = 3

Iteration 4 (right = 3, s[right] = 'B'):
- char_count = {'A': 2, 'B': 2}
- max_count = 2
- Window length = 4, max_count = 2, replacements needed = 2 ≤ k, so window is valid.
- max_length = 4

Result: max_length = 4
```

## Edge Cases and Optimizations

- **Empty String**: If the input string is empty, the function will return 0.
- **Single Character**: If the string has only one character, the function will return 1.
- **All Same Characters**: If all characters in the string are the same, the function will return the length of the string.
- **k = 0**: If k is 0, the function will return the length of the longest substring with the same character.
- **k ≥ Length of String**: If k is greater than or equal to the length of the string, the function will return the length of the string.

**Optimization**: We don't need to decrement `max_count` when shrinking the window, as it doesn't affect the result. This is because we're only interested in the maximum count of any character in the current window, and this value can only increase or stay the same as we expand the window.

## Common Mistakes

1. **Not handling the window correctly**: Make sure to shrink the window from the left when it becomes invalid.
2. **Decrementing max_count**: Don't decrement `max_count` when shrinking the window, as it doesn't affect the result.
3. **Off-by-one errors**: Be careful with the calculation of the window size (right - left + 1).

## Related Problems

1. **Longest Substring Without Repeating Characters**: Find the length of the longest substring without repeating characters.
2. **Longest Substring with At Most K Distinct Characters**: Find the length of the longest substring that contains at most k distinct characters.
3. **Minimum Window Substring**: Find the minimum window in a string which will contain all the characters in another string.
4. **Permutation in String**: Given two strings s1 and s2, return true if s2 contains a permutation of s1. 