# Group Anagrams

## Problem Statement

Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**
```
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

**Example 2:**
```
Input: strs = [""]
Output: [[""]]
```

**Example 3:**
```
Input: strs = ["a"]
Output: [["a"]]
```

## Intuition

The key insight for this problem is to recognize that anagrams have the same characters with the same frequencies. We need to group strings that are anagrams of each other. There are several approaches to solve this problem:

1. **Sorted String as Key**: Sort each string and use the sorted string as a key in a hash map. Strings that are anagrams of each other will have the same sorted string.

2. **Character Count as Key**: Count the frequency of each character in the string and use this count as a key in a hash map. Strings that are anagrams of each other will have the same character count.

The sorted string approach is more straightforward and easier to implement, but the character count approach can be more efficient for longer strings.

## Visual Explanation

Let's visualize the sorted string approach with Example 1:

```
strs = ["eat", "tea", "tan", "ate", "nat", "bat"]

Step 1: Sort each string and use it as a key in a hash map.
"eat" -> sort -> "aet" (key)
"tea" -> sort -> "aet" (key)
"tan" -> sort -> "ant" (key)
"ate" -> sort -> "aet" (key)
"nat" -> sort -> "ant" (key)
"bat" -> sort -> "abt" (key)

Step 2: Group strings by their sorted keys.
{
  "aet": ["eat", "tea", "ate"],
  "ant": ["tan", "nat"],
  "abt": ["bat"]
}

Step 3: Return the groups as a list of lists.
[["eat", "tea", "ate"], ["tan", "nat"], ["bat"]]
```

Let's also visualize the character count approach:

```
strs = ["eat", "tea", "tan", "ate", "nat", "bat"]

Step 1: Count the frequency of each character in each string and use it as a key.
"eat" -> {a:1, e:1, t:1} -> "a1e1t1" (key)
"tea" -> {a:1, e:1, t:1} -> "a1e1t1" (key)
"tan" -> {a:1, n:1, t:1} -> "a1n1t1" (key)
"ate" -> {a:1, e:1, t:1} -> "a1e1t1" (key)
"nat" -> {a:1, n:1, t:1} -> "a1n1t1" (key)
"bat" -> {a:1, b:1, t:1} -> "a1b1t1" (key)

Step 2: Group strings by their character count keys.
{
  "a1e1t1": ["eat", "tea", "ate"],
  "a1n1t1": ["tan", "nat"],
  "a1b1t1": ["bat"]
}

Step 3: Return the groups as a list of lists.
[["eat", "tea", "ate"], ["tan", "nat"], ["bat"]]
```

![Group Anagrams Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

### Sorted String Approach:
1. Initialize an empty hash map where the keys will be sorted strings and the values will be lists of original strings.
2. Iterate through each string in the input array:
   - Sort the characters of the string to get a key.
   - If the key exists in the hash map, append the original string to the corresponding list.
   - If the key doesn't exist, create a new entry with the key and a list containing the original string.
3. Return the values of the hash map as a list of lists.

### Character Count Approach:
1. Initialize an empty hash map where the keys will be character count strings and the values will be lists of original strings.
2. Iterate through each string in the input array:
   - Count the frequency of each character in the string.
   - Convert the character count to a string representation (e.g., "a1b2c3").
   - If the key exists in the hash map, append the original string to the corresponding list.
   - If the key doesn't exist, create a new entry with the key and a list containing the original string.
3. Return the values of the hash map as a list of lists.

## Code Implementation

### Python (Sorted String Approach)

```python
def groupAnagrams(strs):
    # Initialize a hash map to store groups of anagrams
    anagram_groups = {}
    
    # Iterate through each string in the input array
    for s in strs:
        # Sort the characters of the string to get a key
        sorted_s = ''.join(sorted(s))
        
        # If the key exists, append the original string to the corresponding list
        # If the key doesn't exist, create a new entry
        if sorted_s in anagram_groups:
            anagram_groups[sorted_s].append(s)
        else:
            anagram_groups[sorted_s] = [s]
    
    # Return the values of the hash map as a list of lists
    return list(anagram_groups.values())
```

### Python (Character Count Approach)

```python
def groupAnagrams(strs):
    # Initialize a hash map to store groups of anagrams
    anagram_groups = {}
    
    # Iterate through each string in the input array
    for s in strs:
        # Count the frequency of each character
        count = [0] * 26
        for char in s:
            count[ord(char) - ord('a')] += 1
        
        # Convert the count to a tuple (immutable) to use as a key
        count_tuple = tuple(count)
        
        # If the key exists, append the original string to the corresponding list
        # If the key doesn't exist, create a new entry
        if count_tuple in anagram_groups:
            anagram_groups[count_tuple].append(s)
        else:
            anagram_groups[count_tuple] = [s]
    
    # Return the values of the hash map as a list of lists
    return list(anagram_groups.values())
