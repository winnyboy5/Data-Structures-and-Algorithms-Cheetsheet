# Clone Graph

## Problem Statement

Given a reference of a node in a connected undirected graph, return a deep copy (clone) of the graph. Each node in the graph contains a value (int) and a list (List[Node]) of its neighbors.

**Example 1:**
```
Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
Output: [[2,4],[1,3],[2,4],[1,3]]
Explanation: 
There are 4 nodes in the graph.
1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
```

**Example 2:**
```
Input: adjList = [[]]
Output: [[]]
Explanation: Note that the input contains one empty list. The graph consists of only one node with val = 1 and it does not have any neighbors.
```

**Example 3:**
```
Input: adjList = []
Output: []
Explanation: This an empty graph, it does not have any nodes.
```

## Intuition

The key insight for this problem is to use a hash map to keep track of nodes that have already been cloned. As we traverse the original graph, we create new nodes for the clone graph and store the mapping between original nodes and their clones in the hash map. This allows us to handle cycles in the graph and avoid creating duplicate nodes.

We can use either depth-first search (DFS) or breadth-first search (BFS) to traverse the original graph. In this solution, we'll use DFS for its simplicity.

## Visual Explanation

Let's visualize the solution with Example 1:

```
Original Graph:
1 -- 2
|    |
4 -- 3

Step 1: Start DFS from node 1
- Create a clone of node 1: clone_1
- Add mapping: 1 -> clone_1 to the hash map
- Process neighbors of node 1: [2, 4]

Step 2: Process neighbor 2
- Create a clone of node 2: clone_2
- Add mapping: 2 -> clone_2 to the hash map
- Add clone_2 to clone_1's neighbors
- Process neighbors of node 2: [1, 3]
  - Node 1 is already cloned, so add clone_1 to clone_2's neighbors
  - Process neighbor 3...

Step 3: Process neighbor 3
- Create a clone of node 3: clone_3
- Add mapping: 3 -> clone_3 to the hash map
- Add clone_3 to clone_2's neighbors
- Process neighbors of node 3: [2, 4]
  - Node 2 is already cloned, so add clone_2 to clone_3's neighbors
  - Process neighbor 4...

Step 4: Process neighbor 4
- Create a clone of node 4: clone_4
- Add mapping: 4 -> clone_4 to the hash map
- Add clone_4 to clone_3's neighbors
- Process neighbors of node 4: [1, 3]
  - Node 1 is already cloned, so add clone_1 to clone_4's neighbors
  - Node 3 is already cloned, so add clone_3 to clone_4's neighbors

Step 5: Back to Step 2, continue processing neighbors of node 1
- Process neighbor 4 (already handled in Step 4)
- Add clone_4 to clone_1's neighbors

Final Cloned Graph:
clone_1 -- clone_2
   |         |
clone_4 -- clone_3
```

