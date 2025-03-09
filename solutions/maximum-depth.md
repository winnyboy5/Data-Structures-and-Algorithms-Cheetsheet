# Maximum Depth of Binary Tree

## Problem Statement

Given the root of a binary tree, return its maximum depth.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example 1:**
```
    3
   / \
  9  20
    /  \
   15   7

Input: root = [3,9,20,null,null,15,7]
Output: 3
```

**Example 2:**
```
    1
     \
      2

Input: root = [1,null,2]
Output: 2
```

**Example 3:**
```
Input: root = []
Output: 0
```

**Example 4:**
```
Input: root = [0]
Output: 1
```

## Intuition

The maximum depth of a binary tree is the number of nodes along the longest path from the root node down to the farthest leaf node. This problem can be solved using recursion, where the maximum depth of a tree is 1 (for the root) plus the maximum of the depths of its left and right subtrees.

For an empty tree, the depth is 0. For a tree with only a root node, the depth is 1.

## Visual Explanation

Let's visualize the solution with Example 1:

```
    3
   / \
  9  20
    /  \
   15   7

For the root node (3):
- Left subtree (9): Depth = 1 (just the node 9)
- Right subtree (20, 15, 7): Depth = 2 (node 20 plus either node 15 or node 7)
- Maximum depth = 1 + max(1, 2) = 3
```

Let's also visualize Example 2:

```
    1
     \
      2

For the root node (1):
- Left subtree: Depth = 0 (empty)
- Right subtree (2): Depth = 1 (just the node 2)
- Maximum depth = 1 + max(0, 1) = 2
```

