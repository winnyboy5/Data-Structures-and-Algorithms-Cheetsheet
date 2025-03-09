# Graph Algorithms

Graph algorithms are a set of instructions that traverse graph data structures. They are essential for solving problems in various domains, including social networks, transportation networks, and computer networks.

## Table of Contents
- [Graph Representation](#graph-representation)
- [Graph Traversal](#graph-traversal)
- [Shortest Path Algorithms](#shortest-path-algorithms)
- [Minimum Spanning Tree](#minimum-spanning-tree)
- [Topological Sorting](#topological-sorting)
- [Strongly Connected Components](#strongly-connected-components)
- [Network Flow](#network-flow)
- [Related Blind 75 & Grind 75 Problems](#related-blind-75--grind-75-problems)
- [Tips and Tricks](#tips-and-tricks)

## Graph Representation

### Adjacency Matrix
A 2D array where `matrix[i][j]` represents an edge from vertex `i` to vertex `j`.

#### Visual Representation
```
Graph:
A --- B
|     |
|     |
C --- D

Adjacency Matrix:
  | A | B | C | D
--+---+---+---+---
A | 0 | 1 | 1 | 0
B | 1 | 0 | 0 | 1
C | 1 | 0 | 0 | 1
D | 0 | 1 | 1 | 0
```

#### Implementation

```python
# Undirected graph using adjacency matrix
class Graph:
    def __init__(self, vertices):
        self.V = vertices
        self.graph = [[0 for _ in range(vertices)] for _ in range(vertices)]
    
    def add_edge(self, u, v):
        self.graph[u][v] = 1
        self.graph[v][u] = 1  # For undirected graph
```

### Adjacency List
A collection of lists where the list at index `i` contains all vertices adjacent to vertex `i`.

#### Visual Representation
```
Graph:
A --- B
|     |
|     |
C --- D

Adjacency List:
A: [B, C]
B: [A, D]
C: [A, D]
D: [B, C]
```

#### Implementation

```python
# Undirected graph using adjacency list
class Graph:
    def __init__(self, vertices):
        self.V = vertices
        self.graph = [[] for _ in range(vertices)]
    
    def add_edge(self, u, v):
        self.graph[u].append(v)
        self.graph[v].append(u)  # For undirected graph
```

### Edge List
A list of all edges in the graph.

#### Visual Representation
```
Graph:
A --- B
|     |
|     |
C --- D

Edge List:
[(A, B), (A, C), (B, D), (C, D)]
```

#### Implementation

```python
# Undirected graph using edge list
class Graph:
    def __init__(self):
        self.edges = []
    
    def add_edge(self, u, v):
        self.edges.append((u, v))
```

## Graph Traversal

### Breadth-First Search (BFS)
Explores all vertices at the current depth before moving to vertices at the next depth level.

#### Visual Explanation
```
Graph:
    A
   / \
  B   C
 /|   |\
D E   F G

BFS starting from A:
Level 0: A
Level 1: B, C
Level 2: D, E, F, G

Traversal order: A, B, C, D, E, F, G
```

#### Implementation

```python
from collections import deque

def bfs(graph, start):
    visited = [False] * len(graph)
    queue = deque([start])
    visited[start] = True
    result = []
    
    while queue:
        vertex = queue.popleft()
        result.append(vertex)
        
        for neighbor in graph[vertex]:
            if not visited[neighbor]:
                visited[neighbor] = True
                queue.append(neighbor)
    
    return result
```

### Depth-First Search (DFS)
Explores as far as possible along each branch before backtracking.

#### Visual Explanation
```
Graph:
    A
   / \
  B   C
 /|   |\
D E   F G

DFS starting from A (exploring left branches first):
A -> B -> D -> E -> C -> F -> G

Traversal order: A, B, D, E, C, F, G
```

#### Implementation

```python
def dfs(graph, start):
    visited = [False] * len(graph)
    result = []
    
    def dfs_util(vertex):
        visited[vertex] = True
        result.append(vertex)
        
        for neighbor in graph[vertex]:
            if not visited[neighbor]:
                dfs_util(neighbor)
    
    dfs_util(start)
    return result
```

## Shortest Path Algorithms

### Dijkstra's Algorithm
Finds the shortest path from a source vertex to all other vertices in a weighted graph with non-negative edge weights.

#### Visual Explanation
```
Weighted Graph:
    A
   /|\
  2 | 4
 /  |  \
B   1   C
 \  |  /
  3 | 1
   \|/
    D

Dijkstra's Algorithm starting from A:
1. Initialize distances: A=0, B=∞, C=∞, D=∞
2. Process A: Update B=2, C=4, D=1
3. Process D (closest unvisited): Update B=min(2, 1+3)=2, C=min(4, 1+1)=2
4. Process B (closest unvisited): No updates
5. Process C (closest unvisited): No updates

Final shortest distances from A: B=2, C=2, D=1
```

#### Implementation

```python
import heapq

def dijkstra(graph, start):
    # Initialize distances with infinity for all vertices except start
    distances = [float('inf')] * len(graph)
    distances[start] = 0
    
    # Priority queue to store vertices to be processed
    priority_queue = [(0, start)]
    
    while priority_queue:
        # Get the vertex with the smallest distance
        current_distance, current_vertex = heapq.heappop(priority_queue)
        
        # If we've already found a shorter path, skip
        if current_distance > distances[current_vertex]:
            continue
        
        # Process all neighbors
        for neighbor, weight in graph[current_vertex]:
            distance = current_distance + weight
            
            # If we found a shorter path, update and add to queue
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(priority_queue, (distance, neighbor))
    
    return distances
```

### Bellman-Ford Algorithm
Finds the shortest path from a source vertex to all other vertices in a weighted graph, even with negative edge weights.

#### Visual Explanation
```
Weighted Graph (with negative edges):
    A
   / \
  2   -1
 /     \
B       C
 \     /
  -2   3
   \ /
    D

Bellman-Ford Algorithm starting from A:
1. Initialize distances: A=0, B=∞, C=∞, D=∞
2. Iteration 1:
   - Process edge A->B: B=2
   - Process edge A->C: C=-1
   - Process edge B->D: D=0
   - Process edge C->D: D=min(0, -1+3)=0
3. Iteration 2:
   - Process edge A->B: No change
   - Process edge A->C: No change
   - Process edge B->D: No change
   - Process edge C->D: No change

Final shortest distances from A: B=2, C=-1, D=0
```

#### Implementation

```python
def bellman_ford(graph, start, V):
    # Initialize distances with infinity for all vertices except start
    distances = [float('inf')] * V
    distances[start] = 0
    
    # Relax all edges V-1 times
    for _ in range(V - 1):
        for u, v, w in graph:
            if distances[u] != float('inf') and distances[u] + w < distances[v]:
                distances[v] = distances[u] + w
    
    # Check for negative weight cycles
    for u, v, w in graph:
        if distances[u] != float('inf') and distances[u] + w < distances[v]:
            print("Graph contains negative weight cycle")
            return None
    
    return distances
```

### Floyd-Warshall Algorithm
Finds the shortest paths between all pairs of vertices in a weighted graph.

#### Visual Explanation
```
Weighted Graph:
    A
   / \
  2   5
 /     \
B       C
 \     /
  1   1
   \ /
    D

Floyd-Warshall Algorithm:
1. Initialize distance matrix with direct edge weights
2. For each vertex k as an intermediate:
   - For each pair of vertices (i, j):
     - If going through k is shorter, update distance[i][j]

Final distance matrix:
  | A | B | C | D
--+---+---+---+---
A | 0 | 2 | 5 | 3
B | 2 | 0 | 3 | 1
C | 5 | 3 | 0 | 1
D | 3 | 1 | 1 | 0
```

#### Implementation

```python
def floyd_warshall(graph, V):
    # Initialize distance matrix with direct edge weights
    dist = [[float('inf') for _ in range(V)] for _ in range(V)]
    
    # Set distance to self as 0
    for i in range(V):
        dist[i][i] = 0
    
    # Set direct edge weights
    for u, v, w in graph:
        dist[u][v] = w
    
    # Consider each vertex as an intermediate
    for k in range(V):
        for i in range(V):
            for j in range(V):
                if dist[i][k] != float('inf') and dist[k][j] != float('inf'):
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
    
    return dist
```

## Minimum Spanning Tree

### Kruskal's Algorithm
Finds a minimum spanning tree by selecting edges in increasing order of weight, avoiding cycles.

#### Visual Explanation
```
Weighted Graph:
A -- 7 -- B
|         |
2         8
|         |
C -- 3 -- D
|         |
4         9
|         |
E -- 5 -- F

Kruskal's Algorithm:
1. Sort edges by weight: (A,C)=2, (C,D)=3, (C,E)=4, (E,F)=5, (A,B)=7, (B,D)=8, (D,F)=9
2. Select edges in order, avoiding cycles:
   - Add (A,C)=2
   - Add (C,D)=3
   - Add (C,E)=4
   - Add (E,F)=5
   - Add (A,B)=7

Final MST: (A,C), (C,D), (C,E), (E,F), (A,B) with total weight 21
```

#### Implementation

```python
def kruskal(graph, V):
    # Sort edges by weight
    graph.sort(key=lambda x: x[2])
    
    # Initialize parent array for Union-Find
    parent = list(range(V))
    
    def find(i):
        if parent[i] != i:
            parent[i] = find(parent[i])
        return parent[i]
    
    def union(i, j):
        parent[find(i)] = find(j)
    
    mst = []
    total_weight = 0
    
    for u, v, w in graph:
        if find(u) != find(v):  # Check if adding this edge creates a cycle
            union(u, v)
            mst.append((u, v, w))
            total_weight += w
            
            if len(mst) == V - 1:
                break  # MST is complete
    
    return mst, total_weight
```

### Prim's Algorithm
Finds a minimum spanning tree by growing a single tree from a starting vertex.

#### Visual Explanation
```
Weighted Graph:
A -- 7 -- B
|         |
2         8
|         |
C -- 3 -- D
|         |
4         9
|         |
E -- 5 -- F

Prim's Algorithm starting from A:
1. Start with vertex A
2. Add the minimum weight edge connected to the tree: (A,C)=2
3. Add the minimum weight edge connected to the tree: (C,D)=3
4. Add the minimum weight edge connected to the tree: (C,E)=4
5. Add the minimum weight edge connected to the tree: (E,F)=5
6. Add the minimum weight edge connected to the tree: (A,B)=7

Final MST: (A,C), (C,D), (C,E), (E,F), (A,B) with total weight 21
```

#### Implementation

```python
import heapq

def prim(graph, V, start):
    # Initialize priority queue with edges from start vertex
    priority_queue = [(w, start, v) for v, w in graph[start]]
    heapq.heapify(priority_queue)
    
    visited = [False] * V
    visited[start] = True
    
    mst = []
    total_weight = 0
    
    while priority_queue and len(mst) < V - 1:
        w, u, v = heapq.heappop(priority_queue)
        
        if not visited[v]:
            visited[v] = True
            mst.append((u, v, w))
            total_weight += w
            
            # Add all edges from the newly added vertex
            for next_v, next_w in graph[v]:
                if not visited[next_v]:
                    heapq.heappush(priority_queue, (next_w, v, next_v))
    
    return mst, total_weight
```

## Topological Sorting

Topological sorting is a linear ordering of vertices in a directed acyclic graph (DAG) such that for every directed edge (u, v), vertex u comes before vertex v in the ordering.

#### Visual Explanation
```
Directed Acyclic Graph:
A --> B --> C
|     |
v     v
D --> E

Topological Sort:
1. Start with vertices with no incoming edges: A
2. Remove A and its outgoing edges, add to result: [A]
3. Vertices with no incoming edges: B, D
4. Remove B and its outgoing edges, add to result: [A, B]
5. Vertices with no incoming edges: C, D, E
6. Remove C and its outgoing edges, add to result: [A, B, C]
7. Vertices with no incoming edges: D, E
8. Remove D and its outgoing edges, add to result: [A, B, C, D]
9. Vertices with no incoming edges: E
10. Remove E and its outgoing edges, add to result: [A, B, C, D, E]

Final topological sort: A, B, C, D, E
```

#### Implementation

```python
def topological_sort(graph, V):
    # Calculate in-degree for each vertex
    in_degree = [0] * V
    for i in range(V):
        for j in graph[i]:
            in_degree[j] += 1
    
    # Initialize queue with vertices that have no incoming edges
    queue = deque()
    for i in range(V):
        if in_degree[i] == 0:
            queue.append(i)
    
    result = []
    while queue:
        vertex = queue.popleft()
        result.append(vertex)
        
        # Reduce in-degree of adjacent vertices
        for neighbor in graph[vertex]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    
    # Check if there was a cycle
    if len(result) != V:
        print("Graph contains a cycle")
        return None
    
    return result
```

## Strongly Connected Components

### Kosaraju's Algorithm
Finds all strongly connected components (SCCs) in a directed graph.

#### Visual Explanation
```
Directed Graph:
A --> B --> C
^     |     |
|     v     v
F <-- E <-- D

Kosaraju's Algorithm:
1. Perform DFS on the original graph to compute finishing times
   Finishing order: F, E, D, C, B, A
2. Transpose the graph (reverse all edges)
3. Perform DFS on the transposed graph in order of decreasing finishing time
   - Start with A: Visit A, B, C, D, E, F (all vertices)
   - This forms one SCC: {A, B, C, D, E, F}

If the graph was:
A --> B --> C
^     |
|     v
F <-- E     D

The SCCs would be: {A, B, E, F}, {C}, {D}
```

#### Implementation

```python
def kosaraju(graph, V):
    # Step 1: Perform DFS and store vertices in order of finishing time
    visited = [False] * V
    stack = []
    
    def dfs1(v):
        visited[v] = True
        for neighbor in graph[v]:
            if not visited[neighbor]:
                dfs1(neighbor)
        stack.append(v)
    
    for i in range(V):
        if not visited[i]:
            dfs1(i)
    
    # Step 2: Transpose the graph
    transposed = [[] for _ in range(V)]
    for i in range(V):
        for j in graph[i]:
            transposed[j].append(i)
    
    # Step 3: Perform DFS on the transposed graph
    visited = [False] * V
    sccs = []
    
    def dfs2(v, component):
        visited[v] = True
        component.append(v)
        for neighbor in transposed[v]:
            if not visited[neighbor]:
                dfs2(neighbor, component)
    
    while stack:
        vertex = stack.pop()
        if not visited[vertex]:
            component = []
            dfs2(vertex, component)
            sccs.append(component)
    
    return sccs
```

### Tarjan's Algorithm
Finds all strongly connected components in a directed graph in a single DFS pass.

#### Visual Explanation
```
Directed Graph:
A --> B --> C
^     |     |
|     v     v
F <-- E <-- D

Tarjan's Algorithm:
1. Perform DFS and assign discovery time and low-link values
2. When backtracking, update low-link values
3. If a vertex's discovery time equals its low-link value, it's the root of an SCC

For this graph, all vertices form one SCC: {A, B, C, D, E, F}
```

#### Implementation

```python
def tarjan(graph, V):
    disc = [-1] * V  # Discovery time
    low = [-1] * V   # Earliest visited vertex reachable from subtree
    stack = []
    in_stack = [False] * V
    sccs = []
    time = [0]  # Using a list to make it mutable in the nested function
    
    def dfs(u):
        disc[u] = low[u] = time[0]
        time[0] += 1
        stack.append(u)
        in_stack[u] = True
        
        for v in graph[u]:
            if disc[v] == -1:  # If v is not visited
                dfs(v)
                low[u] = min(low[u], low[v])
            elif in_stack[v]:  # Back edge to a vertex in the current SCC
                low[u] = min(low[u], disc[v])
        
        # If u is the root of an SCC
        if low[u] == disc[u]:
            component = []
            while True:
                v = stack.pop()
                in_stack[v] = False
                component.append(v)
                if v == u:
                    break
            sccs.append(component)
    
    for i in range(V):
        if disc[i] == -1:
            dfs(i)
    
    return sccs
```

## Network Flow

### Ford-Fulkerson Algorithm
Finds the maximum flow in a flow network.

#### Visual Explanation
```
Flow Network:
    A
   / \
  3   3
 /     \
S       T
 \     /
  2   2
   \ /
    B

Ford-Fulkerson Algorithm:
1. Initialize flow to 0
2. Find an augmenting path from S to T (e.g., S->A->T with capacity 3)
3. Augment flow by 3, update residual capacities
4. Find another augmenting path (e.g., S->B->T with capacity 2)
5. Augment flow by 2, update residual capacities
6. No more augmenting paths, maximum flow = 3 + 2 = 5
```

#### Implementation

```python
def ford_fulkerson(graph, source, sink):
    def bfs(residual_graph, source, sink, parent):
        visited = [False] * len(residual_graph)
        queue = deque([source])
        visited[source] = True
        
        while queue:
            u = queue.popleft()
            for v, capacity in enumerate(residual_graph[u]):
                if not visited[v] and capacity > 0:
                    queue.append(v)
                    visited[v] = True
                    parent[v] = u
        
        return visited[sink]
    
    # Create a residual graph
    residual_graph = [row[:] for row in graph]
    parent = [-1] * len(graph)
    max_flow = 0
    
    # Augment the flow while there is a path from source to sink
    while bfs(residual_graph, source, sink, parent):
        # Find the maximum flow through the path found
        path_flow = float('inf')
        s = sink
        while s != source:
            path_flow = min(path_flow, residual_graph[parent[s]][s])
            s = parent[s]
        
        # Add path flow to overall flow
        max_flow += path_flow
        
        # Update residual capacities
        v = sink
        while v != source:
            u = parent[v]
            residual_graph[u][v] -= path_flow
            residual_graph[v][u] += path_flow
            v = parent[v]
    
    return max_flow
```

### Edmonds-Karp Algorithm
An implementation of the Ford-Fulkerson method that uses BFS to find augmenting paths.

#### Implementation

```python
def edmonds_karp(graph, source, sink):
    def bfs(residual_graph, source, sink, parent):
        visited = [False] * len(residual_graph)
        queue = deque([source])
        visited[source] = True
        
        while queue:
            u = queue.popleft()
            for v, capacity in enumerate(residual_graph[u]):
                if not visited[v] and capacity > 0:
                    queue.append(v)
                    visited[v] = True
                    parent[v] = u
        
        return visited[sink]
    
    # Create a residual graph
    residual_graph = [row[:] for row in graph]
    parent = [-1] * len(graph)
    max_flow = 0
    
    # Augment the flow while there is a path from source to sink
    while bfs(residual_graph, source, sink, parent):
        # Find the maximum flow through the path found
        path_flow = float('inf')
        s = sink
        while s != source:
            path_flow = min(path_flow, residual_graph[parent[s]][s])
            s = parent[s]
        
        # Add path flow to overall flow
        max_flow += path_flow
        
        # Update residual capacities
        v = sink
        while v != source:
            u = parent[v]
            residual_graph[u][v] -= path_flow
            residual_graph[v][u] += path_flow
            v = parent[v]
    
    return max_flow
```

## Related Blind 75 & Grind 75 Problems

1. **Number of Islands** (Blind 75 #200)
   - Problem: Count the number of islands in a 2D grid.
   - Solution: Use DFS or BFS to explore connected land cells.
   - [LeetCode #200](https://leetcode.com/problems/number-of-islands/)

2. **Clone Graph** (Blind 75 #133)
   - Problem: Deep copy a graph.
   - Solution: Use BFS or DFS with a hash map to track visited nodes.
   - [LeetCode #133](https://leetcode.com/problems/clone-graph/)

3. **Course Schedule** (Blind 75 #207)
   - Problem: Determine if it's possible to finish all courses given prerequisites.
   - Solution: Use topological sort to detect cycles in the graph.
   - [LeetCode #207](https://leetcode.com/problems/course-schedule/)

4. **Pacific Atlantic Water Flow** (Blind 75 #417)
   - Problem: Find cells where water can flow to both Pacific and Atlantic oceans.
   - Solution: Use BFS or DFS from ocean boundaries.
   - [LeetCode #417](https://leetcode.com/problems/pacific-atlantic-water-flow/)

5. **Graph Valid Tree** (Blind 75 #261)
   - Problem: Determine if an undirected graph is a valid tree.
   - Solution: Check if the graph is connected and has no cycles.
   - [LeetCode #261](https://leetcode.com/problems/graph-valid-tree/)

6. **Network Delay Time** (Grind 75)
   - Problem: Find the time it takes for all nodes to receive a signal.
   - Solution: Use Dijkstra's algorithm to find the shortest paths.
   - [LeetCode #743](https://leetcode.com/problems/network-delay-time/)

## Tips and Tricks

1. **Choosing the Right Graph Representation**:
   - Adjacency Matrix: Good for dense graphs and quick edge lookups
   - Adjacency List: Good for sparse graphs and iterating over neighbors
   - Edge List: Good for algorithms that process all edges (e.g., Kruskal's)

2. **Choosing the Right Algorithm**:
   - For traversal: BFS (level by level) or DFS (deep exploration)
   - For shortest paths:
     - Dijkstra's: Non-negative weights
     - Bellman-Ford: Can handle negative weights
     - Floyd-Warshall: All pairs shortest paths
   - For minimum spanning tree:
     - Kruskal's: Sort edges and add them if they don't create cycles
     - Prim's: Grow a single tree from a starting vertex
   - For topological sorting: Kahn's algorithm or DFS-based approach
   - For strongly connected components: Kosaraju's or Tarjan's algorithm
   - For maximum flow: Ford-Fulkerson or Edmonds-Karp

3. **Common Graph Problem Patterns**:
   - Connectivity: Use DFS or BFS to check if vertices are connected
   - Cycle Detection: Use DFS with a visited set or Union-Find
   - Bipartite Graph: Use BFS or DFS with two-coloring
   - Shortest Path: Use BFS (unweighted) or Dijkstra's/Bellman-Ford (weighted)
   - Minimum Spanning Tree: Use Kruskal's or Prim's algorithm
   - Topological Sort: Use Kahn's algorithm or DFS-based approach

4. **Optimization Techniques**:
   - Use appropriate data structures (e.g., priority queue for Dijkstra's)
   - Precompute and cache results when possible
   - Consider bidirectional search for path finding
   - Use Union-Find for efficient cycle detection and component tracking

5. **Debugging Graph Algorithms**:
   - Visualize the graph and algorithm steps
   - Test with small, simple graphs first
   - Check edge cases: empty graph, disconnected graph, self-loops, etc.
   - Verify that the graph representation is correct 