# Graphs

A graph is a non-linear data structure consisting of nodes (vertices) and edges that connect these nodes. Graphs are used to represent networks of relationships between objects.

## Visual Representation

```
Undirected Graph:
    A --- B
    |     |
    |     |
    C --- D --- E

Directed Graph:
    A --→ B
    ↑     ↓
    |     |
    C ←-- D --→ E

Weighted Graph:
    A ---5--- B
    |         |
    |         |
    2         3
    |         |
    C ---1--- D ---4--- E
```

## Types of Graphs

1. **Undirected Graph**: Edges have no direction, representing a two-way relationship.
2. **Directed Graph (Digraph)**: Edges have direction, representing a one-way relationship.
3. **Weighted Graph**: Edges have weights or costs associated with them.
4. **Unweighted Graph**: Edges have no weights or costs.
5. **Cyclic Graph**: Contains at least one cycle (a path that starts and ends at the same vertex).
6. **Acyclic Graph**: Contains no cycles.
7. **Connected Graph**: There is a path between every pair of vertices.
8. **Disconnected Graph**: There are some vertices that cannot be reached from others.
9. **Complete Graph**: Every vertex is connected to every other vertex.
10. **Bipartite Graph**: Vertices can be divided into two disjoint sets such that no two vertices within the same set are adjacent.

## Graph Representations

### 1. Adjacency Matrix

A 2D array where matrix[i][j] represents an edge from vertex i to vertex j.

```
For the undirected graph above:
    A B C D E
A   0 1 1 0 0
B   1 0 0 1 0
C   1 0 0 1 0
D   0 1 1 0 1
E   0 0 0 1 0
```

### 2. Adjacency List

A collection of lists or arrays where the index represents a vertex and the list contains its adjacent vertices.

```
For the undirected graph above:
A: [B, C]
B: [A, D]
C: [A, D]
D: [B, C, E]
E: [D]
```

## Implementation in Python and JavaScript

### Python

#### Adjacency Matrix

```python
class Graph:
    def __init__(self, num_vertices):
        self.num_vertices = num_vertices
        self.matrix = [[0 for _ in range(num_vertices)] for _ in range(num_vertices)]
    
    def add_edge(self, v1, v2, weight=1):
        self.matrix[v1][v2] = weight
        # For undirected graph, uncomment the line below
        # self.matrix[v2][v1] = weight
    
    def remove_edge(self, v1, v2):
        self.matrix[v1][v2] = 0
        # For undirected graph, uncomment the line below
        # self.matrix[v2][v1] = 0
    
    def has_edge(self, v1, v2):
        return self.matrix[v1][v2] != 0
    
    def get_neighbors(self, vertex):
        neighbors = []
        for i in range(self.num_vertices):
            if self.matrix[vertex][i] != 0:
                neighbors.append(i)
        return neighbors
    
    def print_graph(self):
        for i in range(self.num_vertices):
            for j in range(self.num_vertices):
                print(self.matrix[i][j], end=" ")
            print()

# Example usage
graph = Graph(5)  # 5 vertices labeled 0 to 4 (A to E)
graph.add_edge(0, 1)  # A to B
graph.add_edge(0, 2)  # A to C
graph.add_edge(1, 3)  # B to D
graph.add_edge(2, 3)  # C to D
graph.add_edge(3, 4)  # D to E

print("Adjacency Matrix:")
graph.print_graph()

print("Neighbors of vertex 3 (D):", graph.get_neighbors(3))
```

#### Adjacency List

```python
class Graph:
    def __init__(self, num_vertices):
        self.num_vertices = num_vertices
        self.adj_list = [[] for _ in range(num_vertices)]
    
    def add_edge(self, v1, v2, weight=1):
        # For weighted graph, store (vertex, weight) tuples
        self.adj_list[v1].append((v2, weight))
        # For undirected graph, uncomment the line below
        # self.adj_list[v2].append((v1, weight))
    
    def remove_edge(self, v1, v2):
        self.adj_list[v1] = [(v, w) for v, w in self.adj_list[v1] if v != v2]
        # For undirected graph, uncomment the line below
        # self.adj_list[v2] = [(v, w) for v, w in self.adj_list[v2] if v != v1]
    
    def has_edge(self, v1, v2):
        return any(v == v2 for v, _ in self.adj_list[v1])
    
    def get_neighbors(self, vertex):
        return [v for v, _ in self.adj_list[vertex]]
    
    def print_graph(self):
        for i in range(self.num_vertices):
            print(f"Vertex {i}:", end=" ")
            for neighbor, weight in self.adj_list[i]:
                print(f"({neighbor}, {weight})", end=" ")
            print()

# Example usage
graph = Graph(5)  # 5 vertices labeled 0 to 4 (A to E)
graph.add_edge(0, 1)  # A to B
graph.add_edge(0, 2)  # A to C
graph.add_edge(1, 3)  # B to D
graph.add_edge(2, 3)  # C to D
graph.add_edge(3, 4)  # D to E

print("Adjacency List:")
graph.print_graph()

print("Neighbors of vertex 3 (D):", graph.get_neighbors(3))
```

