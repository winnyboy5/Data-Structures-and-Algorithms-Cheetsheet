# Backtracking Pattern

Backtracking is an algorithmic technique for solving problems recursively by trying to build a solution incrementally, one piece at a time, removing those solutions that fail to satisfy the constraints of the problem at any point in time (by backtracking).

## Visual Representation

```
Decision Tree for generating permutations of [1, 2, 3]:

                        []
                /       |       \
              [1]      [2]      [3]
            /   \     /   \     /   \
        [1,2] [1,3] [2,1] [2,3] [3,1] [3,2]
          |     |     |     |     |     |
      [1,2,3] [1,3,2] [2,1,3] [2,3,1] [3,1,2] [3,2,1]
```

## When to Use Backtracking

- When you need to find all (or some) solutions to a problem
- When you need to explore all possible combinations or permutations
- When the problem can be broken down into a sequence of decisions
- When you need to satisfy multiple constraints
- When a brute force approach would be too inefficient

## Basic Backtracking Implementation

### Python Implementation (N-Queens Problem)

```python
def solve_n_queens(n):
    result = []
    
    def is_valid(board, row, col):
        # Check if there is a queen in the same column
        for i in range(row):
            if board[i] == col:
                return False
            
            # Check if there is a queen in the diagonal
            if abs(board[i] - col) == abs(i - row):
                return False
        
        return True
    
    def backtrack(board, row):
        if row == n:
            # Add the solution to the result
            solution = ['.' * col + 'Q' + '.' * (n - col - 1) for col in board]
            result.append(solution)
            return
        
        for col in range(n):
            if is_valid(board, row, col):
                board[row] = col
                backtrack(board, row + 1)
                # Backtrack: remove the queen from the current position
                board[row] = -1
    
    # Initialize the board with -1 (no queen placed)
    board = [-1] * n
    backtrack(board, 0)
    
    return result

# Example usage
n = 4
solutions = solve_n_queens(n)
for solution in solutions:
    for row in solution:
        print(row)
    print()
```

### JavaScript Implementation (N-Queens Problem)

```javascript
function solveNQueens(n) {
    const result = [];
    
    function isValid(board, row, col) {
        // Check if there is a queen in the same column
        for (let i = 0; i < row; i++) {
            if (board[i] === col) {
                return false;
            }
            
            // Check if there is a queen in the diagonal
            if (Math.abs(board[i] - col) === Math.abs(i - row)) {
                return false;
            }
        }
        
        return true;
    }
    
    function backtrack(board, row) {
        if (row === n) {
            // Add the solution to the result
            const solution = board.map(col => '.'.repeat(col) + 'Q' + '.'.repeat(n - col - 1));
            result.push(solution);
            return;
        }
        
        for (let col = 0; col < n; col++) {
            if (isValid(board, row, col)) {
                board[row] = col;
                backtrack(board, row + 1);
                // Backtrack: remove the queen from the current position
                board[row] = -1;
            }
        }
    }
    
    // Initialize the board with -1 (no queen placed)
    const board = Array(n).fill(-1);
    backtrack(board, 0);
    
    return result;
}

// Example usage
const n = 4;
const solutions = solveNQueens(n);
for (const solution of solutions) {
    for (const row of solution) {
        console.log(row);
    }
    console.log();
}
```

## Common Problems and Solutions

### 1. Permutations

**Problem:** Given an array of distinct integers, return all possible permutations.

**Python Solution:**
```python
def permute(nums):
    result = []
    
    def backtrack(current, remaining):
        # If we've used all numbers, add the current permutation to the result
        if not remaining:
            result.append(current[:])
            return
        
        for i in range(len(remaining)):
            # Add the current number to the permutation
            current.append(remaining[i])
            
            # Recursively generate permutations with the remaining numbers
            backtrack(current, remaining[:i] + remaining[i+1:])
            
            # Backtrack: remove the current number
            current.pop()
    
    backtrack([], nums)
    return result

# Example usage
nums = [1, 2, 3]
print(permute(nums))
# Output: [[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1]]
```

