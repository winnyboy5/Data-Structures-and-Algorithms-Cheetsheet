# Tries

A trie (pronounced "try" or "tree") is a tree-like data structure used to store a dynamic set of strings. Tries are particularly useful for implementing dictionaries, autocomplete features, and spell-checking algorithms.

## Visual Representation

```
Trie containing the words "cat", "car", "dog", "door":

             ┌───┐
             │   │ (root)
             └─┬─┘
        ┌──────┴───────┐
     ┌──┴──┐        ┌──┴──┐
     │  c  │        │  d  │
     └──┬──┘        └──┬──┘
        │              │
     ┌──┴──┐        ┌──┴──┐
     │  a  │        │  o  │
     └──┬──┘        └──┬──┘
        │              │
    ┌───┴───┐      ┌───┴───┐
┌───┴──┐ ┌──┴───┐ │   g   │ │   o   │
│  t*  │ │  r*  │ └───┬───┘ └───┬───┘
└──────┘ └──────┘     │         │
                   ┌──┴───┐  ┌──┴───┐
                   │  *   │  │  r*  │
                   └──────┘  └──────┘
```

*Note: Nodes marked with * indicate the end of a word.*

## Key Operations

1. **Insert**: Add a word to the trie
2. **Search**: Check if a word exists in the trie
3. **Delete**: Remove a word from the trie
4. **Prefix Search**: Find all words with a given prefix
5. **Autocomplete**: Suggest completions for a given prefix

## Implementation in Python and JavaScript

### Python

```python
class TrieNode:
    def __init__(self):
        self.children = {}  # Dictionary to store child nodes
        self.is_end_of_word = False  # Flag to mark the end of a word

class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word):
        """Insert a word into the trie."""
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True
    
    def search(self, word):
        """Search for a word in the trie. Returns True if the word exists."""
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end_of_word
    
    def starts_with(self, prefix):
        """Check if there is any word in the trie that starts with the given prefix."""
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True
    
    def get_words_with_prefix(self, prefix):
        """Get all words in the trie with the given prefix."""
        result = []
        node = self.root
        
        # Navigate to the node corresponding to the prefix
        for char in prefix:
            if char not in node.children:
                return result
            node = node.children[char]
        
        # Collect all words starting from the prefix node
        self._collect_words(node, prefix, result)
        return result
    
    def _collect_words(self, node, prefix, result):
        """Helper function to collect all words with a given prefix."""
        if node.is_end_of_word:
            result.append(prefix)
        
        for char, child_node in node.children.items():
            self._collect_words(child_node, prefix + char, result)
    
    def delete(self, word):
        """Delete a word from the trie."""
        self._delete_recursive(self.root, word, 0)
    
    def _delete_recursive(self, node, word, index):
        """Helper function to delete a word from the trie."""
        # Base case: reached the end of the word
        if index == len(word):
            # Word doesn't exist in the trie
            if not node.is_end_of_word:
                return False
            
            # Mark the node as not the end of a word
            node.is_end_of_word = False
            
            # Return True if the node has no children, indicating it can be deleted
            return len(node.children) == 0
        
        char = word[index]
        
        # Word doesn't exist in the trie
        if char not in node.children:
            return False
        
        # Recursively delete the rest of the word
        should_delete_child = self._delete_recursive(node.children[char], word, index + 1)
        
        # If the child node should be deleted
        if should_delete_child:
            del node.children[char]
            # Return True if this node has no children and is not the end of a word
            return len(node.children) == 0 and not node.is_end_of_word
        
        return False

# Example usage
trie = Trie()
words = ["cat", "car", "dog", "door", "done"]
for word in words:
    trie.insert(word)

print(trie.search("cat"))    # Output: True
print(trie.search("can"))    # Output: False
print(trie.starts_with("ca"))  # Output: True
print(trie.get_words_with_prefix("do"))  # Output: ['dog', 'door', 'done']

trie.delete("door")
print(trie.search("door"))   # Output: False
print(trie.search("dog"))    # Output: True
```

### JavaScript