### JavaScript

#### Adjacency Matrix

```javascript
class Graph {
    constructor(numVertices) {
        this.numVertices = numVertices;
        this.matrix = Array(numVertices).fill().map(() => Array(numVertices).fill(0));
    }
    
    addEdge(v1, v2, weight = 1) {
        this.matrix[v1][v2] = weight;
        // For undirected graph, uncomment the line below
        // this.matrix[v2][v1] = weight;
    }
    
    removeEdge(v1, v2) {
        this.matrix[v1][v2] = 0;
        // For undirected graph, uncomment the line below
        // this.matrix[v2][v1] = 0;
    }
    
    hasEdge(v1, v2) {
        return this.matrix[v1][v2] !== 0;
    }
    
    getNeighbors(vertex) {
        const neighbors = [];
        for (let i = 0; i < this.numVertices; i++) {
            if (this.matrix[vertex][i] !== 0) {
                neighbors.push(i);
            }
        }
        return neighbors;
    }
    
    printGraph() {
        for (let i = 0; i < this.numVertices; i++) {
            console.log(this.matrix[i].join(' '));
        }
    }
}

// Example usage
const graph = new Graph(5);  // 5 vertices labeled 0 to 4 (A to E)
graph.addEdge(0, 1);  // A to B
graph.addEdge(0, 2);  // A to C
graph.addEdge(1, 3);  // B to D
graph.addEdge(2, 3);  // C to D
graph.addEdge(3, 4);  // D to E

console.log("Adjacency Matrix:");
graph.printGraph();

console.log("Neighbors of vertex 3 (D):", graph.getNeighbors(3));
```

#### Adjacency List

```javascript
class Graph {
    constructor(numVertices) {
        this.numVertices = numVertices;
        this.adjList = Array(numVertices).fill().map(() => []);
    }
    
    addEdge(v1, v2, weight = 1) {
        // For weighted graph, store [vertex, weight] arrays
        this.adjList[v1].push([v2, weight]);
        // For undirected graph, uncomment the line below
        // this.adjList[v2].push([v1, weight]);
    }
    
    removeEdge(v1, v2) {
        this.adjList[v1] = this.adjList[v1].filter(([v, _]) => v !== v2);
        // For undirected graph, uncomment the line below
        // this.adjList[v2] = this.adjList[v2].filter(([v, _]) => v !== v1);
    }
    
    hasEdge(v1, v2) {
        return this.adjList[v1].some(([v, _]) => v === v2);
    }
    
    getNeighbors(vertex) {
        return this.adjList[vertex].map(([v, _]) => v);
    }
    
    printGraph() {
        for (let i = 0; i < this.numVertices; i++) {
            const neighbors = this.adjList[i].map(([v, w]) => `(${v}, ${w})`).join(' ');
            console.log(`Vertex ${i}: ${neighbors}`);
        }
    }
}

// Example usage
const graph = new Graph(5);  // 5 vertices labeled 0 to 4 (A to E)
graph.addEdge(0, 1);  // A to B
graph.addEdge(0, 2);  // A to C
graph.addEdge(1, 3);  // B to D
graph.addEdge(2, 3);  // C to D
graph.addEdge(3, 4);  // D to E

console.log("Adjacency List:");
graph.printGraph();

console.log("Neighbors of vertex 3 (D):", graph.getNeighbors(3));
```

## Graph Traversal Algorithms

### 1. Breadth-First Search (BFS)

BFS explores all the vertices at the current level before moving to the next level.

**Python:**
```python
from collections import deque

def bfs(graph, start_vertex):
    visited = [False] * graph.num_vertices
    result = []
    queue = deque([start_vertex])
    visited[start_vertex] = True
    
    while queue:
        vertex = queue.popleft()
        result.append(vertex)
        
        for neighbor in graph.get_neighbors(vertex):
            if not visited[neighbor]:
                visited[neighbor] = True
                queue.append(neighbor)
    
    return result

# Example usage
# Using the graph defined above
print("BFS starting from vertex 0 (A):", bfs(graph, 0))
```

