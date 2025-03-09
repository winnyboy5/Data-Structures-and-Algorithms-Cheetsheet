# Topological Sort Pattern

Topological Sort is a linear ordering of vertices in a directed acyclic graph (DAG) such that for every directed edge (u, v), vertex u comes before vertex v in the ordering. In simpler terms, if there is a path from vertex A to vertex B in the graph, then vertex A appears before vertex B in the topological ordering.

## Visual Representation

```
Directed Acyclic Graph:
    A → C
   ↙     ↘
  B       E
   ↘     ↙
    D → F

Topological Sort: A, B, C, D, E, F
```

## When to Use Topological Sort

- When you need to find an ordering of tasks based on their dependencies
- When dealing with course prerequisites or build dependencies
- When scheduling jobs or tasks with dependencies
- When determining the order of compilation for files in a build system
- When finding a valid sequence for operations that have dependencies

## Basic Topological Sort Implementation

### Python Implementation (Using DFS)

```python
from collections import defaultdict

def topological_sort(vertices, edges):
    # Build the graph
    graph = defaultdict(list)
    for parent, child in edges:
        graph[parent].append(child)
    
    # Initialize visited and result arrays
    visited = set()
    temp_visited = set()  # For cycle detection
    result = []
    
    def dfs(node):
        # If node is already in temp_visited, we have a cycle
        if node in temp_visited:
            return False
        
        # If node is already visited, skip it
        if node in visited:
            return True
        
        # Mark node as temporarily visited for cycle detection
        temp_visited.add(node)
        
        # Visit all neighbors
        for neighbor in graph[node]:
            if not dfs(neighbor):
                return False
        
        # Remove from temp_visited and add to visited
        temp_visited.remove(node)
        visited.add(node)
        
        # Add to result
        result.append(node)
        return True
    
    # Visit all vertices
    for vertex in range(vertices):
        if vertex not in visited:
            if not dfs(vertex):
                return []  # Return empty list if cycle detected
    
    # Reverse the result to get the topological order
    return result[::-1]

# Example usage
vertices = 6
edges = [(0, 2), (0, 1), (1, 3), (2, 4), (3, 5), (4, 5)]
print(topological_sort(vertices, edges))  # Output: [0, 1, 2, 3, 4, 5]
```

### Python Implementation (Using Kahn's Algorithm)

```python
from collections import defaultdict, deque

def topological_sort_kahn(vertices, edges):
    # Build the graph and in-degree count
    graph = defaultdict(list)
    in_degree = {i: 0 for i in range(vertices)}
    
    for parent, child in edges:
        graph[parent].append(child)
        in_degree[child] += 1
    
    # Initialize queue with all vertices that have 0 in-degree
    queue = deque([v for v in range(vertices) if in_degree[v] == 0])
    result = []
    
    # Process vertices in topological order
    while queue:
        vertex = queue.popleft()
        result.append(vertex)
        
        # Reduce in-degree of neighbors and add to queue if in-degree becomes 0
        for neighbor in graph[vertex]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    
    # If result doesn't include all vertices, there's a cycle
    if len(result) != vertices:
        return []
    
    return result

# Example usage
vertices = 6
edges = [(0, 2), (0, 1), (1, 3), (2, 4), (3, 5), (4, 5)]
print(topological_sort_kahn(vertices, edges))  # Output: [0, 1, 2, 3, 4, 5]
```

### JavaScript Implementation (Using DFS)

