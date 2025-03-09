# Course Schedule

## Problem Statement

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you must take course `bi` first if you want to take course `ai`.

For example, the pair `[0, 1]` indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

**Example 1:**
```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.
```

**Example 2:**
```
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
```

## Intuition

This problem is essentially asking if there is a cycle in a directed graph. If there is a cycle, it means there's a circular dependency between courses, making it impossible to complete all courses. If there is no cycle, we can find a valid order to take all courses.

We can use either Depth-First Search (DFS) or Breadth-First Search (BFS) to detect cycles in the graph. In this solution, we'll explore both approaches.

## Visual Explanation

Let's visualize the solution with Example 1:

```
numCourses = 2, prerequisites = [[1,0]]

Step 1: Build the adjacency list representation of the graph
Graph:
0 -> []
1 -> [0]

Step 2: Perform DFS to detect cycles
- Start DFS from course 0:
  - Mark 0 as being visited
  - No outgoing edges from 0, so no cycles detected
  - Mark 0 as visited
- Start DFS from course 1:
  - Mark 1 as being visited
  - Visit neighbor 0:
    - 0 is already visited, so no cycles detected
  - Mark 1 as visited

No cycles detected, so return true.
```

Now let's visualize Example 2:

```
numCourses = 2, prerequisites = [[1,0],[0,1]]

Step 1: Build the adjacency list representation of the graph
Graph:
0 -> [1]
1 -> [0]

Step 2: Perform DFS to detect cycles
- Start DFS from course 0:
  - Mark 0 as being visited
  - Visit neighbor 1:
    - Mark 1 as being visited
    - Visit neighbor 0:
      - 0 is already being visited, so a cycle is detected
      - Return false

A cycle is detected, so return false.
```