**JavaScript:**
```javascript
function bfs(graph, startVertex) {
    const visited = Array(graph.numVertices).fill(false);
    const result = [];
    const queue = [startVertex];
    visited[startVertex] = true;
    
    while (queue.length > 0) {
        const vertex = queue.shift();
        result.push(vertex);
        
        for (const neighbor of graph.getNeighbors(vertex)) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                queue.push(neighbor);
            }
        }
    }
    
    return result;
}

// Example usage
// Using the graph defined above
console.log("BFS starting from vertex 0 (A):", bfs(graph, 0));
```

### 2. Depth-First Search (DFS)

DFS explores as far as possible along each branch before backtracking.

**Python:**
```python
def dfs(graph, start_vertex):
    visited = [False] * graph.num_vertices
    result = []
    
    def dfs_recursive(vertex):
        visited[vertex] = True
        result.append(vertex)
        
        for neighbor in graph.get_neighbors(vertex):
            if not visited[neighbor]:
                dfs_recursive(neighbor)
    
    dfs_recursive(start_vertex)
    return result

# Example usage
# Using the graph defined above
print("DFS starting from vertex 0 (A):", dfs(graph, 0))
```

**JavaScript:**
```javascript
function dfs(graph, startVertex) {
    const visited = Array(graph.numVertices).fill(false);
    const result = [];
    
    function dfsRecursive(vertex) {
        visited[vertex] = true;
        result.push(vertex);
        
        for (const neighbor of graph.getNeighbors(vertex)) {
            if (!visited[neighbor]) {
                dfsRecursive(neighbor);
            }
        }
    }
    
    dfsRecursive(startVertex);
    return result;
}

// Example usage
// Using the graph defined above
console.log("DFS starting from vertex 0 (A):", dfs(graph, 0));
```

## Common Graph Algorithms

### 1. Dijkstra's Algorithm (Shortest Path)

Finds the shortest path from a source vertex to all other vertices in a weighted graph.

**Python:**
```python
import heapq

def dijkstra(graph, start_vertex):
    # Initialize distances with infinity for all vertices except the start vertex
    distances = [float('inf')] * graph.num_vertices
    distances[start_vertex] = 0
    
    # Priority queue to store vertices to be processed
    priority_queue = [(0, start_vertex)]
    
    while priority_queue:
        current_distance, current_vertex = heapq.heappop(priority_queue)
        
        # If we've already found a shorter path, skip
        if current_distance > distances[current_vertex]:
            continue
        
        # Check all neighbors of the current vertex
        for neighbor, weight in graph.adj_list[current_vertex]:
            distance = current_distance + weight
            
            # If we found a shorter path to the neighbor, update it
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(priority_queue, (distance, neighbor))
    
    return distances

# Example usage
# Create a weighted graph
weighted_graph = Graph(5)
weighted_graph.add_edge(0, 1, 5)  # A to B with weight 5
weighted_graph.add_edge(0, 2, 2)  # A to C with weight 2
weighted_graph.add_edge(1, 3, 3)  # B to D with weight 3
weighted_graph.add_edge(2, 3, 1)  # C to D with weight 1
weighted_graph.add_edge(3, 4, 4)  # D to E with weight 4

print("Shortest distances from vertex 0 (A):", dijkstra(weighted_graph, 0))
```

**JavaScript:**
```javascript
class PriorityQueue {
    constructor() {
        this.queue = [];
    }
    
    enqueue(element, priority) {
        this.queue.push({ element, priority });
        this.sort();
    }
    
    dequeue() {
        if (this.isEmpty()) {
            return null;
        }
        return this.queue.shift().element;
    }
    
    isEmpty() {
        return this.queue.length === 0;
    }
    
    sort() {
        this.queue.sort((a, b) => a.priority - b.priority);
    }
}

function dijkstra(graph, startVertex) {
    // Initialize distances with infinity for all vertices except the start vertex
    const distances = Array(graph.numVertices).fill(Infinity);
    distances[startVertex] = 0;
    
    // Priority queue to store vertices to be processed
    const priorityQueue = new PriorityQueue();
    priorityQueue.enqueue(startVertex, 0);
    
    while (!priorityQueue.isEmpty()) {
        const currentVertex = priorityQueue.dequeue();
        
        // Check all neighbors of the current vertex
        for (const [neighbor, weight] of graph.adjList[currentVertex]) {
            const distance = distances[currentVertex] + weight;
            
            // If we found a shorter path to the neighbor, update it
            if (distance < distances[neighbor]) {
                distances[neighbor] = distance;
                priorityQueue.enqueue(neighbor, distance);
            }
        }
    }
    
    return distances;
}

// Example usage
// Create a weighted graph
const weightedGraph = new Graph(5);
weightedGraph.addEdge(0, 1, 5);  // A to B with weight 5
weightedGraph.addEdge(0, 2, 2);  // A to C with weight 2
weightedGraph.addEdge(1, 3, 3);  // B to D with weight 3
weightedGraph.addEdge(2, 3, 1);  // C to D with weight 1
weightedGraph.addEdge(3, 4, 4);  // D to E with weight 4

console.log("Shortest distances from vertex 0 (A):", dijkstra(weightedGraph, 0));
```

