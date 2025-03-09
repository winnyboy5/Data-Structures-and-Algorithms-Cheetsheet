# Valid Anagram

## Problem Statement

Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**
```
Input: s = "anagram", t = "nagaram"
Output: true
```

**Example 2:**
```
Input: s = "rat", t = "car"
Output: false
```

## Intuition

The key insight for this problem is to recognize that two strings are anagrams if and only if they have the same characters with the same frequencies. There are several approaches to solve this problem:

1. **Sorting**: Sort both strings and compare them. If they are equal after sorting, they are anagrams.
2. **Character Counting**: Count the frequency of each character in both strings and compare the counts.
3. **Hash Map**: Use a hash map to track the frequency of characters in the first string, then decrement the counts while iterating through the second string.

The character counting approach is the most efficient for this problem, especially when the input strings contain only lowercase English letters (as specified in the constraints).

## Visual Explanation

Let's visualize the character counting approach with Example 1:

```
s = "anagram", t = "nagaram"

Step 1: Initialize a frequency array of size 26 (for lowercase English letters) with all zeros.
freq = [0, 0, 0, ..., 0]

Step 2: Iterate through string s and increment the frequency for each character.
'a' appears 3 times in s, so freq['a' - 'a'] = 3
'n' appears 1 time in s, so freq['n' - 'a'] = 1
'g' appears 1 time in s, so freq['g' - 'a'] = 1
'r' appears 1 time in s, so freq['r' - 'a'] = 1
'm' appears 1 time in s, so freq['m' - 'a'] = 1

After processing s, freq = [3, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0]
                            a  b  c  d  e  f  g  h  i  j  k  l  m  n  o  p  q  r  s  t  u  v  w  x  y  z

Step 3: Iterate through string t and decrement the frequency for each character.
'n' appears in t, so freq['n' - 'a'] = 1 - 1 = 0
'a' appears in t, so freq['a' - 'a'] = 3 - 1 = 2
'g' appears in t, so freq['g' - 'a'] = 1 - 1 = 0
'a' appears in t, so freq['a' - 'a'] = 2 - 1 = 1
'r' appears in t, so freq['r' - 'a'] = 1 - 1 = 0
'a' appears in t, so freq['a' - 'a'] = 1 - 1 = 0
'm' appears in t, so freq['m' - 'a'] = 1 - 1 = 0

After processing t, freq = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
                            a  b  c  d  e  f  g  h  i  j  k  l  m  n  o  p  q  r  s  t  u  v  w  x  y  z

Step 4: Check if all frequencies are zero. If yes, the strings are anagrams.
All frequencies are zero, so s and t are anagrams.
```

Let's also visualize Example 2:

```
s = "rat", t = "car"

Step 1: Initialize a frequency array of size 26 with all zeros.
freq = [0, 0, 0, ..., 0]

Step 2: Iterate through string s and increment the frequency for each character.
'r' appears 1 time in s, so freq['r' - 'a'] = 1
'a' appears 1 time in s, so freq['a' - 'a'] = 1
't' appears 1 time in s, so freq['t' - 'a'] = 1

After processing s, freq = [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0]
                            a  b  c  d  e  f  g  h  i  j  k  l  m  n  o  p  q  r  s  t  u  v  w  x  y  z

Step 3: Iterate through string t and decrement the frequency for each character.
'c' appears in t, so freq['c' - 'a'] = 0 - 1 = -1
'a' appears in t, so freq['a' - 'a'] = 1 - 1 = 0
'r' appears in t, so freq['r' - 'a'] = 1 - 1 = 0

After processing t, freq = [0, 0, -1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0]
                            a  b  c   d  e  f  g  h  i  j  k  l  m  n  o  p  q  r  s  t  u  v  w  x  y  z

Step 4: Check if all frequencies are zero. If yes, the strings are anagrams.
Not all frequencies are zero (freq['c'] = -1, freq['t'] = 1), so s and t are not anagrams.
```

