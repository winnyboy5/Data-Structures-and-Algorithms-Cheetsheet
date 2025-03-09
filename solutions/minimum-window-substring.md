# Minimum Window Substring

## Problem Statement

Given two strings `s` and `t`, return the minimum window substring of `s` such that every character in `t` (including duplicates) is included in the window. If there is no such substring, return the empty string `""`.

The testcases will be generated such that the answer is unique.

**Example 1:**
```
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
```

**Example 2:**
```
Input: s = "a", t = "a"
Output: "a"
Explanation: The entire string s is the minimum window.
```

**Example 3:**
```
Input: s = "a", t = "aa"
Output: ""
Explanation: Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.
```

## Intuition

The key insight for this problem is to use a sliding window approach with two pointers. We'll maintain a window that contains all characters from `t`, and then try to minimize this window.

We need to keep track of:
1. The frequency of each character in `t` (what we need)
2. The frequency of each character in our current window (what we have)
3. The number of unique characters from `t` that are fully satisfied in our current window

When our window contains all characters from `t`, we try to shrink it from the left to find the minimum valid window.

## Visual Explanation

Let's visualize the solution with Example 1: `s = "ADOBECODEBANC", t = "ABC"`

```
Step 1: Initialize the target character counts from t.
        t = "ABC"
        target_count = {'A': 1, 'B': 1, 'C': 1}
        required = 3 (number of unique characters in t)

Step 2: Initialize the sliding window.
        s = "ADOBECODEBANC"
             ^
            l,r
        window_count = {}
        formed = 0 (number of characters from t fully satisfied in the current window)

Step 3: Expand the window until we have all characters from t.
        s = "ADOBECODEBANC"
             ^
             l r
        window_count = {'A': 1}
        formed = 1 (A is fully satisfied)
        
        s = "ADOBECODEBANC"
             ^
             l  r
        window_count = {'A': 1, 'D': 1}
        formed = 1 (A is fully satisfied)
        
        s = "ADOBECODEBANC"
             ^
             l   r
        window_count = {'A': 1, 'D': 1, 'O': 1}
        formed = 1 (A is fully satisfied)
        
        s = "ADOBECODEBANC"
             ^
             l    r
        window_count = {'A': 1, 'D': 1, 'O': 1, 'B': 1}
        formed = 2 (A and B are fully satisfied)
        
        s = "ADOBECODEBANC"
             ^
             l     r
        window_count = {'A': 1, 'D': 1, 'O': 1, 'B': 1, 'E': 1}
        formed = 2 (A and B are fully satisfied)
        
        s = "ADOBECODEBANC"
             ^
             l      r
        window_count = {'A': 1, 'D': 1, 'O': 1, 'B': 1, 'E': 1, 'C': 1}
        formed = 3 (A, B, and C are fully satisfied)
        
        We have all characters from t, so we can try to shrink the window.

Step 4: Shrink the window from the left as much as possible while still containing all characters from t.
        s = "ADOBECODEBANC"
              ^
              l     r
        window_count = {'A': 0, 'D': 1, 'O': 1, 'B': 1, 'E': 1, 'C': 1}
        formed = 2 (B and C are fully satisfied, A is not)
        
        We can't shrink further without losing a required character, so we expand the window again.

Step 5: Continue expanding the window.
        s = "ADOBECODEBANC"
              ^
              l       r
        window_count = {'A': 0, 'D': 1, 'O': 2, 'B': 1, 'E': 1, 'C': 1}
        formed = 2 (B and C are fully satisfied, A is not)
        
        s = "ADOBECODEBANC"
              ^
              l        r
        window_count = {'A': 0, 'D': 2, 'O': 2, 'B': 1, 'E': 1, 'C': 1}
        formed = 2 (B and C are fully satisfied, A is not)
        
        s = "ADOBECODEBANC"
              ^
              l         r
        window_count = {'A': 0, 'D': 2, 'O': 2, 'B': 1, 'E': 2, 'C': 1}
        formed = 2 (B and C are fully satisfied, A is not)
        
        s = "ADOBECODEBANC"
              ^
              l          r
        window_count = {'A': 0, 'D': 2, 'O': 2, 'B': 2, 'E': 2, 'C': 1}
        formed = 2 (B and C are fully satisfied, A is not)
        
        s = "ADOBECODEBANC"
              ^
              l           r
        window_count = {'A': 1, 'D': 2, 'O': 2, 'B': 2, 'E': 2, 'C': 1}
        formed = 3 (A, B, and C are fully satisfied)
        
        We have all characters from t again, so we can try to shrink the window.

Step 6: Shrink the window from the left as much as possible.
        s = "ADOBECODEBANC"
               ^
               l          r
        window_count = {'A': 1, 'D': 1, 'O': 2, 'B': 2, 'E': 2, 'C': 1}
        formed = 3 (A, B, and C are fully satisfied)
        
        s = "ADOBECODEBANC"
                ^
                l         r
        window_count = {'A': 1, 'D': 1, 'O': 1, 'B': 2, 'E': 2, 'C': 1}
        formed = 3 (A, B, and C are fully satisfied)
        
        s = "ADOBECODEBANC"
                 ^
                 l        r
        window_count = {'A': 1, 'D': 1, 'O': 1, 'B': 1, 'E': 2, 'C': 1}
        formed = 3 (A, B, and C are fully satisfied)
        
        s = "ADOBECODEBANC"
                  ^
                  l       r
        window_count = {'A': 1, 'D': 1, 'O': 1, 'B': 1, 'E': 1, 'C': 1}
        formed = 3 (A, B, and C are fully satisfied)
        
        s = "ADOBECODEBANC"
                   ^
                   l      r
        window_count = {'A': 1, 'D': 1, 'O': 1, 'B': 1, 'E': 1, 'C': 0}
        formed = 2 (A and B are fully satisfied, C is not)
        
        We can't shrink further without losing a required character, so we expand the window again.

Step 7: Continue expanding the window.
        s = "ADOBECODEBANC"
                   ^
                   l       r
        window_count = {'A': 1, 'D': 1, 'O': 1, 'B': 1, 'E': 1, 'C': 0, 'N': 1}
        formed = 2 (A and B are fully satisfied, C is not)
        
        s = "ADOBECODEBANC"
                   ^
                   l        r
        window_count = {'A': 1, 'D': 1, 'O': 1, 'B': 1, 'E': 1, 'C': 1, 'N': 1}
        formed = 3 (A, B, and C are fully satisfied)
        
        We have all characters from t again, so we can try to shrink the window.

Step 8: Shrink the window from the left as much as possible.
        s = "ADOBECODEBANC"
                    ^
                    l       r
        window_count = {'A': 1, 'D': 0, 'O': 1, 'B': 1, 'E': 1, 'C': 1, 'N': 1}
        formed = 3 (A, B, and C are fully satisfied)
        
        s = "ADOBECODEBANC"
                     ^
                     l      r
        window_count = {'A': 1, 'D': 0, 'O': 0, 'B': 1, 'E': 1, 'C': 1, 'N': 1}
        formed = 3 (A, B, and C are fully satisfied)
        
        s = "ADOBECODEBANC"
                      ^
                      l     r
        window_count = {'A': 1, 'D': 0, 'O': 0, 'B': 0, 'E': 1, 'C': 1, 'N': 1}
        formed = 2 (A and C are fully satisfied, B is not)
        
        We can't shrink further without losing a required character, so we expand the window again.

Step 9: Continue expanding the window.
        s = "ADOBECODEBANC"
                      ^
                      l      r
        window_count = {'A': 1, 'D': 0, 'O': 0, 'B': 0, 'E': 1, 'C': 1, 'N': 1, 'A': 1}
        formed = 2 (A and C are fully satisfied, B is not)
        
        s = "ADOBECODEBANC"
                      ^
                      l       r
        window_count = {'A': 1, 'D': 0, 'O': 0, 'B': 1, 'E': 1, 'C': 1, 'N': 1, 'A': 1}
        formed = 3 (A, B, and C are fully satisfied)
        
        We have all characters from t again, so we can try to shrink the window.

Step 10: Shrink the window from the left as much as possible.
         s = "ADOBECODEBANC"
                       ^
                       l      r
         window_count = {'A': 1, 'D': 0, 'O': 0, 'B': 1, 'E': 0, 'C': 1, 'N': 1, 'A': 1}
         formed = 3 (A, B, and C are fully satisfied)
         
         s = "ADOBECODEBANC"
                        ^
                        l     r
         window_count = {'A': 1, 'D': 0, 'O': 0, 'B': 1, 'E': 0, 'C': 0, 'N': 1, 'A': 1}
         formed = 2 (A and B are fully satisfied, C is not)
         
         We can't shrink further without losing a required character, so we expand the window again.

Step 11: Continue expanding the window.
         s = "ADOBECODEBANC"
                        ^
                        l      r
         window_count = {'A': 1, 'D': 0, 'O': 0, 'B': 1, 'E': 0, 'C': 0, 'N': 1, 'A': 1, 'N': 1}
         formed = 2 (A and B are fully satisfied, C is not)
         
         s = "ADOBECODEBANC"
                        ^
                        l       r
         window_count = {'A': 1, 'D': 0, 'O': 0, 'B': 1, 'E': 0, 'C': 1, 'N': 1, 'A': 1, 'N': 1}
         formed = 3 (A, B, and C are fully satisfied)
         
         We have all characters from t again, so we can try to shrink the window.

Step 12: Shrink the window from the left as much as possible.
         s = "ADOBECODEBANC"
                         ^
                         l      r
         window_count = {'A': 0, 'D': 0, 'O': 0, 'B': 1, 'E': 0, 'C': 1, 'N': 1, 'A': 1, 'N': 1}
         formed = 2 (B and C are fully satisfied, A is not)
         
         We can't shrink further without losing a required character, so we expand the window again.

Step 13: We've reached the end of the string, so we're done.
         The minimum window substring we found is "BANC" (from index 9 to 12).
```

