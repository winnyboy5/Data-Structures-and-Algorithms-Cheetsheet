# Subtree of Another Tree

## Problem Statement

Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values as `subRoot` and `false` otherwise.

A subtree of a binary tree `tree` is a tree that consists of a node in `tree` and all of this node's descendants. The tree `tree` could also be considered as a subtree of itself.

**Example 1:**
```
     3
    / \
   4   5
  / \
 1   2
```
```
   4
  / \
 1   2
```
```
Input: root = [3,4,5,1,2], subRoot = [4,1,2]
Output: true
```

**Example 2:**
```
     3
    / \
   4   5
  / \
 1   2
    /
   0
```
```
   4
  / \
 1   2
```
```
Input: root = [3,4,5,1,2,null,null,null,null,0], subRoot = [4,1,2]
Output: false
```

## Intuition

The key insight for this problem is to recognize that we need to check if the `subRoot` tree appears anywhere within the `root` tree. This requires two types of checks:

1. **Is the current subtree identical to the `subRoot`?** - We need a helper function to check if two trees are identical.
2. **If not, could the `subRoot` be a subtree of the left or right children?** - We need to recursively check both children.

The problem can be solved using a recursive approach where we:
1. Check if the current node of `root` and its subtree is identical to `subRoot`.
2. If not, recursively check if `subRoot` is a subtree of the left child of `root`.
3. If not, recursively check if `subRoot` is a subtree of the right child of `root`.

## Visual Explanation

Let's visualize the solution with Example 1:

```
Root Tree:
     3
    / \
   4   5
  / \
 1   2

SubRoot Tree:
   4
  / \
 1   2
```

1. Start at the root node (3) of the main tree.
2. Check if the subtree rooted at 3 is identical to the subRoot tree. It's not.
3. Recursively check the left child (4):
   - Check if the subtree rooted at 4 is identical to the subRoot tree.
   - Compare node values and structure:
     - Both roots are 4 ✓
     - Both left children are 1 ✓
     - Both right children are 2 ✓
     - Both trees have the same structure ✓
   - The subtree rooted at 4 is identical to the subRoot tree!
4. Return true.

Now let's visualize Example 2:

```
Root Tree:
     3
    / \
   4   5
  / \
 1   2
    /
   0

SubRoot Tree:
   4
  / \
 1   2
```

1. Start at the root node (3) of the main tree.
2. Check if the subtree rooted at 3 is identical to the subRoot tree. It's not.
3. Recursively check the left child (4):
   - Check if the subtree rooted at 4 is identical to the subRoot tree.
   - Compare node values and structure:
     - Both roots are 4 ✓
     - Both left children are 1 ✓
     - Both right children are 2 ✓
     - But the right child of 2 in the main tree has a child 0, while in the subRoot tree it doesn't ✗
   - The subtree rooted at 4 is NOT identical to the subRoot tree.
4. Recursively check the right child (5):
   - Check if the subtree rooted at 5 is identical to the subRoot tree. It's not.
5. Return false.

