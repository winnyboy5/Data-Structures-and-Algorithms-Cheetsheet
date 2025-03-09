# Number of Islands

## Problem Statement

Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return the number of islands.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**
```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```

**Example 2:**
```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

## Intuition

The key insight for this problem is to use either Depth-First Search (DFS) or Breadth-First Search (BFS) to explore the grid. When we encounter a land cell (`'1'`), we explore all connected land cells in all four directions (up, down, left, right) and mark them as visited to avoid counting them again.

Each time we find a new unvisited land cell, we increment our island count and perform a search to mark all connected land cells as visited. This way, we count each island exactly once.

## Visual Explanation

Let's visualize the solution with Example 2:

```
Initial Grid:
[
  ["1","1","0","0","0"],  // Row 0
  ["1","1","0","0","0"],  // Row 1
  ["0","0","1","0","0"],  // Row 2
  ["0","0","0","1","1"]   // Row 3
]

Step 1: Start from position (0,0), which is "1" (land).
        Count = 1
        Perform DFS/BFS to mark all connected land cells as visited.
        
        After DFS/BFS, the grid becomes:
        [
          ["V","V","0","0","0"],  // V = visited
          ["V","V","0","0","0"],
          ["0","0","1","0","0"],
          ["0","0","0","1","1"]
        ]

Step 2: Continue scanning the grid and find the next unvisited land at (2,2).
        Count = 2
        Perform DFS/BFS to mark all connected land cells as visited.
        
        After DFS/BFS, the grid becomes:
        [
          ["V","V","0","0","0"],
          ["V","V","0","0","0"],
          ["0","0","V","0","0"],
          ["0","0","0","1","1"]
        ]

Step 3: Continue scanning and find the next unvisited land at (3,3).
        Count = 3
        Perform DFS/BFS to mark all connected land cells as visited.
        
        After DFS/BFS, the grid becomes:
        [
          ["V","V","0","0","0"],
          ["V","V","0","0","0"],
          ["0","0","V","0","0"],
          ["0","0","0","V","V"]
        ]

Step 4: No more unvisited land cells. Return count = 3.
```

![Number of Islands Visualization](https://i.imgur.com/JFcND5j.png)

## Step-by-Step Approach

### DFS Approach:

1. Initialize a counter `numIslands` to 0.
2. Iterate through each cell in the grid:
   - If the cell is land (`'1'`) and not visited, increment `numIslands`.
   - Perform DFS starting from this cell to mark all connected land cells as visited.
3. In the DFS function:
   - Check if the current position is valid (within grid boundaries, is land, and not visited).
   - Mark the current cell as visited.
   - Recursively call DFS for all four adjacent cells (up, down, left, right).
4. Return `numIslands`.

### BFS Approach:

1. Initialize a counter `numIslands` to 0.
2. Iterate through each cell in the grid:
   - If the cell is land (`'1'`) and not visited, increment `numIslands`.
   - Perform BFS starting from this cell to mark all connected land cells as visited.
3. In the BFS function:
   - Use a queue to keep track of cells to visit.
   - For each cell in the queue, mark it as visited and add all valid adjacent land cells to the queue.
4. Return `numIslands`.

## Code Implementation

### Python (DFS)

```python
def numIslands(grid):
    if not grid:
        return 0
    
    rows, cols = len(grid), len(grid[0])
    count = 0
    
    def dfs(r, c):
        # Check if the position is valid
        if (r < 0 or r >= rows or c < 0 or c >= cols or
            grid[r][c] == '0' or grid[r][c] == 'V'):
            return
        
        # Mark as visited
        grid[r][c] = 'V'
        
        # Explore all four directions
        dfs(r+1, c)  # Down
        dfs(r-1, c)  # Up
        dfs(r, c+1)  # Right
        dfs(r, c-1)  # Left
    
    # Iterate through the grid
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1':
                count += 1
                dfs(r, c)
    
    return count
```

### Python (BFS)

```python
from collections import deque

def numIslands(grid):
    if not grid:
        return 0
    
    rows, cols = len(grid), len(grid[0])
    count = 0
    
    def bfs(r, c):
        queue = deque([(r, c)])
        grid[r][c] = 'V'  # Mark as visited
        
        while queue:
            curr_r, curr_c = queue.popleft()
            
            # Explore all four directions
            directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]  # Down, Up, Right, Left
            
            for dr, dc in directions:
                new_r, new_c = curr_r + dr, curr_c + dc
                
                # Check if the position is valid
                if (0 <= new_r < rows and 0 <= new_c < cols and
                    grid[new_r][new_c] == '1'):
                    grid[new_r][new_c] = 'V'  # Mark as visited
                    queue.append((new_r, new_c))
    
    # Iterate through the grid
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1':
                count += 1
                bfs(r, c)
    
    return count
