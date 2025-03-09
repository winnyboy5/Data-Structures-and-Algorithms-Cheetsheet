# Pacific Atlantic Water Flow

## Problem Statement

There is an `m x n` rectangular island that borders both the Pacific Ocean and Atlantic Ocean. The Pacific Ocean touches the island's left and top edges, and the Atlantic Ocean touches the island's right and bottom edges.

The island is partitioned into a grid of square cells. You are given an `m x n` integer matrix `heights` where `heights[r][c]` represents the height above sea level of the cell at coordinate `(r, c)`.

The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is less than or equal to the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.

Return a 2D list of grid coordinates `result` where `result[i] = [ri, ci]` denotes that rain water can flow from cell `(ri, ci)` to both the Pacific and Atlantic oceans.

**Example 1:**
```
Input: heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]
Output: [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]
```

**Example 2:**
```
Input: heights = [[2,1],[1,2]]
Output: [[0,0],[0,1],[1,0],[1,1]]
```

## Intuition

The key insight for this problem is to approach it from the oceans' perspective rather than from each cell. Instead of checking if water can flow from each cell to both oceans (which would be inefficient), we can start from the cells adjacent to each ocean and work our way inward.

We'll perform two separate traversals:
1. From the Pacific Ocean (left and top edges)
2. From the Atlantic Ocean (right and bottom edges)

For each traversal, we'll mark cells that can be reached from the respective ocean. Then, the cells that can be reached from both oceans are the ones where water can flow to both oceans.

This approach works because water flow is bidirectional: if water can flow from cell A to cell B, then water can also flow from cell B to cell A (in the reverse direction).

## Visual Explanation

Let's visualize the solution with Example 1:

```
Heights:
[1, 2, 2, 3, 5]
[3, 2, 3, 4, 4]
[2, 4, 5, 3, 1]
[6, 7, 1, 4, 5]
[5, 1, 1, 2, 4]

Step 1: Identify cells adjacent to the Pacific Ocean (left and top edges)
Pacific borders: (0,0), (0,1), (0,2), (0,3), (0,4), (1,0), (2,0), (3,0), (4,0)

Step 2: Perform DFS from each Pacific border cell
Cells reachable from Pacific:
[1, 1, 1, 1, 1]
[1, 1, 1, 1, 1]
[1, 1, 1, 0, 0]
[1, 1, 0, 0, 0]
[1, 0, 0, 0, 0]
(1 means reachable, 0 means not reachable)

Step 3: Identify cells adjacent to the Atlantic Ocean (right and bottom edges)
Atlantic borders: (0,4), (1,4), (2,4), (3,4), (4,4), (4,0), (4,1), (4,2), (4,3)

Step 4: Perform DFS from each Atlantic border cell
Cells reachable from Atlantic:
[0, 0, 0, 0, 1]
[0, 0, 0, 1, 1]
[0, 0, 1, 1, 1]
[1, 1, 0, 1, 1]
[1, 0, 0, 1, 1]

Step 5: Find cells reachable from both oceans (intersection)
Cells reachable from both:
[0, 0, 0, 0, 1]
[0, 0, 0, 1, 1]
[0, 0, 1, 0, 0]
[1, 1, 0, 0, 0]
[1, 0, 0, 0, 0]

Result: [(0,4), (1,3), (1,4), (2,2), (3,0), (3,1), (4,0)]
```

