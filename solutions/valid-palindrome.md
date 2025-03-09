# Valid Palindrome

## Problem Statement

Given a string `s`, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

**Example 1:**
```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```

**Example 2:**
```
Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.
```

**Example 3:**
```
Input: s = " "
Output: true
Explanation: s is an empty string "" after removing non-alphanumeric characters. Since an empty string reads the same forward and backward, it is a palindrome.
```

## Intuition

A palindrome is a string that reads the same backward as forward. To check if a string is a palindrome, we need to compare characters from the beginning and end of the string, moving inward. For this problem, we need to:

1. Ignore non-alphanumeric characters (letters and numbers only)
2. Ignore case (treat uppercase and lowercase letters as the same)

The most straightforward approach is to use two pointers: one starting from the beginning of the string and the other from the end. We move these pointers inward, skipping non-alphanumeric characters, and compare the characters at each step.

## Visual Explanation

Let's visualize the solution with Example 1: `"A man, a plan, a canal: Panama"`

```
Step 1: Initialize two pointers, left at the beginning and right at the end.
        "A man, a plan, a canal: Panama"
         ^                        ^
         left                     right

Step 2: Compare characters at left and right (ignoring case).
        'A' == 'a' (case-insensitive) ✓
        Move both pointers inward.
        "A man, a plan, a canal: Panama"
          ^                      ^
          left                   right

Step 3: Skip non-alphanumeric characters.
        left encounters ' ', skip it.
        "A man, a plan, a canal: Panama"
           ^                     ^
           left                  right

Step 4: Compare characters at left and right.
        'm' == 'm' ✓
        Move both pointers inward.
        "A man, a plan, a canal: Panama"
            ^                   ^
            left                right

Step 5: Skip non-alphanumeric characters.
        right encounters ':', skip it.
        "A man, a plan, a canal: Panama"
            ^                  ^
            left               right

Step 6: Continue this process, comparing alphanumeric characters and skipping others.
        'a' == 'a' ✓
        'n' == 'n' ✓
        'a' == 'a' ✓
        'p' == 'p' ✓
        'l' == 'l' ✓
        'a' == 'a' ✓
        'n' == 'n' ✓
        'a' == 'a' ✓
        'c' == 'c' ✓
        'a' == 'a' ✓
        'n' == 'n' ✓
        'a' == 'a' ✓
        'l' == 'l' ✓
        'p' == 'p' ✓
        'a' == 'a' ✓

Step 7: All characters match, so the string is a palindrome.
        Return true.
```