```

### JavaScript (Sorted String Approach)

```javascript
function groupAnagrams(strs) {
    // Initialize a hash map to store groups of anagrams
    const anagramGroups = new Map();
    
    // Iterate through each string in the input array
    for (let s of strs) {
        // Sort the characters of the string to get a key
        const sortedS = s.split('').sort().join('');
        
        // If the key exists, append the original string to the corresponding list
        // If the key doesn't exist, create a new entry
        if (anagramGroups.has(sortedS)) {
            anagramGroups.get(sortedS).push(s);
        } else {
            anagramGroups.set(sortedS, [s]);
        }
    }
    
    // Return the values of the hash map as an array of arrays
    return Array.from(anagramGroups.values());
}
```

### JavaScript (Character Count Approach)

```javascript
function groupAnagrams(strs) {
    // Initialize a hash map to store groups of anagrams
    const anagramGroups = new Map();
    
    // Iterate through each string in the input array
    for (let s of strs) {
        // Count the frequency of each character
        const count = new Array(26).fill(0);
        for (let char of s) {
            count[char.charCodeAt(0) - 'a'.charCodeAt(0)]++;
        }
        
        // Convert the count to a string to use as a key
        const countKey = count.join('#');
        
        // If the key exists, append the original string to the corresponding list
        // If the key doesn't exist, create a new entry
        if (anagramGroups.has(countKey)) {
            anagramGroups.get(countKey).push(s);
        } else {
            anagramGroups.set(countKey, [s]);
        }
    }
    
    // Return the values of the hash map as an array of arrays
    return Array.from(anagramGroups.values());
}
```

## Time and Space Complexity

### Sorted String Approach:
- **Time Complexity**: O(n * k * log k), where n is the number of strings and k is the maximum length of a string. Sorting each string takes O(k * log k) time.
- **Space Complexity**: O(n * k), where n is the number of strings and k is the maximum length of a string. We need to store all the strings in the hash map.

### Character Count Approach:
- **Time Complexity**: O(n * k), where n is the number of strings and k is the maximum length of a string. Counting the characters in each string takes O(k) time.
- **Space Complexity**: O(n * k), where n is the number of strings and k is the maximum length of a string. We need to store all the strings in the hash map.

## Detailed Walkthrough with Example

Let's trace through the sorted string approach with Example 1: `strs = ["eat", "tea", "tan", "ate", "nat", "bat"]`

```
Initialize anagram_groups = {}

Process "eat":
  sorted_s = "aet"
  anagram_groups = {"aet": ["eat"]}

Process "tea":
  sorted_s = "aet"
  anagram_groups = {"aet": ["eat", "tea"]}

Process "tan":
  sorted_s = "ant"
  anagram_groups = {"aet": ["eat", "tea"], "ant": ["tan"]}

Process "ate":
  sorted_s = "aet"
  anagram_groups = {"aet": ["eat", "tea", "ate"], "ant": ["tan"]}

Process "nat":
  sorted_s = "ant"
  anagram_groups = {"aet": ["eat", "tea", "ate"], "ant": ["tan", "nat"]}

Process "bat":
  sorted_s = "abt"
  anagram_groups = {"aet": ["eat", "tea", "ate"], "ant": ["tan", "nat"], "abt": ["bat"]}

Return list(anagram_groups.values()) = [["eat", "tea", "ate"], ["tan", "nat"], ["bat"]]
```

## Edge Cases and Optimizations

- **Empty Array**: If the input array is empty, we should return an empty list.
- **Empty String**: If the input array contains an empty string, it will be grouped with other empty strings (if any).
- **Single Character Strings**: If the input array contains single character strings, they will be grouped with other strings that have the same character.

**Optimization**: For the character count approach, instead of converting the count array to a string or tuple, we can use a custom hash function that combines the character counts in a way that uniquely identifies the anagram group. This can reduce the space complexity.

## Common Mistakes

1. **Not handling empty strings**: Make sure to handle empty strings correctly.
2. **Using the wrong data structure**: For grouping anagrams, a hash map is the most efficient data structure.
3. **Not considering the time complexity of sorting**: Sorting each string can be expensive for long strings. The character count approach can be more efficient in such cases.

## Related Problems

1. **Valid Anagram**: Determine if two strings are anagrams of each other.
2. **Find All Anagrams in a String**: Find all start indices of anagrams of a pattern in a string.
3. **Permutation in String**: Determine if one string is a permutation of a substring of another string.
4. **Group Shifted Strings**: Group strings that can be shifted to match each other. 