```javascript
function topologicalSort(vertices, edges) {
    // Build the graph
    const graph = Array(vertices).fill().map(() => []);
    for (const [parent, child] of edges) {
        graph[parent].push(child);
    }
    
    // Initialize visited and result arrays
    const visited = new Set();
    const tempVisited = new Set();  // For cycle detection
    const result = [];
    
    function dfs(node) {
        // If node is already in tempVisited, we have a cycle
        if (tempVisited.has(node)) {
            return false;
        }
        
        // If node is already visited, skip it
        if (visited.has(node)) {
            return true;
        }
        
        // Mark node as temporarily visited for cycle detection
        tempVisited.add(node);
        
        // Visit all neighbors
        for (const neighbor of graph[node]) {
            if (!dfs(neighbor)) {
                return false;
            }
        }
        
        // Remove from tempVisited and add to visited
        tempVisited.delete(node);
        visited.add(node);
        
        // Add to result
        result.push(node);
        return true;
    }
    
    // Visit all vertices
    for (let vertex = 0; vertex < vertices; vertex++) {
        if (!visited.has(vertex)) {
            if (!dfs(vertex)) {
                return [];  // Return empty array if cycle detected
            }
        }
    }
    
    // Reverse the result to get the topological order
    return result.reverse();
}

// Example usage
const vertices = 6;
const edges = [[0, 2], [0, 1], [1, 3], [2, 4], [3, 5], [4, 5]];
console.log(topologicalSort(vertices, edges));  // Output: [0, 1, 2, 3, 4, 5]
```

### JavaScript Implementation (Using Kahn's Algorithm)

```javascript
function topologicalSortKahn(vertices, edges) {
    // Build the graph and in-degree count
    const graph = Array(vertices).fill().map(() => []);
    const inDegree = Array(vertices).fill(0);
    
    for (const [parent, child] of edges) {
        graph[parent].push(child);
        inDegree[child]++;
    }
    
    // Initialize queue with all vertices that have 0 in-degree
    const queue = [];
    for (let v = 0; v < vertices; v++) {
        if (inDegree[v] === 0) {
            queue.push(v);
        }
    }
    
    const result = [];
    
    // Process vertices in topological order
    while (queue.length > 0) {
        const vertex = queue.shift();
        result.push(vertex);
        
        // Reduce in-degree of neighbors and add to queue if in-degree becomes 0
        for (const neighbor of graph[vertex]) {
            inDegree[neighbor]--;
            if (inDegree[neighbor] === 0) {
                queue.push(neighbor);
            }
        }
    }
    
    // If result doesn't include all vertices, there's a cycle
    if (result.length !== vertices) {
        return [];
    }
    
    return result;
}

// Example usage
const vertices = 6;
const edges = [[0, 2], [0, 1], [1, 3], [2, 4], [3, 5], [4, 5]];
console.log(topologicalSortKahn(vertices, edges));  // Output: [0, 1, 2, 3, 4, 5]
```

## Common Problems and Solutions

### 1. Course Schedule

**Problem:** There are a total of n courses you have to take, labeled from 0 to n-1. Some courses have prerequisites. Given the total number of courses and a list of prerequisite pairs, is it possible to finish all courses?

**Python Solution:**
```python
def can_finish(num_courses, prerequisites):
    # Build the graph
    graph = defaultdict(list)
    for course, prereq in prerequisites:
        graph[prereq].append(course)
    
    # Initialize visited sets
    visited = set()
    temp_visited = set()
    
    def has_cycle(course):
        # If course is in temp_visited, we have a cycle
        if course in temp_visited:
            return True
        
        # If course is already visited, no cycle from this course
        if course in visited:
            return False
        
        # Mark course as temporarily visited
        temp_visited.add(course)
        
        # Check all neighbors
        for next_course in graph[course]:
            if has_cycle(next_course):
                return True
        
        # Remove from temp_visited and add to visited
        temp_visited.remove(course)
        visited.add(course)
        
        return False
    
    # Check for cycles starting from each course
    for course in range(num_courses):
        if course not in visited:
            if has_cycle(course):
                return False  # Cycle detected, cannot finish all courses
    
    return True  # No cycles, can finish all courses

# Example usage
num_courses = 4
prerequisites = [[1, 0], [2, 1], [3, 2]]
print(can_finish(num_courses, prerequisites))  # Output: True

num_courses = 2
prerequisites = [[1, 0], [0, 1]]
print(can_finish(num_courses, prerequisites))  # Output: False
```

