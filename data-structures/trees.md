# Trees

A tree is a hierarchical data structure consisting of nodes connected by edges. Each tree has a root node, and each node can have zero or more child nodes. Trees are widely used for representing hierarchical relationships.

## Visual Representation

```
Binary Tree:
        ┌───┐
        │ 1 │
        └─┬─┘
     ┌────┴────┐
  ┌──┴──┐   ┌──┴──┐
  │  2  │   │  3  │
  └──┬──┘   └──┬──┘
  ┌──┴──┐   ┌──┴──┐
  │  4  │   │  5  │
  └─────┘   └─────┘

Binary Search Tree (BST):
        ┌───┐
        │ 8 │
        └─┬─┘
     ┌────┴────┐
  ┌──┴──┐   ┌──┴──┐
  │  3  │   │ 10  │
  └──┬──┘   └──┬──┘
  ┌──┴──┐      └──┐
  │  1  │      ┌──┴──┐
  └─────┘      │ 14  │
               └─────┘
```

## Types of Trees

1. **Binary Tree**: Each node has at most two children, referred to as the left child and the right child.
2. **Binary Search Tree (BST)**: A binary tree where the left child contains only nodes with values less than the parent node, and the right child only contains nodes with values greater than the parent node.
3. **AVL Tree**: A self-balancing binary search tree where the heights of the two child subtrees of any node differ by at most one.
4. **Red-Black Tree**: A self-balancing binary search tree with additional properties that ensure the tree remains balanced during insertions and deletions.
5. **B-Tree**: A self-balancing tree data structure that maintains sorted data and allows searches, sequential access, insertions, and deletions in logarithmic time.
6. **Heap**: A specialized tree-based data structure that satisfies the heap property.
7. **Trie**: A tree-like data structure used to store a dynamic set of strings.

## Implementation in Python and JavaScript

### Python

#### Binary Tree

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

# Example usage
# Create a binary tree:
#        1
#       / \
#      2   3
#     / \
#    4   5
root = TreeNode(1)
root.left = TreeNode(2)
root.right = TreeNode(3)
root.left.left = TreeNode(4)
root.left.right = TreeNode(5)
```

#### Binary Search Tree (BST)

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class BST:
    def __init__(self):
        self.root = None
    
    def insert(self, val):
        if not self.root:
            self.root = TreeNode(val)
            return
        
        self._insert_recursive(self.root, val)
    
    def _insert_recursive(self, node, val):
        if val < node.val:
            if node.left is None:
                node.left = TreeNode(val)
            else:
                self._insert_recursive(node.left, val)
        else:
            if node.right is None:
                node.right = TreeNode(val)
            else:
                self._insert_recursive(node.right, val)
    
    def search(self, val):
        return self._search_recursive(self.root, val)
    
    def _search_recursive(self, node, val):
        if node is None or node.val == val:
            return node
        
        if val < node.val:
            return self._search_recursive(node.left, val)
        return self._search_recursive(node.right, val)
    
    def delete(self, val):
        self.root = self._delete_recursive(self.root, val)
    
    def _delete_recursive(self, node, val):
        if node is None:
            return None
        
        if val < node.val:
            node.left = self._delete_recursive(node.left, val)
        elif val > node.val:
            node.right = self._delete_recursive(node.right, val)
        else:
            # Node with only one child or no child
            if node.left is None:
                return node.right
            elif node.right is None:
                return node.left
            
            # Node with two children
            # Get the inorder successor (smallest in the right subtree)
            node.val = self._min_value(node.right)
            
            # Delete the inorder successor
            node.right = self._delete_recursive(node.right, node.val)
        
        return node
    
    def _min_value(self, node):
        current = node
        while current.left:
            current = current.left
        return current.val

# Example usage
bst = BST()
bst.insert(8)
bst.insert(3)
bst.insert(10)
bst.insert(1)
bst.insert(14)

# Search for a value
result = bst.search(10)
print(result.val if result else "Not found")  # Output: 10

# Delete a value
bst.delete(3)
```

### JavaScript

#### Binary Tree

```javascript
class TreeNode {
    constructor(val = 0, left = null, right = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

// Example usage
// Create a binary tree:
//        1
//       / \
//      2   3
//     / \
//    4   5
const root = new TreeNode(1);
root.left = new TreeNode(2);
root.right = new TreeNode(3);
root.left.left = new TreeNode(4);
root.left.right = new TreeNode(5);
```

#### Binary Search Tree (BST)

