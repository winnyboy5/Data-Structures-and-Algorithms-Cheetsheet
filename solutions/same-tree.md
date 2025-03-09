# Same Tree

## Problem Statement

Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

**Example 1:**
```
    1         1
   / \       / \
  2   3     2   3

Input: p = [1,2,3], q = [1,2,3]
Output: true
```

**Example 2:**
```
    1         1
   /           \
  2             2

Input: p = [1,2], q = [1,null,2]
Output: false
```

**Example 3:**
```
    1         1
   / \       / \
  2   1     1   2

Input: p = [1,2,1], q = [1,1,2]
Output: false
```

## Intuition

To determine if two binary trees are the same, we need to check if they have the same structure and the same values at corresponding nodes. This problem can be solved using recursion, where we compare the current nodes of both trees and then recursively check their left and right subtrees.

Two trees are the same if:
1. Both are null (empty trees are identical)
2. Both have the same value at the current node
3. Their left subtrees are the same
4. Their right subtrees are the same

If any of these conditions fail, the trees are not the same.

## Visual Explanation

Let's visualize the solution with Example 1:

```
    1         1
   / \       / \
  2   3     2   3

Compare the root nodes: 1 == 1 ✓
Compare the left subtrees:
  - Compare the nodes: 2 == 2 ✓
  - Compare the left subtrees: null == null ✓
  - Compare the right subtrees: null == null ✓
Compare the right subtrees:
  - Compare the nodes: 3 == 3 ✓
  - Compare the left subtrees: null == null ✓
  - Compare the right subtrees: null == null ✓

Result: true
```

Let's also visualize Example 2:

```
    1         1
   /           \
  2             2

Compare the root nodes: 1 == 1 ✓
Compare the left subtrees:
  - p's left is 2, q's left is null ✗

Result: false
```

