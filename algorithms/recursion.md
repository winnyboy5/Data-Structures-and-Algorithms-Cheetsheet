# Recursion

Recursion is a programming technique where a function calls itself to solve a problem. It's a powerful approach for solving problems that can be broken down into smaller, similar subproblems.

## Table of Contents
- [Understanding Recursion](#understanding-recursion)
- [Recursion vs. Iteration](#recursion-vs-iteration)
- [Recursion Patterns](#recursion-patterns)
- [Common Recursive Problems](#common-recursive-problems)
- [Tail Recursion](#tail-recursion)
- [Memoization and Dynamic Programming](#memoization-and-dynamic-programming)
- [Related Blind 75 & Grind 75 Problems](#related-blind-75--grind-75-problems)
- [Tips and Tricks](#tips-and-tricks)

## Understanding Recursion

### Key Components of Recursion
1. **Base Case**: The condition that stops the recursion
2. **Recursive Case**: The part where the function calls itself with a modified input

### Visual Explanation

Let's visualize recursion with a simple factorial function:

```
factorial(4) = 4 * factorial(3)
                    |
                    +--> 3 * factorial(2)
                              |
                              +--> 2 * factorial(1)
                                        |
                                        +--> 1 * factorial(0)
                                                  |
                                                  +--> 1 (base case)
                                        1 * 1 = 1
                              2 * 1 = 2
                    3 * 2 = 6
          4 * 6 = 24
```

### Implementation

#### Python
```python
def factorial(n):
    # Base case
    if n == 0:
        return 1
    
    # Recursive case
    return n * factorial(n - 1)
```

#### JavaScript
```javascript
function factorial(n) {
    // Base case
    if (n === 0) {
        return 1;
    }
    
    // Recursive case
    return n * factorial(n - 1);
}
```

## Recursion vs. Iteration

### Comparison

| Aspect | Recursion | Iteration |
|--------|-----------|-----------|
| Implementation | Often simpler and more elegant | Can be more complex |
| Memory Usage | Uses call stack (more memory) | Typically uses less memory |
| Performance | Can be slower due to function call overhead | Usually faster |
| Infinite Loop Risk | Stack overflow if base case is missing | Infinite loop if termination condition is wrong |
| Problem Suitability | Tree traversals, divide and conquer algorithms | Simple repetitive tasks |

### Example: Factorial with Iteration

#### Python
```python
def factorial_iterative(n):
    result = 1
    for i in range(1, n + 1):
        result *= i
    return result
```

#### JavaScript
```javascript
function factorialIterative(n) {
    let result = 1;
    for (let i = 1; i <= n; i++) {
        result *= i;
    }
    return result;
}
```

## Recursion Patterns

### 1. Linear Recursion
A function calls itself once in each recursive step.

#### Example: Sum of Array Elements

```python
def sum_array(arr):
    # Base case
    if len(arr) == 0:
        return 0
    
    # Recursive case
    return arr[0] + sum_array(arr[1:])
```

### 2. Binary Recursion
A function calls itself twice in each recursive step.

#### Example: Fibonacci Sequence

```python
def fibonacci(n):
    # Base cases
    if n <= 1:
        return n
    
    # Recursive case (binary recursion)
    return fibonacci(n - 1) + fibonacci(n - 2)
```

### 3. Tail Recursion
The recursive call is the last operation in the function.

#### Example: Factorial with Tail Recursion

```python
def factorial_tail(n, accumulator=1):
    # Base case
    if n == 0:
        return accumulator
    
    # Recursive case (tail recursion)
    return factorial_tail(n - 1, n * accumulator)
```

### 4. Mutual Recursion
Two or more functions call each other.

#### Example: Even and Odd Checker

```python
def is_even(n):
    if n == 0:
        return True
    return is_odd(n - 1)

def is_odd(n):
    if n == 0:
        return False
    return is_even(n - 1)
```

### 5. Nested Recursion
A function passes the result of a recursive call as the parameter for another recursive call.

#### Example: Ackermann Function

```python
def ackermann(m, n):
    if m == 0:
        return n + 1
    if n == 0:
        return ackermann(m - 1, 1)
    return ackermann(m - 1, ackermann(m, n - 1))
```

## Common Recursive Problems

### 1. Tower of Hanoi

#### Visual Explanation
```
Moving 3 disks from A to C using B as auxiliary:

Initial: A[3,2,1], B[], C[]
Step 1: Move disk 1 from A to C: A[3,2], B[], C[1]
Step 2: Move disk 2 from A to B: A[3], B[2], C[1]
Step 3: Move disk 1 from C to B: A[3], B[2,1], C[]
Step 4: Move disk 3 from A to C: A[], B[2,1], C[3]
Step 5: Move disk 1 from B to A: A[1], B[2], C[3]
Step 6: Move disk 2 from B to C: A[1], B[], C[3,2]
Step 7: Move disk 1 from A to C: A[], B[], C[3,2,1]
```

#### Implementation

```python
def tower_of_hanoi(n, source, auxiliary, target):
    if n == 1:
        print(f"Move disk 1 from {source} to {target}")
        return
    
    tower_of_hanoi(n - 1, source, target, auxiliary)
    print(f"Move disk {n} from {source} to {target}")
    tower_of_hanoi(n - 1, auxiliary, source, target)
```

### 2. Binary Tree Traversal

#### Visual Explanation
```
Binary Tree:
      1
     / \
    2   3
   / \
  4   5

Preorder (Root, Left, Right): 1, 2, 4, 5, 3
Inorder (Left, Root, Right): 4, 2, 5, 1, 3
Postorder (Left, Right, Root): 4, 5, 2, 3, 1
```

#### Implementation

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def preorder_traversal(root):
    if not root:
        return []
    
    result = [root.val]  # Visit root
    result.extend(preorder_traversal(root.left))  # Visit left subtree
    result.extend(preorder_traversal(root.right))  # Visit right subtree
    
    return result

def inorder_traversal(root):
    if not root:
        return []
    
    result = inorder_traversal(root.left)  # Visit left subtree
    result.append(root.val)  # Visit root
    result.extend(inorder_traversal(root.right))  # Visit right subtree
    
    return result

def postorder_traversal(root):
    if not root:
        return []
    
    result = postorder_traversal(root.left)  # Visit left subtree
    result.extend(postorder_traversal(root.right))  # Visit right subtree
    result.append(root.val)  # Visit root
    
    return result
```

### 3. Merge Sort

#### Visual Explanation
```
Original array: [38, 27, 43, 3, 9, 82, 10]

Divide:
[38, 27, 43, 3, 9, 82, 10]
[38, 27, 43, 3] [9, 82, 10]
[38, 27] [43, 3] [9, 82] [10]
[38] [27] [43] [3] [9] [82] [10]

Conquer (merge):
[27, 38] [3, 43] [9, 82] [10]
[3, 27, 38, 43] [9, 10, 82]
[3, 9, 10, 27, 38, 43, 82]
```

#### Implementation

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    
    # Divide
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    
    # Conquer (merge)
    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0
    
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    
    result.extend(left[i:])
    result.extend(right[j:])
    
    return result
```

### 4. Permutations

#### Visual Explanation
```
Generate all permutations of "ABC":

Start with: []
Choose A: [A]
Choose B: [A,B], [B,A]
Choose C: [A,B,C], [A,C,B], [B,A,C], [B,C,A], [C,A,B], [C,B,A]
```

#### Implementation

```python
def generate_permutations(elements):
    if len(elements) <= 1:
        return [elements]
    
    result = []
    for i in range(len(elements)):
        # Choose current element
        current = elements[i]
        # Generate permutations of remaining elements
        remaining_elements = elements[:i] + elements[i+1:]
        remaining_permutations = generate_permutations(remaining_elements)
        
        # Add current element to each permutation of remaining elements
        for perm in remaining_permutations:
            result.append([current] + perm)
    
    return result
```

### 5. Subset Generation (Power Set)

#### Visual Explanation
```
Generate all subsets of {1, 2, 3}:

Start with: []
Include/exclude 1: [], [1]
Include/exclude 2: [], [1], [2], [1,2]
Include/exclude 3: [], [1], [2], [1,2], [3], [1,3], [2,3], [1,2,3]
```

#### Implementation

```python
def generate_subsets(elements):
    if not elements:
        return [[]]
    
    # Take the first element
    first = elements[0]
    # Generate all subsets without the first element
    subsets_without_first = generate_subsets(elements[1:])
    
    # Add the first element to each subset to create new subsets
    subsets_with_first = []
    for subset in subsets_without_first:
        subsets_with_first.append([first] + subset)
    
    # Combine both types of subsets
    return subsets_without_first + subsets_with_first
```

## Tail Recursion

Tail recursion is a special form of recursion where the recursive call is the last operation in the function. This allows compilers to optimize the recursion into iteration, avoiding stack overflow for deep recursions.

### Example: Factorial with Tail Recursion

#### Python
```python
def factorial_tail(n, accumulator=1):
    if n == 0:
        return accumulator
    return factorial_tail(n - 1, n * accumulator)
```

#### JavaScript
```javascript
function factorialTail(n, accumulator = 1) {
    if (n === 0) {
        return accumulator;
    }
    return factorialTail(n - 1, n * accumulator);
}
```

### Benefits of Tail Recursion
- Prevents stack overflow for deep recursions (in languages that optimize tail calls)
- Can be as efficient as iteration
- Maintains the clarity and elegance of recursive solutions

### Languages with Tail Call Optimization
- Scheme
- Haskell
- Scala
- JavaScript (in strict mode, ES6+)
- Swift

Note: Python does not optimize tail recursion by design.

## Memoization and Dynamic Programming

Memoization is a technique to optimize recursive functions by caching previously computed results.

### Example: Fibonacci with Memoization

#### Python
```python
def fibonacci_memo(n, memo={}):
    if n in memo:
        return memo[n]
    
    if n <= 1:
        return n
    
    memo[n] = fibonacci_memo(n - 1, memo) + fibonacci_memo(n - 2, memo)
    return memo[n]
```

#### JavaScript
```javascript
function fibonacciMemo(n, memo = {}) {
    if (n in memo) {
        return memo[n];
    }
    
    if (n <= 1) {
        return n;
    }
    
    memo[n] = fibonacciMemo(n - 1, memo) + fibonacciMemo(n - 2, memo);
    return memo[n];
}
```

### Time Complexity Improvement
- Without memoization: O(2^n)
- With memoization: O(n)

## Related Blind 75 & Grind 75 Problems

1. **Merge k Sorted Lists** (Blind 75 #23)
   - Problem: Merge k sorted linked lists into one sorted linked list.
   - Solution: Use a divide-and-conquer approach with recursion.
   - [LeetCode #23](https://leetcode.com/problems/merge-k-sorted-lists/)

2. **Validate Binary Search Tree** (Blind 75 #98)
   - Problem: Determine if a binary tree is a valid binary search tree.
   - Solution: Use recursive traversal with range validation.
   - [LeetCode #98](https://leetcode.com/problems/validate-binary-search-tree/)

3. **Combination Sum** (Blind 75 #39)
   - Problem: Find all unique combinations of candidates that sum to a target.
   - Solution: Use backtracking with recursion.
   - [LeetCode #39](https://leetcode.com/problems/combination-sum/)

4. **Word Search** (Blind 75 #79)
   - Problem: Determine if a word exists in a 2D board of characters.
   - Solution: Use recursive DFS with backtracking.
   - [LeetCode #79](https://leetcode.com/problems/word-search/)

5. **Permutations** (Grind 75)
   - Problem: Generate all possible permutations of an array of distinct integers.
   - Solution: Use recursive backtracking.
   - [LeetCode #46](https://leetcode.com/problems/permutations/)

6. **Subsets** (Grind 75)
   - Problem: Generate all possible subsets of a set of distinct integers.
   - Solution: Use recursive inclusion/exclusion.
   - [LeetCode #78](https://leetcode.com/problems/subsets/)

## Tips and Tricks

1. **Identifying Recursive Problems**:
   - Problems that can be broken down into smaller, similar subproblems
   - Problems involving trees, graphs, or divide-and-conquer approaches
   - Problems that ask for all possible combinations or arrangements

2. **Designing Recursive Solutions**:
   - Always define a clear base case to prevent infinite recursion
   - Ensure the recursive case makes progress toward the base case
   - Think about what information needs to be passed to each recursive call
   - Consider using helper functions with additional parameters for tracking state

3. **Optimizing Recursive Solutions**:
   - Use memoization to avoid redundant calculations
   - Convert to tail recursion when possible
   - Consider iterative solutions for performance-critical code
   - Be mindful of the call stack depth (especially in languages without tail call optimization)

4. **Common Pitfalls**:
   - Missing base case leading to stack overflow
   - Inefficient recursive structure causing exponential time complexity
   - Not properly handling edge cases
   - Excessive memory usage due to large call stacks

5. **Debugging Recursive Functions**:
   - Trace through small examples by hand
   - Add print statements to visualize the recursion tree
   - Check base cases first
   - Verify that each recursive call makes progress toward the base case 