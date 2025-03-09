# Implement Trie (Prefix Tree)

## Problem Statement

A trie (pronounced as "try") or prefix tree is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the Trie class:

- `Trie()` Initializes the trie object.
- `void insert(String word)` Inserts the string `word` into the trie.
- `boolean search(String word)` Returns `true` if the string `word` is in the trie (i.e., was inserted before), and `false` otherwise.
- `boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the prefix `prefix`, and `false` otherwise.

**Example:**

```
Input:
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
Output:
[null, null, true, false, true, null, true]

Explanation:
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // return True
trie.search("app");     // return False
trie.startsWith("app"); // return True
trie.insert("app");
trie.search("app");     // return True
```

## Intuition

A Trie is a tree-like data structure where each node represents a single character of a word. The path from the root to a particular node forms a prefix or a complete word. Each node typically has multiple children, one for each possible character that can follow the current character in a word.

The key insights for implementing a Trie are:
1. Each node in the Trie represents a character.
2. Each node can have multiple children, one for each possible next character.
3. Some nodes are marked as "end of word" to indicate that the path from the root to this node forms a complete word.

This structure allows for efficient prefix-based operations, such as checking if a word exists or finding all words with a given prefix.

## Visual Explanation

Let's visualize a Trie with the words "apple" and "app" inserted:

```
       root
      /
     a
    /
   p
  / \
 p   p
/     \
l      l
|      |
e      e
```

In this visualization:
- Each node represents a character.
- The path from the root to a node spells out a prefix or a complete word.
- Nodes marked with a double circle (or in our implementation, with `is_end_of_word = True`) indicate the end of a word.

Let's trace through the operations in the example:

1. `trie.insert("apple")`:
   - Create nodes for 'a', 'p', 'p', 'l', 'e'.
   - Mark the 'e' node as the end of a word.

2. `trie.search("apple")`:
   - Follow the path from the root: 'a' -> 'p' -> 'p' -> 'l' -> 'e'.
   - Check if the 'e' node is marked as the end of a word. It is, so return `True`.

3. `trie.search("app")`:
   - Follow the path from the root: 'a' -> 'p' -> 'p'.
   - Check if the 'p' node is marked as the end of a word. It's not, so return `False`.

4. `trie.startsWith("app")`:
   - Follow the path from the root: 'a' -> 'p' -> 'p'.
   - The path exists, so return `True`.

5. `trie.insert("app")`:
   - Follow the path from the root: 'a' -> 'p' -> 'p'.
   - Mark the 'p' node as the end of a word.

6. `trie.search("app")`:
   - Follow the path from the root: 'a' -> 'p' -> 'p'.
   - Check if the 'p' node is marked as the end of a word. It is now, so return `True`.

![Trie Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Define a `TrieNode` class with:
   - A dictionary/map to store children nodes, where the key is a character and the value is another `TrieNode`.
   - A boolean flag to indicate if the node represents the end of a word.

2. Define a `Trie` class with:
   - A constructor that initializes the root node.
   - An `insert` method that adds a word to the trie.
   - A `search` method that checks if a word exists in the trie.
   - A `startsWith` method that checks if there's any word in the trie that starts with a given prefix.

3. For the `insert` method:
   - Start from the root node.
   - For each character in the word, check if the character exists as a child of the current node.
   - If not, create a new node for that character.
   - Move to the child node.
   - After processing all characters, mark the last node as the end of a word.

4. For the `search` method:
   - Start from the root node.
   - For each character in the word, check if the character exists as a child of the current node.
   - If not, return `False`.
   - Move to the child node.
   - After processing all characters, check if the last node is marked as the end of a word. If yes, return `True`; otherwise, return `False`.

5. For the `startsWith` method:
   - Similar to the `search` method, but we don't need to check if the last node is marked as the end of a word.
   - If we can traverse the trie for all characters in the prefix, return `True`; otherwise, return `False`.

## Code Implementation

### Python

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True
    
    def search(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end_of_word
    
    def startsWith(self, prefix):
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True
```

### JavaScript

```javascript
class TrieNode {
    constructor() {
        this.children = {};
        this.isEndOfWord = false;
    }
}

class Trie {
    constructor() {
        this.root = new TrieNode();
    }
    
    insert(word) {
        let node = this.root;
        for (const char of word) {
            if (!(char in node.children)) {
                node.children[char] = new TrieNode();
            }
            node = node.children[char];
        }
        node.isEndOfWord = true;
    }
    
    search(word) {
        let node = this.root;
        for (const char of word) {
            if (!(char in node.children)) {
                return false;
            }
            node = node.children[char];
        }
        return node.isEndOfWord;
    }
    
    startsWith(prefix) {
        let node = this.root;
        for (const char of prefix) {
            if (!(char in node.children)) {
                return false;
            }
            node = node.children[char];
        }
        return true;
    }
}
```

