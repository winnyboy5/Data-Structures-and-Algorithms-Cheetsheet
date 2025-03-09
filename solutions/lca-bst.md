# Lowest Common Ancestor of a Binary Search Tree

## Problem Statement

Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the definition of LCA on Wikipedia: "The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself)."

**Example 1:**
```
        6
      /   \
     2     8
    / \   / \
   0   4 7   9
      / \
     3   5

Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
Output: 6
Explanation: The LCA of nodes 2 and 8 is 6.
```

**Example 2:**
```
        6
      /   \
     2     8
    / \   / \
   0   4 7   9
      / \
     3   5

Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
Output: 2
Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
```

**Example 3:**
```
Input: root = [2,1], p = 2, q = 1
Output: 2
```

## Intuition

The key insight for this problem is to leverage the properties of a Binary Search Tree:
- All nodes in the left subtree of a node have values less than the node's value.
- All nodes in the right subtree of a node have values greater than the node's value.

Given these properties, we can determine the LCA by comparing the values of p and q with the current node:
- If both p and q are less than the current node, the LCA must be in the left subtree.
- If both p and q are greater than the current node, the LCA must be in the right subtree.
- If one is less and one is greater (or one is equal to the current node), then the current node is the LCA.

This approach is efficient because we can make a decision at each node without having to explore both subtrees.

## Visual Explanation

Let's visualize the solution with Example 1:

```
        6
      /   \
     2     8
    / \   / \
   0   4 7   9
      / \
     3   5

p = 2, q = 8

Start at the root (6):
- p.val (2) < root.val (6)
- q.val (8) > root.val (6)
- Since p and q are on different sides of the root, the root is the LCA.
- Return 6.
```

Now let's visualize Example 2:

```
        6
      /   \
     2     8
    / \   / \
   0   4 7   9
      / \
     3   5

p = 2, q = 4

Start at the root (6):
- p.val (2) < root.val (6)
- q.val (4) < root.val (6)
- Since both p and q are less than the root, the LCA must be in the left subtree.
- Move to the left child (2).

At node 2:
- p.val (2) == node.val (2)
- Since p is the current node, the current node is the LCA.
- Return 2.
```

![LCA BST Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Start from the root of the BST.
2. If both p and q are less than the current node, recursively search in the left subtree.
3. If both p and q are greater than the current node, recursively search in the right subtree.
4. If one is less and one is greater (or one is equal to the current node), then the current node is the LCA.

## Code Implementation

### Python

```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

def lowestCommonAncestor(root, p, q):
    # Base case: if root is None
    if not root:
        return None
    
    # If both p and q are less than root, LCA is in the left subtree
    if p.val < root.val and q.val < root.val:
        return lowestCommonAncestor(root.left, p, q)
    
    # If both p and q are greater than root, LCA is in the right subtree
    if p.val > root.val and q.val > root.val:
        return lowestCommonAncestor(root.right, p, q)
    
    # If one is less and one is greater (or one is equal to root),
    # then the current node is the LCA
    return root
```

### Python (Iterative)

```python
def lowestCommonAncestor(root, p, q):
    # Start from the root
    current = root
    
    while current:
        # If both p and q are less than current, go to the left subtree
        if p.val < current.val and q.val < current.val:
            current = current.left
        # If both p and q are greater than current, go to the right subtree
        elif p.val > current.val and q.val > current.val:
            current = current.right
        # If one is less and one is greater (or one is equal to current),
        # then the current node is the LCA
        else:
            return current
    
    return None
```

### JavaScript

```javascript
function TreeNode(val) {
    this.val = val;
    this.left = this.right = null;
}

function lowestCommonAncestor(root, p, q) {
    // Base case: if root is null
    if (!root) {
        return null;
    }
    
    // If both p and q are less than root, LCA is in the left subtree
    if (p.val < root.val && q.val < root.val) {
        return lowestCommonAncestor(root.left, p, q);
    }
    
    // If both p and q are greater than root, LCA is in the right subtree
    if (p.val > root.val && q.val > root.val) {
        return lowestCommonAncestor(root.right, p, q);
    }
    
    // If one is less and one is greater (or one is equal to root),
    // then the current node is the LCA
    return root;
}
```

### JavaScript (Iterative)

```javascript
function lowestCommonAncestor(root, p, q) {
    // Start from the root
    let current = root;
    
    while (current) {
        // If both p and q are less than current, go to the left subtree
        if (p.val < current.val && q.val < current.val) {
            current = current.left;
        }
        // If both p and q are greater than current, go to the right subtree
        else if (p.val > current.val && q.val > current.val) {
            current = current.right;
        }
        // If one is less and one is greater (or one is equal to current),
        // then the current node is the LCA
        else {
            return current;
        }
    }
    
    return null;
}
```

## Time and Space Complexity

### Recursive Approach:
- **Time Complexity**: O(h), where h is the height of the BST. In the worst case, we might need to traverse from the root to a leaf node, which takes O(h) time. For a balanced BST, h = log(n), where n is the number of nodes.
- **Space Complexity**: O(h) for the recursion stack. In the worst case, the recursion stack can grow to the height of the tree.

### Iterative Approach:
- **Time Complexity**: O(h), same as the recursive approach.
- **Space Complexity**: O(1), as we only use a constant amount of extra space regardless of the input size.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 1:

```
        6
      /   \
     2     8
    / \   / \
   0   4 7   9
      / \
     3   5

p = 2, q = 8

Start at the root (6):
- p.val (2) < root.val (6)? Yes
- q.val (8) < root.val (6)? No
- Since one is less and one is greater, the current node is the LCA.
- Return 6.
```

Now let's trace through Example 2:

```
        6
      /   \
     2     8
    / \   / \
   0   4 7   9
      / \
     3   5

p = 2, q = 4

Start at the root (6):
- p.val (2) < root.val (6)? Yes
- q.val (4) < root.val (6)? Yes
- Since both are less than the root, the LCA must be in the left subtree.
- Move to the left child (2).

At node 2:
- p.val (2) < node.val (2)? No (they are equal)
- q.val (4) < node.val (2)? No
- Since one is equal and one is greater, the current node is the LCA.
- Return 2.
```

## Edge Cases and Optimizations

- **Empty Tree**: If the root is null, we return null.
- **One Node Tree**: If the tree has only one node, and either p or q is that node, then that node is the LCA.
- **p or q is the Root**: If either p or q is the root, then the root is the LCA.
- **p and q are the Same Node**: If p and q are the same node, then that node is the LCA.

**Optimization**: The iterative approach is more space-efficient than the recursive approach, as it doesn't use the call stack.

## Common Mistakes

1. **Not leveraging BST properties**: A common mistake is to use a general binary tree LCA algorithm, which is less efficient for a BST.
2. **Incorrect comparison**: Make sure to compare the values of the nodes, not the nodes themselves.
3. **Not handling the case where p or q is the current node**: Remember that a node can be a descendant of itself according to the LCA definition.

## Related Problems

1. **Lowest Common Ancestor of a Binary Tree**: Find the LCA in a general binary tree (not necessarily a BST).
2. **Closest Binary Search Tree Value**: Find the value in a BST that is closest to a given target value.
3. **Kth Smallest Element in a BST**: Find the kth smallest element in a BST.
4. **Validate Binary Search Tree**: Determine if a binary tree is a valid BST.
5. **Convert Sorted Array to Binary Search Tree**: Convert a sorted array to a height-balanced BST. 