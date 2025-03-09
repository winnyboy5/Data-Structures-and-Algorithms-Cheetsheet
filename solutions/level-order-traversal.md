# Binary Tree Level Order Traversal

## Problem Statement

Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

**Example 1:**
```
    3
   / \
  9  20
    /  \
   15   7

Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]
```

**Example 2:**
```
Input: root = [1]
Output: [[1]]
```

**Example 3:**
```
Input: root = []
Output: []
```

## Intuition

Level order traversal requires us to visit all nodes at the current level before moving on to the next level. This is a perfect use case for a Breadth-First Search (BFS) approach using a queue data structure.

The key insight is that we can use a queue to keep track of nodes at the current level, and then process them one by one, adding their children to the queue for the next level. By keeping track of the number of nodes at each level, we can separate the values into their respective levels.

## Visual Explanation

Let's visualize the solution with Example 1:

```
Tree:
    3
   / \
  9  20
    /  \
   15   7

Step 1: Initialize a queue with the root node (3).
        Queue: [3]
        Result: []

Step 2: Process level 0.
        - Dequeue 3, add its value to the current level.
        - Enqueue its children (9, 20).
        Queue: [9, 20]
        Current level: [3]
        Result: [[3]]

Step 3: Process level 1.
        - Dequeue 9, add its value to the current level.
        - 9 has no children.
        Queue: [20]
        Current level: [9]
        
        - Dequeue 20, add its value to the current level.
        - Enqueue its children (15, 7).
        Queue: [15, 7]
        Current level: [9, 20]
        Result: [[3], [9, 20]]

Step 4: Process level 2.
        - Dequeue 15, add its value to the current level.
        - 15 has no children.
        Queue: [7]
        Current level: [15]
        
        - Dequeue 7, add its value to the current level.
        - 7 has no children.
        Queue: []
        Current level: [15, 7]
        Result: [[3], [9, 20], [15, 7]]

Step 5: Queue is empty, return the result.
        Result: [[3], [9, 20], [15, 7]]
```

![Level Order Traversal Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. If the root is null, return an empty list.
2. Initialize a queue with the root node.
3. Initialize an empty result list to store the level order traversal.
4. While the queue is not empty:
   - Get the number of nodes at the current level (size of the queue).
   - Initialize an empty list for the current level.
   - For each node at the current level:
     - Dequeue a node from the queue.
     - Add its value to the current level list.
     - Enqueue its left and right children if they exist.
   - Add the current level list to the result.
5. Return the result.

## Code Implementation

### Python

```python
from collections import deque

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def levelOrder(root):
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        current_level = []
        
        for _ in range(level_size):
            node = queue.popleft()
            current_level.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(current_level)
    
    return result
```

### JavaScript

```javascript
function TreeNode(val, left, right) {
    this.val = (val === undefined ? 0 : val);
    this.left = (left === undefined ? null : left);
    this.right = (right === undefined ? null : right);
}

function levelOrder(root) {
    if (!root) {
        return [];
    }
    
    const result = [];
    const queue = [root];
    
    while (queue.length > 0) {
        const levelSize = queue.length;
        const currentLevel = [];
        
        for (let i = 0; i < levelSize; i++) {
            const node = queue.shift();
            currentLevel.push(node.val);
            
            if (node.left) {
                queue.push(node.left);
            }
            if (node.right) {
                queue.push(node.right);
            }
        }
        
        result.push(currentLevel);
    }
    
    return result;
}
```

## Time and Space Complexity

- **Time Complexity**: O(n), where n is the number of nodes in the tree. We visit each node exactly once.
- **Space Complexity**: O(n), where n is the number of nodes in the tree. In the worst case, the queue might contain all nodes at the last level, which could be up to n/2 nodes for a complete binary tree.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 1:

```
Tree:
    3
   / \
  9  20
    /  \
   15   7

Initialize:
- queue = [3]
- result = []

Iteration 1 (Level 0):
- level_size = 1
- current_level = []
- Dequeue 3, add to current_level: [3]
- Enqueue left child (9) and right child (20): queue = [9, 20]
- Add current_level to result: result = [[3]]

Iteration 2 (Level 1):
- level_size = 2
- current_level = []
- Dequeue 9, add to current_level: [9]
- 9 has no children
- Dequeue 20, add to current_level: [9, 20]
- Enqueue left child (15) and right child (7): queue = [15, 7]
- Add current_level to result: result = [[3], [9, 20]]

Iteration 3 (Level 2):
- level_size = 2
- current_level = []
- Dequeue 15, add to current_level: [15]
- 15 has no children
- Dequeue 7, add to current_level: [15, 7]
- 7 has no children
- Add current_level to result: result = [[3], [9, 20], [15, 7]]

Queue is empty, return result: [[3], [9, 20], [15, 7]]
```

## Alternative Approaches

### Recursive Approach

We can also solve this problem using a recursive approach with a depth parameter:

```python
def levelOrder(root):
    result = []
    
    def dfs(node, depth):
        if not node:
            return
        
        # If this is the first node at this depth
        if len(result) == depth:
            result.append([])
        
        # Add the current node's value to the current depth's list
        result[depth].append(node.val)
        
        # Recursively process left and right children
        dfs(node.left, depth + 1)
        dfs(node.right, depth + 1)
    
    dfs(root, 0)
    return result
```

This approach has the same time and space complexity as the iterative approach but might be more intuitive for those comfortable with recursion.

## Edge Cases and Optimizations

- **Empty Tree**: If the root is null, we return an empty list.
- **Single Node Tree**: If the tree has only one node, we return a list containing a single list with the node's value.
- **Skewed Tree**: If the tree is skewed (all nodes have only left or only right children), the algorithm still works correctly.

**Optimization**: In the JavaScript implementation, using `shift()` on an array is an O(n) operation. For better performance, we could use a proper queue implementation or simulate a queue with two pointers.

## Common Mistakes

1. **Not tracking level sizes**: A common mistake is not keeping track of the number of nodes at each level, which can lead to mixing nodes from different levels.
2. **Using DFS instead of BFS**: Level order traversal requires BFS. Using DFS would give a different traversal order.
3. **Not handling null nodes**: Make sure to check if a node is null before trying to access its children.

## Related Problems

1. **Binary Tree Zigzag Level Order Traversal**: Similar to level order traversal, but alternate between traversing left-to-right and right-to-left.
2. **Average of Levels in Binary Tree**: Find the average value of nodes at each level.
3. **Binary Tree Right Side View**: Return the values of the nodes you can see from the right side of the tree.
4. **Minimum Depth of Binary Tree**: Find the minimum depth (shortest path from root to leaf) of a binary tree.
5. **Maximum Depth of Binary Tree**: Find the maximum depth (longest path from root to leaf) of a binary tree. 