# Binary Tree Maximum Path Sum

## Problem Statement

Given the root of a binary tree, return the maximum path sum of any non-empty path.

A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. A path need not pass through the root.

The path sum is the sum of the values of the nodes in the path.

**Example 1:**
```
    1
   / \
  2   3

Input: root = [1,2,3]
Output: 6
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.
```

**Example 2:**
```
    -10
    / \
   9  20
     /  \
    15   7

Input: root = [-10,9,20,null,null,15,7]
Output: 42
Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.
```

## Intuition

This problem is challenging because a path can take various shapes in a binary tree. It doesn't have to start or end at the root, and it doesn't have to follow a straight line down the tree. A path can go up from a child to its parent and then down to another child.

The key insight is to use a recursive approach where, for each node, we calculate:
1. The maximum path sum that includes the node and one of its subtrees (or none of them).
2. The maximum path sum that includes the node and both of its subtrees.

The first value is what we return up the recursion stack (since a valid path in the parent can only include one subtree of the current node), while the second value is used to update our global maximum path sum.

## Visual Explanation

Let's visualize the solution with Example 2:

```
    -10
    / \
   9  20
     /  \
    15   7

For each node, we calculate:
1. max_single: Maximum path sum starting from this node and going down (can include at most one subtree)
2. max_path: Maximum path sum that includes this node and possibly both subtrees

For leaf nodes (9, 15, 7):
- Node 9: max_single = 9, max_path = 9
- Node 15: max_single = 15, max_path = 15
- Node 7: max_single = 7, max_path = 7

For node 20:
- Left subtree (15) max_single = 15
- Right subtree (7) max_single = 7
- max_single = 20 + max(15, 7) = 20 + 15 = 35
- max_path = 20 + 15 + 7 = 42 (update global max)

For root node (-10):
- Left subtree (9) max_single = 9
- Right subtree (20) max_single = 35
- max_single = -10 + max(9, 35) = -10 + 35 = 25
- max_path = -10 + 9 + 35 = 34

The global maximum path sum is 42, from the path 15 -> 20 -> 7.
```

![Binary Tree Maximum Path Sum Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Define a recursive function that calculates the maximum path sum for each subtree.
2. For each node, calculate:
   - The maximum path sum that includes the node and at most one of its subtrees (max_single).
   - The maximum path sum that includes the node and possibly both of its subtrees (max_path).
3. Update the global maximum path sum with max_path.
4. Return max_single to the parent node (since a valid path in the parent can only include one subtree of the current node).
5. Handle negative values: If max_single is negative, return 0 instead (it's better not to include this subtree).

## Code Implementation

### Python

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def maxPathSum(root):
    # Initialize global maximum
    max_sum = float('-inf')
    
    def max_gain(node):
        nonlocal max_sum
        
        # Base case: empty node
        if not node:
            return 0
        
        # Recursively compute the maximum path sum for left and right subtrees
        # If the path sum is negative, we don't include that subtree
        left_gain = max(max_gain(node.left), 0)
        right_gain = max(max_gain(node.right), 0)
        
        # The maximum path sum that includes the current node and possibly both subtrees
        current_path_sum = node.val + left_gain + right_gain
        
        # Update the global maximum
        max_sum = max(max_sum, current_path_sum)
        
        # Return the maximum path sum that includes the current node and at most one subtree
        # (since a valid path in the parent can only include one subtree of the current node)
        return node.val + max(left_gain, right_gain)
    
    # Start the recursion from the root
    max_gain(root)
    
    return max_sum
```

### JavaScript

```javascript
function TreeNode(val, left, right) {
    this.val = (val === undefined ? 0 : val);
    this.left = (left === undefined ? null : left);
    this.right = (right === undefined ? null : right);
}

function maxPathSum(root) {
    // Initialize global maximum
    let maxSum = -Infinity;
    
    function maxGain(node) {
        // Base case: empty node
        if (!node) {
            return 0;
        }
        
        // Recursively compute the maximum path sum for left and right subtrees
        // If the path sum is negative, we don't include that subtree
        const leftGain = Math.max(maxGain(node.left), 0);
        const rightGain = Math.max(maxGain(node.right), 0);
        
        // The maximum path sum that includes the current node and possibly both subtrees
        const currentPathSum = node.val + leftGain + rightGain;
        
        // Update the global maximum
        maxSum = Math.max(maxSum, currentPathSum);
        
        // Return the maximum path sum that includes the current node and at most one subtree
        // (since a valid path in the parent can only include one subtree of the current node)
        return node.val + Math.max(leftGain, rightGain);
    }
    
    // Start the recursion from the root
    maxGain(root);
    
    return maxSum;
}
```

## Time and Space Complexity

- **Time Complexity**: O(n), where n is the number of nodes in the tree. We visit each node exactly once.
- **Space Complexity**: O(h), where h is the height of the tree. This is the space used by the recursion stack. In the worst case (a skewed tree), this could be O(n).

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 1:

```
    1
   / \
  2   3

Call max_gain(1):
  Call max_gain(2):
    Left subtree is null, left_gain = 0
    Right subtree is null, right_gain = 0
    current_path_sum = 2 + 0 + 0 = 2
    Update max_sum = 2
    Return 2 + max(0, 0) = 2
  
  Call max_gain(3):
    Left subtree is null, left_gain = 0
    Right subtree is null, right_gain = 0
    current_path_sum = 3 + 0 + 0 = 3
    Update max_sum = 3
    Return 3 + max(0, 0) = 3
  
  left_gain = 2
  right_gain = 3
  current_path_sum = 1 + 2 + 3 = 6
  Update max_sum = 6
  Return 1 + max(2, 3) = 4

Result: max_sum = 6
```

## Edge Cases and Optimizations

- **Empty Tree**: If the root is null, we return 0 (though this is not explicitly handled in the problem statement).
- **Single Node Tree**: If the tree has only one node, we return the value of that node.
- **All Negative Values**: If all nodes have negative values, the maximum path sum will be the value of the node with the largest value.
- **Negative Root with Positive Children**: The maximum path might not include the root if it has a negative value.

**Optimization**: We can avoid unnecessary recursion by checking if a node is null before making the recursive call, but this is already handled in our implementation.

## Common Mistakes

1. **Not handling negative values correctly**: It's important to use `max(subtree_gain, 0)` to avoid including negative path sums.
2. **Confusing max_single and max_path**: Remember that max_single is what we return up the recursion stack (since a valid path in the parent can only include one subtree of the current node), while max_path is used to update our global maximum.
3. **Not updating the global maximum at each node**: The maximum path sum might not include the root, so we need to update the global maximum at each node.

## Related Problems

1. **Path Sum**: Determine if the tree has a root-to-leaf path such that adding up all the values along the path equals a given sum.
2. **Path Sum II**: Find all root-to-leaf paths where each path's sum equals a given sum.
3. **Binary Tree Diameter**: Find the length of the longest path between any two nodes in a tree.
4. **Maximum Width of Binary Tree**: Find the maximum width of the given tree.
5. **Lowest Common Ancestor of a Binary Tree**: Find the lowest common ancestor of two given nodes in the tree. 