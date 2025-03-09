# Word Search

## Problem Statement

Given an m x n grid of characters `board` and a string `word`, return `true` if `word` exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example 1:**
```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true
```

**Example 2:**
```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
Output: true
```

**Example 3:**
```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
Output: false
```

## Intuition

This problem is a classic application of backtracking. The key insight is to explore all possible paths from each cell in the grid to see if we can form the target word. We start from each cell that matches the first character of the word and then recursively explore its neighbors to match subsequent characters.

To avoid using the same cell multiple times, we can mark visited cells temporarily during our exploration. If we reach a dead end (can't match the next character), we backtrack by unmarking the cell and try a different path.

## Visual Explanation

Let's visualize the solution with Example 1:

```
Board:
[["A","B","C","E"],
 ["S","F","C","S"],
 ["A","D","E","E"]]

Word: "ABCCED"

Step 1: Find all cells that match the first character 'A'
- (0,0): 'A'
- (2,0): 'A'

Step 2: Start DFS from each of these cells
Starting from (0,0):
- Mark (0,0) as visited
- Check neighbors for 'B' (second character)
  - (0,1): 'B' matches!
  - Mark (0,1) as visited
  - Check neighbors for 'C' (third character)
    - (0,2): 'C' matches!
    - Mark (0,2) as visited
    - Check neighbors for 'C' (fourth character)
      - (1,2): 'C' matches!
      - Mark (1,2) as visited
      - Check neighbors for 'E' (fifth character)
        - (0,3): 'E' matches!
        - Mark (0,3) as visited
        - Check neighbors for 'D' (sixth character)
          - (1,3): 'S' doesn't match
          - Backtrack and unmark (0,3)
        - (1,2): Already visited
        - (0,1): Already visited
        - Backtrack and unmark (1,2)
      - (2,2): 'E' matches!
      - Mark (2,2) as visited
      - Check neighbors for 'D' (sixth character)
        - (2,1): 'D' matches!
        - Mark (2,1) as visited
        - We've matched all characters of the word!
        - Return true

Since we found a valid path, the answer is true.
```

![Word Search Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Iterate through each cell in the grid.
2. If the current cell matches the first character of the word, start a depth-first search (DFS) from this cell.
3. In the DFS:
   - If we've matched all characters of the word, return true.
   - Mark the current cell as visited (to avoid using it again in the current path).
   - Recursively explore all four adjacent cells (up, right, down, left) to match the next character.
   - If any of the recursive calls returns true, return true.
   - Otherwise, backtrack by unmarking the current cell and return false.
4. If no path is found from any starting cell, return false.

## Code Implementation

### Python

```python
def exist(board, word):
    if not board or not board[0]:
        return False
    
    m, n = len(board), len(board[0])
    
    def dfs(i, j, k):
        # If we've matched all characters, return True
        if k == len(word):
            return True
        
        # Check if current position is out of bounds or doesn't match the current character
        if (i < 0 or i >= m or j < 0 or j >= n or board[i][j] != word[k]):
            return False
        
        # Mark the current cell as visited
        temp = board[i][j]
        board[i][j] = '#'
        
        # Explore all four directions
        found = (dfs(i+1, j, k+1) or 
                 dfs(i-1, j, k+1) or 
                 dfs(i, j+1, k+1) or 
                 dfs(i, j-1, k+1))
        
        # Backtrack: restore the cell
        board[i][j] = temp
        
        return found
    
    # Try starting from each cell
    for i in range(m):
        for j in range(n):
            if board[i][j] == word[0] and dfs(i, j, 0):
                return True
    
    return False
```

### JavaScript

```javascript
function exist(board, word) {
    if (!board.length || !board[0].length) {
        return false;
    }
    
    const m = board.length;
    const n = board[0].length;
    
    function dfs(i, j, k) {
        // If we've matched all characters, return true
        if (k === word.length) {
            return true;
        }
        
        // Check if current position is out of bounds or doesn't match the current character
        if (i < 0 || i >= m || j < 0 || j >= n || board[i][j] !== word[k]) {
            return false;
        }
        
        // Mark the current cell as visited
        const temp = board[i][j];
        board[i][j] = '#';
        
        // Explore all four directions
        const found = dfs(i+1, j, k+1) || 
                      dfs(i-1, j, k+1) || 
                      dfs(i, j+1, k+1) || 
                      dfs(i, j-1, k+1);
        
        // Backtrack: restore the cell
        board[i][j] = temp;
        
        return found;
    }
    
    // Try starting from each cell
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (board[i][j] === word[0] && dfs(i, j, 0)) {
                return true;
            }
        }
    }
    
    return false;
}
```

## Time and Space Complexity

- **Time Complexity**: O(m × n × 4^L), where m is the number of rows, n is the number of columns, and L is the length of the word. For each cell, we potentially explore 4 directions for each character in the word.
- **Space Complexity**: O(L) for the recursion stack, where L is the length of the word. In the worst case, the recursion depth is equal to the length of the word.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2:

```
Board:
[["A","B","C","E"],
 ["S","F","C","S"],
 ["A","D","E","E"]]

Word: "SEE"

Step 1: Find all cells that match the first character 'S'
- (1,0): 'S'
- (1,3): 'S'

Step 2: Start DFS from each of these cells
Starting from (1,0):
- Mark (1,0) as visited
- Check neighbors for 'E' (second character)
  - (0,0): 'A' doesn't match
  - (2,0): 'A' doesn't match
  - (1,1): 'F' doesn't match
  - Backtrack and unmark (1,0)

Starting from (1,3):
- Mark (1,3) as visited
- Check neighbors for 'E' (second character)
  - (0,3): 'E' matches!
  - Mark (0,3) as visited
  - Check neighbors for 'E' (third character)
    - (0,2): 'C' doesn't match
    - (1,2): 'C' doesn't match
    - Backtrack and unmark (0,3)
  - (2,3): 'E' matches!
  - Mark (2,3) as visited
  - Check neighbors for 'E' (third character)
    - (2,2): 'E' matches!
    - Mark (2,2) as visited
    - We've matched all characters of the word!
    - Return true

Since we found a valid path, the answer is true.
```

## Edge Cases and Optimizations

- **Empty Board or Word**: If the board is empty or the word is empty, return false.
- **Word Longer than Board**: If the word is longer than the total number of cells in the board, return false.
- **First Character Not in Board**: Before starting the DFS, we can check if the first character of the word exists in the board. If not, we can immediately return false.
- **Pruning**: We can stop the search early if we know it's impossible to find the word. For example, if the remaining characters in the word don't exist in the board.

## Common Mistakes

1. **Not marking visited cells**: Forgetting to mark cells as visited can lead to infinite loops or incorrect results.
2. **Not backtracking**: After exploring a path, we need to restore the cell to its original state to allow it to be used in other paths.
3. **Incorrect boundary checks**: Make sure to check if the current position is within the bounds of the board before accessing it.
4. **Not checking all starting positions**: We need to try starting the search from every cell that matches the first character of the word.

## Related Problems

1. **Word Search II**: Find all words in a board from a given dictionary.
2. **Number of Islands**: Count the number of islands in a 2D grid.
3. **Surrounded Regions**: Capture all regions surrounded by 'X'.
4. **Pacific Atlantic Water Flow**: Find cells that can flow to both Pacific and Atlantic oceans. 