# Number of Connected Components in an Undirected Graph

## Problem Statement

You have a graph of `n` nodes. You are given an integer `n` and an array `edges` where `edges[i] = [ai, bi]` indicates that there is an undirected edge between nodes `ai` and `bi` in the graph.

Return the number of connected components in the graph.

**Example 1:**
```
Input: n = 5, edges = [[0,1],[1,2],[3,4]]
Output: 2
```

**Example 2:**
```
Input: n = 5, edges = [[0,1],[1,2],[2,3],[3,4]]
Output: 1
```

## Intuition

A connected component in an undirected graph is a subgraph in which any two vertices are connected to each other by paths. The problem asks us to count the number of such connected components.

There are two common approaches to solve this problem:

1. **Depth-First Search (DFS) or Breadth-First Search (BFS)**: We can use either DFS or BFS to traverse the graph. For each unvisited node, we start a new traversal and increment our counter. The final count will be the number of connected components.

2. **Union-Find (Disjoint Set)**: We can use the Union-Find data structure to group connected nodes together. Initially, each node is in its own set. For each edge, we union the sets containing the two nodes. The final number of distinct sets is the number of connected components.

## Visual Explanation

Let's visualize the solution with Example 1: `n = 5, edges = [[0,1],[1,2],[3,4]]`

```
Component 1:    Component 2:
    0               3
    |               |
    1               4
    |
    2
```

1. We have 5 nodes (0 to 4) and 3 edges.
2. Nodes 0, 1, and 2 form one connected component.
3. Nodes 3 and 4 form another connected component.
4. Total number of connected components: 2

Now let's look at Example 2: `n = 5, edges = [[0,1],[1,2],[2,3],[3,4]]`

```
Component 1:
    0
    |
    1
    |
    2
    |
    3
    |
    4
```

1. We have 5 nodes (0 to 4) and 4 edges.
2. All nodes are connected, forming a single connected component.
3. Total number of connected components: 1

