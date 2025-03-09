# Add and Search Word - Data structure design

## Problem Statement

Design a data structure that supports adding new words and finding if a string matches any previously added string.

Implement the `WordDictionary` class:

- `WordDictionary()` Initializes the object.
- `void addWord(word)` Adds `word` to the data structure, it can be matched later.
- `bool search(word)` Returns `true` if there is any string in the data structure that matches `word`, or `false` otherwise. `word` may contain dots `'.'` where dots can be matched with any letter.

**Example:**
```
Input:
["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
[[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]

Output:
[null,null,null,null,false,true,true,true]

Explanation:
WordDictionary wordDictionary = new WordDictionary();
wordDictionary.addWord("bad");
wordDictionary.addWord("dad");
wordDictionary.addWord("mad");
wordDictionary.search("pad"); // return False
wordDictionary.search("bad"); // return True
wordDictionary.search(".ad"); // return True
wordDictionary.search("b.."); // return True
```

## Intuition

The key insight for this problem is to use a Trie (Prefix Tree) data structure, which is particularly efficient for dictionary operations. A Trie allows us to store words in a way that makes prefix-based searches very efficient.

The main challenge is handling the wildcard character `.` which can match any letter. To solve this, we need to modify the standard Trie search algorithm to explore all possible paths when a `.` is encountered.

## Visual Explanation

Let's visualize the Trie after adding the words "bad", "dad", and "mad":

```
        root
       /  |  \
      b   d   m
     /    |    \
    a     a     a
   /      |      \
  d       d       d
 /        |        \
*         *         *
```

Each path from the root to a leaf node with `*` represents a word in our dictionary.

Now, let's see how the search works:

1. Search for "pad":
   - Start at the root
   - Look for child 'p' → Not found
   - Return false

2. Search for "bad":
   - Start at the root
   - Look for child 'b' → Found
   - Look for child 'a' → Found
   - Look for child 'd' → Found
   - Check if it's the end of a word → Yes
   - Return true

3. Search for ".ad":
   - Start at the root
   - Look for any child (because of '.') → Found 'b', 'd', 'm'
   - For each child, look for child 'a' → All found
   - For each 'a', look for child 'd' → All found
   - Check if any path ends with a word → Yes
   - Return true

4. Search for "b..":
   - Start at the root
   - Look for child 'b' → Found
   - Look for any child (because of '.') → Found 'a'
   - Look for any child (because of '.') → Found 'd'
   - Check if it's the end of a word → Yes
   - Return true