**JavaScript Solution:**
```javascript
function permute(nums) {
    const result = [];
    
    function backtrack(current, remaining) {
        // If we've used all numbers, add the current permutation to the result
        if (remaining.length === 0) {
            result.push([...current]);
            return;
        }
        
        for (let i = 0; i < remaining.length; i++) {
            // Add the current number to the permutation
            current.push(remaining[i]);
            
            // Recursively generate permutations with the remaining numbers
            const newRemaining = [...remaining.slice(0, i), ...remaining.slice(i + 1)];
            backtrack(current, newRemaining);
            
            // Backtrack: remove the current number
            current.pop();
        }
    }
    
    backtrack([], nums);
    return result;
}

// Example usage
const nums = [1, 2, 3];
console.log(permute(nums));
// Output: [[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1]]
```

**Time Complexity:** O(n!) where n is the length of the input array  
**Space Complexity:** O(n!) for storing all permutations

### 2. Combination Sum

**Problem:** Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations of candidates where the chosen numbers sum to target. You may return the combinations in any order.

**Python Solution:**
```python
def combination_sum(candidates, target):
    result = []
    
    def backtrack(start, current, remaining):
        if remaining == 0:
            result.append(current[:])
            return
        
        if remaining < 0:
            return
        
        for i in range(start, len(candidates)):
            current.append(candidates[i])
            backtrack(i, current, remaining - candidates[i])
            current.pop()
    
    backtrack(0, [], target)
    return result

# Example usage
candidates = [2, 3, 6, 7]
target = 7
print(combination_sum(candidates, target))
# Output: [[2, 2, 3], [7]]
```

**JavaScript Solution:**
```javascript
function combinationSum(candidates, target) {
    const result = [];
    
    function backtrack(start, current, remaining) {
        if (remaining === 0) {
            result.push([...current]);
            return;
        }
        
        if (remaining < 0) {
            return;
        }
        
        for (let i = start; i < candidates.length; i++) {
            current.push(candidates[i]);
            backtrack(i, current, remaining - candidates[i]);
            current.pop();
        }
    }
    
    backtrack(0, [], target);
    return result;
}

// Example usage
const candidates = [2, 3, 6, 7];
const target = 7;
console.log(combinationSum(candidates, target));
// Output: [[2, 2, 3], [7]]
```

**Time Complexity:** O(2^n) where n is the length of the candidates array  
**Space Complexity:** O(target/min) where min is the minimum value in candidates

### 3. Sudoku Solver

**Problem:** Write a program to solve a Sudoku puzzle by filling the empty cells.

**Python Solution:**
```python
def solve_sudoku(board):
    def is_valid(row, col, num):
        # Check row
        for x in range(9):
            if board[row][x] == num:
                return False
        
        # Check column
        for x in range(9):
            if board[x][col] == num:
                return False
        
        # Check 3x3 box
        start_row, start_col = 3 * (row // 3), 3 * (col // 3)
        for i in range(3):
            for j in range(3):
                if board[i + start_row][j + start_col] == num:
                    return False
        
        return True
    
    def backtrack():
        # Find an empty cell
        for row in range(9):
            for col in range(9):
                if board[row][col] == '.':
                    # Try each number from 1 to 9
                    for num in map(str, range(1, 10)):
                        if is_valid(row, col, num):
                            # Place the number
                            board[row][col] = num
                            
                            # Recursively solve the rest of the puzzle
                            if backtrack():
                                return True
                            
                            # Backtrack: remove the number
                            board[row][col] = '.'
                    
                    # If no number works, return False
                    return False
        
        # If all cells are filled, the puzzle is solved
        return True
    
    backtrack()
    return board

# Example usage
board = [
    ["5","3",".",".","7",".",".",".","."],
    ["6",".",".","1","9","5",".",".","."],
    [".","9","8",".",".",".",".","6","."],
    ["8",".",".",".","6",".",".",".","3"],
    ["4",".",".","8",".","3",".",".","1"],
    ["7",".",".",".","2",".",".",".","6"],
    [".","6",".",".",".",".","2","8","."],
    [".",".",".","4","1","9",".",".","5"],
    [".",".",".",".","8",".",".","7","9"]
]
solve_sudoku(board)
for row in board:
    print(row)
```

