# Validate Binary Search Tree

## Problem Statement

Given the root of a binary tree, determine if it is a valid binary search tree (BST).

A valid BST is defined as follows:
- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than the node's key.
- Both the left and right subtrees must also be binary search trees.

**Example 1:**
```
    2
   / \
  1   3

Input: root = [2,1,3]
Output: true
```

**Example 2:**
```
    5
   / \
  1   4
     / \
    3   6

Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

## Intuition

The key insight for this problem is to understand that a BST has a specific property: for any node, all values in its left subtree must be less than the node's value, and all values in its right subtree must be greater than the node's value.

This means that each node has a valid range of values it can contain, and this range gets narrower as we traverse down the tree. For example, if a node has a value of 10, then all nodes in its left subtree must have values less than 10, and all nodes in its right subtree must have values greater than 10.

We can use a recursive approach to validate this property for each node in the tree, updating the valid range as we traverse down.

## Visual Explanation

Let's visualize the solution with Example 1:

```
    2
   / \
  1   3

For the root node (2):
- Valid range: (-∞, +∞)
- 2 is within the range, so we continue

For the left child (1):
- Valid range: (-∞, 2)
- 1 is within the range, so we continue

For the right child (3):
- Valid range: (2, +∞)
- 3 is within the range, so we continue

All nodes are within their valid ranges, so the tree is a valid BST.
```

Now let's visualize Example 2:

```
    5
   / \
  1   4
     / \
    3   6

For the root node (5):
- Valid range: (-∞, +∞)
- 5 is within the range, so we continue

For the left child (1):
- Valid range: (-∞, 5)
- 1 is within the range, so we continue

For the right child (4):
- Valid range: (5, +∞)
- 4 is NOT within the range (4 < 5), so the tree is not a valid BST.
```

![Validate BST Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Use a recursive helper function that takes a node, a minimum value, and a maximum value.
2. For each node, check if its value is within the valid range (min < node.val < max).
3. If the node's value is out of range, return false.
4. Recursively check the left subtree with an updated maximum value (the current node's value).
5. Recursively check the right subtree with an updated minimum value (the current node's value).
6. If both subtrees are valid BSTs and the current node's value is within range, return true.

## Code Implementation

### Python

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def isValidBST(root):
    def validate(node, min_val=float('-inf'), max_val=float('inf')):
        # Empty trees are valid BSTs
        if not node:
            return True
        
        # Check if the current node's value is within the valid range
        if node.val <= min_val or node.val >= max_val:
            return False
        
        # Recursively check left and right subtrees
        # For left subtree, the max value is the current node's value
        # For right subtree, the min value is the current node's value
        return (validate(node.left, min_val, node.val) and 
                validate(node.right, node.val, max_val))
    
    return validate(root)
```

### JavaScript

```javascript
function isValidBST(root) {
    function validate(node, minVal = -Infinity, maxVal = Infinity) {
        // Empty trees are valid BSTs
        if (!node) {
            return true;
        }
        
        // Check if the current node's value is within the valid range
        if (node.val <= minVal || node.val >= maxVal) {
            return false;
        }
        
        // Recursively check left and right subtrees
        // For left subtree, the max value is the current node's value
        // For right subtree, the min value is the current node's value
        return validate(node.left, minVal, node.val) && 
               validate(node.right, node.val, maxVal);
    }
    
    return validate(root);
}
```

## Alternative Approach: Inorder Traversal

Another approach is to use inorder traversal. In a BST, inorder traversal should yield values in ascending order. We can perform an inorder traversal and check if the values are strictly increasing.

### Python (Inorder Traversal)

```python
def isValidBST(root):
    prev = float('-inf')
    
    def inorder(node):
        nonlocal prev
        if not node:
            return True
        
        # Check left subtree
        if not inorder(node.left):
            return False
        
        # Check current node
        if node.val <= prev:
            return False
        prev = node.val
        
        # Check right subtree
        return inorder(node.right)
    
    return inorder(root)
```

### JavaScript (Inorder Traversal)

```javascript
function isValidBST(root) {
    let prev = -Infinity;
    
    function inorder(node) {
        if (!node) {
            return true;
        }
        
        // Check left subtree
        if (!inorder(node.left)) {
            return false;
        }
        
        // Check current node
        if (node.val <= prev) {
            return false;
        }
        prev = node.val;
        
        // Check right subtree
        return inorder(node.right);
    }
    
    return inorder(root);
}
```

## Time and Space Complexity

- **Time Complexity**: O(n), where n is the number of nodes in the tree. We visit each node exactly once.
- **Space Complexity**: O(h), where h is the height of the tree. This is the space used by the recursion stack. In the worst case (a skewed tree), this could be O(n).

## Detailed Walkthrough with Example

Let's trace through the recursive approach with Example 2:

```
    5
   / \
  1   4
     / \
    3   6

Call validate(5, -∞, +∞):
- 5 is within range (-∞, +∞), so continue
- Recursively call validate(1, -∞, 5) for the left subtree:
  - 1 is within range (-∞, 5), so continue
  - Recursively call validate(null, -∞, 1) for the left subtree:
    - Empty tree is valid, return true
  - Recursively call validate(null, 1, 5) for the right subtree:
    - Empty tree is valid, return true
  - Both subtrees are valid, return true
- Recursively call validate(4, 5, +∞) for the right subtree:
  - 4 is NOT within range (5, +∞) because 4 < 5
  - Return false
- Left subtree is valid, but right subtree is not, so return false

Result: false
```

## Edge Cases and Optimizations

- **Empty Tree**: An empty tree is considered a valid BST.
- **Single Node**: A tree with a single node is always a valid BST.
- **Duplicate Values**: According to the problem statement, a BST should not contain duplicate values. If node.val == min_val or node.val == max_val, we should return false.
- **Integer Overflow**: Be careful with using Integer.MIN_VALUE and Integer.MAX_VALUE as initial bounds in languages with fixed-size integers. It's safer to use language-specific infinity values or null to represent unbounded ranges.

## Common Mistakes

1. **Not handling the range correctly**: Remember that for a BST, all values in the left subtree must be strictly less than the node's value, and all values in the right subtree must be strictly greater than the node's value.
2. **Using only the parent-child relationship**: It's not enough to check that a node's value is greater than its left child and less than its right child. You need to ensure that all nodes in the left subtree are less than the current node, and all nodes in the right subtree are greater than the current node.
3. **Not considering the entire path from root to leaf**: The valid range for a node depends on all its ancestors, not just its parent.

## Related Problems

1. **Kth Smallest Element in a BST**: Find the kth smallest element in a BST.
2. **Convert Sorted Array to Binary Search Tree**: Convert a sorted array to a height-balanced BST.
3. **Recover Binary Search Tree**: Restore a BST where two nodes have been swapped.
4. **Binary Search Tree Iterator**: Implement an iterator over a BST with O(1) space complexity. 