### 2. Topological Sort

Sorts the vertices of a directed acyclic graph (DAG) such that for every directed edge (u, v), vertex u comes before vertex v in the ordering.

**Python:**
```python
def topological_sort(graph):
    visited = [False] * graph.num_vertices
    stack = []
    
    def dfs(vertex):
        visited[vertex] = True
        
        for neighbor in graph.get_neighbors(vertex):
            if not visited[neighbor]:
                dfs(neighbor)
        
        stack.append(vertex)
    
    for i in range(graph.num_vertices):
        if not visited[i]:
            dfs(i)
    
    return stack[::-1]  # Reverse the stack to get the topological order

# Example usage
# Create a directed acyclic graph (DAG)
dag = Graph(6)
dag.add_edge(5, 2)
dag.add_edge(5, 0)
dag.add_edge(4, 0)
dag.add_edge(4, 1)
dag.add_edge(2, 3)
dag.add_edge(3, 1)

print("Topological Sort:", topological_sort(dag))
```

**JavaScript:**
```javascript
function topologicalSort(graph) {
    const visited = Array(graph.numVertices).fill(false);
    const stack = [];
    
    function dfs(vertex) {
        visited[vertex] = true;
        
        for (const neighbor of graph.getNeighbors(vertex)) {
            if (!visited[neighbor]) {
                dfs(neighbor);
            }
        }
        
        stack.push(vertex);
    }
    
    for (let i = 0; i < graph.numVertices; i++) {
        if (!visited[i]) {
            dfs(i);
        }
    }
    
    return stack.reverse();  // Reverse the stack to get the topological order
}

// Example usage
// Create a directed acyclic graph (DAG)
const dag = new Graph(6);
dag.addEdge(5, 2);
dag.addEdge(5, 0);
dag.addEdge(4, 0);
dag.addEdge(4, 1);
dag.addEdge(2, 3);
dag.addEdge(3, 1);

console.log("Topological Sort:", topologicalSort(dag));
```

### 3. Minimum Spanning Tree (Kruskal's Algorithm)

Finds a subset of the edges that forms a tree that includes every vertex, where the total weight of all the edges in the tree is minimized.

**Python:**
```python
def kruskal(graph):
    # Edge list representation: (weight, u, v)
    edges = []
    for u in range(graph.num_vertices):
        for v, weight in graph.adj_list[u]:
            if u < v:  # To avoid duplicate edges in undirected graph
                edges.append((weight, u, v))
    
    # Sort edges by weight
    edges.sort()
    
    # Initialize parent array for Union-Find
    parent = list(range(graph.num_vertices))
    
    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]
    
    def union(x, y):
        parent[find(x)] = find(y)
    
    mst = []
    for weight, u, v in edges:
        if find(u) != find(v):
            union(u, v)
            mst.append((u, v, weight))
    
    return mst

# Example usage
# Create an undirected weighted graph
undirected_graph = Graph(5)
undirected_graph.add_edge(0, 1, 2)
undirected_graph.add_edge(0, 3, 6)
undirected_graph.add_edge(1, 2, 3)
undirected_graph.add_edge(1, 3, 8)
undirected_graph.add_edge(1, 4, 5)
undirected_graph.add_edge(2, 4, 7)
undirected_graph.add_edge(3, 4, 9)
# Add reverse edges for undirected graph
for u in range(undirected_graph.num_vertices):
    for v, weight in list(undirected_graph.adj_list[u]):
        undirected_graph.add_edge(v, u, weight)

print("Minimum Spanning Tree (Kruskal's):", kruskal(undirected_graph))
```

