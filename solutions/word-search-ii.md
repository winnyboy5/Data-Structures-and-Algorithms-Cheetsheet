# Word Search II

## Problem Statement

Given an `m x n` board of characters and a list of strings `words`, return all words on the board.

Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

**Example 1:**
```
Input: 
board = [
  ["o","a","a","n"],
  ["e","t","a","e"],
  ["i","h","k","r"],
  ["i","f","l","v"]
]
words = ["oath","pea","eat","rain"]

Output: ["eat","oath"]
```

**Example 2:**
```
Input: 
board = [
  ["a","b"],
  ["c","d"]
]
words = ["abcb"]

Output: []
```

## Intuition

The key insight for this problem is to use a Trie (Prefix Tree) data structure to efficiently search for words on the board. Instead of searching for each word separately, we can build a Trie from the list of words and then perform a single traversal of the board to find all words.

This approach is much more efficient than checking each word individually because:
1. We can prune our search early if a prefix doesn't exist in the Trie.
2. We can find multiple words in a single traversal of the board.
3. We avoid redundant searches for common prefixes.

## Visual Explanation

Let's visualize the solution with Example 1:

First, we build a Trie from the words ["oath", "pea", "eat", "rain"]:

```
        root
       /  |  \  \
      o   p   e  r
     /    |   |   \
    a     e   a    a
   /          |     \
  t           t      i
 /                    \
h                      n
```

Each path from the root to a leaf node represents a word in our list.

Now, we perform a DFS on the board, starting from each cell. For each cell, we check if its character is a child of the root in our Trie. If it is, we continue the DFS with the corresponding Trie node.

For example, starting from the cell (0,0) with character 'o':
1. 'o' is a child of the root, so we continue.
2. We explore adjacent cells: (0,1) with 'a' and (1,0) with 'e'.
3. 'a' is a child of 'o' in our Trie, so we continue with (0,1).
4. We explore adjacent cells of (0,1): (0,0) with 'o', (0,2) with 'a', and (1,1) with 't'.
5. We've already visited (0,0), so we skip it.
6. 'a' is not a child of 'a' in our Trie, so we skip (0,2).
7. 't' is a child of 'a' in our Trie, so we continue with (1,1).
8. We explore adjacent cells of (1,1): (0,1) with 'a', (1,0) with 'e', (1,2) with 'a', and (2,1) with 'h'.
9. We've already visited (0,1) and (1,0), so we skip them.
10. 'a' is not a child of 't' in our Trie, so we skip (1,2).
11. 'h' is a child of 't' in our Trie, so we continue with (2,1).
12. We've found the word "oath", so we add it to our result.

We repeat this process starting from each cell on the board.