**JavaScript Solution:**
```javascript
function solveSudoku(board) {
    function isValid(row, col, num) {
        // Check row
        for (let x = 0; x < 9; x++) {
            if (board[row][x] === num) {
                return false;
            }
        }
        
        // Check column
        for (let x = 0; x < 9; x++) {
            if (board[x][col] === num) {
                return false;
            }
        }
        
        // Check 3x3 box
        const startRow = 3 * Math.floor(row / 3);
        const startCol = 3 * Math.floor(col / 3);
        for (let i = 0; i < 3; i++) {
            for (let j = 0; j < 3; j++) {
                if (board[i + startRow][j + startCol] === num) {
                    return false;
                }
            }
        }
        
        return true;
    }
    
    function backtrack() {
        // Find an empty cell
        for (let row = 0; row < 9; row++) {
            for (let col = 0; col < 9; col++) {
                if (board[row][col] === '.') {
                    // Try each number from 1 to 9
                    for (let num = 1; num <= 9; num++) {
                        const numStr = num.toString();
                        if (isValid(row, col, numStr)) {
                            // Place the number
                            board[row][col] = numStr;
                            
                            // Recursively solve the rest of the puzzle
                            if (backtrack()) {
                                return true;
                            }
                            
                            // Backtrack: remove the number
                            board[row][col] = '.';
                        }
                    }
                    
                    // If no number works, return false
                    return false;
                }
            }
        }
        
        // If all cells are filled, the puzzle is solved
        return true;
    }
    
    backtrack();
    return board;
}

// Example usage
const board = [
    ["5","3",".",".","7",".",".",".","."],
    ["6",".",".","1","9","5",".",".","."],
    [".","9","8",".",".",".",".","6","."],
    ["8",".",".",".","6",".",".",".","3"],
    ["4",".",".","8",".","3",".",".","1"],
    ["7",".",".",".","2",".",".",".","6"],
    [".","6",".",".",".",".","2","8","."],
    [".",".",".","4","1","9",".",".","5"],
    [".",".",".",".","8",".",".","7","9"]
];
solveSudoku(board);
for (const row of board) {
    console.log(row);
}
```

**Time Complexity:** O(9^(n*n)) where n is the size of the board (9 for standard Sudoku)  
**Space Complexity:** O(n*n) for the recursion stack

### 4. Word Search

**Problem:** Given a 2D board and a word, find if the word exists in the grid. The word can be constructed from letters of sequentially adjacent cells, where "adjacent" cells are horizontally or vertically neighboring.

**Python Solution:**
```python
def exist(board, word):
    if not board or not board[0]:
        return False
    
    rows, cols = len(board), len(board[0])
    
    def backtrack(row, col, index):
        # If we've matched all characters, return True
        if index == len(word):
            return True
        
        # Check if current position is out of bounds or doesn't match the current character
        if (row < 0 or row >= rows or col < 0 or col >= cols or
            board[row][col] != word[index]):
            return False
        
        # Mark the current cell as visited
        temp = board[row][col]
        board[row][col] = '#'
        
        # Try all four directions
        result = (backtrack(row + 1, col, index + 1) or
                  backtrack(row - 1, col, index + 1) or
                  backtrack(row, col + 1, index + 1) or
                  backtrack(row, col - 1, index + 1))
        
        # Restore the cell
        board[row][col] = temp
        
        return result
    
    # Try starting from each cell
    for row in range(rows):
        for col in range(cols):
            if backtrack(row, col, 0):
                return True
    
    return False

# Example usage
board = [
    ['A', 'B', 'C', 'E'],
    ['S', 'F', 'C', 'S'],
    ['A', 'D', 'E', 'E']
]
word = "ABCCED"
print(exist(board, word))  # Output: True
```

