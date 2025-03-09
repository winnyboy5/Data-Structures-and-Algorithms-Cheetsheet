# Construct Binary Tree from Preorder and Inorder Traversal

## Problem Statement

Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return the binary tree.

**Example 1:**
```
    3
   / \
  9  20
    /  \
   15   7
```
```
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
```

**Example 2:**
```
Input: preorder = [-1], inorder = [-1]
Output: [-1]
```

## Intuition

The key insight for this problem is understanding how preorder and inorder traversals uniquely define a binary tree:

1. **Preorder traversal**: The first element is always the root of the tree. The elements that follow are the preorder traversal of the left subtree, followed by the preorder traversal of the right subtree.

2. **Inorder traversal**: Elements to the left of the root in the inorder traversal belong to the left subtree, and elements to the right belong to the right subtree.

By combining these properties, we can recursively construct the binary tree:
1. Identify the root from the preorder array (it's the first element).
2. Find the position of this root in the inorder array.
3. Elements to the left of this position in the inorder array form the left subtree.
4. Elements to the right of this position in the inorder array form the right subtree.
5. Recursively build the left and right subtrees.

## Visual Explanation

Let's visualize the solution with Example 1:

```
Preorder: [3, 9, 20, 15, 7]
Inorder:  [9, 3, 15, 20, 7]
```

1. The first element in preorder is 3, so the root of the tree is 3.
2. Find 3 in the inorder array. It's at index 1.
3. Elements to the left of 3 in inorder ([9]) form the left subtree.
4. Elements to the right of 3 in inorder ([15, 20, 7]) form the right subtree.

```
       3
      / \
     /   \
    /     \
   9      20
         /  \
        15   7
```

Now, recursively build the left subtree:
- Preorder for left subtree: [9]
- Inorder for left subtree: [9]
- The root is 9, and there are no elements to the left or right, so it's a leaf node.

And recursively build the right subtree:
- Preorder for right subtree: [20, 15, 7]
- Inorder for right subtree: [15, 20, 7]
- The root is 20, elements to the left are [15], and elements to the right are [7].

```
       3
      / \
     9  20
       /  \
      15   7
```

Continue this process until the entire tree is constructed.

![Construct Binary Tree Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Create a helper function that takes the preorder array, inorder array, and the start and end indices for both arrays.
2. If the start index is greater than the end index, return null (base case).
3. The first element of the preorder array is the root of the current subtree.
4. Find the index of this root element in the inorder array.
5. Calculate the size of the left subtree (index of root in inorder - start index of inorder).
6. Recursively build the left subtree using:
   - Preorder: [preStart + 1, preStart + leftSize]
   - Inorder: [inStart, rootIndex - 1]
7. Recursively build the right subtree using:
   - Preorder: [preStart + leftSize + 1, preEnd]
   - Inorder: [rootIndex + 1, inEnd]
8. Return the root node.

## Code Implementation

### Python

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def buildTree(preorder, inorder):
    # Create a hashmap to store the indices of elements in inorder array
    # This allows O(1) lookup time instead of O(n) when finding the root index
    inorder_map = {val: idx for idx, val in enumerate(inorder)}
    
    def build(preStart, preEnd, inStart, inEnd):
        # Base case: if there are no elements to construct the tree
        if preStart > preEnd:
            return None
        
        # The first element in preorder is the root
        root_val = preorder[preStart]
        root = TreeNode(root_val)
        
        # Find the position of the root in inorder
        root_idx = inorder_map[root_val]
        
        # Calculate the size of the left subtree
        left_size = root_idx - inStart
        
        # Recursively build left and right subtrees
        root.left = build(preStart + 1, preStart + left_size, inStart, root_idx - 1)
        root.right = build(preStart + left_size + 1, preEnd, root_idx + 1, inEnd)
        
        return root
    
    return build(0, len(preorder) - 1, 0, len(inorder) - 1)
```

### JavaScript

```javascript
function TreeNode(val, left, right) {
    this.val = (val === undefined ? 0 : val);
    this.left = (left === undefined ? null : left);
    this.right = (right === undefined ? null : right);
}

function buildTree(preorder, inorder) {
    // Create a map to store the indices of elements in inorder array
    const inorderMap = new Map();
    for (let i = 0; i < inorder.length; i++) {
        inorderMap.set(inorder[i], i);
    }
    
    function build(preStart, preEnd, inStart, inEnd) {
        // Base case: if there are no elements to construct the tree
        if (preStart > preEnd) {
            return null;
        }
        
        // The first element in preorder is the root
        const rootVal = preorder[preStart];
        const root = new TreeNode(rootVal);
        
        // Find the position of the root in inorder
        const rootIdx = inorderMap.get(rootVal);
        
        // Calculate the size of the left subtree
        const leftSize = rootIdx - inStart;
        
        // Recursively build left and right subtrees
        root.left = build(preStart + 1, preStart + leftSize, inStart, rootIdx - 1);
        root.right = build(preStart + leftSize + 1, preEnd, rootIdx + 1, inEnd);
        
        return root;
    }
    
    return build(0, preorder.length - 1, 0, inorder.length - 1);
}
```

## Time and Space Complexity

### Time Complexity
- **O(n)**, where n is the number of nodes in the tree.
- We process each node exactly once.
- Using a hashmap for the inorder array allows us to find the root's position in O(1) time.

### Space Complexity
- **O(n)** for the hashmap that stores the indices of elements in the inorder array.
- **O(h)** for the recursion stack, where h is the height of the tree. In the worst case (a skewed tree), this could be O(n).
- Overall, the space complexity is O(n).

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 1:

```
Preorder: [3, 9, 20, 15, 7]
Inorder:  [9, 3, 15, 20, 7]
```

1. Create inorder_map: {9: 0, 3: 1, 15: 2, 20: 3, 7: 4}
2. Call build(0, 4, 0, 4):
   - root_val = preorder[0] = 3
   - root = TreeNode(3)
   - root_idx = inorder_map[3] = 1
   - left_size = 1 - 0 = 1
   - Recursively build left subtree: build(1, 1, 0, 0)
     - root_val = preorder[1] = 9
     - root = TreeNode(9)
     - root_idx = inorder_map[9] = 0
     - left_size = 0 - 0 = 0
     - Recursively build left subtree: build(2, 1, 0, -1) returns null
     - Recursively build right subtree: build(2, 1, 1, 0) returns null
     - Return TreeNode(9)
   - Recursively build right subtree: build(2, 4, 2, 4)
     - root_val = preorder[2] = 20
     - root = TreeNode(20)
     - root_idx = inorder_map[20] = 3
     - left_size = 3 - 2 = 1
     - Recursively build left subtree: build(3, 3, 2, 2)
       - root_val = preorder[3] = 15
       - root = TreeNode(15)
       - root_idx = inorder_map[15] = 2
       - left_size = 2 - 2 = 0
       - Recursively build left subtree: build(4, 3, 2, 1) returns null
       - Recursively build right subtree: build(4, 3, 3, 2) returns null
       - Return TreeNode(15)
     - Recursively build right subtree: build(4, 4, 4, 4)
       - root_val = preorder[4] = 7
       - root = TreeNode(7)
       - root_idx = inorder_map[7] = 4
       - left_size = 4 - 4 = 0
       - Recursively build left subtree: build(5, 4, 4, 3) returns null
       - Recursively build right subtree: build(5, 4, 5, 4) returns null
       - Return TreeNode(7)
     - Return TreeNode(20) with left=TreeNode(15) and right=TreeNode(7)
   - Return TreeNode(3) with left=TreeNode(9) and right=TreeNode(20)

3. Final tree:
```
    3
   / \
  9  20
    /  \
   15   7
```

## Edge Cases and Optimizations

- **Empty Arrays**: If the input arrays are empty, return null.
- **Single Node**: If the arrays have only one element, return a tree with just the root node.
- **Duplicate Values**: This solution assumes that there are no duplicate values in the tree. If there are duplicates, we would need to modify our approach.

**Optimization**: We use a hashmap to store the indices of elements in the inorder array. This allows us to find the position of the root in O(1) time instead of O(n).

## Common Mistakes

1. **Not handling empty arrays**: Make sure to check if the arrays are empty before starting the construction.
2. **Incorrect indices for recursive calls**: Be careful with the indices when making recursive calls for the left and right subtrees.
3. **Assuming no duplicates**: The standard solution assumes that there are no duplicate values in the tree. If there are duplicates, additional information would be needed to uniquely construct the tree.

## Related Problems

1. **Construct Binary Tree from Inorder and Postorder Traversal**: Similar problem but using postorder traversal instead of preorder.
2. **Serialize and Deserialize Binary Tree**: Convert a binary tree to a string and back.
3. **Construct Binary Search Tree from Preorder Traversal**: Construct a BST from just the preorder traversal.
4. **Recover Binary Search Tree**: Restore a BST where two nodes have been swapped. 