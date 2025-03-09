# Alien Dictionary

## Problem Statement

There is a new alien language that uses the English alphabet. However, the order of the letters is unknown to you. You are given a list of strings `words` from the alien language's dictionary, where the strings in `words` are sorted lexicographically by the rules of this new language.

Return a string of the unique letters in the new alien language sorted in lexicographically increasing order by the new language's rules. If there is no solution, return "". If there are multiple solutions, return any of them.

**Example 1:**
```
Input: words = ["wrt","wrf","er","ett","rftt"]
Output: "wertf"
```

**Example 2:**
```
Input: words = ["z","x"]
Output: "zx"
```

**Example 3:**
```
Input: words = ["z","x","z"]
Output: ""
Explanation: The order is invalid, so return "".
```

## Intuition

The key insight for this problem is to recognize that the sorted order of words gives us information about the relative ordering of characters in the alien language. By comparing adjacent words, we can extract character ordering relationships.

For example, if "wrt" comes before "wrf", we know that 't' must come before 'f' in the alien language. We can represent these relationships as a directed graph, where each character is a node, and an edge from character 'a' to 'b' means 'a' comes before 'b' in the alien language.

Once we have this graph, we can perform a topological sort to find a valid ordering of the characters. If the graph contains a cycle, then there is no valid ordering, and we should return an empty string.

## Visual Explanation

Let's visualize the solution with Example 1: `words = ["wrt","wrf","er","ett","rftt"]`

1. First, we build a graph by comparing adjacent words:
   - "wrt" vs "wrf": 't' comes before 'f'
   - "wrf" vs "er": 'w' comes before 'e'
   - "er" vs "ett": 'r' comes before 't'
   - "ett" vs "rftt": 'e' comes before 'r'

2. The resulting graph looks like:
   ```
   w → e
   e → r
   r → t
   t → f
   ```

3. Performing a topological sort on this graph gives us: "wertf"