## Time and Space Complexity

### Time Complexity:
- **Insert**: O(m), where m is the length of the word being inserted. We need to traverse or create a node for each character in the word.
- **Search**: O(m), where m is the length of the word being searched. We need to traverse a node for each character in the word.
- **StartsWith**: O(m), where m is the length of the prefix. We need to traverse a node for each character in the prefix.

### Space Complexity:
- **Overall**: O(n * m), where n is the number of words and m is the average length of the words. In the worst case, if there are no common prefixes among the words, we would need to store n * m nodes.

## Detailed Walkthrough with Example

Let's trace through the operations in the example step by step:

1. `trie = new Trie()`:
   - Initialize a new Trie with an empty root node.

2. `trie.insert("apple")`:
   - Start at the root node.
   - For 'a': Create a new node for 'a' and move to it.
   - For 'p': Create a new node for 'p' and move to it.
   - For 'p': Create a new node for 'p' and move to it.
   - For 'l': Create a new node for 'l' and move to it.
   - For 'e': Create a new node for 'e' and move to it.
   - Mark the 'e' node as the end of a word.

3. `trie.search("apple")`:
   - Start at the root node.
   - For 'a': Move to the 'a' node.
   - For 'p': Move to the 'p' node.
   - For 'p': Move to the 'p' node.
   - For 'l': Move to the 'l' node.
   - For 'e': Move to the 'e' node.
   - Check if the 'e' node is marked as the end of a word. It is, so return `True`.

4. `trie.search("app")`:
   - Start at the root node.
   - For 'a': Move to the 'a' node.
   - For 'p': Move to the 'p' node.
   - For 'p': Move to the 'p' node.
   - Check if the 'p' node is marked as the end of a word. It's not, so return `False`.

5. `trie.startsWith("app")`:
   - Start at the root node.
   - For 'a': Move to the 'a' node.
   - For 'p': Move to the 'p' node.
   - For 'p': Move to the 'p' node.
   - The path exists, so return `True`.

6. `trie.insert("app")`:
   - Start at the root node.
   - For 'a': Move to the 'a' node.
   - For 'p': Move to the 'p' node.
   - For 'p': Move to the 'p' node.
   - Mark the 'p' node as the end of a word.

7. `trie.search("app")`:
   - Start at the root node.
   - For 'a': Move to the 'a' node.
   - For 'p': Move to the 'p' node.
   - For 'p': Move to the 'p' node.
   - Check if the 'p' node is marked as the end of a word. It is now, so return `True`.

## Alternative Approaches

### Using an Array for Children

Instead of using a dictionary/map to store children, we can use an array of size 26 (for lowercase English letters) or 128 (for ASCII characters). This can be more efficient in terms of lookup time but might waste space if the trie is sparse.

```python
class TrieNode:
    def __init__(self):
        self.children = [None] * 26  # Assuming only lowercase English letters
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def _char_to_index(self, char):
        return ord(char) - ord('a')
    
    def insert(self, word):
        node = self.root
        for char in word:
            index = self._char_to_index(char)
            if not node.children[index]:
                node.children[index] = TrieNode()
            node = node.children[index]
        node.is_end_of_word = True
    
    def search(self, word):
        node = self.root
        for char in word:
            index = self._char_to_index(char)
            if not node.children[index]:
                return False
            node = node.children[index]
        return node.is_end_of_word
    
    def startsWith(self, prefix):
        node = self.root
        for char in prefix:
            index = self._char_to_index(char)
            if not node.children[index]:
                return False
            node = node.children[index]
        return True
```

## Edge Cases and Optimizations

- **Empty String**: The code handles empty strings correctly. An empty string is a valid word if the root node is marked as the end of a word.
- **Case Sensitivity**: The implementation is case-sensitive. If case-insensitive operations are required, we can convert all characters to lowercase before processing.
- **Memory Optimization**: If memory is a concern, we can use a more compact representation for nodes with few children, such as a linked list instead of a dictionary or array.

## Common Mistakes

1. **Not marking the end of a word**: A common mistake is to forget to mark the last node of a word as the end of a word, which would make the `search` method always return `False`.
2. **Confusing `search` and `startsWith`**: The `search` method checks if a word exists in the trie, while the `startsWith` method checks if there's any word in the trie that starts with a given prefix.
3. **Not handling edge cases**: Make sure to handle edge cases like empty strings or characters that are not in the expected range.

## Related Problems

1. **Add and Search Word - Data structure design**: Extend the Trie to support wildcard search.
2. **Word Search II**: Use a Trie to efficiently find all words from a dictionary in a 2D board.
3. **Replace Words**: Replace words in a sentence with their root prefix using a Trie.
4. **Implement Magic Dictionary**: Implement a dictionary that supports searching for a word with one character replaced.
5. **Design Search Autocomplete System**: Design a search autocomplete system for a search engine. 