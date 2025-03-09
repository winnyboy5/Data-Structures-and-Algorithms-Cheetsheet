# Encode and Decode Strings

## Problem Statement

Design an algorithm to encode a list of strings to a string. The encoded string is then sent over the network and is decoded back to the original list of strings.

Implement the `encode` and `decode` methods:

- `encode(strs)`: Encodes a list of strings to a single string.
- `decode(s)`: Decodes a single string to a list of strings.

**Example 1:**
```
Input: strs = ["Hello", "World"]
Output: ["Hello", "World"]
Explanation:
Machine 1:
Codec encoder = new Codec();
String encoded_string = encoder.encode(strs);

Machine 2:
Codec decoder = new Codec();
String[] decoded_string = decoder.decode(encoded_string);
// decoded_string = ["Hello", "World"]
```

**Example 2:**
```
Input: strs = [""]
Output: [""]
```

**Example 3:**
```
Input: strs = ["Hello", "World", ""]
Output: ["Hello", "World", ""]
```

**Constraints:**
- 1 <= strs.length <= 200
- 0 <= strs[i].length <= 200
- strs[i] contains any possible characters out of 256 valid ASCII characters.

## Intuition

The key challenge in this problem is to design an encoding scheme that can handle any possible character, including delimiters, and still be able to decode the string back to the original list of strings.

There are several approaches to solve this problem:

1. **Length-Based Encoding**: Prefix each string with its length, followed by a delimiter.
2. **Escape Character Encoding**: Use an escape character to handle special characters.
3. **Non-ASCII Delimiter**: Use a character outside the ASCII range as a delimiter.

The length-based encoding is the most robust approach, as it can handle any character without ambiguity.

## Visual Explanation

Let's visualize the length-based encoding approach with Example 1: `strs = ["Hello", "World"]`

```
Step 1: Encode the list of strings.
        strs = ["Hello", "World"]
        
        For "Hello":
        - Length: 5
        - Encoded: "5#Hello"
        
        For "World":
        - Length: 5
        - Encoded: "5#World"
        
        Combined: "5#Hello5#World"

Step 2: Decode the encoded string.
        encoded = "5#Hello5#World"
        
        Parse "5#" -> Length: 5
        Extract 5 characters: "Hello"
        Remaining: "5#World"
        
        Parse "5#" -> Length: 5
        Extract 5 characters: "World"
        Remaining: ""
        
        Result: ["Hello", "World"]
```

Let's visualize with Example 3: `strs = ["Hello", "World", ""]`

```
Step 1: Encode the list of strings.
        strs = ["Hello", "World", ""]
        
        For "Hello":
        - Length: 5
        - Encoded: "5#Hello"
        
        For "World":
        - Length: 5
        - Encoded: "5#World"
        
        For "":
        - Length: 0
        - Encoded: "0#"
        
        Combined: "5#Hello5#World0#"

Step 2: Decode the encoded string.
        encoded = "5#Hello5#World0#"
        
        Parse "5#" -> Length: 5
        Extract 5 characters: "Hello"
        Remaining: "5#World0#"
        
        Parse "5#" -> Length: 5
        Extract 5 characters: "World"
        Remaining: "0#"
        
        Parse "0#" -> Length: 0
        Extract 0 characters: ""
        Remaining: ""
        
        Result: ["Hello", "World", ""]
```

