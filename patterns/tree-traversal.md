# Tree Traversal Patterns

Tree traversal is a fundamental technique for processing tree data structures. This guide covers both Breadth-First Search (BFS) and Depth-First Search (DFS) approaches for tree traversal.

## Table of Contents
- [Breadth-First Search (BFS)](#breadth-first-search-bfs)
- [Depth-First Search (DFS)](#depth-first-search-dfs)
- [Level Order Traversal](#level-order-traversal)
- [Zigzag Level Order Traversal](#zigzag-level-order-traversal)
- [Vertical Order Traversal](#vertical-order-traversal)
- [Boundary Traversal](#boundary-traversal)
- [Related Blind 75 & Grind 75 Problems](#related-blind-75--grind-75-problems)
- [Tips and Tricks](#tips-and-tricks)

## Breadth-First Search (BFS)

BFS traverses a tree level by level, visiting all nodes at the current depth before moving to nodes at the next depth level.

### Visual Explanation
```
Tree:
        1
       / \
      2   3
     / \   \
    4   5   6
       /
      7

BFS Traversal: 1, 2, 3, 4, 5, 6, 7
```

### Implementation

#### Python
```python
from collections import deque

def bfs(root):
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        node = queue.popleft()
        result.append(node.val)
        
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    
    return result
```

#### JavaScript
```javascript
function bfs(root) {
    if (!root) {
        return [];
    }
    
    const result = [];
    const queue = [root];
    
    while (queue.length > 0) {
        const node = queue.shift();
        result.push(node.val);
        
        if (node.left) {
            queue.push(node.left);
        }
        if (node.right) {
            queue.push(node.right);
        }
    }
    
    return result;
}
```

### Time & Space Complexity
- **Time Complexity**: O(n) where n is the number of nodes in the tree
- **Space Complexity**: O(w) where w is the maximum width of the tree

## Depth-First Search (DFS)

DFS explores as far as possible along each branch before backtracking. There are three main types of DFS traversals for binary trees:

### 1. Preorder Traversal (Root, Left, Right)

#### Visual Explanation
```
Tree:
        1
       / \
      2   3
     / \   \
    4   5   6
       /
      7

Preorder Traversal: 1, 2, 4, 5, 7, 3, 6
```

#### Implementation

##### Python
```python
def preorder_traversal(root):
    result = []
    
    def dfs(node):
        if not node:
            return
        
        # Visit root
        result.append(node.val)
        
        # Visit left subtree
        dfs(node.left)
        
        # Visit right subtree
        dfs(node.right)
    
    dfs(root)
    return result
```

##### JavaScript
```javascript
function preorderTraversal(root) {
    const result = [];
    
    function dfs(node) {
        if (!node) {
            return;
        }
        
        // Visit root
        result.push(node.val);
        
        // Visit left subtree
        dfs(node.left);
        
        // Visit right subtree
        dfs(node.right);
    }
    
    dfs(root);
    return result;
}
```

### 2. Inorder Traversal (Left, Root, Right)

#### Visual Explanation
```
Tree:
        1
       / \
      2   3
     / \   \
    4   5   6
       /
      7

Inorder Traversal: 4, 2, 7, 5, 1, 3, 6
```

#### Implementation

##### Python
```python
def inorder_traversal(root):
    result = []
    
    def dfs(node):
        if not node:
            return
        
        # Visit left subtree
        dfs(node.left)
        
        # Visit root
        result.append(node.val)
        
        # Visit right subtree
        dfs(node.right)
    
    dfs(root)
    return result
```

##### JavaScript
```javascript
function inorderTraversal(root) {
    const result = [];
    
    function dfs(node) {
        if (!node) {
            return;
        }
        
        // Visit left subtree
        dfs(node.left);
        
        // Visit root
        result.push(node.val);
        
        // Visit right subtree
        dfs(node.right);
    }
    
    dfs(root);
    return result;
}
```

### 3. Postorder Traversal (Left, Right, Root)

#### Visual Explanation
```
Tree:
        1
       / \
      2   3
     / \   \
    4   5   6
       /
      7

Postorder Traversal: 4, 7, 5, 2, 6, 3, 1
```

#### Implementation

##### Python
```python
def postorder_traversal(root):
    result = []
    
    def dfs(node):
        if not node:
            return
        
        # Visit left subtree
        dfs(node.left)
        
        # Visit right subtree
        dfs(node.right)
        
        # Visit root
        result.append(node.val)
    
    dfs(root)
    return result
```

##### JavaScript
```javascript
function postorderTraversal(root) {
    const result = [];
    
    function dfs(node) {
        if (!node) {
            return;
        }
        
        // Visit left subtree
        dfs(node.left);
        
        // Visit right subtree
        dfs(node.right);
        
        // Visit root
        result.push(node.val);
    }
    
    dfs(root);
    return result;
}
```

### Time & Space Complexity for DFS
- **Time Complexity**: O(n) where n is the number of nodes in the tree
- **Space Complexity**: O(h) where h is the height of the tree (due to the recursion stack)

## Level Order Traversal

Level order traversal is a BFS approach that groups nodes by their level in the tree.

### Visual Explanation
```
Tree:
        1
       / \
      2   3
     / \   \
    4   5   6
       /
      7

Level Order Traversal:
Level 0: [1]
Level 1: [2, 3]
Level 2: [4, 5, 6]
Level 3: [7]
```

### Implementation

#### Python
```python
from collections import deque

def level_order_traversal(root):
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        level = []
        
        for _ in range(level_size):
            node = queue.popleft()
            level.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(level)
    
    return result
```

#### JavaScript
```javascript
function levelOrderTraversal(root) {
    if (!root) {
        return [];
    }
    
    const result = [];
    const queue = [root];
    
    while (queue.length > 0) {
        const levelSize = queue.length;
        const level = [];
        
        for (let i = 0; i < levelSize; i++) {
            const node = queue.shift();
            level.push(node.val);
            
            if (node.left) {
                queue.push(node.left);
            }
            if (node.right) {
                queue.push(node.right);
            }
        }
        
        result.push(level);
    }
    
    return result;
}
```

### Time & Space Complexity
- **Time Complexity**: O(n) where n is the number of nodes in the tree
- **Space Complexity**: O(w) where w is the maximum width of the tree

## Zigzag Level Order Traversal

Zigzag level order traversal is a variation of level order traversal where each level is traversed in alternating directions.

### Visual Explanation
```
Tree:
        1
       / \
      2   3
     / \   \
    4   5   6
       /
      7

Zigzag Level Order Traversal:
Level 0 (Left to Right): [1]
Level 1 (Right to Left): [3, 2]
Level 2 (Left to Right): [4, 5, 6]
Level 3 (Right to Left): [7]
```

### Implementation

#### Python
```python
from collections import deque

def zigzag_level_order(root):
    if not root:
        return []
    
    result = []
    queue = deque([root])
    left_to_right = True
    
    while queue:
        level_size = len(queue)
        level = [0] * level_size  # Pre-allocate level array
        
        for i in range(level_size):
            node = queue.popleft()
            
            # Determine position based on direction
            position = i if left_to_right else level_size - 1 - i
            level[position] = node.val
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(level)
        left_to_right = not left_to_right  # Toggle direction
    
    return result
```

#### JavaScript
```javascript
function zigzagLevelOrder(root) {
    if (!root) {
        return [];
    }
    
    const result = [];
    const queue = [root];
    let leftToRight = true;
    
    while (queue.length > 0) {
        const levelSize = queue.length;
        const level = new Array(levelSize);
        
        for (let i = 0; i < levelSize; i++) {
            const node = queue.shift();
            
            // Determine position based on direction
            const position = leftToRight ? i : levelSize - 1 - i;
            level[position] = node.val;
            
            if (node.left) {
                queue.push(node.left);
            }
            if (node.right) {
                queue.push(node.right);
            }
        }
        
        result.push(level);
        leftToRight = !leftToRight;  // Toggle direction
    }
    
    return result;
}
```

### Time & Space Complexity
- **Time Complexity**: O(n) where n is the number of nodes in the tree
- **Space Complexity**: O(w) where w is the maximum width of the tree

## Vertical Order Traversal

Vertical order traversal groups nodes by their horizontal distance from the root.

### Visual Explanation
```
Tree:
        1
       / \
      2   3
     / \   \
    4   5   6
       /
      7

Vertical Order Traversal:
Column -2: [4]
Column -1: [2]
Column 0: [1, 5, 7]
Column 1: [3]
Column 2: [6]
```

### Implementation

#### Python
```python
from collections import defaultdict, deque

def vertical_order_traversal(root):
    if not root:
        return []
    
    # Map to store nodes at each column
    column_map = defaultdict(list)
    
    # Queue for BFS: (node, column)
    queue = deque([(root, 0)])
    
    # Track min and max column
    min_col = max_col = 0
    
    while queue:
        node, col = queue.popleft()
        
        # Add node to its column
        column_map[col].append(node.val)
        
        # Update min and max column
        min_col = min(min_col, col)
        max_col = max(max_col, col)
        
        # Add children to queue
        if node.left:
            queue.append((node.left, col - 1))
        if node.right:
            queue.append((node.right, col + 1))
    
    # Construct result from min to max column
    result = []
    for col in range(min_col, max_col + 1):
        result.append(column_map[col])
    
    return result
```

#### JavaScript
```javascript
function verticalOrderTraversal(root) {
    if (!root) {
        return [];
    }
    
    // Map to store nodes at each column
    const columnMap = new Map();
    
    // Queue for BFS: [node, column]
    const queue = [[root, 0]];
    
    // Track min and max column
    let minCol = 0;
    let maxCol = 0;
    
    while (queue.length > 0) {
        const [node, col] = queue.shift();
        
        // Add node to its column
        if (!columnMap.has(col)) {
            columnMap.set(col, []);
        }
        columnMap.get(col).push(node.val);
        
        // Update min and max column
        minCol = Math.min(minCol, col);
        maxCol = Math.max(maxCol, col);
        
        // Add children to queue
        if (node.left) {
            queue.push([node.left, col - 1]);
        }
        if (node.right) {
            queue.push([node.right, col + 1]);
        }
    }
    
    // Construct result from min to max column
    const result = [];
    for (let col = minCol; col <= maxCol; col++) {
        if (columnMap.has(col)) {
            result.push(columnMap.get(col));
        }
    }
    
    return result;
}
```

### Time & Space Complexity
- **Time Complexity**: O(n log n) where n is the number of nodes in the tree (due to sorting in some implementations)
- **Space Complexity**: O(n) for storing all nodes

## Boundary Traversal

Boundary traversal visits the boundary of a binary tree in an anti-clockwise direction.

### Visual Explanation
```
Tree:
        1
       / \
      2   3
     / \   \
    4   5   6
       /
      7

Boundary Traversal:
Left Boundary (excluding leaves): [1, 2]
Leaves (left to right): [4, 7, 6]
Right Boundary (excluding leaves, in reverse): [3]
Result: [1, 2, 4, 7, 6, 3]
```

### Implementation

#### Python
```python
def boundary_traversal(root):
    if not root:
        return []
    
    result = [root.val]
    
    # Helper function to add left boundary (excluding leaves)
    def add_left_boundary(node):
        if not node or (not node.left and not node.right):
            return
        
        result.append(node.val)
        
        if node.left:
            add_left_boundary(node.left)
        else:
            add_left_boundary(node.right)
    
    # Helper function to add leaves (left to right)
    def add_leaves(node):
        if not node:
            return
        
        if not node.left and not node.right:
            result.append(node.val)
            return
        
        add_leaves(node.left)
        add_leaves(node.right)
    
    # Helper function to add right boundary (excluding leaves, in reverse)
    def add_right_boundary(node):
        if not node or (not node.left and not node.right):
            return
        
        if node.right:
            add_right_boundary(node.right)
        else:
            add_right_boundary(node.left)
        
        result.append(node.val)  # Add after recursion (reverse order)
    
    # Skip root for left and right boundary
    if root.left:
        add_left_boundary(root.left)
    
    # Add all leaves
    add_leaves(root.left)
    add_leaves(root.right)
    
    # Add right boundary in reverse order
    if root.right:
        add_right_boundary(root.right)
    
    return result
```

#### JavaScript
```javascript
function boundaryTraversal(root) {
    if (!root) {
        return [];
    }
    
    const result = [root.val];
    
    // Helper function to add left boundary (excluding leaves)
    function addLeftBoundary(node) {
        if (!node || (!node.left && !node.right)) {
            return;
        }
        
        result.push(node.val);
        
        if (node.left) {
            addLeftBoundary(node.left);
        } else {
            addLeftBoundary(node.right);
        }
    }
    
    // Helper function to add leaves (left to right)
    function addLeaves(node) {
        if (!node) {
            return;
        }
        
        if (!node.left && !node.right) {
            result.push(node.val);
            return;
        }
        
        addLeaves(node.left);
        addLeaves(node.right);
    }
    
    // Helper function to add right boundary (excluding leaves, in reverse)
    function addRightBoundary(node) {
        if (!node || (!node.left && !node.right)) {
            return;
        }
        
        if (node.right) {
            addRightBoundary(node.right);
        } else {
            addRightBoundary(node.left);
        }
        
        result.push(node.val);  // Add after recursion (reverse order)
    }
    
    // Skip root for left and right boundary
    if (root.left) {
        addLeftBoundary(root.left);
    }
    
    // Add all leaves
    addLeaves(root.left);
    addLeaves(root.right);
    
    // Add right boundary in reverse order
    if (root.right) {
        addRightBoundary(root.right);
    }
    
    return result;
}
```

### Time & Space Complexity
- **Time Complexity**: O(n) where n is the number of nodes in the tree
- **Space Complexity**: O(h) where h is the height of the tree (due to the recursion stack)

## Related Blind 75 & Grind 75 Problems

1. **Binary Tree Level Order Traversal** (Blind 75 #102)
   - Problem: Return the level order traversal of a binary tree's values.
   - Solution: Use BFS with a queue to traverse the tree level by level.
   - [LeetCode #102](https://leetcode.com/problems/binary-tree-level-order-traversal/)

2. **Binary Tree Zigzag Level Order Traversal** (Blind 75 #103)
   - Problem: Return the zigzag level order traversal of a binary tree's values.
   - Solution: Modify the level order traversal to alternate the direction of traversal.
   - [LeetCode #103](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

3. **Binary Tree Right Side View** (Blind 75 #199)
   - Problem: Return the values visible from the right side of a binary tree.
   - Solution: Use level order traversal and take the rightmost node at each level.
   - [LeetCode #199](https://leetcode.com/problems/binary-tree-right-side-view/)

4. **Validate Binary Search Tree** (Blind 75 #98)
   - Problem: Determine if a binary tree is a valid binary search tree.
   - Solution: Use inorder traversal or recursive validation with min/max constraints.
   - [LeetCode #98](https://leetcode.com/problems/validate-binary-search-tree/)

5. **Kth Smallest Element in a BST** (Blind 75 #230)
   - Problem: Find the kth smallest element in a binary search tree.
   - Solution: Use inorder traversal to get elements in ascending order.
   - [LeetCode #230](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

6. **Construct Binary Tree from Preorder and Inorder Traversal** (Blind 75 #105)
   - Problem: Construct a binary tree from preorder and inorder traversal arrays.
   - Solution: Use recursion to divide and conquer the arrays.
   - [LeetCode #105](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

## Tips and Tricks

1. **Choosing the Right Traversal**:
   - Use BFS (level order) when you need to process nodes level by level or find the shortest path.
   - Use DFS when you need to explore as far as possible along each branch before backtracking.
   - Use inorder traversal for BSTs to get elements in sorted order.
   - Use preorder traversal when you need to create a copy of the tree or serialize it.
   - Use postorder traversal when you need to delete a tree or process children before parents.

2. **Iterative vs. Recursive Implementation**:
   - Recursive implementations are usually cleaner and easier to understand.
   - Iterative implementations avoid stack overflow for very deep trees.
   - For BFS, always use an iterative approach with a queue.
   - For DFS, you can use either recursive or iterative (with a stack) approaches.

3. **Common Patterns**:
   - Use a queue for BFS and a stack (or recursion) for DFS.
   - For level-aware BFS, process nodes level by level using the queue size.
   - For vertical or diagonal traversals, use a hash map to group nodes by position.
   - For boundary traversal, handle left boundary, leaves, and right boundary separately.

4. **Optimization Techniques**:
   - Use a sentinel value (e.g., null) to mark the end of a level in BFS.
   - Use Morris traversal for O(1) space inorder traversal.
   - Avoid unnecessary queue operations by using a single loop with level size.
   - Use iterative deepening for memory-constrained environments.

5. **Debugging Tree Traversals**:
   - Visualize the tree and trace through the algorithm step by step.
   - Test with small, simple trees first.
   - Check edge cases: empty tree, single node, skewed tree, complete tree.
   - Verify that all nodes are visited exactly once. 