![Pacific Atlantic Water Flow Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Create two boolean matrices to track cells reachable from the Pacific and Atlantic oceans.
2. Perform DFS from the cells adjacent to the Pacific Ocean (left and top edges):
   - Mark cells that can be reached from the Pacific.
   - A cell can be reached if its height is greater than or equal to the height of the current cell.
3. Perform DFS from the cells adjacent to the Atlantic Ocean (right and bottom edges):
   - Mark cells that can be reached from the Atlantic.
   - A cell can be reached if its height is greater than or equal to the height of the current cell.
4. Find the intersection of cells reachable from both oceans.
5. Return the coordinates of these cells.

## Code Implementation

### Python

```python
def pacificAtlantic(heights):
    if not heights or not heights[0]:
        return []
    
    m, n = len(heights), len(heights[0])
    pacific_reachable = [[False] * n for _ in range(m)]
    atlantic_reachable = [[False] * n for _ in range(m)]
    
    def dfs(r, c, reachable):
        # Mark the current cell as reachable
        reachable[r][c] = True
        
        # Directions: up, right, down, left
        directions = [(-1, 0), (0, 1), (1, 0), (0, -1)]
        
        for dr, dc in directions:
            nr, nc = r + dr, c + dc
            
            # Check if the new cell is within bounds, not visited, and has height >= current cell
            if (0 <= nr < m and 0 <= nc < n and 
                not reachable[nr][nc] and 
                heights[nr][nc] >= heights[r][c]):
                dfs(nr, nc, reachable)
    
    # DFS from Pacific borders (left and top edges)
    for i in range(m):
        dfs(i, 0, pacific_reachable)
    for j in range(n):
        dfs(0, j, pacific_reachable)
    
    # DFS from Atlantic borders (right and bottom edges)
    for i in range(m):
        dfs(i, n-1, atlantic_reachable)
    for j in range(n):
        dfs(m-1, j, atlantic_reachable)
    
    # Find cells reachable from both oceans
    result = []
    for i in range(m):
        for j in range(n):
            if pacific_reachable[i][j] and atlantic_reachable[i][j]:
                result.append([i, j])
    
    return result
```

### JavaScript

```javascript
function pacificAtlantic(heights) {
    if (!heights.length || !heights[0].length) {
        return [];
    }
    
    const m = heights.length;
    const n = heights[0].length;
    const pacificReachable = Array(m).fill().map(() => Array(n).fill(false));
    const atlanticReachable = Array(m).fill().map(() => Array(n).fill(false));
    
    function dfs(r, c, reachable) {
        // Mark the current cell as reachable
        reachable[r][c] = true;
        
        // Directions: up, right, down, left
        const directions = [[-1, 0], [0, 1], [1, 0], [0, -1]];
        
        for (const [dr, dc] of directions) {
            const nr = r + dr;
            const nc = c + dc;
            
            // Check if the new cell is within bounds, not visited, and has height >= current cell
            if (nr >= 0 && nr < m && nc >= 0 && nc < n && 
                !reachable[nr][nc] && 
                heights[nr][nc] >= heights[r][c]) {
                dfs(nr, nc, reachable);
            }
        }
    }
    
    // DFS from Pacific borders (left and top edges)
    for (let i = 0; i < m; i++) {
        dfs(i, 0, pacificReachable);
    }
    for (let j = 0; j < n; j++) {
        dfs(0, j, pacificReachable);
    }
    
    // DFS from Atlantic borders (right and bottom edges)
    for (let i = 0; i < m; i++) {
        dfs(i, n-1, atlanticReachable);
    }
    for (let j = 0; j < n; j++) {
        dfs(m-1, j, atlanticReachable);
    }
    
    // Find cells reachable from both oceans
    const result = [];
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (pacificReachable[i][j] && atlanticReachable[i][j]) {
                result.push([i, j]);
            }
        }
    }
    
    return result;
}
```

## BFS Approach

We can also solve this problem using BFS:

### Python (BFS)

```python
from collections import deque

def pacificAtlantic(heights):
    if not heights or not heights[0]:
        return []
    
    m, n = len(heights), len(heights[0])
    pacific_reachable = [[False] * n for _ in range(m)]
    atlantic_reachable = [[False] * n for _ in range(m)]
    
    def bfs(queue, reachable):
        directions = [(-1, 0), (0, 1), (1, 0), (0, -1)]
        
        while queue:
            r, c = queue.popleft()
            
            for dr, dc in directions:
                nr, nc = r + dr, c + dc
                
                if (0 <= nr < m and 0 <= nc < n and 
                    not reachable[nr][nc] and 
                    heights[nr][nc] >= heights[r][c]):
                    reachable[nr][nc] = True
                    queue.append((nr, nc))
    
    # BFS from Pacific borders
    pacific_queue = deque()
    for i in range(m):
        pacific_reachable[i][0] = True
        pacific_queue.append((i, 0))
    for j in range(n):
        pacific_reachable[0][j] = True
        pacific_queue.append((0, j))
    bfs(pacific_queue, pacific_reachable)
    
    # BFS from Atlantic borders
    atlantic_queue = deque()
    for i in range(m):
        atlantic_reachable[i][n-1] = True
        atlantic_queue.append((i, n-1))
    for j in range(n):
        atlantic_reachable[m-1][j] = True
        atlantic_queue.append((m-1, j))
    bfs(atlantic_queue, atlantic_reachable)
    
    # Find cells reachable from both oceans
    result = []
    for i in range(m):
        for j in range(n):
            if pacific_reachable[i][j] and atlantic_reachable[i][j]:
                result.append([i, j])
    
    return result
```

## Time and Space Complexity

- **Time Complexity**: O(m × n), where m is the number of rows and n is the number of columns. We visit each cell at most twice (once for Pacific and once for Atlantic).
- **Space Complexity**: O(m × n) for the two boolean matrices and the recursion stack (DFS) or queue (BFS).

## Detailed Walkthrough with Example

Let's trace through the DFS algorithm with a smaller example:

```
Heights:
[2, 1]
[1, 2]

Step 1: DFS from Pacific borders (left and top edges)
Pacific borders: (0,0), (0,1), (1,0)

DFS(0,0, pacific_reachable):
- Mark (0,0) as reachable from Pacific
- Check neighbors: (0,1) and (1,0)
  - heights[0][1] = 1 < heights[0][0] = 2, so water can't flow from (0,0) to (0,1)
  - heights[1][0] = 1 < heights[0][0] = 2, so water can't flow from (0,0) to (1,0)

DFS(0,1, pacific_reachable):
- Mark (0,1) as reachable from Pacific
- Check neighbors: (0,0) and (1,1)
  - heights[0][0] = 2 > heights[0][1] = 1, so water can flow from (0,1) to (0,0)
  - heights[1][1] = 2 > heights[0][1] = 1, so water can flow from (0,1) to (1,1)
  - DFS(1,1, pacific_reachable):
    - Mark (1,1) as reachable from Pacific
    - Check neighbors: (0,1), (1,0)
      - (0,1) is already visited
      - heights[1][0] = 1 < heights[1][1] = 2, so water can't flow from (1,1) to (1,0)

DFS(1,0, pacific_reachable):
- Mark (1,0) as reachable from Pacific
- Check neighbors: (0,0), (1,1)
  - heights[0][0] = 2 > heights[1][0] = 1, so water can flow from (1,0) to (0,0)
  - heights[1][1] = 2 > heights[1][0] = 1, so water can flow from (1,0) to (1,1)
  - (1,1) is already visited

Pacific reachable:
[1, 1]
[1, 1]

Step 2: DFS from Atlantic borders (right and bottom edges)
Atlantic borders: (0,1), (1,0), (1,1)

DFS(0,1, atlantic_reachable):
- Mark (0,1) as reachable from Atlantic
- Check neighbors: (0,0), (1,1)
  - heights[0][0] = 2 > heights[0][1] = 1, so water can flow from (0,1) to (0,0)
  - DFS(0,0, atlantic_reachable):
    - Mark (0,0) as reachable from Atlantic
    - Check neighbors: (0,1), (1,0)
      - (0,1) is already visited
      - heights[1][0] = 1 < heights[0][0] = 2, so water can't flow from (0,0) to (1,0)
  - heights[1][1] = 2 > heights[0][1] = 1, so water can flow from (0,1) to (1,1)
  - (1,1) is already visited

DFS(1,0, atlantic_reachable):
- Mark (1,0) as reachable from Atlantic
- Check neighbors: (0,0), (1,1)
  - heights[0][0] = 2 > heights[1][0] = 1, so water can flow from (1,0) to (0,0)
  - (0,0) is already visited
  - heights[1][1] = 2 > heights[1][0] = 1, so water can flow from (1,0) to (1,1)
  - (1,1) is already visited

DFS(1,1, atlantic_reachable):
- Mark (1,1) as reachable from Atlantic
- Check neighbors: (0,1), (1,0)
  - (0,1) is already visited
  - (1,0) is already visited

Atlantic reachable:
[1, 1]
[1, 1]

Step 3: Find cells reachable from both oceans
Cells reachable from both:
[1, 1]
[1, 1]

Result: [(0,0), (0,1), (1,0), (1,1)]
```

## Edge Cases and Optimizations

- **Empty Matrix**: If the matrix is empty, return an empty array.
- **Single Cell**: If the matrix has only one cell, it can flow to both oceans, so return its coordinates.
- **Optimization**: Instead of using two separate DFS/BFS traversals, we can combine them into a single traversal by using a single function with different starting points and reachable matrices.

## Common Mistakes

1. **Incorrect direction of water flow**: Remember that water flows from higher to lower elevation. In this problem, we're working backwards, so we check if the neighboring cell's height is greater than or equal to the current cell's height.
2. **Not marking cells as visited**: Make sure to mark cells as visited to avoid revisiting them.
3. **Not checking all border cells**: Make sure to start the traversal from all cells adjacent to each ocean.

## Related Problems

1. **Number of Islands**: Count the number of islands in a 2D grid.
2. **Surrounded Regions**: Capture all regions surrounded by 'X'.
3. **Walls and Gates**: Fill each empty room with the distance to its nearest gate.
4. **Course Schedule**: Determine if it's possible to finish all courses given the prerequisites. 