![Alien Dictionary Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Build a graph where each node is a character, and an edge from 'a' to 'b' means 'a' comes before 'b'.
   - Initialize a set of all unique characters in the words.
   - Compare adjacent words to find ordering relationships.
   - For each pair of adjacent words, find the first differing character and add an edge.

2. Perform a topological sort on the graph:
   - Calculate the in-degree of each node (number of characters that come before it).
   - Start with nodes that have an in-degree of 0 (no characters come before them).
   - Process each node, decrementing the in-degree of its neighbors.
   - If a neighbor's in-degree becomes 0, add it to the queue of nodes to process.
   - If we can't process all nodes, there's a cycle, and we return an empty string.

3. Return the characters in the order they were processed during the topological sort.

## Code Implementation

### Python

```python
from collections import defaultdict, deque

def alienOrder(words):
    # Step 1: Build the graph
    graph = defaultdict(set)
    in_degree = {c: 0 for word in words for c in word}
    
    # Compare adjacent words to find ordering relationships
    for i in range(len(words) - 1):
        word1, word2 = words[i], words[i + 1]
        # Check for invalid case: if word1 is a prefix of word2, they are already in order
        if len(word1) > len(word2) and word1[:len(word2)] == word2:
            return ""
        
        # Find the first differing character
        for j in range(min(len(word1), len(word2))):
            if word1[j] != word2[j]:
                if word2[j] not in graph[word1[j]]:
                    graph[word1[j]].add(word2[j])
                    in_degree[word2[j]] += 1
                break
    
    # Step 2: Topological sort
    result = []
    queue = deque([c for c in in_degree if in_degree[c] == 0])
    
    while queue:
        char = queue.popleft()
        result.append(char)
        
        for neighbor in graph[char]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    
    # If we couldn't process all characters, there's a cycle
    if len(result) != len(in_degree):
        return ""
    
    return ''.join(result)
```

### JavaScript

```javascript
function alienOrder(words) {
    // Step 1: Build the graph
    const graph = new Map();
    const inDegree = new Map();
    
    // Initialize the graph and in-degree map
    for (const word of words) {
        for (const char of word) {
            if (!graph.has(char)) {
                graph.set(char, new Set());
                inDegree.set(char, 0);
            }
        }
    }
    
    // Compare adjacent words to find ordering relationships
    for (let i = 0; i < words.length - 1; i++) {
        const word1 = words[i];
        const word2 = words[i + 1];
        
        // Check for invalid case: if word1 is a prefix of word2, they are already in order
        if (word1.length > word2.length && word1.startsWith(word2)) {
            return "";
        }
        
        // Find the first differing character
        for (let j = 0; j < Math.min(word1.length, word2.length); j++) {
            if (word1[j] !== word2[j]) {
                if (!graph.get(word1[j]).has(word2[j])) {
                    graph.get(word1[j]).add(word2[j]);
                    inDegree.set(word2[j], inDegree.get(word2[j]) + 1);
                }
                break;
            }
        }
    }
    
    // Step 2: Topological sort
    const result = [];
    const queue = [];
    
    // Add all nodes with in-degree 0 to the queue
    for (const [char, degree] of inDegree) {
        if (degree === 0) {
            queue.push(char);
        }
    }
    
    while (queue.length > 0) {
        const char = queue.shift();
        result.push(char);
        
        for (const neighbor of graph.get(char)) {
            inDegree.set(neighbor, inDegree.get(neighbor) - 1);
            if (inDegree.get(neighbor) === 0) {
                queue.push(neighbor);
            }
        }
    }
    
    // If we couldn't process all characters, there's a cycle
    if (result.length !== inDegree.size) {
        return "";
    }
    
    return result.join('');
}
```

## Time and Space Complexity

- **Time Complexity**: O(C), where C is the total length of all words in the input list. We need to iterate through all characters to build the graph and perform the topological sort.
- **Space Complexity**: O(1) or O(U), where U is the number of unique characters in the alien language. Since we're dealing with lowercase English letters, this is bounded by 26, which is a constant. However, if we consider the general case with an arbitrary alphabet, it would be O(U).

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 1: `words = ["wrt","wrf","er","ett","rftt"]`

1. Initialize the graph and in-degree map:
   - Unique characters: 'w', 'r', 't', 'f', 'e'
   - graph = {w: [], r: [], t: [], f: [], e: []}
   - in_degree = {w: 0, r: 0, t: 0, f: 0, e: 0}

2. Compare adjacent words:
   - "wrt" vs "wrf": First differing character is at index 2 ('t' vs 'f')
     - Add edge: t → f
     - Update in_degree: {w: 0, r: 0, t: 0, f: 1, e: 0}
   - "wrf" vs "er": First differing character is at index 0 ('w' vs 'e')
     - Add edge: w → e
     - Update in_degree: {w: 0, r: 0, t: 0, f: 1, e: 1}
   - "er" vs "ett": First differing character is at index 1 ('r' vs 't')
     - Add edge: r → t
     - Update in_degree: {w: 0, r: 0, t: 1, f: 1, e: 1}
   - "ett" vs "rftt": First differing character is at index 0 ('e' vs 'r')
     - Add edge: e → r
     - Update in_degree: {w: 0, r: 1, t: 1, f: 1, e: 1}

3. Perform topological sort:
   - Initial queue: ['w'] (characters with in-degree 0)
   - Process 'w':
     - Add 'w' to result: result = ['w']
     - Decrement in-degree of neighbors: 'e' in-degree becomes 0
     - Queue: ['e']
   - Process 'e':
     - Add 'e' to result: result = ['w', 'e']
     - Decrement in-degree of neighbors: 'r' in-degree becomes 0
     - Queue: ['r']
   - Process 'r':
     - Add 'r' to result: result = ['w', 'e', 'r']
     - Decrement in-degree of neighbors: 't' in-degree becomes 0
     - Queue: ['t']
   - Process 't':
     - Add 't' to result: result = ['w', 'e', 'r', 't']
     - Decrement in-degree of neighbors: 'f' in-degree becomes 0
     - Queue: ['f']
   - Process 'f':
     - Add 'f' to result: result = ['w', 'e', 'r', 't', 'f']
     - No neighbors to process

4. Check if all characters were processed:
   - result.length = 5, in_degree.size = 5
   - All characters were processed, so return "wertf"

## Edge Cases and Optimizations

- **Empty Input**: If the input list is empty, we should return an empty string.
- **Single Word**: If there's only one word, we can return the unique characters in any order.
- **Prefix Issue**: If a longer word is a prefix of a shorter word that comes after it in the sorted list, the ordering is invalid. For example, if ["abc", "ab"] is the input, it's invalid because "abc" should come after "ab" in lexicographical order.
- **Cycle Detection**: If there's a cycle in the character relationships, there's no valid ordering, and we should return an empty string.
- **Multiple Valid Orderings**: If there are multiple valid orderings, we can return any of them. The topological sort will give us one valid ordering.

## Common Mistakes

1. **Not handling the prefix issue**: Make sure to check if a longer word is a prefix of a shorter word that comes after it.
2. **Incorrect graph building**: When comparing adjacent words, only the first differing character gives us information about the ordering.
3. **Not detecting cycles**: If the topological sort can't process all characters, there's a cycle, and we should return an empty string.

## Related Problems

1. **Course Schedule**: Determine if it's possible to finish all courses given prerequisites.
2. **Course Schedule II**: Return the ordering of courses you should take to finish all courses.
3. **Minimum Height Trees**: Find all the trees with minimum height in an undirected graph.
4. **Sequence Reconstruction**: Determine if a sequence can be uniquely reconstructed from its subsequences. 