![Clone Graph Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Create a hash map to store the mapping between original nodes and their clones.
2. Implement a DFS function that takes an original node as input:
   - If the node is null, return null.
   - If the node has already been cloned (exists in the hash map), return its clone.
   - Create a new node with the same value as the original node.
   - Add the mapping between the original node and its clone to the hash map.
   - For each neighbor of the original node, recursively clone the neighbor and add it to the clone's neighbors list.
   - Return the clone.
3. Start the DFS from the given node.

## Code Implementation

### Python

```python
class Node:
    def __init__(self, val=0, neighbors=None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []

def cloneGraph(node):
    if not node:
        return None
    
    # Hash map to store the mapping between original nodes and their clones
    cloned = {}
    
    def dfs(original):
        # If the node has already been cloned, return its clone
        if original in cloned:
            return cloned[original]
        
        # Create a new node with the same value
        clone = Node(original.val)
        
        # Add the mapping between the original node and its clone
        cloned[original] = clone
        
        # Clone all neighbors and add them to the clone's neighbors list
        for neighbor in original.neighbors:
            clone.neighbors.append(dfs(neighbor))
        
        return clone
    
    return dfs(node)
```

### JavaScript

```javascript
/**
 * Definition for a Node.
 * function Node(val, neighbors) {
 *    this.val = val === undefined ? 0 : val;
 *    this.neighbors = neighbors === undefined ? [] : neighbors;
 * };
 */

function cloneGraph(node) {
    if (!node) {
        return null;
    }
    
    // Hash map to store the mapping between original nodes and their clones
    const cloned = new Map();
    
    function dfs(original) {
        // If the node has already been cloned, return its clone
        if (cloned.has(original)) {
            return cloned.get(original);
        }
        
        // Create a new node with the same value
        const clone = new Node(original.val);
        
        // Add the mapping between the original node and its clone
        cloned.set(original, clone);
        
        // Clone all neighbors and add them to the clone's neighbors list
        for (const neighbor of original.neighbors) {
            clone.neighbors.push(dfs(neighbor));
        }
        
        return clone;
    }
    
    return dfs(node);
}
```

## BFS Approach

We can also solve this problem using BFS:

### Python (BFS)

```python
from collections import deque

def cloneGraph(node):
    if not node:
        return None
    
    # Hash map to store the mapping between original nodes and their clones
    cloned = {node: Node(node.val)}
    
    # Queue for BFS
    queue = deque([node])
    
    while queue:
        original = queue.popleft()
        
        # Process all neighbors
        for neighbor in original.neighbors:
            # If the neighbor hasn't been cloned yet
            if neighbor not in cloned:
                # Create a clone for the neighbor
                cloned[neighbor] = Node(neighbor.val)
                # Add the neighbor to the queue for further processing
                queue.append(neighbor)
            
            # Add the cloned neighbor to the cloned node's neighbors list
            cloned[original].neighbors.append(cloned[neighbor])
    
    return cloned[node]
```

### JavaScript (BFS)

```javascript
function cloneGraph(node) {
    if (!node) {
        return null;
    }
    
    // Hash map to store the mapping between original nodes and their clones
    const cloned = new Map();
    cloned.set(node, new Node(node.val));
    
    // Queue for BFS
    const queue = [node];
    
    while (queue.length > 0) {
        const original = queue.shift();
        
        // Process all neighbors
        for (const neighbor of original.neighbors) {
            // If the neighbor hasn't been cloned yet
            if (!cloned.has(neighbor)) {
                // Create a clone for the neighbor
                cloned.set(neighbor, new Node(neighbor.val));
                // Add the neighbor to the queue for further processing
                queue.push(neighbor);
            }
            
            // Add the cloned neighbor to the cloned node's neighbors list
            cloned.get(original).neighbors.push(cloned.get(neighbor));
        }
    }
    
    return cloned.get(node);
}
```

## Time and Space Complexity

- **Time Complexity**: O(V + E), where V is the number of vertices (nodes) and E is the number of edges in the graph. We need to visit each node and process each edge once.
- **Space Complexity**: O(V) for the hash map that stores the mapping between original nodes and their clones, and for the recursion stack (DFS) or queue (BFS).

## Detailed Walkthrough with Example

Let's trace through the DFS algorithm with Example 1:

```
Original Graph:
1 -- 2
|    |
4 -- 3

Initialize:
cloned = {}

Call dfs(1):
- Create clone_1 = Node(1)
- Add mapping: 1 -> clone_1 to cloned
- Process neighbors of 1: [2, 4]
  - Call dfs(2):
    - Create clone_2 = Node(2)
    - Add mapping: 2 -> clone_2 to cloned
    - Process neighbors of 2: [1, 3]
      - Call dfs(1):
        - 1 is already in cloned, return clone_1
      - Add clone_1 to clone_2's neighbors
      - Call dfs(3):
        - Create clone_3 = Node(3)
        - Add mapping: 3 -> clone_3 to cloned
        - Process neighbors of 3: [2, 4]
          - Call dfs(2):
            - 2 is already in cloned, return clone_2
          - Add clone_2 to clone_3's neighbors
          - Call dfs(4):
            - Create clone_4 = Node(4)
            - Add mapping: 4 -> clone_4 to cloned
            - Process neighbors of 4: [1, 3]
              - Call dfs(1):
                - 1 is already in cloned, return clone_1
              - Add clone_1 to clone_4's neighbors
              - Call dfs(3):
                - 3 is already in cloned, return clone_3
              - Add clone_3 to clone_4's neighbors
            - Return clone_4
          - Add clone_4 to clone_3's neighbors
        - Return clone_3
      - Add clone_3 to clone_2's neighbors
    - Return clone_2
  - Add clone_2 to clone_1's neighbors
  - Call dfs(4):
    - 4 is already in cloned, return clone_4
  - Add clone_4 to clone_1's neighbors
- Return clone_1

Final Cloned Graph:
clone_1 -- clone_2
   |         |
clone_4 -- clone_3
```

## Edge Cases and Optimizations

- **Empty Graph**: If the input node is null, return null.
- **Single Node Graph**: If the graph consists of only one node with no neighbors, create a clone of that node and return it.
- **Cycles**: The hash map ensures that we don't get stuck in cycles by keeping track of nodes that have already been cloned.

## Common Mistakes

1. **Not handling cycles**: Forgetting to check if a node has already been cloned can lead to infinite recursion in graphs with cycles.
2. **Not cloning neighbors**: Make sure to recursively clone all neighbors of a node and add them to the clone's neighbors list.
3. **Modifying the original graph**: Be careful not to modify the original graph during the cloning process.

## Related Problems

1. **Course Schedule**: Determine if it's possible to finish all courses given the prerequisites.
2. **Number of Islands**: Count the number of islands in a 2D grid.
3. **Graph Valid Tree**: Check if an undirected graph is a valid tree.
4. **Pacific Atlantic Water Flow**: Find cells that can flow to both Pacific and Atlantic oceans. 