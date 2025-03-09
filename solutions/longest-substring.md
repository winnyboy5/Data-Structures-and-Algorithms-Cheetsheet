# Longest Substring Without Repeating Characters

## Problem Statement

Given a string `s`, find the length of the longest substring without repeating characters.

**Example 1:**
```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

**Example 2:**
```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**
```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

## Intuition

The key insight for this problem is to use a sliding window approach with a hash map (or set) to keep track of characters in the current window. As we iterate through the string, we expand the window by adding characters to it. When we encounter a character that's already in our window, we need to contract the window from the left until we remove the duplicate character.

This approach allows us to find the longest substring without repeating characters in a single pass through the string.

## Visual Explanation

Let's visualize the solution with Example 1: `s = "abcabcbb"`

```
Step 1: Initialize an empty set and two pointers (left and right) at the start of the string.
        s = "abcabcbb"
             ^
           l,r
        set = {}
        max_length = 0

Step 2: Add s[right] = 'a' to the set and update max_length.
        s = "abcabcbb"
             ^
             l r
        set = {'a'}
        max_length = 1

Step 3: Add s[right] = 'b' to the set and update max_length.
        s = "abcabcbb"
             ^
             l  r
        set = {'a', 'b'}
        max_length = 2

Step 4: Add s[right] = 'c' to the set and update max_length.
        s = "abcabcbb"
             ^
             l   r
        set = {'a', 'b', 'c'}
        max_length = 3

Step 5: s[right] = 'a' is already in the set, so we need to contract the window.
        Remove s[left] = 'a' from the set and increment left.
        s = "abcabcbb"
              ^
              l   r
        set = {'b', 'c'}
        max_length = 3

Step 6: Add s[right] = 'a' to the set and update max_length.
        s = "abcabcbb"
              ^
              l    r
        set = {'b', 'c', 'a'}
        max_length = 3

Step 7: s[right] = 'b' is already in the set, so we need to contract the window.
        Remove s[left] = 'b' from the set and increment left.
        s = "abcabcbb"
               ^
               l    r
        set = {'c', 'a'}
        max_length = 3

Step 8: Add s[right] = 'b' to the set and update max_length.
        s = "abcabcbb"
               ^
               l     r
        set = {'c', 'a', 'b'}
        max_length = 3

Step 9: s[right] = 'c' is already in the set, so we need to contract the window.
        Remove s[left] = 'c' from the set and increment left.
        s = "abcabcbb"
                ^
                l     r
        set = {'a', 'b'}
        max_length = 3

Step 10: Add s[right] = 'c' to the set and update max_length.
         s = "abcabcbb"
                ^
                l      r
         set = {'a', 'b', 'c'}
         max_length = 3

Step 11: s[right] = 'b' is already in the set, so we need to contract the window.
         Remove s[left] = 'a' from the set and increment left.
         s = "abcabcbb"
                 ^
                 l      r
         set = {'b', 'c'}
         max_length = 3

Step 12: s[right] = 'b' is already in the set, so we need to contract the window.
         Remove s[left] = 'b' from the set and increment left.
         s = "abcabcbb"
                  ^
                  l      r
         set = {'c'}
         max_length = 3

Step 13: Add s[right] = 'b' to the set and update max_length.
         s = "abcabcbb"
                  ^
                  l       r
         set = {'c', 'b'}
         max_length = 3

Step 14: s[right] = 'b' is already in the set, so we need to contract the window.
         Remove s[left] = 'c' from the set and increment left.
         s = "abcabcbb"
                   ^
                   l       r
         set = {'b'}
         max_length = 3

Step 15: s[right] = 'b' is already in the set, so we need to contract the window.
         Remove s[left] = 'b' from the set and increment left.
         s = "abcabcbb"
                    ^
                    l       r
         set = {}
         max_length = 3

Step 16: Add s[right] = 'b' to the set and update max_length.
         s = "abcabcbb"
                    ^
                    l        r
         set = {'b'}
         max_length = 3

Result: max_length = 3
```

![Longest Substring Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize an empty hash set to store characters in the current window.
2. Initialize two pointers, `left` and `right`, both starting at the beginning of the string.
3. Initialize a variable `max_length` to track the length of the longest substring without repeating characters.
4. Iterate through the string with the `right` pointer:
   - If the current character is not in the set, add it to the set and update `max_length` if the current window size is larger.
   - If the current character is already in the set, remove characters from the left of the window (by incrementing the `left` pointer) until the duplicate character is removed from the set.
5. Return `max_length`.

## Code Implementation

### Python

```python
def lengthOfLongestSubstring(s):
    # Initialize a set to store characters in the current window
    char_set = set()
    
    # Initialize pointers and max_length
    left = 0
    max_length = 0
    
    # Iterate through the string with the right pointer
    for right in range(len(s)):
        # If the current character is already in the set,
        # remove characters from the left until the duplicate is removed
        while s[right] in char_set:
            char_set.remove(s[left])
            left += 1
        
        # Add the current character to the set
        char_set.add(s[right])
        
        # Update max_length if the current window size is larger
        max_length = max(max_length, right - left + 1)
    
    return max_length