**JavaScript:**
```javascript
function kruskal(graph) {
    // Edge list representation: [weight, u, v]
    const edges = [];
    for (let u = 0; u < graph.numVertices; u++) {
        for (const [v, weight] of graph.adjList[u]) {
            if (u < v) {  // To avoid duplicate edges in undirected graph
                edges.push([weight, u, v]);
            }
        }
    }
    
    // Sort edges by weight
    edges.sort((a, b) => a[0] - b[0]);
    
    // Initialize parent array for Union-Find
    const parent = Array(graph.numVertices).fill().map((_, i) => i);
    
    function find(x) {
        if (parent[x] !== x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }
    
    function union(x, y) {
        parent[find(x)] = find(y);
    }
    
    const mst = [];
    for (const [weight, u, v] of edges) {
        if (find(u) !== find(v)) {
            union(u, v);
            mst.push([u, v, weight]);
        }
    }
    
    return mst;
}

// Example usage
// Create an undirected weighted graph
const undirectedGraph = new Graph(5);
undirectedGraph.addEdge(0, 1, 2);
undirectedGraph.addEdge(0, 3, 6);
undirectedGraph.addEdge(1, 2, 3);
undirectedGraph.addEdge(1, 3, 8);
undirectedGraph.addEdge(1, 4, 5);
undirectedGraph.addEdge(2, 4, 7);
undirectedGraph.addEdge(3, 4, 9);
// Add reverse edges for undirected graph
for (let u = 0; u < undirectedGraph.numVertices; u++) {
    for (const [v, weight] of [...undirectedGraph.adjList[u]]) {
        undirectedGraph.addEdge(v, u, weight);
    }
}

console.log("Minimum Spanning Tree (Kruskal's):", kruskal(undirectedGraph));
```

## Time and Space Complexity

| Operation | Adjacency Matrix | Adjacency List |
|-----------|------------------|----------------|
| Add Edge  | O(1)             | O(1)           |
| Remove Edge | O(1)           | O(E)           |
| Check if edge exists | O(1)  | O(E)           |
| Find all neighbors | O(V)    | O(E)           |
| BFS/DFS   | O(V²)            | O(V + E)       |
| Dijkstra  | O(V²)            | O(E log V)     |
| Topological Sort | O(V²)     | O(V + E)       |
| Kruskal's MST | O(E log E)   | O(E log E)     |

Space Complexity:
- Adjacency Matrix: O(V²)
- Adjacency List: O(V + E)

Where V is the number of vertices and E is the number of edges.

## Advantages and Disadvantages

### Adjacency Matrix

**Advantages:**
1. Simple implementation
2. O(1) edge lookup
3. Good for dense graphs

**Disadvantages:**
1. O(V²) space complexity
2. O(V²) time complexity for traversal
3. Inefficient for sparse graphs

### Adjacency List

**Advantages:**
1. Space-efficient for sparse graphs
2. O(V + E) time complexity for traversal
3. Efficient for most graph algorithms

**Disadvantages:**
1. O(E) edge lookup
2. More complex implementation
3. Less efficient for dense graphs

## When to Use Graphs

- When representing relationships between objects
- When modeling networks (social, computer, transportation)
- When solving problems involving paths, connectivity, or flow
- When dealing with hierarchical structures that can have multiple paths
- When implementing algorithms like shortest path, minimum spanning tree, or topological sort

## Common Graph Problems from Blind 75 and Grind 75

1. **Number of Islands** (Medium): Count the number of islands in a 2D grid.
2. **Clone Graph** (Medium): Deep copy a graph.
3. **Course Schedule** (Medium): Determine if it's possible to finish all courses given prerequisites.
4. **Pacific Atlantic Water Flow** (Medium): Find cells where water can flow to both oceans.
5. **Word Ladder** (Hard): Find the shortest transformation sequence from one word to another.
6. **Graph Valid Tree** (Medium): Determine if an undirected graph is a valid tree.
7. **Network Delay Time** (Medium): Find the time it takes for all nodes to receive a signal.
8. **Alien Dictionary** (Hard): Determine the order of characters in an alien language.

## How to Identify Graph Problems

Look for these clues in the problem statement:

1. The problem involves relationships between objects.
2. The problem mentions networks, connections, or paths.
3. The problem requires finding the shortest path or minimum cost path.
4. The problem involves detecting cycles or topological ordering.
5. The problem can be modeled as a grid or matrix where cells are connected.
6. Keywords like "network," "connection," "path," "route," or "dependency." 