![Maximum Depth Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. If the tree is empty (root is null), return 0.
2. Otherwise, recursively calculate:
   - The maximum depth of the left subtree
   - The maximum depth of the right subtree
3. Return 1 (for the root) plus the maximum of the two depths.

## Code Implementation

### Python (Recursive)

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def maxDepth(root):
    # Base case: empty tree
    if not root:
        return 0
    
    # Recursive case: 1 (for the root) + max depth of subtrees
    left_depth = maxDepth(root.left)
    right_depth = maxDepth(root.right)
    
    return 1 + max(left_depth, right_depth)
```

### Python (Iterative - BFS)

```python
from collections import deque

def maxDepth(root):
    # Base case: empty tree
    if not root:
        return 0
    
    # Use BFS to traverse the tree level by level
    queue = deque([(root, 1)])  # (node, depth)
    max_depth = 0
    
    while queue:
        node, depth = queue.popleft()
        max_depth = max(max_depth, depth)
        
        if node.left:
            queue.append((node.left, depth + 1))
        if node.right:
            queue.append((node.right, depth + 1))
    
    return max_depth
```

### Python (Iterative - DFS)

```python
def maxDepth(root):
    # Base case: empty tree
    if not root:
        return 0
    
    # Use DFS to traverse the tree
    stack = [(root, 1)]  # (node, depth)
    max_depth = 0
    
    while stack:
        node, depth = stack.pop()
        max_depth = max(max_depth, depth)
        
        if node.right:
            stack.append((node.right, depth + 1))
        if node.left:
            stack.append((node.left, depth + 1))
    
    return max_depth
```

### JavaScript (Recursive)

```javascript
function maxDepth(root) {
    // Base case: empty tree
    if (!root) {
        return 0;
    }
    
    // Recursive case: 1 (for the root) + max depth of subtrees
    const leftDepth = maxDepth(root.left);
    const rightDepth = maxDepth(root.right);
    
    return 1 + Math.max(leftDepth, rightDepth);
}
```

### JavaScript (Iterative - BFS)

```javascript
function maxDepth(root) {
    // Base case: empty tree
    if (!root) {
        return 0;
    }
    
    // Use BFS to traverse the tree level by level
    const queue = [[root, 1]];  // [node, depth]
    let maxDepth = 0;
    
    while (queue.length > 0) {
        const [node, depth] = queue.shift();
        maxDepth = Math.max(maxDepth, depth);
        
        if (node.left) {
            queue.push([node.left, depth + 1]);
        }
        if (node.right) {
            queue.push([node.right, depth + 1]);
        }
    }
    
    return maxDepth;
}
```

### JavaScript (Iterative - DFS)

```javascript
function maxDepth(root) {
    // Base case: empty tree
    if (!root) {
        return 0;
    }
    
    // Use DFS to traverse the tree
    const stack = [[root, 1]];  // [node, depth]
    let maxDepth = 0;
    
    while (stack.length > 0) {
        const [node, depth] = stack.pop();
        maxDepth = Math.max(maxDepth, depth);
        
        if (node.right) {
            stack.push([node.right, depth + 1]);
        }
        if (node.left) {
            stack.push([node.left, depth + 1]);
        }
    }
    
    return maxDepth;
}
```

## Time and Space Complexity

### Recursive Approach:
- **Time Complexity**: O(n), where n is the number of nodes in the tree. We visit each node exactly once.
- **Space Complexity**: O(h), where h is the height of the tree. This is the space used by the recursion stack. In the worst case (a skewed tree), this could be O(n).

### Iterative Approach (BFS/DFS):
- **Time Complexity**: O(n), where n is the number of nodes in the tree. We visit each node exactly once.
- **Space Complexity**: O(w) for BFS, where w is the maximum width of the tree. In the worst case, this could be O(n/2) ≈ O(n) for a complete binary tree. For DFS, it's O(h), where h is the height of the tree.

## Detailed Walkthrough with Example

Let's trace through the recursive algorithm with Example 1:

```
    3
   / \
  9  20
    /  \
   15   7

Call maxDepth(3):
- root is not null, so we proceed
- Call maxDepth(9) for the left subtree:
  - root is not null, so we proceed
  - Call maxDepth(null) for the left subtree of 9:
    - root is null, so return 0
  - Call maxDepth(null) for the right subtree of 9:
    - root is null, so return 0
  - Return 1 + max(0, 0) = 1
- Call maxDepth(20) for the right subtree:
  - root is not null, so we proceed
  - Call maxDepth(15) for the left subtree of 20:
    - root is not null, so we proceed
    - Call maxDepth(null) for the left subtree of 15:
      - root is null, so return 0
    - Call maxDepth(null) for the right subtree of 15:
      - root is null, so return 0
    - Return 1 + max(0, 0) = 1
  - Call maxDepth(7) for the right subtree of 20:
    - root is not null, so we proceed
    - Call maxDepth(null) for the left subtree of 7:
      - root is null, so return 0
    - Call maxDepth(null) for the right subtree of 7:
      - root is null, so return 0
    - Return 1 + max(0, 0) = 1
  - Return 1 + max(1, 1) = 2
- Return 1 + max(1, 2) = 3

Result: 3
```

## Edge Cases and Optimizations

- **Empty Tree**: If the tree is empty (root is null), return 0.
- **Single Node**: If the tree has only one node, return 1.
- **Skewed Tree**: If the tree is skewed (all nodes have only one child), the recursive approach might lead to a stack overflow for very deep trees. In such cases, the iterative approach is preferred.

## Common Mistakes

1. **Not handling the base case**: Make sure to handle the case where the tree is empty (root is null).
2. **Confusing depth with height**: The depth of a node is the number of edges from the root to the node. The height of a node is the number of edges on the longest path from the node to a leaf. In this problem, we're calculating the height of the root node, which is the maximum depth of the tree.
3. **Not considering both subtrees**: Make sure to calculate the maximum depth of both the left and right subtrees and take the maximum.

## Related Problems

1. **Minimum Depth of Binary Tree**: Find the minimum number of nodes along the shortest path from the root node down to the nearest leaf node.
2. **Balanced Binary Tree**: Check if a binary tree is balanced (the depth of the two subtrees of every node never differs by more than 1).
3. **Diameter of Binary Tree**: Find the length of the longest path between any two nodes in a tree.
4. **Binary Tree Level Order Traversal**: Traverse a binary tree level by level and return the result as a list of lists. 