```

### Python (Optimized)

```python
def lengthOfLongestSubstring(s):
    # Initialize a dictionary to store the index of each character
    char_index = {}
    
    # Initialize pointers and max_length
    left = 0
    max_length = 0
    
    # Iterate through the string with the right pointer
    for right in range(len(s)):
        # If the current character is already in the window,
        # update the left pointer to the position after the duplicate
        if s[right] in char_index and char_index[s[right]] >= left:
            left = char_index[s[right]] + 1
        
        # Update the index of the current character
        char_index[s[right]] = right
        
        # Update max_length if the current window size is larger
        max_length = max(max_length, right - left + 1)
    
    return max_length
```

### JavaScript

```javascript
function lengthOfLongestSubstring(s) {
    // Initialize a set to store characters in the current window
    const charSet = new Set();
    
    // Initialize pointers and max_length
    let left = 0;
    let maxLength = 0;
    
    // Iterate through the string with the right pointer
    for (let right = 0; right < s.length; right++) {
        // If the current character is already in the set,
        // remove characters from the left until the duplicate is removed
        while (charSet.has(s[right])) {
            charSet.delete(s[left]);
            left++;
        }
        
        // Add the current character to the set
        charSet.add(s[right]);
        
        // Update maxLength if the current window size is larger
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}
```

### JavaScript (Optimized)

```javascript
function lengthOfLongestSubstring(s) {
    // Initialize a map to store the index of each character
    const charIndex = new Map();
    
    // Initialize pointers and max_length
    let left = 0;
    let maxLength = 0;
    
    // Iterate through the string with the right pointer
    for (let right = 0; right < s.length; right++) {
        // If the current character is already in the window,
        // update the left pointer to the position after the duplicate
        if (charIndex.has(s[right]) && charIndex.get(s[right]) >= left) {
            left = charIndex.get(s[right]) + 1;
        }
        
        // Update the index of the current character
        charIndex.set(s[right], right);
        
        // Update maxLength if the current window size is larger
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}
```

## Time and Space Complexity

### Basic Approach:
- **Time Complexity**: O(n), where n is the length of the string. In the worst case, each character might be visited twice (once by the right pointer and once by the left pointer).
- **Space Complexity**: O(min(n, m)), where n is the length of the string and m is the size of the character set. In the worst case, the set might contain all unique characters in the string.

### Optimized Approach:
- **Time Complexity**: O(n), where n is the length of the string. We iterate through the string once.
- **Space Complexity**: O(min(n, m)), where n is the length of the string and m is the size of the character set. In the worst case, the dictionary might contain all unique characters in the string.

## Detailed Walkthrough with Example

Let's trace through the optimized algorithm with Example 3: `s = "pwwkew"`

```
Initialize:
- char_index = {}
- left = 0
- max_length = 0

Iteration 1 (right = 0, s[right] = 'p'):
- 'p' is not in char_index, so left remains 0
- Update char_index: {'p': 0}
- max_length = max(0, 0 - 0 + 1) = 1

Iteration 2 (right = 1, s[right] = 'w'):
- 'w' is not in char_index, so left remains 0
- Update char_index: {'p': 0, 'w': 1}
- max_length = max(1, 1 - 0 + 1) = 2

Iteration 3 (right = 2, s[right] = 'w'):
- 'w' is in char_index and char_index['w'] = 1 >= left = 0
- Update left = 1 + 1 = 2
- Update char_index: {'p': 0, 'w': 2}
- max_length = max(2, 2 - 2 + 1) = 2

Iteration 4 (right = 3, s[right] = 'k'):
- 'k' is not in char_index, so left remains 2
- Update char_index: {'p': 0, 'w': 2, 'k': 3}
- max_length = max(2, 3 - 2 + 1) = 2

Iteration 5 (right = 4, s[right] = 'e'):
- 'e' is not in char_index, so left remains 2
- Update char_index: {'p': 0, 'w': 2, 'k': 3, 'e': 4}
- max_length = max(2, 4 - 2 + 1) = 3

Iteration 6 (right = 5, s[right] = 'w'):
- 'w' is in char_index and char_index['w'] = 2 >= left = 2
- Update left = 2 + 1 = 3
- Update char_index: {'p': 0, 'w': 5, 'k': 3, 'e': 4}
- max_length = max(3, 5 - 3 + 1) = 3

Result: max_length = 3
```

## Edge Cases and Optimizations

- **Empty String**: If the input string is empty, the function will return 0.
- **Single Character**: If the string has only one character, the function will return 1.
- **All Repeating Characters**: If all characters in the string are the same, the function will return 1.
- **No Repeating Characters**: If there are no repeating characters in the string, the function will return the length of the string.

**Optimization**: In the optimized approach, instead of removing characters from the set one by one, we directly jump the left pointer to the position after the duplicate character. This reduces the number of operations in the inner loop.

## Common Mistakes

1. **Not handling duplicates correctly**: Make sure to update the left pointer correctly when a duplicate is found.
2. **Off-by-one errors**: Be careful with the calculation of the window size (right - left + 1).
3. **Not updating the character index**: Always update the index of the current character in the dictionary, even if it's already present.

## Related Problems

1. **Longest Substring with At Most K Distinct Characters**: Find the length of the longest substring that contains at most k distinct characters.
2. **Longest Substring with At Most Two Distinct Characters**: Find the length of the longest substring that contains at most two distinct characters.
3. **Substring with Concatenation of All Words**: Find all starting indices of substring(s) in a given string that is a concatenation of each word in a given list exactly once.
4. **Minimum Window Substring**: Find the minimum window in a string which will contain all the characters in another string. 