![Trie Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Create a Trie data structure with the following components:
   - A `TrieNode` class with:
     - A dictionary/map to store children nodes (mapping characters to nodes)
     - A boolean flag to indicate if the node represents the end of a word
   - Methods to add a word and search for a word

2. For adding a word:
   - Start at the root of the Trie
   - For each character in the word:
     - If the character doesn't exist as a child, create a new node
     - Move to the child node
   - Mark the last node as the end of a word

3. For searching a word:
   - Start at the root of the Trie
   - For each character in the word:
     - If the character is a dot ('.'):
       - Recursively search all children with the remaining part of the word
       - If any path leads to a match, return true
     - If the character is not a dot:
       - If the character doesn't exist as a child, return false
       - Move to the child node
   - Return true if the current node marks the end of a word, false otherwise

## Code Implementation

### Python

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class WordDictionary:
    def __init__(self):
        self.root = TrieNode()

    def addWord(self, word: str) -> None:
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True

    def search(self, word: str) -> bool:
        def dfs(node, index):
            if index == len(word):
                return node.is_end_of_word
            
            char = word[index]
            if char == '.':
                for child in node.children.values():
                    if dfs(child, index + 1):
                        return True
                return False
            else:
                if char not in node.children:
                    return False
                return dfs(node.children[char], index + 1)
        
        return dfs(self.root, 0)
```

### JavaScript

```javascript
class TrieNode {
    constructor() {
        this.children = {};
        this.isEndOfWord = false;
    }
}

class WordDictionary {
    constructor() {
        this.root = new TrieNode();
    }
    
    addWord(word) {
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
        const dfs = (node, index) => {
            if (index === word.length) {
                return node.isEndOfWord;
            }
            
            const char = word[index];
            if (char === '.') {
                for (const child of Object.values(node.children)) {
                    if (dfs(child, index + 1)) {
                        return true;
                    }
                }
                return false;
            } else {
                if (!(char in node.children)) {
                    return false;
                }
                return dfs(node.children[char], index + 1);
            }
        };
        
        return dfs(this.root, 0);
    }
}
```

## Time and Space Complexity

### Time Complexity:
- **addWord**: O(m), where m is the length of the word. We need to traverse each character of the word once.
- **search**: 
  - Best case (no wildcards): O(m), where m is the length of the word.
  - Worst case (all wildcards): O(n * 26^m), where n is the number of words in the dictionary and m is the length of the search word. This is because in the worst case, we might need to explore all possible paths at each level.

### Space Complexity:
- **Overall**: O(n * m), where n is the number of words and m is the average length of the words. This is the space required to store all the nodes in the Trie.
- **search**: O(m) additional space for the recursion stack during the DFS.

## Detailed Walkthrough with Example

Let's trace through the algorithm with the example from the problem statement:

1. Initialize an empty WordDictionary.

2. Add "bad":
   - Start at the root
   - Add 'b' as a child of the root
   - Add 'a' as a child of 'b'
   - Add 'd' as a child of 'a'
   - Mark 'd' as the end of a word

3. Add "dad":
   - Start at the root
   - Add 'd' as a child of the root
   - Add 'a' as a child of 'd'
   - Add 'd' as a child of 'a'
   - Mark 'd' as the end of a word

4. Add "mad":
   - Start at the root
   - Add 'm' as a child of the root
   - Add 'a' as a child of 'm'
   - Add 'd' as a child of 'a'
   - Mark 'd' as the end of a word

5. Search for "pad":
   - Start at the root
   - Look for child 'p' → Not found
   - Return false

6. Search for "bad":
   - Start at the root
   - Look for child 'b' → Found
   - Look for child 'a' → Found
   - Look for child 'd' → Found
   - Check if it's the end of a word → Yes
   - Return true

7. Search for ".ad":
   - Start at the root
   - Look for any child (because of '.'):
     - Try 'b':
       - Look for child 'a' → Found
       - Look for child 'd' → Found
       - Check if it's the end of a word → Yes
       - Return true
     - (We don't need to try 'd' or 'm' because we already found a match)

8. Search for "b..":
   - Start at the root
   - Look for child 'b' → Found
   - Look for any child (because of '.'):
     - Try 'a':
       - Look for any child (because of '.'):
         - Try 'd':
           - Check if it's the end of a word → Yes
           - Return true

## Edge Cases and Optimizations

- **Empty Word**: The algorithm handles empty words correctly. An empty word would be represented by marking the root node as the end of a word.
- **Long Words**: The algorithm efficiently handles long words by only storing each character once per unique path.
- **Many Words with Common Prefixes**: The Trie is particularly efficient for dictionaries with many words sharing common prefixes.

**Optimization**: For the search method, we can optimize the handling of wildcards by using an iterative approach instead of recursion to avoid the overhead of function calls. Additionally, we can implement early termination when a match is found.

## Common Mistakes

1. **Not handling wildcards correctly**: The key challenge is to handle the wildcard character `.` which requires exploring all possible paths.
2. **Forgetting to mark the end of words**: It's important to mark nodes that represent the end of a word, otherwise, we can't distinguish between prefixes and complete words.
3. **Inefficient wildcard handling**: Naively exploring all paths for each wildcard can lead to exponential time complexity. It's important to implement the search efficiently.

## Related Problems

1. **Implement Trie (Prefix Tree)**: Implement a basic Trie without wildcard search.
2. **Word Search II**: Find all words in a board of characters using a Trie.
3. **Design Search Autocomplete System**: Design a search autocomplete system for a search engine.
4. **Replace Words**: Replace words in a sentence with their root prefix using a Trie. 