![Subtree Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Define a helper function `isIdentical(s, t)` that checks if two trees are identical:
   - If both trees are null, return true.
   - If one tree is null and the other is not, return false.
   - If the values of the current nodes are different, return false.
   - Recursively check if the left subtrees are identical AND the right subtrees are identical.

2. Define the main function `isSubtree(root, subRoot)`:
   - If subRoot is null, return true (an empty tree is a subtree of any tree).
   - If root is null, return false (a non-empty tree cannot be a subtree of an empty tree).
   - If the current subtree is identical to subRoot, return true.
   - Recursively check if subRoot is a subtree of the left child OR the right child.

## Code Implementation

### Python

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def isSubtree(root, subRoot):
    # Helper function to check if two trees are identical
    def isIdentical(s, t):
        # If both trees are empty, they are identical
        if not s and not t:
            return True
        
        # If one tree is empty and the other is not, they are not identical
        if not s or not t:
            return False
        
        # Check if current nodes and their subtrees are identical
        return (s.val == t.val and 
                isIdentical(s.left, t.left) and 
                isIdentical(s.right, t.right))
    
    # Base cases
    if not subRoot:
        return True  # Empty tree is a subtree of any tree
    
    if not root:
        return False  # Non-empty tree cannot be a subtree of an empty tree
    
    # Check if the current subtree is identical to subRoot
    if isIdentical(root, subRoot):
        return True
    
    # Recursively check left and right subtrees
    return isSubtree(root.left, subRoot) or isSubtree(root.right, subRoot)
```

### JavaScript

```javascript
function isSubtree(root, subRoot) {
    // Helper function to check if two trees are identical
    function isIdentical(s, t) {
        // If both trees are empty, they are identical
        if (!s && !t) {
            return true;
        }
        
        // If one tree is empty and the other is not, they are not identical
        if (!s || !t) {
            return false;
        }
        
        // Check if current nodes and their subtrees are identical
        return (s.val === t.val && 
                isIdentical(s.left, t.left) && 
                isIdentical(s.right, t.right));
    }
    
    // Base cases
    if (!subRoot) {
        return true;  // Empty tree is a subtree of any tree
    }
    
    if (!root) {
        return false;  // Non-empty tree cannot be a subtree of an empty tree
    }
    
    // Check if the current subtree is identical to subRoot
    if (isIdentical(root, subRoot)) {
        return true;
    }
    
    // Recursively check left and right subtrees
    return isSubtree(root.left, subRoot) || isSubtree(root.right, subRoot);
}
```

## Time and Space Complexity

### Time Complexity
- **O(m * n)**, where m is the number of nodes in the root tree and n is the number of nodes in the subRoot tree.
- In the worst case, we might need to check for each node in the root tree whether its subtree is identical to the subRoot tree.
- The `isIdentical` function takes O(n) time in the worst case.

### Space Complexity
- **O(h)**, where h is the height of the root tree.
- This is due to the recursion stack. In the worst case (a skewed tree), the height could be O(m).

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 1:

```
Root Tree:
     3
    / \
   4   5
  / \
 1   2

SubRoot Tree:
   4
  / \
 1   2
```

1. Call `isSubtree(root=3, subRoot=4)`:
   - subRoot is not null, continue.
   - root is not null, continue.
   - Call `isIdentical(s=3, t=4)`:
     - Both s and t are not null, continue.
     - s.val (3) != t.val (4), return false.
   - `isIdentical` returned false, so we recursively check left and right subtrees.
   - Call `isSubtree(root=4, subRoot=4)`:
     - Call `isIdentical(s=4, t=4)`:
       - Both s and t are not null, continue.
       - s.val (4) == t.val (4), continue.
       - Call `isIdentical(s=1, t=1)`:
         - Both s and t are not null, continue.
         - s.val (1) == t.val (1), continue.
         - s.left and t.left are both null, `isIdentical(s.left, t.left)` returns true.
         - s.right and t.right are both null, `isIdentical(s.right, t.right)` returns true.
         - Return true.
       - Call `isIdentical(s=2, t=2)`:
         - Both s and t are not null, continue.
         - s.val (2) == t.val (2), continue.
         - s.left and t.left are both null, `isIdentical(s.left, t.left)` returns true.
         - s.right and t.right are both null, `isIdentical(s.right, t.right)` returns true.
         - Return true.
       - `isIdentical(s.left, t.left)` and `isIdentical(s.right, t.right)` both returned true, so return true.
     - `isIdentical` returned true, so return true.
   - `isSubtree(root.left, subRoot)` returned true, so return true.

2. Final result: true

## Edge Cases and Optimizations

- **Empty Trees**: If the subRoot is null, we should return true because an empty tree is a subtree of any tree. If the root is null but the subRoot is not, we should return false.
- **Single Node Trees**: If both trees are single nodes with the same value, they are identical.
- **Identical Trees**: If both root and subRoot are identical, then subRoot is a subtree of root.

**Optimization**: We can use string serialization to convert both trees to strings and then check if the serialized subRoot is a substring of the serialized root. This approach has a time complexity of O(m + n) but requires additional space for the serialized strings.

## Common Mistakes

1. **Not handling null nodes correctly**: Make sure to handle cases where either tree or both trees are null.
2. **Confusing subtree with subset**: A subtree must include all descendants of a node, not just a subset of nodes.
3. **Not checking structure**: Two trees with the same values but different structures are not identical.

## Related Problems

1. **Same Tree**: Check if two binary trees are identical.
2. **Symmetric Tree**: Check if a binary tree is a mirror of itself.
3. **Path Sum**: Check if a tree has a root-to-leaf path with a given sum.
4. **Count Univalue Subtrees**: Count the number of subtrees where all nodes have the same value. 