```

### JavaScript (DFS)

```javascript
function numIslands(grid) {
    if (!grid || grid.length === 0) {
        return 0;
    }
    
    const rows = grid.length;
    const cols = grid[0].length;
    let count = 0;
    
    function dfs(r, c) {
        // Check if the position is valid
        if (r < 0 || r >= rows || c < 0 || c >= cols || 
            grid[r][c] === '0' || grid[r][c] === 'V') {
            return;
        }
        
        // Mark as visited
        grid[r][c] = 'V';
        
        // Explore all four directions
        dfs(r+1, c);  // Down
        dfs(r-1, c);  // Up
        dfs(r, c+1);  // Right
        dfs(r, c-1);  // Left
    }
    
    // Iterate through the grid
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (grid[r][c] === '1') {
                count++;
                dfs(r, c);
            }
        }
    }
    
    return count;
}
```

### JavaScript (BFS)

```javascript
function numIslands(grid) {
    if (!grid || grid.length === 0) {
        return 0;
    }
    
    const rows = grid.length;
    const cols = grid[0].length;
    let count = 0;
    
    function bfs(r, c) {
        const queue = [[r, c]];
        grid[r][c] = 'V';  // Mark as visited
        
        while (queue.length > 0) {
            const [curr_r, curr_c] = queue.shift();
            
            // Explore all four directions
            const directions = [[1, 0], [-1, 0], [0, 1], [0, -1]];  // Down, Up, Right, Left
            
            for (const [dr, dc] of directions) {
                const new_r = curr_r + dr;
                const new_c = curr_c + dc;
                
                // Check if the position is valid
                if (new_r >= 0 && new_r < rows && new_c >= 0 && new_c < cols && 
                    grid[new_r][new_c] === '1') {
                    grid[new_r][new_c] = 'V';  // Mark as visited
                    queue.push([new_r, new_c]);
                }
            }
        }
    }
    
    // Iterate through the grid
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (grid[r][c] === '1') {
                count++;
                bfs(r, c);
            }
        }
    }
    
    return count;
}
```

## Time and Space Complexity

- **Time Complexity**: O(M × N), where M is the number of rows and N is the number of columns in the grid. We visit each cell at most once.
- **Space Complexity**: 
  - For DFS: O(M × N) in the worst case (if the entire grid is filled with land), due to the recursion stack.
  - For BFS: O(min(M, N)) for the queue, as in the worst case, the queue can contain all cells of the shortest side of the grid.

## Detailed Walkthrough with Example

Let's trace through the DFS algorithm with a smaller example:

```
Grid:
[
  ["1","1","0"],
  ["1","0","0"],
  ["0","0","1"]
]

Iteration (0,0): grid[0][0] = '1'
  - Increment count = 1
  - DFS(0, 0)
    - Mark (0,0) as visited: grid[0][0] = 'V'
    - DFS(1, 0) -> grid[1][0] = '1'
      - Mark (1,0) as visited: grid[1][0] = 'V'
      - DFS(2, 0) -> grid[2][0] = '0', return
      - DFS(0, 0) -> Already visited, return
      - DFS(1, 1) -> grid[1][1] = '0', return
      - DFS(1, -1) -> Out of bounds, return
    - DFS(-1, 0) -> Out of bounds, return
    - DFS(0, 1) -> grid[0][1] = '1'
      - Mark (0,1) as visited: grid[0][1] = 'V'
      - DFS(1, 1) -> grid[1][1] = '0', return
      - DFS(-1, 1) -> Out of bounds, return
      - DFS(0, 2) -> grid[0][2] = '0', return
      - DFS(0, 0) -> Already visited, return
    - DFS(0, -1) -> Out of bounds, return

Grid after first island:
[
  ["V","V","0"],
  ["V","0","0"],
  ["0","0","1"]
]

Iteration (0,1): grid[0][1] = 'V', continue
Iteration (0,2): grid[0][2] = '0', continue
Iteration (1,0): grid[1][0] = 'V', continue
Iteration (1,1): grid[1][1] = '0', continue
Iteration (1,2): grid[1][2] = '0', continue
Iteration (2,0): grid[2][0] = '0', continue
Iteration (2,1): grid[2][1] = '0', continue
Iteration (2,2): grid[2][2] = '1'
  - Increment count = 2
  - DFS(2, 2)
    - Mark (2,2) as visited: grid[2][2] = 'V'
    - DFS(3, 2) -> Out of bounds, return
    - DFS(1, 2) -> grid[1][2] = '0', return
    - DFS(2, 3) -> Out of bounds, return
    - DFS(2, 1) -> grid[2][1] = '0', return

Final grid:
[
  ["V","V","0"],
  ["V","0","0"],
  ["0","0","V"]
]

Return count = 2
```

## Edge Cases and Optimizations

- **Empty Grid**: If the grid is empty, return 0.
- **Single Cell Grid**: If the grid has only one cell, return 1 if it's land, 0 if it's water.
- **All Water**: If the grid contains only water, return 0.
- **All Land**: If the grid contains only land, return 1.

**Optimization**: Instead of using a separate visited array, we can modify the original grid to mark cells as visited (e.g., change `'1'` to `'V'`). This saves space but modifies the input.

If we're not allowed to modify the input, we can use a separate visited set or 2D array to keep track of visited cells.

## Common Mistakes

1. **Not checking boundaries**: Always check if the current position is within the grid boundaries before accessing it.
2. **Incorrect direction exploration**: Make sure to explore all four directions (up, down, left, right) and not diagonals.
3. **Not marking cells as visited**: Failing to mark cells as visited can lead to infinite loops or incorrect counts.
4. **Incorrect initialization**: Make sure to initialize the counter correctly and increment it only when a new island is found.

## Related Problems

1. **Number of Closed Islands**: Count the number of islands that are completely surrounded by water.
2. **Max Area of Island**: Find the maximum area of an island in the grid.
3. **Island Perimeter**: Calculate the perimeter of an island in the grid.
4. **Word Search**: Find if a word exists in a 2D board of characters.
5. **Surrounded Regions**: Capture all regions surrounded by 'X' in a board. 