```javascript
class TrieNode {
    constructor() {
        this.children = {};  // Object to store child nodes
        this.isEndOfWord = false;  // Flag to mark the end of a word
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
    
    getWordsWithPrefix(prefix) {
        const result = [];
        let node = this.root;
        
        // Navigate to the node corresponding to the prefix
        for (const char of prefix) {
            if (!(char in node.children)) {
                return result;
            }
            node = node.children[char];
        }
        
        // Collect all words starting from the prefix node
        this._collectWords(node, prefix, result);
        return result;
    }
    
    _collectWords(node, prefix, result) {
        if (node.isEndOfWord) {
            result.push(prefix);
        }
        
        for (const char in node.children) {
            this._collectWords(node.children[char], prefix + char, result);
        }
    }
    
    delete(word) {
        this._deleteRecursive(this.root, word, 0);
    }
    
    _deleteRecursive(node, word, index) {
        // Base case: reached the end of the word
        if (index === word.length) {
            // Word doesn't exist in the trie
            if (!node.isEndOfWord) {
                return false;
            }
            
            // Mark the node as not the end of a word
            node.isEndOfWord = false;
            
            // Return true if the node has no children, indicating it can be deleted
            return Object.keys(node.children).length === 0;
        }
        
        const char = word[index];
        
        // Word doesn't exist in the trie
        if (!(char in node.children)) {
            return false;
        }
        
        // Recursively delete the rest of the word
        const shouldDeleteChild = this._deleteRecursive(node.children[char], word, index + 1);
        
        // If the child node should be deleted
        if (shouldDeleteChild) {
            delete node.children[char];
            // Return true if this node has no children and is not the end of a word
            return Object.keys(node.children).length === 0 && !node.isEndOfWord;
        }
        
        return false;
    }
}

// Example usage
const trie = new Trie();
const words = ["cat", "car", "dog", "door", "done"];
for (const word of words) {
    trie.insert(word);
}

console.log(trie.search("cat"));    // Output: true
console.log(trie.search("can"));    // Output: false
console.log(trie.startsWith("ca"));  // Output: true
console.log(trie.getWordsWithPrefix("do"));  // Output: ['dog', 'door', 'done']

trie.delete("door");
console.log(trie.search("door"));   // Output: false
console.log(trie.search("dog"));    // Output: true
```

## Common Trie Applications

### 1. Autocomplete / Type-Ahead Feature

**Python:**
```python
def autocomplete(trie, prefix, max_suggestions=5):
    """Return a list of autocomplete suggestions for a given prefix."""
    suggestions = trie.get_words_with_prefix(prefix)
    return suggestions[:max_suggestions]

# Example usage
trie = Trie()
words = ["apple", "application", "apply", "banana", "ball", "cat", "category"]
for word in words:
    trie.insert(word)

print(autocomplete(trie, "app"))  # Output: ['apple', 'application', 'apply']
print(autocomplete(trie, "ba"))   # Output: ['banana', 'ball']
```

**JavaScript:**
```javascript
function autocomplete(trie, prefix, maxSuggestions = 5) {
    const suggestions = trie.getWordsWithPrefix(prefix);
    return suggestions.slice(0, maxSuggestions);
}

// Example usage
const trie = new Trie();
const words = ["apple", "application", "apply", "banana", "ball", "cat", "category"];
for (const word of words) {
    trie.insert(word);
}

console.log(autocomplete(trie, "app"));  // Output: ['apple', 'application', 'apply']
console.log(autocomplete(trie, "ba"));   // Output: ['banana', 'ball']
```

### 2. Spell Checker

**Python:**
```python
def is_valid_word(trie, word):
    """Check if a word is valid according to the dictionary."""
    return trie.search(word)

# Example usage
dictionary = Trie()
valid_words = ["apple", "banana", "cat", "dog", "elephant"]
for word in valid_words:
    dictionary.insert(word)

print(is_valid_word(dictionary, "apple"))    # Output: True
print(is_valid_word(dictionary, "appl"))     # Output: False
print(is_valid_word(dictionary, "applee"))   # Output: False
```

**JavaScript:**
```javascript
function isValidWord(trie, word) {
    return trie.search(word);
}

// Example usage
const dictionary = new Trie();
const validWords = ["apple", "banana", "cat", "dog", "elephant"];
for (const word of validWords) {
    dictionary.insert(word);
}

console.log(isValidWord(dictionary, "apple"));    // Output: true
console.log(isValidWord(dictionary, "appl"));     // Output: false
console.log(isValidWord(dictionary, "applee"));   // Output: false
```