**JavaScript Solution:**
```javascript
function exist(board, word) {
    if (!board || !board[0]) {
        return false;
    }
    
    const rows = board.length;
    const cols = board[0].length;
    
    function backtrack(row, col, index) {
        // If we've matched all characters, return true
        if (index === word.length) {
            return true;
        }
        
        // Check if current position is out of bounds or doesn't match the current character
        if (row < 0 || row >= rows || col < 0 || col >= cols ||
            board[row][col] !== word[index]) {
            return false;
        }
        
        // Mark the current cell as visited
        const temp = board[row][col];
        board[row][col] = '#';
        
        // Try all four directions
        const result = backtrack(row + 1, col, index + 1) ||
                       backtrack(row - 1, col, index + 1) ||
                       backtrack(row, col + 1, index + 1) ||
                       backtrack(row, col - 1, index + 1);
        
        // Restore the cell
        board[row][col] = temp;
        
        return result;
    }
    
    // Try starting from each cell
    for (let row = 0; row < rows; row++) {
        for (let col = 0; col < cols; col++) {
            if (backtrack(row, col, 0)) {
                return true;
            }
        }
    }
    
    return false;
}

// Example usage
const board = [
    ['A', 'B', 'C', 'E'],
    ['S', 'F', 'C', 'S'],
    ['A', 'D', 'E', 'E']
];
const word = "ABCCED";
console.log(exist(board, word));  // Output: true
```

**Time Complexity:** O(N * 4^L) where N is the number of cells in the board and L is the length of the word  
**Space Complexity:** O(L) for the recursion stack

### 5. Palindrome Partitioning

**Problem:** Given a string s, partition s such that every substring of the partition is a palindrome. Return all possible palindrome partitioning of s.

**Python Solution:**
```python
def partition(s):
    result = []
    
    def is_palindrome(start, end):
        while start < end:
            if s[start] != s[end]:
                return False
            start += 1
            end -= 1
        return True
    
    def backtrack(start, current):
        if start == len(s):
            result.append(current[:])
            return
        
        for end in range(start, len(s)):
            if is_palindrome(start, end):
                current.append(s[start:end + 1])
                backtrack(end + 1, current)
                current.pop()
    
    backtrack(0, [])
    return result

# Example usage
s = "aab"
print(partition(s))  # Output: [["a", "a", "b"], ["aa", "b"]]
```

**JavaScript Solution:**
```javascript
function partition(s) {
    const result = [];
    
    function isPalindrome(start, end) {
        while (start < end) {
            if (s[start] !== s[end]) {
                return false;
            }
            start++;
            end--;
        }
        return true;
    }
    
    function backtrack(start, current) {
        if (start === s.length) {
            result.push([...current]);
            return;
        }
        
        for (let end = start; end < s.length; end++) {
            if (isPalindrome(start, end)) {
                current.push(s.substring(start, end + 1));
                backtrack(end + 1, current);
                current.pop();
            }
        }
    }
    
    backtrack(0, []);
    return result;
}

// Example usage
const s = "aab";
console.log(partition(s));  // Output: [["a", "a", "b"], ["aa", "b"]]
```

**Time Complexity:** O(N * 2^N) where N is the length of the string  
**Space Complexity:** O(N) for the recursion stack

## Time and Space Complexity

| Algorithm | Time Complexity | Space Complexity |
|-----------|----------------|-----------------|
| N-Queens | O(N!) | O(N) |
| Permutations | O(N!) | O(N!) |
| Combination Sum | O(2^N) | O(target/min) |
| Sudoku Solver | O(9^(N*N)) | O(N*N) |
| Word Search | O(N * 4^L) | O(L) |
| Palindrome Partitioning | O(N * 2^N) | O(N) |