![Valid Palindrome Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize two pointers, `left` at the beginning of the string and `right` at the end.
2. While `left` is less than `right`:
   - If the character at `left` is not alphanumeric, increment `left` and continue.
   - If the character at `right` is not alphanumeric, decrement `right` and continue.
   - Compare the lowercase versions of the characters at `left` and `right`.
   - If they are not equal, return `false`.
   - Otherwise, increment `left` and decrement `right`.
3. If we've compared all characters without finding a mismatch, return `true`.

## Code Implementation

### Python

```python
def isPalindrome(s):
    # Initialize two pointers
    left, right = 0, len(s) - 1
    
    while left < right:
        # Skip non-alphanumeric characters from the left
        while left < right and not s[left].isalnum():
            left += 1
        
        # Skip non-alphanumeric characters from the right
        while left < right and not s[right].isalnum():
            right -= 1
        
        # Compare characters (ignoring case)
        if s[left].lower() != s[right].lower():
            return False
        
        # Move pointers inward
        left += 1
        right -= 1
    
    # If we've compared all characters without finding a mismatch, it's a palindrome
    return True
```

### JavaScript

```javascript
function isPalindrome(s) {
    // Initialize two pointers
    let left = 0;
    let right = s.length - 1;
    
    while (left < right) {
        // Skip non-alphanumeric characters from the left
        while (left < right && !isAlphanumeric(s[left])) {
            left++;
        }
        
        // Skip non-alphanumeric characters from the right
        while (left < right && !isAlphanumeric(s[right])) {
            right--;
        }
        
        // Compare characters (ignoring case)
        if (s[left].toLowerCase() !== s[right].toLowerCase()) {
            return false;
        }
        
        // Move pointers inward
        left++;
        right--;
    }
    
    // If we've compared all characters without finding a mismatch, it's a palindrome
    return true;
}

// Helper function to check if a character is alphanumeric
function isAlphanumeric(char) {
    const code = char.charCodeAt(0);
    return (code >= 48 && code <= 57) ||  // 0-9
           (code >= 65 && code <= 90) ||  // A-Z
           (code >= 97 && code <= 122);   // a-z
}
```

## Alternative Approach

Another approach is to first clean the string by removing all non-alphanumeric characters and converting to lowercase, then check if the cleaned string is equal to its reverse.

### Python (Alternative)

```python
def isPalindrome(s):
    # Clean the string: remove non-alphanumeric characters and convert to lowercase
    cleaned = ''.join(char.lower() for char in s if char.isalnum())
    
    # Check if the cleaned string is equal to its reverse
    return cleaned == cleaned[::-1]
```

### JavaScript (Alternative)

```javascript
function isPalindrome(s) {
    // Clean the string: remove non-alphanumeric characters and convert to lowercase
    const cleaned = s.replace(/[^a-zA-Z0-9]/g, '').toLowerCase();
    
    // Check if the cleaned string is equal to its reverse
    return cleaned === cleaned.split('').reverse().join('');
}
```

## Time and Space Complexity

### Two-Pointer Approach:
- **Time Complexity**: O(n), where n is the length of the string. We traverse the string once.
- **Space Complexity**: O(1), as we only use a constant amount of extra space regardless of the input size.

### Alternative Approach:
- **Time Complexity**: O(n), where n is the length of the string. We traverse the string once to clean it and once to check if it's a palindrome.
- **Space Complexity**: O(n), as we create a new string of length proportional to the input string.

## Detailed Walkthrough with Example

Let's trace through the two-pointer approach with Example 2: `"race a car"`

```
Initialize:
- left = 0, right = 8

Iteration 1:
- s[left] = 'r', s[right] = 'r'
- Both are alphanumeric, so compare: 'r' == 'r' ✓
- Update: left = 1, right = 7

Iteration 2:
- s[left] = 'a', s[right] = 'a'
- Both are alphanumeric, so compare: 'a' == 'a' ✓
- Update: left = 2, right = 6

Iteration 3:
- s[left] = 'c', s[right] = ' '
- s[right] is not alphanumeric, so skip it
- Update: right = 5

Iteration 4:
- s[left] = 'c', s[right] = 'a'
- Both are alphanumeric, so compare: 'c' != 'a' ✗
- Return false

Result: false
```

## Edge Cases and Optimizations

- **Empty String**: An empty string is considered a palindrome.
- **String with Only Non-Alphanumeric Characters**: After removing all non-alphanumeric characters, we get an empty string, which is a palindrome.
- **Single Character**: A string with a single character is always a palindrome.
- **Case Sensitivity**: Remember to convert characters to the same case before comparing.

## Common Mistakes

1. **Not handling non-alphanumeric characters**: Make sure to skip or remove non-alphanumeric characters as specified in the problem.
2. **Not ignoring case**: Remember to convert characters to the same case (either uppercase or lowercase) before comparing.
3. **Off-by-one errors**: Be careful with the boundary conditions in the while loop.

## Related Problems

1. **Palindrome Linked List**: Determine if a linked list is a palindrome.
2. **Valid Palindrome II**: Determine if a string can be a palindrome after deleting at most one character.
3. **Longest Palindromic Substring**: Find the longest palindromic substring in a given string.
4. **Palindromic Substrings**: Count how many palindromic substrings are in a string. 