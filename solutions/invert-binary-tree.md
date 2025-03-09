# Invert Binary Tree

## Problem Statement

Given the root of a binary tree, invert the tree, and return its root.

To invert a binary tree, we swap the left and right children of every node in the tree.

**Example 1:**
```
Input:
     4
   /   \
  2     7
 / \   / \
1   3 6   9

Output:
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

**Example 2:**
```
Input:
     2
   /   \
  1     3

Output:
     2
   /   \
  3     1
```

**Example 3:**
```
Input: []
Output: []
```

## Intuition

The key insight for this problem is that inverting a binary tree is a recursive operation:

1. For each node, swap its left and right children.
2. Recursively invert the left subtree.
3. Recursively invert the right subtree.

This approach works because the inversion of a binary tree is defined as swapping the left and right children of every node, which naturally lends itself to a recursive solution.

## Visual Explanation

Let's visualize the recursive inversion process with Example 1:

```
Original Tree:
     4
   /   \
  2     7
 / \   / \
1   3 6   9

Step 1: Swap the children of the root node (4)
     4
   /   \
  7     2
 / \   / \
6   9 1   3

Step 2: Recursively invert the left subtree (rooted at 7)
     4
   /   \
  7     2
 / \   / \
9   6 1   3

Step 3: Recursively invert the right subtree (rooted at 2)
     4
   /   \
  7     2
 / \   / \
9   6 3   1

Result: Inverted Tree
```

![Invert Binary Tree Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

### Recursive Approach:

1. If the root is null, return null (base case).
2. Swap the left and right children of the current node.
3. Recursively invert the left subtree.
4. Recursively invert the right subtree.
5. Return the root.

### Iterative Approach (using a queue):

1. If the root is null, return null.
2. Create a queue and enqueue the root.
3. While the queue is not empty:
   - Dequeue a node.
   - Swap its left and right children.
   - Enqueue the left child if it exists.
   - Enqueue the right child if it exists.
4. Return the root.

## Code Implementation

### Python (Recursive)

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def invertTree(root):
    # Base case: if the node is None, return None
    if not root:
        return None
    
    # Swap the left and right children
    root.left, root.right = root.right, root.left
    
    # Recursively invert the left and right subtrees
    invertTree(root.left)
    invertTree(root.right)
    
    return root
```

### Python (Iterative)

```python
from collections import deque

def invertTree(root):
    if not root:
        return None
    
    # Create a queue and enqueue the root
    queue = deque([root])
    
    while queue:
        # Dequeue a node
        node = queue.popleft()
        
        # Swap its left and right children
        node.left, node.right = node.right, node.left
        
        # Enqueue the left child if it exists
        if node.left:
            queue.append(node.left)
        
        # Enqueue the right child if it exists
        if node.right:
            queue.append(node.right)
    
    return root
```

### JavaScript (Recursive)

```javascript
function invertTree(root) {
    // Base case: if the node is null, return null
    if (!root) {
        return null;
    }
    
    // Swap the left and right children
    const temp = root.left;
    root.left = root.right;
    root.right = temp;
    
    // Recursively invert the left and right subtrees
    invertTree(root.left);
    invertTree(root.right);
    
    return root;
}
```

### JavaScript (Iterative)

```javascript
function invertTree(root) {
    if (!root) {
        return null;
    }
    
    // Create a queue and enqueue the root
    const queue = [root];
    
    while (queue.length > 0) {
        // Dequeue a node
        const node = queue.shift();
        
        // Swap its left and right children
        const temp = node.left;
        node.left = node.right;
        node.right = temp;
        
        // Enqueue the left child if it exists
        if (node.left) {
            queue.push(node.left);
        }
        
        // Enqueue the right child if it exists
        if (node.right) {
            queue.push(node.right);
        }
    }
    
    return root;
}
```

## Time and Space Complexity

### Recursive Approach:
- **Time Complexity**: O(n), where n is the number of nodes in the tree. We visit each node exactly once.
- **Space Complexity**: O(h), where h is the height of the tree. This is due to the recursion stack. In the worst case (skewed tree), this could be O(n).

### Iterative Approach:
- **Time Complexity**: O(n), where n is the number of nodes in the tree. We visit each node exactly once.
- **Space Complexity**: O(w), where w is the maximum width of the tree. In the worst case, this could be O(n/2) = O(n) for a complete binary tree.

## Detailed Walkthrough with Example

Let's trace through the recursive algorithm with Example 2:

```
Original Tree:
     2
   /   \
  1     3

Call invertTree(2):
  - Swap children: 2.left = 3, 2.right = 1
  - Call invertTree(3) (left child):
    - 3 has no children, so return 3
  - Call invertTree(1) (right child):
    - 1 has no children, so return 1
  - Return 2

Result:
     2
   /   \
  3     1
```

## Edge Cases and Optimizations

- **Empty Tree**: If the tree is empty (root is null), the function will return null.
- **Single Node**: If the tree has only one node (no children), the function will return the node as is.
- **Balanced vs. Skewed Trees**: The algorithm works the same for both balanced and skewed trees, but the space complexity of the recursive approach will be better for balanced trees (O(log n)) than for skewed trees (O(n)).

## Common Mistakes

1. **Not handling the base case**: Always check if the root is null before proceeding.
2. **Incorrect swapping**: Make sure to properly swap the left and right children. In languages without tuple unpacking (like JavaScript), you need to use a temporary variable.
3. **Not returning the root**: Remember to return the root of the inverted tree.

## Related Problems

1. **Symmetric Tree**: Check if a binary tree is a mirror of itself.
2. **Same Tree**: Check if two binary trees are the same.
3. **Subtree of Another Tree**: Check if a binary tree is a subtree of another binary tree.
4. **Flatten Binary Tree to Linked List**: Flatten a binary tree to a linked list in-place. 