![Encode and Decode Strings Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

### Length-Based Encoding

1. **Encode**:
   - For each string in the list, compute its length.
   - Append the length, followed by a delimiter (e.g., "#"), followed by the string itself.
   - Concatenate all encoded strings to form the final encoded string.

2. **Decode**:
   - Initialize an empty result list.
   - Iterate through the encoded string:
     - Parse the length by reading characters until the delimiter.
     - Extract the specified number of characters after the delimiter.
     - Add the extracted string to the result list.
     - Move to the next encoded string.
   - Return the result list.

## Code Implementation

### Python

```python
class Codec:
    def encode(self, strs):
        """Encodes a list of strings to a single string.
        
        :type strs: List[str]
        :rtype: str
        """
        # Use length-based encoding
        encoded = ""
        for s in strs:
            encoded += str(len(s)) + "#" + s
        return encoded
    
    def decode(self, s):
        """Decodes a single string to a list of strings.
        
        :type s: str
        :rtype: List[str]
        """
        result = []
        i = 0
        
        while i < len(s):
            # Find the delimiter
            j = i
            while s[j] != '#':
                j += 1
            
            # Parse the length
            length = int(s[i:j])
            
            # Extract the string
            start = j + 1
            end = start + length
            result.append(s[start:end])
            
            # Move to the next encoded string
            i = end
        
        return result
```

### JavaScript

```javascript
/**
 * Encodes a list of strings to a single string.
 *
 * @param {string[]} strs
 * @return {string}
 */
var encode = function(strs) {
    // Use length-based encoding
    let encoded = "";
    for (const s of strs) {
        encoded += s.length + "#" + s;
    }
    return encoded;
};

/**
 * Decodes a single string to a list of strings.
 *
 * @param {string} s
 * @return {string[]}
 */
var decode = function(s) {
    const result = [];
    let i = 0;
    
    while (i < s.length) {
        // Find the delimiter
        let j = i;
        while (s[j] !== '#') {
            j++;
        }
        
        // Parse the length
        const length = parseInt(s.substring(i, j));
        
        // Extract the string
        const start = j + 1;
        const end = start + length;
        result.push(s.substring(start, end));
        
        // Move to the next encoded string
        i = end;
    }
    
    return result;
};
```

## Alternative Approach: Chunked Transfer Encoding

Another approach is to use chunked transfer encoding, similar to HTTP chunked encoding:

### Python

```python
class Codec:
    def encode(self, strs):
        """Encodes a list of strings to a single string.
        
        :type strs: List[str]
        :rtype: str
        """
        # Use chunked transfer encoding
        encoded = ""
        for s in strs:
            encoded += hex(len(s))[2:] + "\n" + s + "\n"
        encoded += "0\n"
        return encoded
    
    def decode(self, s):
        """Decodes a single string to a list of strings.
        
        :type s: str
        :rtype: List[str]
        """
        result = []
        lines = s.split("\n")
        i = 0
        
        while i < len(lines):
            # Parse the length
            length = int(lines[i], 16)
            
            # Check if we've reached the end
            if length == 0:
                break
            
            # Extract the string
            result.append(lines[i + 1])
            
            # Move to the next chunk
            i += 2
        
        return result
```

### JavaScript

```javascript
/**
 * Encodes a list of strings to a single string.
 *
 * @param {string[]} strs
 * @return {string}
 */
var encode = function(strs) {
    // Use chunked transfer encoding
    let encoded = "";
    for (const s of strs) {
        encoded += s.length.toString(16) + "\n" + s + "\n";
    }
    encoded += "0\n";
    return encoded;
};

/**
 * Decodes a single string to a list of strings.
 *
 * @param {string} s
 * @return {string[]}
 */
var decode = function(s) {
    const result = [];
    const lines = s.split("\n");
    let i = 0;
    
    while (i < lines.length) {
        // Parse the length
        const length = parseInt(lines[i], 16);
        
        // Check if we've reached the end
        if (length === 0) {
            break;
        }
        
        // Extract the string
        result.push(lines[i + 1]);
        
        // Move to the next chunk
        i += 2;
    }
    
    return result;
};
```

## Time and Space Complexity

### Length-Based Encoding:
- **Time Complexity**: 
  - Encode: O(n), where n is the total length of all strings in the input list.
  - Decode: O(n), where n is the length of the encoded string.
- **Space Complexity**: 
  - Encode: O(n), where n is the total length of all strings in the input list.
  - Decode: O(n), where n is the length of the encoded string.

### Chunked Transfer Encoding:
- **Time Complexity**: 
  - Encode: O(n), where n is the total length of all strings in the input list.
  - Decode: O(n), where n is the length of the encoded string.
- **Space Complexity**: 
  - Encode: O(n), where n is the total length of all strings in the input list.
  - Decode: O(n), where n is the length of the encoded string.

## Detailed Walkthrough with Example

Let's trace through the length-based encoding approach with Example 3: `strs = ["Hello", "World", ""]`

```
Encode:
- strs = ["Hello", "World", ""]
- For "Hello": length = 5, encoded = "5#Hello"
- For "World": length = 5, encoded = "5#World"
- For "": length = 0, encoded = "0#"
- Combined: "5#Hello5#World0#"

Decode:
- encoded = "5#Hello5#World0#"
- i = 0
  - j = 0, s[j] = '5' != '#', j = 1
  - j = 1, s[j] = '#', length = 5
  - start = 2, end = 7, result = ["Hello"]
  - i = 7
- i = 7
  - j = 7, s[j] = '5' != '#', j = 8
  - j = 8, s[j] = '#', length = 5
  - start = 9, end = 14, result = ["Hello", "World"]
  - i = 14
- i = 14
  - j = 14, s[j] = '0' != '#', j = 15
  - j = 15, s[j] = '#', length = 0
  - start = 16, end = 16, result = ["Hello", "World", ""]
  - i = 16
- i = 16 >= len(s) = 16, exit loop
- Return result = ["Hello", "World", ""]
```

## Edge Cases and Optimizations

- **Empty List**: The encoding should handle an empty list of strings.
- **Empty String**: The encoding should handle empty strings in the list.
- **Special Characters**: The encoding should handle strings with special characters, including the delimiter itself.

**Optimization**: For the length-based encoding, we can use a more compact representation for the length, such as a fixed-width field or a variable-length encoding like LEB128.

## Common Mistakes

1. **Using a simple delimiter**: Using a simple delimiter like a comma or a pipe character can lead to ambiguity if the strings themselves contain the delimiter.
2. **Not handling empty strings**: Make sure to handle empty strings correctly in both encoding and decoding.
3. **Not parsing the length correctly**: Be careful when parsing the length from the encoded string, especially when dealing with multi-digit lengths.

## Related Problems

1. **Serialize and Deserialize Binary Tree**: Design an algorithm to serialize and deserialize a binary tree.
2. **Serialize and Deserialize BST**: Design an algorithm to serialize and deserialize a binary search tree.
3. **Design a File System**: Design an in-memory file system to simulate the following functions: ls, mkdir, addContentToFile, readContentFromFile.
4. **LRU Cache**: Design and implement a data structure for Least Recently Used (LRU) cache.
5. **LFU Cache**: Design and implement a data structure for Least Frequently Used (LFU) cache. 