## Tips and Tricks for Backtracking

1. **Define the State**: Clearly define what constitutes a state in your problem (e.g., the current combination, the current position in a grid).

2. **Define the Constraints**: Identify the constraints that a valid solution must satisfy.

3. **Pruning**: Eliminate branches of the search tree that cannot lead to a valid solution as early as possible.

4. **Base Case**: Define a clear base case for your recursive function (e.g., when you've found a complete solution).

5. **Avoid Redundant Work**: Use techniques like memoization to avoid solving the same subproblems multiple times.

6. **Marking and Unmarking**: When exploring a state, mark it as visited, and when backtracking, unmark it.

7. **Optimization**: Consider sorting the input or using other preprocessing steps to optimize the backtracking process.

## Common Pitfalls

1. **Infinite Recursion**: Ensure that your recursive function makes progress towards the base case.

2. **Modifying the Input**: Be careful when modifying the input during backtracking, and make sure to restore it when backtracking.

3. **Duplicate Solutions**: When generating combinations or permutations, be careful to avoid duplicate solutions.

4. **Memory Overflow**: For problems with a large search space, the recursion stack can grow very large, potentially causing a stack overflow.

5. **Not Backtracking Properly**: Make sure to undo all changes made during the exploration of a state when backtracking.

## How to Identify Backtracking Problems

Look for these clues in the problem statement:

1. The problem asks for all possible solutions, combinations, or arrangements.
2. The problem involves making a sequence of decisions or choices.
3. The problem has constraints that must be satisfied.
4. The problem can be solved by trying different options and undoing them if they don't work.
5. Keywords like "all possible," "combinations," "arrangements," or "constraints."

## Common Backtracking Problems from Blind 75 and Grind 75

1. **N-Queens** (Hard): Place N queens on an N×N chessboard such that no two queens attack each other.
2. **Permutations** (Medium): Generate all possible permutations of a set of distinct integers.
3. **Combination Sum** (Medium): Find all unique combinations of candidates that sum to a target.
4. **Word Search** (Medium): Find if a word exists in a grid of letters.
5. **Palindrome Partitioning** (Medium): Partition a string such that every substring is a palindrome.
6. **Letter Combinations of a Phone Number** (Medium): Return all possible letter combinations that the number could represent.
7. **Generate Parentheses** (Medium): Generate all combinations of well-formed parentheses.
8. **Sudoku Solver** (Hard): Solve a Sudoku puzzle.
9. **Subsets** (Medium): Generate all possible subsets of a set of distinct integers.
10. **Word Break II** (Hard): Find all possible ways to break a string into a space-separated sequence of dictionary words.

## Backtracking Template

Here's a general template for solving backtracking problems:

```python
def backtracking_template(input_data):
    result = []
    
    def backtrack(state, choices):
        # Base case: if we've found a valid solution
        if is_solution(state):
            result.append(state.copy())
            return
        
        # Explore all possible choices
        for choice in choices:
            # Check if the choice is valid
            if is_valid(state, choice):
                # Make the choice
                state.add(choice)
                
                # Recursively explore with the current choice
                backtrack(state, get_next_choices(choices, choice))
                
                # Undo the choice (backtrack)
                state.remove(choice)
    
    backtrack(initial_state(), initial_choices(input_data))
    return result
```

## Real-World Applications

1. **Constraint Satisfaction Problems**: Solving puzzles like Sudoku, crosswords, or logic puzzles.
2. **Path Finding**: Finding all possible paths in a maze or graph.
3. **Game Playing**: Exploring game trees in games like chess or tic-tac-toe.
4. **Resource Allocation**: Finding all possible ways to allocate resources to tasks.
5. **Circuit Design**: Generating all possible circuit configurations.
6. **Scheduling**: Finding all possible schedules that satisfy certain constraints.
7. **Pattern Recognition**: Finding all occurrences of a pattern in a dataset. 