![Course Schedule Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

### DFS Approach

1. Build an adjacency list representation of the graph from the prerequisites.
2. Create two arrays to track the state of each course:
   - `visited`: Courses that have been fully processed (all dependencies checked).
   - `path`: Courses that are currently being processed (in the current DFS path).
3. Perform DFS on each unvisited course:
   - If a course is already in the current path, a cycle is detected, return false.
   - If a course is already visited, skip it.
   - Mark the course as being in the current path.
   - Recursively visit all its prerequisites.
   - Mark the course as visited and remove it from the current path.
4. If no cycles are detected after checking all courses, return true.

### BFS Approach (Topological Sort)

1. Build an adjacency list representation of the graph from the prerequisites.
2. Calculate the in-degree (number of prerequisites) for each course.
3. Initialize a queue with all courses that have no prerequisites (in-degree = 0).
4. While the queue is not empty:
   - Dequeue a course and add it to the result.
   - Reduce the in-degree of all courses that depend on the current course.
   - If any course's in-degree becomes 0, enqueue it.
5. If the number of courses in the result equals `numCourses`, return true; otherwise, return false.

## Code Implementation

### Python (DFS)

```python
def canFinish(numCourses, prerequisites):
    # Build adjacency list
    graph = [[] for _ in range(numCourses)]
    for course, prereq in prerequisites:
        graph[course].append(prereq)
    
    # 0: unvisited, 1: visiting, 2: visited
    visited = [0] * numCourses
    
    def dfs(course):
        # If the course is being visited, we found a cycle
        if visited[course] == 1:
            return False
        # If the course has been visited, no need to check again
        if visited[course] == 2:
            return True
        
        # Mark the course as being visited
        visited[course] = 1
        
        # Visit all prerequisites
        for prereq in graph[course]:
            if not dfs(prereq):
                return False
        
        # Mark the course as visited
        visited[course] = 2
        return True
    
    # Check each course
    for course in range(numCourses):
        if not dfs(course):
            return False
    
    return True
```

### JavaScript (DFS)

```javascript
function canFinish(numCourses, prerequisites) {
    // Build adjacency list
    const graph = Array(numCourses).fill().map(() => []);
    for (const [course, prereq] of prerequisites) {
        graph[course].push(prereq);
    }
    
    // 0: unvisited, 1: visiting, 2: visited
    const visited = Array(numCourses).fill(0);
    
    function dfs(course) {
        // If the course is being visited, we found a cycle
        if (visited[course] === 1) {
            return false;
        }
        // If the course has been visited, no need to check again
        if (visited[course] === 2) {
            return true;
        }
        
        // Mark the course as being visited
        visited[course] = 1;
        
        // Visit all prerequisites
        for (const prereq of graph[course]) {
            if (!dfs(prereq)) {
                return false;
            }
        }
        
        // Mark the course as visited
        visited[course] = 2;
        return true;
    }
    
    // Check each course
    for (let course = 0; course < numCourses; course++) {
        if (!dfs(course)) {
            return false;
        }
    }
    
    return true;
}
```

### Python (BFS - Topological Sort)

```python
from collections import deque

def canFinish(numCourses, prerequisites):
    # Build adjacency list and calculate in-degrees
    graph = [[] for _ in range(numCourses)]
    in_degree = [0] * numCourses
    
    for course, prereq in prerequisites:
        graph[prereq].append(course)
        in_degree[course] += 1
    
    # Initialize queue with courses that have no prerequisites
    queue = deque([course for course in range(numCourses) if in_degree[course] == 0])
    count = 0
    
    # Process courses in topological order
    while queue:
        current = queue.popleft()
        count += 1
        
        # Reduce in-degree of all courses that depend on the current course
        for next_course in graph[current]:
            in_degree[next_course] -= 1
            if in_degree[next_course] == 0:
                queue.append(next_course)
    
    # If we processed all courses, return true; otherwise, return false
    return count == numCourses
```

### JavaScript (BFS - Topological Sort)

```javascript
function canFinish(numCourses, prerequisites) {
    // Build adjacency list and calculate in-degrees
    const graph = Array(numCourses).fill().map(() => []);
    const inDegree = Array(numCourses).fill(0);
    
    for (const [course, prereq] of prerequisites) {
        graph[prereq].push(course);
        inDegree[course]++;
    }
    
    // Initialize queue with courses that have no prerequisites
    const queue = [];
    for (let course = 0; course < numCourses; course++) {
        if (inDegree[course] === 0) {
            queue.push(course);
        }
    }
    
    let count = 0;
    
    // Process courses in topological order
    while (queue.length > 0) {
        const current = queue.shift();
        count++;
        
        // Reduce in-degree of all courses that depend on the current course
        for (const nextCourse of graph[current]) {
            inDegree[nextCourse]--;
            if (inDegree[nextCourse] === 0) {
                queue.push(nextCourse);
            }
        }
    }
    
    // If we processed all courses, return true; otherwise, return false
    return count === numCourses;
}
```

## Time and Space Complexity

### DFS Approach:
- **Time Complexity**: O(V + E), where V is the number of vertices (courses) and E is the number of edges (prerequisites). We need to visit each vertex and each edge once.
- **Space Complexity**: O(V + E) for the adjacency list, the visited array, and the recursion stack.

### BFS Approach:
- **Time Complexity**: O(V + E), where V is the number of vertices (courses) and E is the number of edges (prerequisites). We need to visit each vertex and each edge once.
- **Space Complexity**: O(V + E) for the adjacency list, the in-degree array, and the queue.

## Detailed Walkthrough with Example

Let's trace through the DFS algorithm with Example 2:

```
numCourses = 2, prerequisites = [[1,0],[0,1]]

Step 1: Build the adjacency list
graph = [
  [1],  // Course 0 depends on course 1
  [0]   // Course 1 depends on course 0
]

Step 2: Initialize visited array
visited = [0, 0]

Step 3: Start DFS from course 0
- Call dfs(0):
  - visited[0] = 1 (mark as being visited)
  - Visit prerequisites of course 0:
    - Call dfs(1):
      - visited[1] = 1 (mark as being visited)
      - Visit prerequisites of course 1:
        - Call dfs(0):
          - visited[0] = 1 (already being visited)
          - Cycle detected, return false
      - Return false
  - Return false
- Return false

Since a cycle is detected, we return false.
```

## Edge Cases and Optimizations

- **No Prerequisites**: If there are no prerequisites, all courses can be taken in any order, so return true.
- **Single Course**: If there's only one course, it can always be taken, so return true.
- **Self-Loop**: If a course depends on itself (e.g., [0,0]), it creates a cycle, so return false.
- **Optimization**: We can use a single array to track the state of each course instead of two separate arrays.

## Common Mistakes

1. **Incorrect graph representation**: Make sure to build the graph correctly. In this problem, if [a, b] means course a depends on course b, then there should be an edge from a to b.
2. **Not handling all courses**: Make sure to check all courses, not just the ones mentioned in the prerequisites.
3. **Confusing DFS states**: Be careful with the states in the DFS approach. A course can be unvisited, being visited (in the current path), or visited (fully processed).

## Related Problems

1. **Course Schedule II**: Return the ordering of courses you should take to finish all courses.
2. **Alien Dictionary**: Determine the order of characters in an alien language.
3. **Graph Valid Tree**: Check if an undirected graph is a valid tree.
4. **Minimum Height Trees**: Find all the MHTs (minimum height trees) in a graph. 