```javascript
class TreeNode {
    constructor(val = 0, left = null, right = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

class BST {
    constructor() {
        this.root = null;
    }
    
    insert(val) {
        if (!this.root) {
            this.root = new TreeNode(val);
            return;
        }
        
        this._insertRecursive(this.root, val);
    }
    
    _insertRecursive(node, val) {
        if (val < node.val) {
            if (node.left === null) {
                node.left = new TreeNode(val);
            } else {
                this._insertRecursive(node.left, val);
            }
        } else {
            if (node.right === null) {
                node.right = new TreeNode(val);
            } else {
                this._insertRecursive(node.right, val);
            }
        }
    }
    
    search(val) {
        return this._searchRecursive(this.root, val);
    }
    
    _searchRecursive(node, val) {
        if (node === null || node.val === val) {
            return node;
        }
        
        if (val < node.val) {
            return this._searchRecursive(node.left, val);
        }
        return this._searchRecursive(node.right, val);
    }
    
    delete(val) {
        this.root = this._deleteRecursive(this.root, val);
    }
    
    _deleteRecursive(node, val) {
        if (node === null) {
            return null;
        }
        
        if (val < node.val) {
            node.left = this._deleteRecursive(node.left, val);
        } else if (val > node.val) {
            node.right = this._deleteRecursive(node.right, val);
        } else {
            // Node with only one child or no child
            if (node.left === null) {
                return node.right;
            } else if (node.right === null) {
                return node.left;
            }
            
            // Node with two children
            // Get the inorder successor (smallest in the right subtree)
            node.val = this._minValue(node.right);
            
            // Delete the inorder successor
            node.right = this._deleteRecursive(node.right, node.val);
        }
        
        return node;
    }
    
    _minValue(node) {
        let current = node;
        while (current.left) {
            current = current.left;
        }
        return current.val;
    }
}

// Example usage
const bst = new BST();
bst.insert(8);
bst.insert(3);
bst.insert(10);
bst.insert(1);
bst.insert(14);

// Search for a value
const result = bst.search(10);
console.log(result ? result.val : "Not found");  // Output: 10

// Delete a value
bst.delete(3);
```

## Tree Traversal Algorithms

### 1. Depth-First Search (DFS)

#### Inorder Traversal (Left, Root, Right)

**Python:**
```python
def inorder_traversal(root):
    result = []
    
    def inorder(node):
        if node:
            inorder(node.left)
            result.append(node.val)
            inorder(node.right)
    
    inorder(root)
    return result

# Example usage
# For the binary tree created above
print(inorder_traversal(root))  # Output: [4, 2, 5, 1, 3]
```

**JavaScript:**
```javascript
function inorderTraversal(root) {
    const result = [];
    
    function inorder(node) {
        if (node) {
            inorder(node.left);
            result.push(node.val);
            inorder(node.right);
        }
    }
    
    inorder(root);
    return result;
}

// Example usage
// For the binary tree created above
console.log(inorderTraversal(root));  // Output: [4, 2, 5, 1, 3]
```

#### Preorder Traversal (Root, Left, Right)

**Python:**
```python
def preorder_traversal(root):
    result = []
    
    def preorder(node):
        if node:
            result.append(node.val)
            preorder(node.left)
            preorder(node.right)
    
    preorder(root)
    return result

# Example usage
print(preorder_traversal(root))  # Output: [1, 2, 4, 5, 3]
```

**JavaScript:**
```javascript
function preorderTraversal(root) {
    const result = [];
    
    function preorder(node) {
        if (node) {
            result.push(node.val);
            preorder(node.left);
            preorder(node.right);
        }
    }
    
    preorder(root);
    return result;
}

// Example usage
console.log(preorderTraversal(root));  // Output: [1, 2, 4, 5, 3]
```

#### Postorder Traversal (Left, Right, Root)

**Python:**
```python
def postorder_traversal(root):
    result = []
    
    def postorder(node):
        if node:
            postorder(node.left)
            postorder(node.right)
            result.append(node.val)
    
    postorder(root)
    return result

# Example usage
print(postorder_traversal(root))  # Output: [4, 5, 2, 3, 1]
```

**JavaScript:**
```javascript
function postorderTraversal(root) {
    const result = [];
    
    function postorder(node) {
        if (node) {
            postorder(node.left);
            postorder(node.right);
            result.push(node.val);
        }
    }
    
    postorder(root);
    return result;
}

// Example usage
console.log(postorderTraversal(root));  // Output: [4, 5, 2, 3, 1]
```

### 2. Breadth-First Search (BFS) / Level Order Traversal

**Python:**
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

# Example usage
print(level_order_traversal(root))  # Output: [[1], [2, 3], [4, 5]]
```

**JavaScript:**
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

// Example usage
console.log(levelOrderTraversal(root));  // Output: [[1], [2, 3], [4, 5]]
```

## Common Tree Algorithms

### 1. Maximum Depth of Binary Tree

**Python:**
```python
def max_depth(root):
    if not root:
        return 0
    
    left_depth = max_depth(root.left)
    right_depth = max_depth(root.right)
    
    return max(left_depth, right_depth) + 1

# Example usage
print(max_depth(root))  # Output: 3
```

**JavaScript:**
```javascript
function maxDepth(root) {
    if (!root) {
        return 0;
    }
    
    const leftDepth = maxDepth(root.left);
    const rightDepth = maxDepth(root.right);
    
    return Math.max(leftDepth, rightDepth) + 1;
}

// Example usage
console.log(maxDepth(root));  // Output: 3
```

### 2. Check if a Binary Tree is Balanced

