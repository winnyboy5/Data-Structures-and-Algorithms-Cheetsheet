# Kth Smallest Element in a BST

## Problem Statement

Given the root of a binary search tree, and an integer k, return the kth smallest value (1-indexed) of all the values of the nodes in the tree.

**Example 1:**
```
    3
   / \
  1   4
   \
    2

Input: root = [3,1,4,null,2], k = 1
Output: 1
```

**Example 2:**
```
        5
       / \
      3   6
     / \
    2   4
   /
  1

Input: root = [5,3,6,2,4,null,null,1], k = 3
Output: 3
```

## Intuition

The key insight for this problem is to leverage the property of a Binary Search Tree (BST): an inorder traversal of a BST visits the nodes in ascending order. Therefore, if we perform an inorder traversal and keep track of the count of nodes visited, the kth node visited will be the kth smallest element.

This approach is efficient because we don't need to traverse the entire tree or sort any values. We can stop the traversal as soon as we find the kth element.

## Visual Explanation

Let's visualize the solution with Example 2:

```
        5
       / \
      3   6
     / \
    2   4
   /
  1

Inorder traversal visits nodes in the following order: 1, 2, 3, 4, 5, 6

For k = 3, we want the 3rd element in this order, which is 3.

Step 1: Start inorder traversal from the root (5).
Step 2: Recursively traverse the left subtree first.
        - Go to node 3
        - Recursively traverse its left subtree
          - Go to node 2
          - Recursively traverse its left subtree
            - Go to node 1
            - No left child, so visit node 1 (count = 1)
            - No right child, so return
          - Visit node 2 (count = 2)
          - No right child, so return
        - Visit node 3 (count = 3)
        - We've found the kth element, so return 3
```

