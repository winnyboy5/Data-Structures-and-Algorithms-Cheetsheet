# Graph Valid Tree

## Problem Statement

You have a graph of `n` nodes labeled from `0` to `n - 1`. You are given an integer `n` and a list of `edges` where `edges[i] = [ai, bi]` indicates that there is an undirected edge between nodes `ai` and `bi` in the graph.

Return `true` if the edges of the given graph make up a valid tree, and `false` otherwise.

**Example 1:**
```
Input: n = 5, edges = [[0,1],[0,2],[0,3],[1,4]]
Output: true
```

**Example 2:**
```
Input: n = 5, edges = [[0,1],[1,2],[2,3],[1,3],[1,4]]
Output: false
```

## Intuition

A graph is a valid tree if and only if it satisfies two conditions:
1. The graph is fully connected (there is a path between every pair of nodes).
2. The graph contains no cycles.

To check these conditions, we can use either Depth-First Search (DFS) or Breadth-First Search (BFS) to traverse the graph. We'll start from any node (typically node 0) and check if we can visit all nodes without encountering any cycles.

Another approach is to use the Union-Find (Disjoint Set) data structure. For a graph to be a valid tree, it must have exactly `n-1` edges connecting `n` nodes. If there are fewer edges, the graph is not fully connected. If there are more edges, the graph must contain a cycle.

## Visual Explanation

Let's visualize the solution with Example 1: `n = 5, edges = [[0,1],[0,2],[0,3],[1,4]]`

```
    0
   /|\
  1 2 3
 /
4
```

1. We start DFS from node 0.
2. We visit nodes 1, 2, 3, and 4 without encountering any cycles.
3. All nodes are visited, and there are no cycles, so the graph is a valid tree.

Now let's look at Example 2: `n = 5, edges = [[0,1],[1,2],[2,3],[1,3],[1,4]]`

```
    0
    |
    1
   /|\
  2 3 4
   \|
    3
```

1. We start DFS from node 0.
2. We visit node 1.
3. From node 1, we visit nodes 2, 3, and 4.
4. When we try to visit node 3 from node 2, we find that node 3 has already been visited, indicating a cycle.
5. The graph contains a cycle, so it's not a valid tree.