![Connected Components Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

### DFS Approach:
1. Build an adjacency list representation of the graph.
2. Initialize a boolean array `visited` to keep track of visited nodes.
3. Initialize a counter `count` to 0.
4. For each node in the graph:
   - If the node has not been visited, increment `count` and perform DFS starting from this node.
   - During DFS, mark all reachable nodes as visited.
5. Return `count`.

### Union-Find Approach:
1. Initialize a Union-Find data structure with n nodes.
2. For each edge (a, b), union the sets containing a and b.
3. Count the number of distinct sets (i.e., the number of nodes that are their own parent).
4. Return the count.

## Code Implementation

### Python (DFS Approach)

```python
def countComponents(n, edges):
    # Build adjacency list
    graph = [[] for _ in range(n)]
    for a, b in edges:
        graph[a].append(b)
        graph[b].append(a)
    
    # Initialize visited array
    visited = [False] * n
    
    # DFS function
    def dfs(node):
        visited[node] = True
        for neighbor in graph[node]:
            if not visited[neighbor]:
                dfs(neighbor)
    
    # Count connected components
    count = 0
    for i in range(n):
        if not visited[i]:
            count += 1
            dfs(i)
    
    return count
```

### Python (Union-Find Approach)

```python
def countComponents(n, edges):
    # Initialize Union-Find data structure
    parent = list(range(n))
    
    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]
    
    def union(x, y):
        parent[find(x)] = find(y)
    
    # Process edges
    for a, b in edges:
        union(a, b)
    
    # Count distinct sets
    return sum(parent[i] == i for i in range(n))
```

### JavaScript (DFS Approach)

```javascript
function countComponents(n, edges) {
    // Build adjacency list
    const graph = Array(n).fill().map(() => []);
    for (const [a, b] of edges) {
        graph[a].push(b);
        graph[b].push(a);
    }
    
    // Initialize visited array
    const visited = Array(n).fill(false);
    
    // DFS function
    function dfs(node) {
        visited[node] = true;
        for (const neighbor of graph[node]) {
            if (!visited[neighbor]) {
                dfs(neighbor);
            }
        }
    }
    
    // Count connected components
    let count = 0;
    for (let i = 0; i < n; i++) {
        if (!visited[i]) {
            count++;
            dfs(i);
        }
    }
    
    return count;
}
```

### JavaScript (Union-Find Approach)

```javascript
function countComponents(n, edges) {
    // Initialize Union-Find data structure
    const parent = Array(n).fill().map((_, i) => i);
    
    function find(x) {
        if (parent[x] !== x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }
    
    function union(x, y) {
        parent[find(x)] = find(y);
    }
    
    // Process edges
    for (const [a, b] of edges) {
        union(a, b);
    }
    
    // Count distinct sets
    let count = 0;
    for (let i = 0; i < n; i++) {
        if (parent[i] === i) {
            count++;
        }
    }
    
    return count;
}
```

## Time and Space Complexity

### DFS Approach:
- **Time Complexity**: O(n + e), where n is the number of nodes and e is the number of edges. We visit each node and edge once during the DFS.
- **Space Complexity**: O(n + e) for the adjacency list and the visited array.

### Union-Find Approach:
- **Time Complexity**: O(n + e * α(n)), where α(n) is the inverse Ackermann function, which grows very slowly and is effectively constant for all practical purposes. The time is dominated by the e union operations.
- **Space Complexity**: O(n) for the parent array.

## Detailed Walkthrough with Example

Let's trace through the DFS approach with Example 1: `n = 5, edges = [[0,1],[1,2],[3,4]]`

1. Build the adjacency list:
   - graph = [[1], [0, 2], [1], [4], [3]]

2. Initialize visited array:
   - visited = [False, False, False, False, False]

3. Start counting connected components:
   - i = 0, not visited, count = 1
   - DFS(0):
     - Mark 0 as visited: visited = [True, False, False, False, False]
     - Neighbors of 0: 1
     - DFS(1):
       - Mark 1 as visited: visited = [True, True, False, False, False]
       - Neighbors of 1: 0, 2
       - Skip 0 (already visited)
       - DFS(2):
         - Mark 2 as visited: visited = [True, True, True, False, False]
         - Neighbors of 2: 1
         - Skip 1 (already visited)
         - No more neighbors, return
       - No more neighbors, return
     - No more neighbors, return
   - i = 1, already visited, skip
   - i = 2, already visited, skip
   - i = 3, not visited, count = 2
   - DFS(3):
     - Mark 3 as visited: visited = [True, True, True, True, False]
     - Neighbors of 3: 4
     - DFS(4):
       - Mark 4 as visited: visited = [True, True, True, True, True]
       - Neighbors of 4: 3
       - Skip 3 (already visited)
       - No more neighbors, return
     - No more neighbors, return
   - i = 4, already visited, skip

4. Return count = 2

Now let's trace through the Union-Find approach with Example 2: `n = 5, edges = [[0,1],[1,2],[2,3],[3,4]]`

1. Initialize the Union-Find data structure:
   - parent = [0, 1, 2, 3, 4]

2. Process edges:
   - Edge [0, 1]:
     - find(0) = 0, find(1) = 1
     - Union: parent = [1, 1, 2, 3, 4]
   - Edge [1, 2]:
     - find(1) = 1, find(2) = 2
     - Union: parent = [1, 1, 1, 3, 4]
   - Edge [2, 3]:
     - find(2) = 1, find(3) = 3
     - Union: parent = [1, 1, 1, 1, 4]
   - Edge [3, 4]:
     - find(3) = 1, find(4) = 4
     - Union: parent = [1, 1, 1, 1, 1]

3. Count distinct sets:
   - parent = [1, 1, 1, 1, 1]
   - No node is its own parent, so we need to find the root of each node
   - After path compression, all nodes have the same root: 1
   - Count = 1

4. Return count = 1

## Edge Cases and Optimizations

- **Empty Graph**: If n = 0, there are no connected components, so we return 0.
- **No Edges**: If there are no edges, each node is its own connected component, so we return n.
- **Optimization for Union-Find**: We can use path compression and union by rank to optimize the Union-Find operations.

## Common Mistakes

1. **Not handling isolated nodes**: Make sure to count nodes that are not connected to any other node as separate connected components.
2. **Incorrect graph representation**: When building the adjacency list, remember to add edges in both directions for an undirected graph.
3. **Not updating the parent array correctly in Union-Find**: After finding the root of a node, update the parent array to point directly to the root for path compression.

## Related Problems

1. **Graph Valid Tree**: Determine if an undirected graph is a valid tree.
2. **Number of Islands**: Count the number of islands in a 2D grid.
3. **Friend Circles**: Find the number of friend circles in a group of students.
4. **Accounts Merge**: Merge accounts that belong to the same person. 