**JavaScript Solution:**
```javascript
function canFinish(numCourses, prerequisites) {
    // Build the graph
    const graph = Array(numCourses).fill().map(() => []);
    for (const [course, prereq] of prerequisites) {
        graph[prereq].push(course);
    }
    
    // Initialize visited sets
    const visited = new Set();
    const tempVisited = new Set();
    
    function hasCycle(course) {
        // If course is in tempVisited, we have a cycle
        if (tempVisited.has(course)) {
            return true;
        }
        
        // If course is already visited, no cycle from this course
        if (visited.has(course)) {
            return false;
        }
        
        // Mark course as temporarily visited
        tempVisited.add(course);
        
        // Check all neighbors
        for (const nextCourse of graph[course]) {
            if (hasCycle(nextCourse)) {
                return true;
            }
        }
        
        // Remove from tempVisited and add to visited
        tempVisited.delete(course);
        visited.add(course);
        
        return false;
    }
    
    // Check for cycles starting from each course
    for (let course = 0; course < numCourses; course++) {
        if (!visited.has(course)) {
            if (hasCycle(course)) {
                return false;  // Cycle detected, cannot finish all courses
            }
        }
    }
    
    return true;  // No cycles, can finish all courses
}

// Example usage
const numCourses = 4;
const prerequisites = [[1, 0], [2, 1], [3, 2]];
console.log(canFinish(numCourses, prerequisites));  // Output: true

const numCourses2 = 2;
const prerequisites2 = [[1, 0], [0, 1]];
console.log(canFinish(numCourses2, prerequisites2));  // Output: false
```

**Time Complexity:** O(V + E) where V is the number of vertices (courses) and E is the number of edges (prerequisites)  
**Space Complexity:** O(V + E)

### 2. Course Schedule II

**Problem:** Given the total number of courses and a list of prerequisite pairs, return the ordering of courses you should take to finish all courses.

**Python Solution:**
```python
def find_order(num_courses, prerequisites):
    # Build the graph
    graph = defaultdict(list)
    for course, prereq in prerequisites:
        graph[prereq].append(course)
    
    # Initialize visited sets and result
    visited = set()
    temp_visited = set()
    result = []
    
    def dfs(course):
        # If course is in temp_visited, we have a cycle
        if course in temp_visited:
            return False
        
        # If course is already visited, skip it
        if course in visited:
            return True
        
        # Mark course as temporarily visited
        temp_visited.add(course)
        
        # Visit all neighbors
        for next_course in graph[course]:
            if not dfs(next_course):
                return False
        
        # Remove from temp_visited and add to visited
        temp_visited.remove(course)
        visited.add(course)
        
        # Add to result
        result.append(course)
        return True
    
    # Visit all courses
    for course in range(num_courses):
        if course not in visited:
            if not dfs(course):
                return []  # Cycle detected, no valid ordering
    
    # Reverse the result to get the correct order
    return result[::-1]

# Example usage
num_courses = 4
prerequisites = [[1, 0], [2, 1], [3, 2]]
print(find_order(num_courses, prerequisites))  # Output: [0, 1, 2, 3]

num_courses = 2
prerequisites = [[1, 0], [0, 1]]
print(find_order(num_courses, prerequisites))  # Output: []
```

**JavaScript Solution:**
```javascript
function findOrder(numCourses, prerequisites) {
    // Build the graph
    const graph = Array(numCourses).fill().map(() => []);
    for (const [course, prereq] of prerequisites) {
        graph[prereq].push(course);
    }
    
    // Initialize visited sets and result
    const visited = new Set();
    const tempVisited = new Set();
    const result = [];
    
    function dfs(course) {
        // If course is in tempVisited, we have a cycle
        if (tempVisited.has(course)) {
            return false;
        }
        
        // If course is already visited, skip it
        if (visited.has(course)) {
            return true;
        }
        
        // Mark course as temporarily visited
        tempVisited.add(course);
        
        // Visit all neighbors
        for (const nextCourse of graph[course]) {
            if (!dfs(nextCourse)) {
                return false;
            }
        }
        
        // Remove from tempVisited and add to visited
        tempVisited.delete(course);
        visited.add(course);
        
        // Add to result
        result.push(course);
        return true;
    }
    
    // Visit all courses
    for (let course = 0; course < numCourses; course++) {
        if (!visited.has(course)) {
            if (!dfs(course)) {
                return [];  // Cycle detected, no valid ordering
            }
        }
    }
    
    // Reverse the result to get the correct order
    return result.reverse();
}

// Example usage
const numCourses = 4;
const prerequisites = [[1, 0], [2, 1], [3, 2]];
console.log(findOrder(numCourses, prerequisites));  // Output: [0, 1, 2, 3]

const numCourses2 = 2;
const prerequisites2 = [[1, 0], [0, 1]];
console.log(findOrder(numCourses2, prerequisites2));  // Output: []
```