![Same Tree Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. If both trees are null, return true (empty trees are identical).
2. If one tree is null and the other is not, return false (trees with different structures are not identical).
3. If the values of the current nodes are different, return false.
4. Recursively check if the left subtrees are the same.
5. Recursively check if the right subtrees are the same.
6. Return true if both the left and right subtrees are the same.

## Code Implementation

### Python (Recursive)

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def isSameTree(p, q):
    # If both trees are empty, they are the same
    if not p and not q:
        return True
    
    # If one tree is empty and the other is not, they are not the same
    if not p or not q:
        return False
    
    # If the values of the current nodes are different, they are not the same
    if p.val != q.val:
        return False
    
    # Recursively check the left and right subtrees
    return isSameTree(p.left, q.left) and isSameTree(p.right, q.right)
```

### Python (Iterative - BFS)

```python
from collections import deque

def isSameTree(p, q):
    # Initialize a queue with the root nodes of both trees
    queue = deque([(p, q)])
    
    while queue:
        node1, node2 = queue.popleft()
        
        # If both nodes are None, continue to the next pair
        if not node1 and not node2:
            continue
        
        # If one node is None and the other is not, or if the values are different, return False
        if not node1 or not node2 or node1.val != node2.val:
            return False
        
        # Add the left and right children to the queue
        queue.append((node1.left, node2.left))
        queue.append((node1.right, node2.right))
    
    return True
```

### Python (Iterative - DFS)

```python
def isSameTree(p, q):
    # Initialize a stack with the root nodes of both trees
    stack = [(p, q)]
    
    while stack:
        node1, node2 = stack.pop()
        
        # If both nodes are None, continue to the next pair
        if not node1 and not node2:
            continue
        
        # If one node is None and the other is not, or if the values are different, return False
        if not node1 or not node2 or node1.val != node2.val:
            return False
        
        # Add the left and right children to the stack
        stack.append((node1.right, node2.right))
        stack.append((node1.left, node2.left))
    
    return True
```

### JavaScript (Recursive)

```javascript
function isSameTree(p, q) {
    // If both trees are empty, they are the same
    if (!p && !q) {
        return true;
    }
    
    // If one tree is empty and the other is not, they are not the same
    if (!p || !q) {
        return false;
    }
    
    // If the values of the current nodes are different, they are not the same
    if (p.val !== q.val) {
        return false;
    }
    
    // Recursively check the left and right subtrees
    return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
```

### JavaScript (Iterative - BFS)

```javascript
function isSameTree(p, q) {
    // Initialize a queue with the root nodes of both trees
    const queue = [[p, q]];
    
    while (queue.length > 0) {
        const [node1, node2] = queue.shift();
        
        // If both nodes are null, continue to the next pair
        if (!node1 && !node2) {
            continue;
        }
        
        // If one node is null and the other is not, or if the values are different, return false
        if (!node1 || !node2 || node1.val !== node2.val) {
            return false;
        }
        
        // Add the left and right children to the queue
        queue.push([node1.left, node2.left]);
        queue.push([node1.right, node2.right]);
    }
    
    return true;
}
```

### JavaScript (Iterative - DFS)

```javascript
function isSameTree(p, q) {
    // Initialize a stack with the root nodes of both trees
    const stack = [[p, q]];
    
    while (stack.length > 0) {
        const [node1, node2] = stack.pop();
        
        // If both nodes are null, continue to the next pair
        if (!node1 && !node2) {
            continue;
        }
        
        // If one node is null and the other is not, or if the values are different, return false
        if (!node1 || !node2 || node1.val !== node2.val) {
            return false;
        }
        
        // Add the left and right children to the stack
        stack.push([node1.right, node2.right]);
        stack.push([node1.left, node2.left]);
    }
    
    return true;
}
```

## Time and Space Complexity

### Recursive Approach:
- **Time Complexity**: O(n), where n is the number of nodes in the smaller tree. We visit each node exactly once.
- **Space Complexity**: O(h), where h is the height of the tree. This is the space used by the recursion stack. In the worst case (a skewed tree), this could be O(n).

### Iterative Approach (BFS/DFS):
- **Time Complexity**: O(n), where n is the number of nodes in the smaller tree. We visit each node exactly once.
- **Space Complexity**: O(w) for BFS, where w is the maximum width of the tree. In the worst case, this could be O(n/2) ≈ O(n) for a complete binary tree. For DFS, it's O(h), where h is the height of the tree.

## Detailed Walkthrough with Example

Let's trace through the recursive algorithm with Example 1:

```
    1         1
   / \       / \
  2   3     2   3

Call isSameTree(p, q):
- p and q are both not null, so we proceed
- p.val (1) == q.val (1), so we proceed
- Call isSameTree(p.left, q.left):
  - p.left (2) and q.left (2) are both not null, so we proceed
  - p.left.val (2) == q.left.val (2), so we proceed
  - Call isSameTree(p.left.left, q.left.left):
    - p.left.left (null) and q.left.left (null) are both null, so return true
  - Call isSameTree(p.left.right, q.left.right):
    - p.left.right (null) and q.left.right (null) are both null, so return true
  - Return true && true = true
- Call isSameTree(p.right, q.right):
  - p.right (3) and q.right (3) are both not null, so we proceed
  - p.right.val (3) == q.right.val (3), so we proceed
  - Call isSameTree(p.right.left, q.right.left):
    - p.right.left (null) and q.right.left (null) are both null, so return true
  - Call isSameTree(p.right.right, q.right.right):
    - p.right.right (null) and q.right.right (null) are both null, so return true
  - Return true && true = true
- Return true && true = true

Result: true
```

## Edge Cases and Optimizations

- **Empty Trees**: If both trees are empty, they are the same.
- **Different Structures**: If one tree is empty and the other is not, or if they have different structures, they are not the same.
- **Different Values**: If the values of corresponding nodes are different, the trees are not the same.
- **Early Termination**: We can return false as soon as we find a difference, which can save some iterations.

## Common Mistakes

1. **Not handling null nodes correctly**: Make sure to check if both nodes are null, or if one is null and the other is not.
2. **Not checking node values**: Make sure to compare the values of corresponding nodes.
3. **Not checking both subtrees**: Make sure to check both the left and right subtrees.

## Related Problems

1. **Symmetric Tree**: Check if a binary tree is a mirror of itself (symmetric around its center).
2. **Subtree of Another Tree**: Check if a binary tree is a subtree of another binary tree.
3. **Merge Two Binary Trees**: Merge two binary trees by adding the values of corresponding nodes.
4. **Find Duplicate Subtrees**: Find all duplicate subtrees in a binary tree. 