![Kth Smallest Element Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

### Recursive Approach

1. Perform an inorder traversal of the BST.
2. Keep track of the number of nodes visited.
3. When the count equals k, we've found the kth smallest element.
4. Return the value of that node.

### Iterative Approach

1. Use a stack to simulate the recursive inorder traversal.
2. Keep track of the number of nodes visited.
3. When the count equals k, we've found the kth smallest element.
4. Return the value of that node.

## Code Implementation

### Python (Recursive)

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def kthSmallest(root, k):
    # Initialize a list to store the result and a counter
    result = [0]  # Using a list to store the result so it can be modified in the recursive function
    count = [0]   # Using a list to store the count so it can be modified in the recursive function
    
    def inorder(node):
        if not node:
            return
        
        # Traverse left subtree
        inorder(node.left)
        
        # Visit current node
        count[0] += 1
        if count[0] == k:
            result[0] = node.val
            return
        
        # Traverse right subtree
        inorder(node.right)
    
    inorder(root)
    return result[0]
```

### Python (Iterative)

```python
def kthSmallest(root, k):
    stack = []
    count = 0
    
    # Iterative inorder traversal
    current = root
    while current or stack:
        # Traverse to the leftmost node
        while current:
            stack.append(current)
            current = current.left
        
        # Visit the current node
        current = stack.pop()
        count += 1
        
        # If we've found the kth element, return it
        if count == k:
            return current.val
        
        # Move to the right subtree
        current = current.right
    
    return -1  # This should not happen if k is valid
```

### JavaScript (Recursive)

```javascript
function kthSmallest(root, k) {
    // Initialize result and count
    let result = 0;
    let count = 0;
    
    // Helper function for inorder traversal
    function inorder(node) {
        if (!node) {
            return;
        }
        
        // Traverse left subtree
        inorder(node.left);
        
        // Visit current node
        count++;
        if (count === k) {
            result = node.val;
            return;
        }
        
        // Traverse right subtree
        inorder(node.right);
    }
    
    inorder(root);
    return result;
}
```

### JavaScript (Iterative)

```javascript
function kthSmallest(root, k) {
    const stack = [];
    let count = 0;
    
    // Iterative inorder traversal
    let current = root;
    while (current || stack.length > 0) {
        // Traverse to the leftmost node
        while (current) {
            stack.push(current);
            current = current.left;
        }
        
        // Visit the current node
        current = stack.pop();
        count++;
        
        // If we've found the kth element, return it
        if (count === k) {
            return current.val;
        }
        
        // Move to the right subtree
        current = current.right;
    }
    
    return -1;  // This should not happen if k is valid
}
```

## Time and Space Complexity

### Recursive Approach:
- **Time Complexity**: O(n), where n is the number of nodes in the tree. In the worst case, we might need to visit all nodes.
- **Space Complexity**: O(h), where h is the height of the tree. This is the space used by the recursion stack. In the worst case (a skewed tree), this could be O(n).

### Iterative Approach:
- **Time Complexity**: O(h + k), where h is the height of the tree and k is the input parameter. We need O(h) time to reach the leftmost node, and then O(k) time to find the kth smallest element.
- **Space Complexity**: O(h), where h is the height of the tree. This is the space used by the stack.

## Detailed Walkthrough with Example

Let's trace through the iterative algorithm with Example 1:

```
    3
   / \
  1   4
   \
    2

k = 1

Initialize:
- stack = []
- count = 0
- current = node 3

Iteration 1:
- Traverse to the leftmost node:
  - Push node 3 to stack: stack = [3]
  - current = node 1
  - Push node 1 to stack: stack = [3, 1]
  - current = null (no left child)
- Pop from stack: current = node 1, stack = [3]
- count = 1
- Since count = k = 1, return current.val = 1

Result: 1
```

Now let's trace through Example 2 with k = 3:

```
        5
       / \
      3   6
     / \
    2   4
   /
  1

k = 3

Initialize:
- stack = []
- count = 0
- current = node 5

Iteration 1:
- Traverse to the leftmost node:
  - Push node 5 to stack: stack = [5]
  - current = node 3
  - Push node 3 to stack: stack = [5, 3]
  - current = node 2
  - Push node 2 to stack: stack = [5, 3, 2]
  - current = node 1
  - Push node 1 to stack: stack = [5, 3, 2, 1]
  - current = null (no left child)
- Pop from stack: current = node 1, stack = [5, 3, 2]
- count = 1
- current = current.right = null

Iteration 2:
- Pop from stack: current = node 2, stack = [5, 3]
- count = 2
- current = current.right = null

Iteration 3:
- Pop from stack: current = node 3, stack = [5]
- count = 3
- Since count = k = 3, return current.val = 3

Result: 3
```

## Follow-up: What if the BST is modified often?

If the BST is modified often (i.e., we have many insert and delete operations) and we need to find the kth smallest frequently, we can augment the BST node to maintain the count of nodes in its left subtree. This way, we can find the kth smallest in O(h) time, where h is the height of the tree.

Here's how it would work:
1. Each node stores the count of nodes in its left subtree.
2. To find the kth smallest, we start from the root:
   - If k equals the left subtree size + 1, the root is the kth smallest.
   - If k is less than or equal to the left subtree size, the kth smallest is in the left subtree.
   - If k is greater than the left subtree size + 1, the kth smallest is in the right subtree, and we need to find the (k - leftSize - 1)th smallest in the right subtree.

## Edge Cases and Optimizations

- **Empty Tree**: If the tree is empty, return null or throw an exception.
- **k Out of Range**: If k is greater than the number of nodes in the tree, return null or throw an exception.
- **Single Node**: If the tree has only one node, return that node's value if k is 1, otherwise return null or throw an exception.

**Optimization**: For the iterative approach, we can stop the traversal as soon as we find the kth element, which can save time if k is small compared to the size of the tree.

## Common Mistakes

1. **Not understanding the property of BST**: Remember that an inorder traversal of a BST visits the nodes in ascending order.
2. **Off-by-one errors**: Be careful with the counting. The problem is 1-indexed, meaning the first element is the smallest.
3. **Not handling edge cases**: Make sure to handle empty trees and invalid k values.

## Related Problems

1. **Validate Binary Search Tree**: Determine if a binary tree is a valid BST.
2. **Inorder Successor in BST**: Find the inorder successor of a given node in a BST.
3. **Convert Sorted Array to Binary Search Tree**: Convert a sorted array to a height-balanced BST.
4. **Binary Search Tree Iterator**: Implement an iterator over a BST with O(1) space complexity. 