![Minimum Window Substring Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Create a dictionary `target_count` to store the frequency of each character in `t`.
2. Initialize `required` to the number of unique characters in `t`.
3. Initialize a sliding window with left and right pointers at the start of `s`.
4. Initialize a dictionary `window_count` to store the frequency of each character in the current window.
5. Initialize `formed` to 0, representing the number of characters from `t` that are fully satisfied in the current window.
6. Expand the window by moving the right pointer:
   - Increment the count of the current character in `window_count`.
   - If the character is in `target_count` and its count in the window matches the required count, increment `formed`.
7. When `formed` equals `required` (all characters from `t` are in the window), try to shrink the window from the left:
   - If the character at the left pointer is not in `target_count` or its count in the window exceeds the required count, shrink the window.
   - If shrinking would break the window, update the minimum window if the current window is smaller.
8. Continue expanding and shrinking the window until the right pointer reaches the end of `s`.
9. Return the minimum window substring found, or an empty string if no valid window exists.

## Code Implementation

### Python

```python
def minWindow(s, t):
    # Edge case: if t is empty or s is shorter than t
    if not t or len(s) < len(t):
        return ""
    
    # Initialize dictionaries to store character frequencies
    target_count = {}
    window_count = {}
    
    # Count the frequency of each character in t
    for char in t:
        target_count[char] = target_count.get(char, 0) + 1
    
    # Initialize variables
    required = len(target_count)  # Number of unique characters in t
    formed = 0  # Number of characters from t fully satisfied in the current window
    left = 0  # Left pointer of the window
    min_len = float('inf')  # Length of the minimum window
    min_window = ""  # Minimum window substring
    
    # Iterate through the string with the right pointer
    for right in range(len(s)):
        # Add the current character to the window
        char = s[right]
        window_count[char] = window_count.get(char, 0) + 1
        
        # Check if the current character helps satisfy a character from t
        if char in target_count and window_count[char] == target_count[char]:
            formed += 1
        
        # Try to shrink the window when all characters from t are in the window
        while left <= right and formed == required:
            char = s[left]
            
            # Update the minimum window if the current window is smaller
            if right - left + 1 < min_len:
                min_len = right - left + 1
                min_window = s[left:right+1]
            
            # Remove the leftmost character from the window
            window_count[char] -= 1
            
            # Check if removing the character breaks the window
            if char in target_count and window_count[char] < target_count[char]:
                formed -= 1
            
            # Shrink the window from the left
            left += 1
    
    return min_window
```

### JavaScript

```javascript
function minWindow(s, t) {
    // Edge case: if t is empty or s is shorter than t
    if (!t || s.length < t.length) {
        return "";
    }
    
    // Initialize maps to store character frequencies
    const targetCount = new Map();
    const windowCount = new Map();
    
    // Count the frequency of each character in t
    for (const char of t) {
        targetCount.set(char, (targetCount.get(char) || 0) + 1);
    }
    
    // Initialize variables
    const required = targetCount.size;  // Number of unique characters in t
    let formed = 0;  // Number of characters from t fully satisfied in the current window
    let left = 0;  // Left pointer of the window
    let minLen = Infinity;  // Length of the minimum window
    let minWindow = "";  // Minimum window substring
    
    // Iterate through the string with the right pointer
    for (let right = 0; right < s.length; right++) {
        // Add the current character to the window
        const char = s[right];
        windowCount.set(char, (windowCount.get(char) || 0) + 1);
        
        // Check if the current character helps satisfy a character from t
        if (targetCount.has(char) && windowCount.get(char) === targetCount.get(char)) {
            formed++;
        }
        
        // Try to shrink the window when all characters from t are in the window
        while (left <= right && formed === required) {
            const char = s[left];
            
            // Update the minimum window if the current window is smaller
            if (right - left + 1 < minLen) {
                minLen = right - left + 1;
                minWindow = s.substring(left, right + 1);
            }
            
            // Remove the leftmost character from the window
            windowCount.set(char, windowCount.get(char) - 1);
            
            // Check if removing the character breaks the window
            if (targetCount.has(char) && windowCount.get(char) < targetCount.get(char)) {
                formed--;
            }
            
            // Shrink the window from the left
            left++;
        }
    }
    
    return minWindow;
}
```

## Time and Space Complexity

- **Time Complexity**: O(n + m), where n is the length of string `s` and m is the length of string `t`. We iterate through both strings once.
- **Space Complexity**: O(k), where k is the number of unique characters in strings `s` and `t`. In the worst case, this could be O(n + m) if all characters are unique.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2: `s = "a", t = "a"`

```
Initialize:
- target_count = {'a': 1}
- required = 1
- window_count = {}
- formed = 0
- left = 0
- min_len = Infinity
- min_window = ""

Iteration 1 (right = 0, s[right] = 'a'):
- window_count = {'a': 1}
- formed = 1 (a is fully satisfied)
- Since formed = required, try to shrink the window:
  - min_len = 1
  - min_window = "a"
  - window_count = {'a': 0}
  - formed = 0
  - left = 1

Result: min_window = "a"
```

Now let's trace through Example 3: `s = "a", t = "aa"`

```
Initialize:
- target_count = {'a': 2}
- required = 1
- window_count = {}
- formed = 0
- left = 0
- min_len = Infinity
- min_window = ""

Iteration 1 (right = 0, s[right] = 'a'):
- window_count = {'a': 1}
- formed = 0 (a is not fully satisfied because we need 2 'a's)
- Since formed != required, we don't shrink the window

Result: min_window = "" (no valid window found)
```

## Edge Cases and Optimizations

- **Empty Strings**: If either `s` or `t` is empty, return an empty string.
- **No Valid Window**: If `s` doesn't contain all characters from `t`, return an empty string.
- **Single Character**: If both `s` and `t` are single characters, return `s` if it equals `t`, otherwise return an empty string.
- **Duplicate Characters**: Handle duplicate characters in `t` by counting their frequency and ensuring the window contains at least that many occurrences.

**Optimization**: We can optimize the solution by only considering characters that are in `t` when expanding the window. This can reduce the number of operations, especially if `s` is much longer than `t` and contains many characters not in `t`.

## Common Mistakes

1. **Not handling duplicate characters**: Make sure to count the frequency of each character in `t` and ensure the window contains at least that many occurrences.
2. **Incorrect window shrinking**: Be careful when shrinking the window to ensure you don't remove a character that's required for a valid window.
3. **Not updating the minimum window**: Make sure to update the minimum window whenever you find a valid window that's smaller than the current minimum.

## Related Problems

1. **Longest Substring Without Repeating Characters**: Find the length of the longest substring without repeating characters.
2. **Longest Substring with At Most K Distinct Characters**: Find the length of the longest substring that contains at most k distinct characters.
3. **Longest Repeating Character Replacement**: Find the length of the longest substring containing the same letter after replacing at most k characters.
4. **Permutation in String**: Given two strings s1 and s2, return true if s2 contains a permutation of s1. 