**Python:**
```python
def is_balanced(root):
    def height(node):
        if not node:
            return 0
        
        left_height = height(node.left)
        if left_height == -1:
            return -1
        
        right_height = height(node.right)
        if right_height == -1:
            return -1
        
        if abs(left_height - right_height) > 1:
            return -1
        
        return max(left_height, right_height) + 1
    
    return height(root) != -1

# Example usage
print(is_balanced(root))  # Output: True
```

**JavaScript:**
```javascript
function isBalanced(root) {
    function height(node) {
        if (!node) {
            return 0;
        }
        
        const leftHeight = height(node.left);
        if (leftHeight === -1) {
            return -1;
        }
        
        const rightHeight = height(node.right);
        if (rightHeight === -1) {
            return -1;
        }
        
        if (Math.abs(leftHeight - rightHeight) > 1) {
            return -1;
        }
        
        return Math.max(leftHeight, rightHeight) + 1;
    }
    
    return height(root) !== -1;
}

// Example usage
console.log(isBalanced(root));  // Output: true
```

### 3. Lowest Common Ancestor (LCA) of a Binary Tree

**Python:**
```python
def lowest_common_ancestor(root, p, q):
    if not root or root == p or root == q:
        return root
    
    left = lowest_common_ancestor(root.left, p, q)
    right = lowest_common_ancestor(root.right, p, q)
    
    if left and right:
        return root
    
    return left if left else right

# Example usage
# Assuming p and q are nodes in the tree
p = root.left.left  # Node with value 4
q = root.right      # Node with value 3
lca = lowest_common_ancestor(root, p, q)
print(lca.val)  # Output: 1 (the root)
```

**JavaScript:**
```javascript
function lowestCommonAncestor(root, p, q) {
    if (!root || root === p || root === q) {
        return root;
    }
    
    const left = lowestCommonAncestor(root.left, p, q);
    const right = lowestCommonAncestor(root.right, p, q);
    
    if (left && right) {
        return root;
    }
    
    return left ? left : right;
}

// Example usage
// Assuming p and q are nodes in the tree
const p = root.left.left;  // Node with value 4
const q = root.right;      // Node with value 3
const lca = lowestCommonAncestor(root, p, q);
console.log(lca.val);  // Output: 1 (the root)
```

## Time and Space Complexity

| Operation | Binary Search Tree (Average) | Binary Search Tree (Worst) | Balanced BST (AVL, Red-Black) |
|-----------|------------------------------|----------------------------|-------------------------------|
| Search    | O(log n)                     | O(n)                       | O(log n)                      |
| Insert    | O(log n)                     | O(n)                       | O(log n)                      |
| Delete    | O(log n)                     | O(n)                       | O(log n)                      |
| Traversal | O(n)                         | O(n)                       | O(n)                          |

Space Complexity: O(h) for recursive implementations, where h is the height of the tree. In the worst case, h can be n for a skewed tree, but for a balanced tree, h is log n.

## Advantages of Trees

1. **Hierarchical Structure**: Trees naturally represent hierarchical relationships.
2. **Efficient Search**: Balanced trees provide O(log n) search time.
3. **Flexible Size**: Trees can grow or shrink dynamically.
4. **Fast Insertions/Deletions**: Balanced trees provide O(log n) insertion and deletion time.
5. **Ordered Storage**: BSTs maintain elements in a sorted order.

## Disadvantages of Trees

1. **Complex Implementation**: Trees are more complex to implement than arrays or linked lists.
2. **Balancing Overhead**: Self-balancing trees require additional operations to maintain balance.
3. **Memory Usage**: Trees require more memory due to pointers.
4. **Traversal Complexity**: Traversing a tree is more complex than traversing a linear data structure.

## When to Use Trees

- When you need to represent hierarchical relationships
- When you need efficient search, insertion, and deletion operations
- When you need to maintain elements in a sorted order
- When you need to perform range queries
- When you need to implement a dictionary or a map

## Common Tree Problems from Blind 75 and Grind 75

1. **Maximum Depth of Binary Tree** (Easy): Find the maximum depth of a binary tree.
2. **Same Tree** (Easy): Check if two binary trees are identical.
3. **Invert/Flip Binary Tree** (Easy): Invert a binary tree (mirror it).
4. **Binary Tree Level Order Traversal** (Medium): Return the level order traversal of a binary tree.
5. **Validate Binary Search Tree** (Medium): Check if a binary tree is a valid BST.
6. **Lowest Common Ancestor of a Binary Tree** (Medium): Find the lowest common ancestor of two nodes.
7. **Binary Tree Maximum Path Sum** (Hard): Find the maximum path sum in a binary tree.
8. **Serialize and Deserialize Binary Tree** (Hard): Design an algorithm to serialize and deserialize a binary tree.

## How to Identify Tree Problems

Look for these clues in the problem statement:

1. The problem involves a hierarchical structure.
2. The problem mentions nodes, parents, children, or ancestors.
3. The problem requires traversing a tree in a specific order.
4. The problem involves finding paths or distances in a tree.
5. The problem requires maintaining a sorted order of elements.
6. Keywords like "tree," "binary tree," "BST," "root," "leaf," or "node." 