![Valid Anagram Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

### Character Counting Approach:
1. If the lengths of the two strings are different, return `false` (they can't be anagrams).
2. Initialize a frequency array of size 26 (for lowercase English letters) with all zeros.
3. Iterate through the first string and increment the frequency for each character.
4. Iterate through the second string and decrement the frequency for each character.
5. Check if all frequencies are zero. If yes, the strings are anagrams; otherwise, they are not.

### Sorting Approach:
1. If the lengths of the two strings are different, return `false`.
2. Sort both strings.
3. Compare the sorted strings. If they are equal, the strings are anagrams; otherwise, they are not.

## Code Implementation

### Python (Character Counting)

```python
def isAnagram(s, t):
    # If the lengths are different, they can't be anagrams
    if len(s) != len(t):
        return False
    
    # Initialize a frequency array for lowercase English letters
    freq = [0] * 26
    
    # Increment frequencies for characters in s
    for char in s:
        freq[ord(char) - ord('a')] += 1
    
    # Decrement frequencies for characters in t
    for char in t:
        freq[ord(char) - ord('a')] -= 1
    
    # Check if all frequencies are zero
    for count in freq:
        if count != 0:
            return False
    
    return True
```

### Python (Sorting)

```python
def isAnagram(s, t):
    # If the lengths are different, they can't be anagrams
    if len(s) != len(t):
        return False
    
    # Sort both strings and compare
    return sorted(s) == sorted(t)
```

### JavaScript (Character Counting)

```javascript
function isAnagram(s, t) {
    // If the lengths are different, they can't be anagrams
    if (s.length !== t.length) {
        return false;
    }
    
    // Initialize a frequency map
    const freq = {};
    
    // Increment frequencies for characters in s
    for (let char of s) {
        freq[char] = (freq[char] || 0) + 1;
    }
    
    // Decrement frequencies for characters in t
    for (let char of t) {
        // If the character doesn't exist in the map or its frequency is already 0,
        // the strings are not anagrams
        if (!freq[char]) {
            return false;
        }
        freq[char]--;
    }
    
    // Check if all frequencies are zero
    for (let char in freq) {
        if (freq[char] !== 0) {
            return false;
        }
    }
    
    return true;
}
```

### JavaScript (Sorting)

```javascript
function isAnagram(s, t) {
    // If the lengths are different, they can't be anagrams
    if (s.length !== t.length) {
        return false;
    }
    
    // Sort both strings and compare
    return s.split('').sort().join('') === t.split('').sort().join('');
}
```

## Time and Space Complexity

### Character Counting Approach:
- **Time Complexity**: O(n), where n is the length of the strings. We iterate through each string once.
- **Space Complexity**: O(1), as we use a fixed-size array of 26 elements (for lowercase English letters) regardless of the input size.

### Sorting Approach:
- **Time Complexity**: O(n log n), where n is the length of the strings. Sorting has a time complexity of O(n log n).
- **Space Complexity**: O(n), as we need to create sorted copies of the strings.

## Detailed Walkthrough with Example

Let's trace through the character counting approach with Example 1: `s = "anagram", t = "nagaram"`

```
Check if lengths are equal:
len(s) = 7, len(t) = 7, so continue.

Initialize freq = [0, 0, 0, ..., 0] (26 zeros)

Process s = "anagram":
s[0] = 'a': freq[0] = 1
s[1] = 'n': freq[13] = 1
s[2] = 'a': freq[0] = 2
s[3] = 'g': freq[6] = 1
s[4] = 'r': freq[17] = 1
s[5] = 'a': freq[0] = 3
s[6] = 'm': freq[12] = 1

After processing s, freq = [3, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0]

Process t = "nagaram":
t[0] = 'n': freq[13] = 0
t[1] = 'a': freq[0] = 2
t[2] = 'g': freq[6] = 0
t[3] = 'a': freq[0] = 1
t[4] = 'r': freq[17] = 0
t[5] = 'a': freq[0] = 0
t[6] = 'm': freq[12] = 0

After processing t, freq = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]

Check if all frequencies are zero:
All frequencies are zero, so return true.
```

## Edge Cases and Optimizations

- **Empty Strings**: If both strings are empty, they are anagrams of each other.
- **Different Lengths**: If the strings have different lengths, they can't be anagrams.
- **Case Sensitivity**: The problem states that the strings contain only lowercase English letters, so we don't need to handle case sensitivity.
- **Unicode Characters**: If the strings can contain Unicode characters, we would need to use a hash map instead of a fixed-size array.

**Optimization**: We can optimize the character counting approach by returning `false` as soon as we encounter a character in the second string that doesn't exist in the frequency map or has a frequency of 0. This allows us to exit early in many cases.

## Common Mistakes

1. **Not checking string lengths**: Always check if the strings have the same length before proceeding.
2. **Using the wrong data structure**: For lowercase English letters, a fixed-size array is more efficient than a hash map.
3. **Not handling case sensitivity**: If the problem doesn't specify case sensitivity, clarify whether the comparison should be case-sensitive or not.

## Related Problems

1. **Group Anagrams**: Group a list of strings such that all anagrams are in the same group.
2. **Valid Palindrome**: Determine if a string is a palindrome, considering only alphanumeric characters and ignoring case.
3. **Find All Anagrams in a String**: Find all start indices of anagrams of a pattern in a string.
4. **Permutation in String**: Determine if one string is a permutation of a substring of another string. 