**Time Complexity:** O(V + E)  
**Space Complexity:** O(V + E)

### 3. Alien Dictionary

**Problem:** There is a new alien language which uses the latin alphabet. The order of the letters is unknown. You receive a list of non-empty words from the dictionary. Find the order of letters in this language.

**Python Solution:**
```python
def alien_order(words):
    # Build the graph
    graph = {c: set() for word in words for c in word}
    
    # Add edges to the graph
    for i in range(len(words) - 1):
        word1, word2 = words[i], words[i + 1]
        min_len = min(len(word1), len(word2))
        
        # Check if word2 is a prefix of word1
        if len(word1) > len(word2) and word1[:min_len] == word2:
            return ""
        
        # Find the first different character
        for j in range(min_len):
            if word1[j] != word2[j]:
                graph[word1[j]].add(word2[j])
                break
    
    # Initialize visited sets and result
    visited = set()
    temp_visited = set()
    result = []
    
    def dfs(char):
        # If char is in temp_visited, we have a cycle
        if char in temp_visited:
            return False
        
        # If char is already visited, skip it
        if char in visited:
            return True
        
        # Mark char as temporarily visited
        temp_visited.add(char)
        
        # Visit all neighbors
        for next_char in graph[char]:
            if not dfs(next_char):
                return False
        
        # Remove from temp_visited and add to visited
        temp_visited.remove(char)
        visited.add(char)
        
        # Add to result
        result.append(char)
        return True
    
    # Visit all characters
    for char in graph:
        if char not in visited:
            if not dfs(char):
                return ""  # Cycle detected, no valid ordering
    
    # Reverse the result to get the correct order
    return "".join(result[::-1])

# Example usage
words = ["wrt", "wrf", "er", "ett", "rftt"]
print(alien_order(words))  # Output: "wertf"

words = ["z", "x"]
print(alien_order(words))  # Output: "zx"

words = ["z", "x", "z"]
print(alien_order(words))  # Output: ""
```

**JavaScript Solution:**
```javascript
function alienOrder(words) {
    // Build the graph
    const graph = {};
    for (const word of words) {
        for (const c of word) {
            if (!graph[c]) {
                graph[c] = new Set();
            }
        }
    }
    
    // Add edges to the graph
    for (let i = 0; i < words.length - 1; i++) {
        const word1 = words[i];
        const word2 = words[i + 1];
        const minLen = Math.min(word1.length, word2.length);
        
        // Check if word2 is a prefix of word1
        if (word1.length > word2.length && word1.substring(0, minLen) === word2) {
            return "";
        }
        
        // Find the first different character
        for (let j = 0; j < minLen; j++) {
            if (word1[j] !== word2[j]) {
                graph[word1[j]].add(word2[j]);
                break;
            }
        }
    }
    
    // Initialize visited sets and result
    const visited = new Set();
    const tempVisited = new Set();
    const result = [];
    
    function dfs(char) {
        // If char is in tempVisited, we have a cycle
        if (tempVisited.has(char)) {
            return false;
        }
        
        // If char is already visited, skip it
        if (visited.has(char)) {
            return true;
        }
        
        // Mark char as temporarily visited
        tempVisited.add(char);
        
        // Visit all neighbors
        for (const nextChar of graph[char]) {
            if (!dfs(nextChar)) {
                return false;
            }
        }
        
        // Remove from tempVisited and add to visited
        tempVisited.delete(char);
        visited.add(char);
        
        // Add to result
        result.push(char);
        return true;
    }
    
    // Visit all characters
    for (const char in graph) {
        if (!visited.has(char)) {
            if (!dfs(char)) {
                return "";  // Cycle detected, no valid ordering
            }
        }
    }
    
    // Reverse the result to get the correct order
    return result.reverse().join("");
}

// Example usage
const words = ["wrt", "wrf", "er", "ett", "rftt"];
console.log(alienOrder(words));  // Output: "wertf"

const words2 = ["z", "x"];
console.log(alienOrder(words2));  // Output: "zx"

const words3 = ["z", "x", "z"];
console.log(alienOrder(words3));  // Output: ""
```