![Word Search II Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Build a Trie from the list of words.
2. For each cell on the board:
   - Start a DFS from that cell.
   - If the current character is a child of the current Trie node, continue the DFS with the corresponding Trie node.
   - If we reach a Trie node that marks the end of a word, add that word to our result.
   - Mark the current cell as visited during the DFS to avoid using the same cell multiple times.
   - Restore the cell after the DFS backtracks.
3. Return the list of found words.

## Code Implementation

### Python

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False
        self.word = None

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
        node.word = word

def findWords(board, words):
    if not board or not words:
        return []
    
    # Build Trie
    trie = Trie()
    for word in words:
        trie.insert(word)
    
    result = set()
    rows, cols = len(board), len(board[0])
    
    def dfs(row, col, node):
        # Check if we're out of bounds or if the current cell doesn't match
        if (row < 0 or row >= rows or col < 0 or col >= cols or
            board[row][col] not in node.children):
            return
        
        char = board[row][col]
        next_node = node.children[char]
        
        # If we've found a word, add it to the result
        if next_node.is_end_of_word:
            result.add(next_node.word)
        
        # Mark the current cell as visited
        board[row][col] = '#'
        
        # Explore adjacent cells
        dfs(row + 1, col, next_node)
        dfs(row - 1, col, next_node)
        dfs(row, col + 1, next_node)
        dfs(row, col - 1, next_node)
        
        # Restore the cell
        board[row][col] = char
    
    # Start DFS from each cell
    for row in range(rows):
        for col in range(cols):
            dfs(row, col, trie.root)
    
    return list(result)
```

### JavaScript

```javascript
class TrieNode {
    constructor() {
        this.children = {};
        this.isEndOfWord = false;
        this.word = null;
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
        node.word = word;
    }
}

function findWords(board, words) {
    if (!board || !board.length || !words || !words.length) {
        return [];
    }
    
    // Build Trie
    const trie = new Trie();
    for (const word of words) {
        trie.insert(word);
    }
    
    const result = new Set();
    const rows = board.length;
    const cols = board[0].length;
    
    function dfs(row, col, node) {
        // Check if we're out of bounds or if the current cell doesn't match
        if (row < 0 || row >= rows || col < 0 || col >= cols || 
            !(board[row][col] in node.children)) {
            return;
        }
        
        const char = board[row][col];
        const nextNode = node.children[char];
        
        // If we've found a word, add it to the result
        if (nextNode.isEndOfWord) {
            result.add(nextNode.word);
        }
        
        // Mark the current cell as visited
        board[row][col] = '#';
        
        // Explore adjacent cells
        dfs(row + 1, col, nextNode);
        dfs(row - 1, col, nextNode);
        dfs(row, col + 1, nextNode);
        dfs(row, col - 1, nextNode);
        
        // Restore the cell
        board[row][col] = char;
    }
    
    // Start DFS from each cell
    for (let row = 0; row < rows; row++) {
        for (let col = 0; col < cols; col++) {
            dfs(row, col, trie.root);
        }
    }
    
    return Array.from(result);
}
```

## Optimization: Pruning the Trie

We can optimize our solution by pruning the Trie as we find words. Once we've found a word, we can remove it from the Trie to avoid finding it again. This can significantly improve performance for large boards and word lists.

### Python (with Trie Pruning)

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False
        self.word = None

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
        node.word = word

def findWords(board, words):
    if not board or not words:
        return []
    
    # Build Trie
    trie = Trie()
    for word in words:
        trie.insert(word)
    
    result = []
    rows, cols = len(board), len(board[0])
    
    def dfs(row, col, parent, node):
        if (row < 0 or row >= rows or col < 0 or col >= cols or
            board[row][col] not in node.children):
            return
        
        char = board[row][col]
        next_node = node.children[char]
        
        # If we've found a word, add it to the result and remove it from the Trie
        if next_node.is_end_of_word:
            result.append(next_node.word)
            next_node.is_end_of_word = False  # Mark as visited
        
        # Mark the current cell as visited
        board[row][col] = '#'
        
        # Explore adjacent cells
        dfs(row + 1, col, next_node, next_node)
        dfs(row - 1, col, next_node, next_node)
        dfs(row, col + 1, next_node, next_node)
        dfs(row, col - 1, next_node, next_node)
        
        # Restore the cell
        board[row][col] = char
        
        # Prune the Trie: if the node has no children and is not the end of a word, remove it
        if not next_node.children and not next_node.is_end_of_word:
            del node.children[char]
    
    # Start DFS from each cell
    for row in range(rows):
        for col in range(cols):
            dfs(row, col, None, trie.root)
    
    return result
```

## Time and Space Complexity

### Time Complexity:
- Building the Trie: O(k), where k is the total number of characters in all words.
- DFS on the board: O(m * n * 4^L), where m and n are the dimensions of the board, and L is the maximum length of a word. The factor 4^L comes from the fact that at each cell, we have at most 4 directions to explore, and we can explore up to L levels deep.
- Overall: O(k + m * n * 4^L)

### Space Complexity:
- Trie: O(k), where k is the total number of characters in all words.
- DFS recursion stack: O(L), where L is the maximum length of a word.
- Result set: O(k), in the worst case where all words are found.
- Overall: O(k + L)

## Detailed Walkthrough with Example

Let's trace through the algorithm with a simpler example:

Board:
```
[
  ["a", "b"],
  ["c", "d"]
]
```

Words: ["ab", "cd", "ac", "bd"]

1. Build the Trie:
   ```
         root
        /  |  \  \
       a   c   b  d
      /    |   |   \
     b     d   d    null
   ```

2. Start DFS from each cell:
   - Cell (0,0) with 'a':
     - 'a' is a child of the root, so we continue.
     - We explore adjacent cells: (0,1) with 'b' and (1,0) with 'c'.
     - 'b' is a child of 'a' in our Trie, so we continue with (0,1).
     - We've found the word "ab", so we add it to our result.
     - 'c' is not a child of 'a' in our Trie, so we skip (1,0).
   - Cell (0,1) with 'b':
     - 'b' is a child of the root, so we continue.
     - We explore adjacent cells: (0,0) with 'a' and (1,1) with 'd'.
     - 'a' is not a child of 'b' in our Trie, so we skip (0,0).
     - 'd' is a child of 'b' in our Trie, so we continue with (1,1).
     - We've found the word "bd", so we add it to our result.
   - Cell (1,0) with 'c':
     - 'c' is a child of the root, so we continue.
     - We explore adjacent cells: (0,0) with 'a' and (1,1) with 'd'.
     - 'a' is not a child of 'c' in our Trie, so we skip (0,0).
     - 'd' is a child of 'c' in our Trie, so we continue with (1,1).
     - We've found the word "cd", so we add it to our result.
   - Cell (1,1) with 'd':
     - 'd' is a child of the root, but it has no children in our Trie, so we stop.

3. Result: ["ab", "bd", "cd"]

Note that we didn't find "ac" because the cells 'a' and 'c' are not adjacent on the board.

## Edge Cases and Optimizations

- **Empty Board or Word List**: The algorithm handles empty inputs correctly.
- **No Words Found**: The algorithm returns an empty list if no words are found.
- **Duplicate Words**: We use a set to avoid duplicate results.
- **Long Words**: The algorithm efficiently handles long words by using a Trie.

**Optimizations**:
1. **Trie Pruning**: As mentioned earlier, we can prune the Trie as we find words to avoid redundant searches.
2. **Early Termination**: We can stop the DFS early if we've found all the words.
3. **Character Frequency**: We can check if the board contains all the characters needed for a word before starting the DFS.

## Common Mistakes

1. **Not marking cells as visited**: It's important to mark cells as visited during the DFS to avoid using the same cell multiple times.
2. **Not restoring cells**: After the DFS backtracks, we need to restore the cell to its original value for other paths.
3. **Inefficient word lookup**: Using a list to check if a word exists is inefficient. The Trie allows us to check in O(L) time, where L is the length of the word.

## Related Problems

1. **Word Search**: Find if a single word exists in a board.
2. **Boggle**: Find all valid words in a board of characters.
3. **Add and Search Word - Data structure design**: Design a data structure that supports adding new words and finding if a string matches any previously added string.
4. **Implement Trie (Prefix Tree)**: Implement a basic Trie without the board search functionality. 