![Graph Valid Tree Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

### DFS Approach:
1. Build an adjacency list representation of the graph.
2. Use a set to keep track of visited nodes.
3. Perform DFS starting from node 0.
   - For each node, mark it as visited.
   - For each unvisited neighbor, recursively perform DFS.
   - If we encounter a visited neighbor that is not the parent of the current node, we've found a cycle.
4. After DFS, check if all nodes have been visited.
5. Return true if all nodes are visited and no cycles are found, false otherwise.

### Union-Find Approach:
1. Initialize a Union-Find data structure with n nodes.
2. For each edge (a, b):
   - If a and b are already in the same set, we've found a cycle.
   - Otherwise, union the sets containing a and b.
3. After processing all edges, check if all nodes are in the same set.
4. Return true if all nodes are in the same set and there are exactly n-1 edges, false otherwise.

## Code Implementation

### Python (DFS Approach)

```python
def validTree(n, edges):
    if len(edges) != n - 1:
        return False
    
    # Build adjacency list
    graph = [[] for _ in range(n)]
    for a, b in edges:
        graph[a].append(b)
        graph[b].append(a)
    
    # DFS to check for cycles and connectivity
    visited = set()
    
    def dfs(node, parent):
        visited.add(node)
        for neighbor in graph[node]:
            if neighbor == parent:
                continue
            if neighbor in visited:
                return False
            if not dfs(neighbor, node):
                return False
        return True
    
    # Start DFS from node 0
    if not dfs(0, -1):
        return False
    
    # Check if all nodes are visited
    return len(visited) == n
```

### Python (Union-Find Approach)

```python
def validTree(n, edges):
    if len(edges) != n - 1:
        return False
    
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
        if find(a) == find(b):
            return False
        union(a, b)
    
    # Check if all nodes are in the same set
    root = find(0)
    return all(find(i) == root for i in range(n))
```

### JavaScript (DFS Approach)

```javascript
function validTree(n, edges) {
    if (edges.length !== n - 1) {
        return false;
    }
    
    // Build adjacency list
    const graph = Array(n).fill().map(() => []);
    for (const [a, b] of edges) {
        graph[a].push(b);
        graph[b].push(a);
    }
    
    // DFS to check for cycles and connectivity
    const visited = new Set();
    
    function dfs(node, parent) {
        visited.add(node);
        for (const neighbor of graph[node]) {
            if (neighbor === parent) {
                continue;
            }
            if (visited.has(neighbor)) {
                return false;
            }
            if (!dfs(neighbor, node)) {
                return false;
            }
        }
        return true;
    }
    
    // Start DFS from node 0
    if (!dfs(0, -1)) {
        return false;
    }
    
    // Check if all nodes are visited
    return visited.size === n;
}
```

### JavaScript (Union-Find Approach)

```javascript
function validTree(n, edges) {
    if (edges.length !== n - 1) {
        return false;
    }
    
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
        if (find(a) === find(b)) {
            return false;
        }
        union(a, b);
    }
    
    // Check if all nodes are in the same set
    const root = find(0);
    for (let i = 0; i < n; i++) {
        if (find(i) !== root) {
            return false;
        }
    }
    
    return true;
}
```

## Time and Space Complexity

### DFS Approach:
- **Time Complexity**: O(n + e), where n is the number of nodes and e is the number of edges. We visit each node and edge once during the DFS.
- **Space Complexity**: O(n + e) for the adjacency list and the visited set.

### Union-Find Approach:
- **Time Complexity**: O(n + e * α(n)), where α(n) is the inverse Ackermann function, which grows very slowly and is effectively constant for all practical purposes. The time is dominated by the e union operations.
- **Space Complexity**: O(n) for the parent array.

## Detailed Walkthrough with Example

Let's trace through the DFS approach with Example 1: `n = 5, edges = [[0,1],[0,2],[0,3],[1,4]]`

1. Check if the number of edges is n-1:
   - n = 5, edges.length = 4, n-1 = 4, so we continue.

2. Build the adjacency list:
   - graph = [[1, 2, 3], [0, 4], [0], [0], [1]]

3. Start DFS from node 0:
   - visited = {0}
   - Neighbors of 0: 1, 2, 3
   - Visit 1:
     - visited = {0, 1}
     - Neighbors of 1: 0, 4
     - Skip 0 (parent)
     - Visit 4:
       - visited = {0, 1, 4}
       - Neighbors of 4: 1
       - Skip 1 (parent)
       - No more neighbors, return true
     - No more neighbors, return true
   - Visit 2:
     - visited = {0, 1, 4, 2}
     - Neighbors of 2: 0
     - Skip 0 (parent)
     - No more neighbors, return true
   - Visit 3:
     - visited = {0, 1, 4, 2, 3}
     - Neighbors of 3: 0
     - Skip 0 (parent)
     - No more neighbors, return true
   - No more neighbors, return true

4. Check if all nodes are visited:
   - visited = {0, 1, 2, 3, 4}, n = 5
   - All nodes are visited, so return true.

Now let's trace through the Union-Find approach with Example 2: `n = 5, edges = [[0,1],[1,2],[2,3],[1,3],[1,4]]`

1. Check if the number of edges is n-1:
   - n = 5, edges.length = 5, n-1 = 4
   - edges.length > n-1, so we might have a cycle, but let's continue.

2. Initialize the Union-Find data structure:
   - parent = [0, 1, 2, 3, 4]

3. Process edges:
   - Edge [0, 1]:
     - find(0) = 0, find(1) = 1, they are in different sets
     - Union: parent = [1, 1, 2, 3, 4]
   - Edge [1, 2]:
     - find(1) = 1, find(2) = 2, they are in different sets
     - Union: parent = [1, 1, 1, 3, 4]
   - Edge [2, 3]:
     - find(2) = 1, find(3) = 3, they are in different sets
     - Union: parent = [1, 1, 1, 1, 4]
   - Edge [1, 3]:
     - find(1) = 1, find(3) = 1, they are in the same set
     - We've found a cycle, so return false.

## Edge Cases and Optimizations

- **Empty Graph**: If n = 1 and edges is empty, the graph is a valid tree (a single node with no edges).
- **Disconnected Graph**: If the graph has fewer than n-1 edges, it cannot be fully connected, so it's not a valid tree.
- **Graph with Cycles**: If the graph has more than n-1 edges, it must contain a cycle, so it's not a valid tree.
- **Optimization**: We can immediately return false if the number of edges is not exactly n-1, as this is a necessary condition for a valid tree.

## Common Mistakes

1. **Not checking the number of edges**: A valid tree with n nodes must have exactly n-1 edges.
2. **Not handling disconnected components**: Even if there are no cycles, the graph must be fully connected to be a valid tree.
3. **Incorrect cycle detection**: When using DFS, we need to be careful not to confuse a parent-child relationship with a cycle.

## Related Problems

1. **Number of Connected Components in an Undirected Graph**: Count the number of connected components in an undirected graph.
2. **Redundant Connection**: Find an edge that can be removed so that the resulting graph is a tree.
3. **Course Schedule**: Determine if it's possible to finish all courses given prerequisites (cycle detection in a directed graph).
4. **Is Graph Bipartite?**: Determine if a graph can be divided into two independent sets. 