**Time Complexity:** O(C) where C is the total length of all words combined  
**Space Complexity:** O(1) or O(U) where U is the number of unique letters

## Time and Space Complexity

| Algorithm | Time Complexity | Space Complexity |
|-----------|----------------|-----------------|
| Topological Sort (DFS) | O(V + E) | O(V + E) |
| Topological Sort (Kahn's) | O(V + E) | O(V + E) |

## Tips and Tricks for Topological Sort

1. **Cycle Detection**: Topological sort only works on directed acyclic graphs (DAGs). Always check for cycles.

2. **Two Approaches**: You can implement topological sort using either DFS or Kahn's algorithm (BFS with in-degree tracking).

3. **Temporary Visited Set**: When using DFS, use a temporary visited set to detect cycles.

4. **In-degree Tracking**: When using Kahn's algorithm, keep track of the in-degree of each vertex.

5. **Multiple Valid Orderings**: There can be multiple valid topological orderings for a given graph.

6. **Empty Result**: If the result doesn't include all vertices, there's a cycle in the graph.

7. **Preprocessing**: Build the graph and in-degree counts before starting the algorithm.

## Common Pitfalls

1. **Not Checking for Cycles**: Forgetting to check for cycles can lead to incorrect results.

2. **Incorrect Graph Representation**: Using the wrong direction for edges can lead to incorrect orderings.

3. **Not Handling Disconnected Components**: Make sure to visit all vertices, even if they're not connected.

4. **Confusing Edge Direction**: In topological sort, edges point from prerequisites to dependents.

5. **Not Reversing the Result**: When using DFS, remember to reverse the result to get the correct order.

## How to Identify Topological Sort Problems

Look for these clues in the problem statement:

1. The problem involves a directed graph or dependencies between elements.
2. The problem asks for an ordering of elements based on their dependencies.
3. Keywords like "prerequisites," "dependencies," "ordering," or "schedule."
4. The problem involves tasks that must be completed in a specific order.
5. The problem asks if a valid ordering exists.

## Common Topological Sort Problems from Blind 75 and Grind 75

1. **Course Schedule** (Medium): Determine if it's possible to finish all courses given prerequisites.
2. **Course Schedule II** (Medium): Return the ordering of courses to take to finish all courses.
3. **Alien Dictionary** (Hard): Find the order of characters in an alien language.
4. **Minimum Height Trees** (Medium): Find all the trees with minimum height in an undirected graph.
5. **Sequence Reconstruction** (Medium): Determine if a sequence can be uniquely reconstructed from its subsequences.
6. **Parallel Courses** (Medium): Find the minimum number of semesters needed to take all courses.

## Topological Sort Template

Here's a general template for solving topological sort problems:

```python
def topological_sort_template(graph):
    visited = set()
    temp_visited = set()
    result = []
    
    def dfs(node):
        if node in temp_visited:
            return False  # Cycle detected
        if node in visited:
            return True
        
        temp_visited.add(node)
        
        for neighbor in graph[node]:
            if not dfs(neighbor):
                return False
        
        temp_visited.remove(node)
        visited.add(node)
        result.append(node)
        return True
    
    for node in graph:
        if node not in visited:
            if not dfs(node):
                return []  # Cycle detected, no valid ordering
    
    return result[::-1]  # Reverse to get the correct order
```

## Real-World Applications

1. **Build Systems**: Determining the order in which to compile files based on their dependencies.
2. **Task Scheduling**: Scheduling tasks with dependencies in project management.
3. **Course Prerequisites**: Planning a sequence of courses to take based on prerequisites.
4. **Package Management**: Resolving dependencies in package managers like npm, pip, etc.
5. **Data Processing Pipelines**: Determining the order of operations in data processing workflows.
6. **Circuit Design**: Analyzing the signal flow in electronic circuits. 