### 3. Longest Common Prefix

**Python:**
```python
def longest_common_prefix(words):
    """Find the longest common prefix among a list of words."""
    if not words:
        return ""
    
    # Build a trie with all words
    trie = Trie()
    for word in words:
        trie.insert(word)
    
    # Start from the root and traverse as long as there's only one child
    prefix = ""
    node = trie.root
    
    while node and len(node.children) == 1 and not node.is_end_of_word:
        char = list(node.children.keys())[0]
        prefix += char
        node = node.children[char]
    
    return prefix

# Example usage
words1 = ["flower", "flow", "flight"]
words2 = ["dog", "racecar", "car"]
print(longest_common_prefix(words1))  # Output: "fl"
print(longest_common_prefix(words2))  # Output: ""
```

**JavaScript:**
```javascript
function longestCommonPrefix(words) {
    if (words.length === 0) {
        return "";
    }
    
    // Build a trie with all words
    const trie = new Trie();
    for (const word of words) {
        trie.insert(word);
    }
    
    // Start from the root and traverse as long as there's only one child
    let prefix = "";
    let node = trie.root;
    
    while (node && Object.keys(node.children).length === 1 && !node.isEndOfWord) {
        const char = Object.keys(node.children)[0];
        prefix += char;
        node = node.children[char];
    }
    
    return prefix;
}

// Example usage
const words1 = ["flower", "flow", "flight"];
const words2 = ["dog", "racecar", "car"];
console.log(longestCommonPrefix(words1));  // Output: "fl"
console.log(longestCommonPrefix(words2));  // Output: ""
```

## Time and Space Complexity

| Operation | Time Complexity | Space Complexity |
|-----------|----------------|-----------------|
| Insert    | O(m)           | O(m)            |
| Search    | O(m)           | O(1)            |
| Delete    | O(m)           | O(1)            |
| Prefix Search | O(n + m)   | O(n)            |

Where:
- m is the length of the word
- n is the number of words with the given prefix

## Advantages of Tries

1. **Fast Prefix Lookups**: O(m) time complexity for prefix operations, where m is the length of the prefix.
2. **Space Efficiency**: Tries can be more space-efficient than storing each word separately, especially when there are many common prefixes.
3. **Autocomplete**: Tries are ideal for implementing autocomplete features.
4. **Spell Checking**: Tries can be used to efficiently check if a word is valid.
5. **Sorting**: Words stored in a trie are automatically sorted lexicographically.

## Disadvantages of Tries

1. **Memory Usage**: Tries can use more memory than other data structures for storing a small number of words with few common prefixes.
2. **Implementation Complexity**: Tries are more complex to implement than simple arrays or hash tables.
3. **Not Suitable for All Applications**: Tries are specialized for string operations and may not be the best choice for other types of data.

## When to Use Tries

- When you need fast prefix lookups
- When implementing autocomplete or type-ahead features
- When building a spell checker
- When you need to find all words with a common prefix
- When you need to sort strings lexicographically
- When memory usage is not a critical constraint

## Common Trie Problems from Blind 75 and Grind 75

1. **Implement Trie (Prefix Tree)** (Medium): Implement a trie with insert, search, and startsWith methods.
2. **Word Search II** (Hard): Find all words in a board of characters using a trie.
3. **Design Add and Search Words Data Structure** (Medium): Design a data structure that supports adding new words and finding if a string matches any previously added string.
4. **Replace Words** (Medium): Replace words in a sentence with their root prefix.
5. **Maximum XOR of Two Numbers in an Array** (Medium): Find the maximum XOR of two numbers in an array using a trie.

## How to Identify Trie Problems

Look for these clues in the problem statement:

1. The problem involves a dictionary of words or strings.
2. The problem requires prefix matching or searching.
3. The problem involves autocomplete or type-ahead features.
4. The problem requires finding all words with a common prefix.
5. The problem involves spell checking or word validation.
6. Keywords like "prefix," "dictionary," "word," "autocomplete," or "type-ahead." 