# Backtracking

Backtracking is an algorithmic technique for solving problems recursively by trying to build a solution incrementally, one piece at a time, and removing those solutions that fail to satisfy the constraints of the problem at any point.

## Table of Contents
- [Understanding Backtracking](#understanding-backtracking)
- [Backtracking Template](#backtracking-template)
- [Common Backtracking Problems](#common-backtracking-problems)
- [Optimization Techniques](#optimization-techniques)
- [Related Blind 75 & Grind 75 Problems](#related-blind-75--grind-75-problems)
- [Tips and Tricks](#tips-and-tricks)

## Understanding Backtracking

### Key Concepts
1. **Incremental Solution Building**: Build the solution one step at a time.
2. **Constraint Checking**: Check if the current partial solution satisfies all constraints.
3. **Recursive Exploration**: Recursively explore all possible next steps.
4. **Backtracking**: If a path leads to a dead end, backtrack to the previous step and try a different path.

### Visual Explanation

Let's visualize backtracking with the classic N-Queens problem (placing N queens on an N×N chessboard so that no two queens threaten each other):

```
For a 4×4 board:

Step 1: Place queen in first row, first column
   Q . . .
   . . . .
   . . . .
   . . . .

Step 2: Try to place queen in second row
   Q . . .
   . . Q .  (columns 1 and 3 are under attack, so try column 2)
   . . . .
   . . . .

Step 3: Try to place queen in third row
   Q . . .
   . . Q .
   . . . .  (all columns are under attack, backtrack)

Step 4: Backtrack to second row, try next position
   Q . . .
   . . . Q
   . . . .
   . . . .

Step 5: Try to place queen in third row
   Q . . .
   . . . Q
   . Q . .  (columns 0, 2, 3 are under attack, so try column 1)
   . . . .

Step 6: Try to place queen in fourth row
   Q . . .
   . . . Q
   . Q . .
   . . Q .  (valid solution found)
```

## Backtracking Template

Most backtracking algorithms follow a similar structure:

```python
def backtrack(candidate, remaining_choices):
    if is_solution(candidate):
        output_solution(candidate)
        return
    
    for choice in remaining_choices:
        if is_valid(candidate, choice):
            # Make a choice
            add_to_candidate(candidate, choice)
            
            # Recursively solve with the new candidate
            backtrack(candidate, get_remaining_choices(remaining_choices, choice))
            
            # Undo the choice (backtrack)
            remove_from_candidate(candidate, choice)
```

## Common Backtracking Problems

### 1. N-Queens Problem

#### Problem
Place N queens on an N×N chessboard so that no two queens threaten each other.

#### Visual Explanation
```
For a 4×4 board, one solution is:
. Q . .
. . . Q
Q . . .
. . Q .
```

#### Implementation

```python
def solve_n_queens(n):
    def create_board(state):
        board = []
        for row in range(n):
            board.append(''.join('Q' if col == state[row] else '.' for col in range(n)))
        return board
    
    def backtrack(row, columns, diagonals, anti_diagonals, state):
        if row == n:
            solutions.append(create_board(state))
            return
        
        for col in range(n):
            curr_diagonal = row - col
            curr_anti_diagonal = row + col
            
            # If the queen is not placeable
            if (col in columns or 
                curr_diagonal in diagonals or 
                curr_anti_diagonal in anti_diagonals):
                continue
            
            # "Add" the queen to the board
            columns.add(col)
            diagonals.add(curr_diagonal)
            anti_diagonals.add(curr_anti_diagonal)
            state[row] = col
            
            # Move on to the next row
            backtrack(row + 1, columns, diagonals, anti_diagonals, state)
            
            # "Remove" the queen from the board (backtrack)
            columns.remove(col)
            diagonals.remove(curr_diagonal)
            anti_diagonals.remove(curr_anti_diagonal)
            state[row] = -1
    
    solutions = []
    state = [-1] * n
    backtrack(0, set(), set(), set(), state)
    return solutions
```

### 2. Sudoku Solver

#### Problem
Fill a 9×9 grid with digits so that each column, each row, and each of the nine 3×3 subgrids contains all of the digits from 1 to 9.

#### Visual Explanation
```
Initial board:
5 3 . | . 7 . | . . .
6 . . | 1 9 5 | . . .
. 9 8 | . . . | . 6 .
---------------------
8 . . | . 6 . | . . 3
4 . . | 8 . 3 | . . 1
7 . . | . 2 . | . . 6
---------------------
. 6 . | . . . | 2 8 .
. . . | 4 1 9 | . . 5
. . . | . 8 . | . 7 9

Backtracking process:
1. Find an empty cell
2. Try placing digits 1-9
3. Check if the digit is valid in the current position
4. If valid, recursively try to fill the rest of the board
5. If the recursive call fails, backtrack and try a different digit
```

#### Implementation

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
                    # Try placing digits 1-9
                    for num in map(str, range(1, 10)):
                        if is_valid(row, col, num):
                            # Place the digit
                            board[row][col] = num
                            
                            # Recursively try to fill the rest of the board
                            if backtrack():
                                return True
                            
                            # If placing the digit doesn't lead to a solution, backtrack
                            board[row][col] = '.'
                    
                    # If no digit can be placed, this path is invalid
                    return False
        
        # If we've filled all cells, we've found a solution
        return True
    
    backtrack()
    return board
```

### 3. Permutations

#### Problem
Generate all possible permutations of a set of distinct integers.

#### Visual Explanation
```
For the set [1, 2, 3]:

Start with an empty permutation: []

Backtracking tree:
                  []
        /         |         \
      [1]        [2]        [3]
     /   \      /   \      /   \
  [1,2] [1,3] [2,1] [2,3] [3,1] [3,2]
    |     |     |     |     |     |
[1,2,3][1,3,2][2,1,3][2,3,1][3,1,2][3,2,1]

Result: [[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]]
```

#### Implementation

```python
def permute(nums):
    def backtrack(curr_perm):
        if len(curr_perm) == len(nums):
            result.append(curr_perm[:])
            return
        
        for num in nums:
            if num not in curr_perm:
                # Make a choice
                curr_perm.append(num)
                
                # Recursively generate permutations
                backtrack(curr_perm)
                
                # Undo the choice (backtrack)
                curr_perm.pop()
    
    result = []
    backtrack([])
    return result
```

### 4. Combination Sum

#### Problem
Find all unique combinations of candidates where the chosen numbers sum to a target.

#### Visual Explanation
```
For candidates [2, 3, 6, 7] and target 7:

Backtracking tree (showing only valid paths):
                  []
        /         |         \
      [2]        [3]        [7]
     /   \        |
  [2,2]  [2,3]   [3,3]
    |
 [2,2,3]

Result: [[2,2,3], [7]]
```

#### Implementation

```python
def combination_sum(candidates, target):
    def backtrack(start, curr_sum, curr_comb):
        if curr_sum == target:
            result.append(curr_comb[:])
            return
        
        if curr_sum > target:
            return
        
        for i in range(start, len(candidates)):
            # Make a choice
            curr_comb.append(candidates[i])
            
            # Recursively find combinations
            # Note: we can reuse the same element, so we pass i instead of i+1
            backtrack(i, curr_sum + candidates[i], curr_comb)
            
            # Undo the choice (backtrack)
            curr_comb.pop()
    
    result = []
    backtrack(0, 0, [])
    return result
```

### 5. Word Search

#### Problem
Given a 2D board and a word, find if the word exists in the grid. The word can be constructed from letters of sequentially adjacent cells, where "adjacent" cells are horizontally or vertically neighboring.

#### Visual Explanation
```
Board:
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Word: "ABCCED"

Backtracking process:
1. Start at each cell and try to find the word
2. For each cell, explore in four directions (up, right, down, left)
3. Mark visited cells to avoid revisiting
4. If the current path doesn't lead to a solution, backtrack
```

#### Implementation

```python
def exist(board, word):
    if not board or not board[0]:
        return False
    
    rows, cols = len(board), len(board[0])
    
    def backtrack(row, col, index):
        # If we've matched all characters, we've found the word
        if index == len(word):
            return True
        
        # Check if current position is out of bounds or doesn't match the current character
        if (row < 0 or row >= rows or col < 0 or col >= cols or
            board[row][col] != word[index]):
            return False
        
        # Mark the current cell as visited
        temp = board[row][col]
        board[row][col] = '#'
        
        # Explore in four directions
        found = (backtrack(row + 1, col, index + 1) or
                 backtrack(row - 1, col, index + 1) or
                 backtrack(row, col + 1, index + 1) or
                 backtrack(row, col - 1, index + 1))
        
        # Restore the cell (backtrack)
        board[row][col] = temp
        
        return found
    
    # Try starting from each cell
    for i in range(rows):
        for j in range(cols):
            if backtrack(i, j, 0):
                return True
    
    return False
```

## Optimization Techniques

### 1. Pruning

Pruning is a technique to eliminate choices that are guaranteed not to lead to a valid solution, reducing the search space.

#### Example: N-Queens with Pruning
```python
def solve_n_queens_with_pruning(n):
    def is_valid(board, row, col):
        # Check if a queen can be placed at board[row][col]
        
        # Check column
        for i in range(row):
            if board[i] == col:
                return False
        
        # Check upper-left diagonal
        for i, j in zip(range(row-1, -1, -1), range(col-1, -1, -1)):
            if board[i] == j:
                return False
        
        # Check upper-right diagonal
        for i, j in zip(range(row-1, -1, -1), range(col+1, n)):
            if board[i] == j:
                return False
        
        return True
    
    def backtrack(row, board):
        if row == n:
            solutions.append(board[:])
            return
        
        for col in range(n):
            if is_valid(board, row, col):
                board[row] = col
                backtrack(row + 1, board)
                board[row] = -1  # Backtrack
    
    solutions = []
    board = [-1] * n
    backtrack(0, board)
    return solutions
```

### 2. State Representation

Efficient state representation can significantly reduce the time and space complexity of backtracking algorithms.

#### Example: Sudoku with Bit Manipulation
```python
def solve_sudoku_with_bits(board):
    # Initialize sets to track used numbers in each row, column, and box
    rows = [0] * 9
    cols = [0] * 9
    boxes = [0] * 9
    
    # Fill the sets with initial values from the board
    for i in range(9):
        for j in range(9):
            if board[i][j] != '.':
                num = int(board[i][j])
                bit = 1 << (num - 1)
                rows[i] |= bit
                cols[j] |= bit
                boxes[(i // 3) * 3 + j // 3] |= bit
    
    def backtrack(row, col):
        if row == 9:
            return True
        
        # Move to the next cell
        next_col = col + 1
        next_row = row
        if next_col == 9:
            next_col = 0
            next_row += 1
        
        # If the current cell is filled, move to the next cell
        if board[row][col] != '.':
            return backtrack(next_row, next_col)
        
        # Try placing digits 1-9
        for num in range(1, 10):
            bit = 1 << (num - 1)
            box_idx = (row // 3) * 3 + col // 3
            
            # Check if the digit can be placed
            if not (rows[row] & bit or cols[col] & bit or boxes[box_idx] & bit):
                # Place the digit
                board[row][col] = str(num)
                rows[row] |= bit
                cols[col] |= bit
                boxes[box_idx] |= bit
                
                # Recursively try to fill the rest of the board
                if backtrack(next_row, next_col):
                    return True
                
                # If placing the digit doesn't lead to a solution, backtrack
                board[row][col] = '.'
                rows[row] &= ~bit
                cols[col] &= ~bit
                boxes[box_idx] &= ~bit
        
        return False
    
    backtrack(0, 0)
    return board
```

### 3. Memoization

Memoization can be combined with backtracking to avoid redundant computations.

#### Example: Word Break with Memoization
```python
def word_break(s, wordDict):
    word_set = set(wordDict)
    memo = {}
    
    def backtrack(start):
        if start == len(s):
            return True
        
        if start in memo:
            return memo[start]
        
        for end in range(start + 1, len(s) + 1):
            if s[start:end] in word_set and backtrack(end):
                memo[start] = True
                return True
        
        memo[start] = False
        return False
    
    return backtrack(0)
```

## Related Blind 75 & Grind 75 Problems

1. **Word Search** (Blind 75 #79)
   - Problem: Find if a word exists in a 2D board.
   - Solution: Use backtracking to explore all possible paths.
   - [LeetCode #79](https://leetcode.com/problems/word-search/)

2. **Combination Sum** (Blind 75 #39)
   - Problem: Find all unique combinations that sum to a target.
   - Solution: Use backtracking to explore all possible combinations.
   - [LeetCode #39](https://leetcode.com/problems/combination-sum/)

3. **Palindrome Partitioning** (Blind 75 #131)
   - Problem: Partition a string such that every substring is a palindrome.
   - Solution: Use backtracking to try different partitioning points.
   - [LeetCode #131](https://leetcode.com/problems/palindrome-partitioning/)

4. **Letter Combinations of a Phone Number** (Blind 75 #17)
   - Problem: Return all possible letter combinations from a phone number.
   - Solution: Use backtracking to generate all combinations.
   - [LeetCode #17](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

5. **N-Queens** (Grind 75)
   - Problem: Place N queens on an N×N chessboard without threatening each other.
   - Solution: Use backtracking with pruning to find all valid configurations.
   - [LeetCode #51](https://leetcode.com/problems/n-queens/)

6. **Subsets** (Grind 75)
   - Problem: Generate all possible subsets of a set of distinct integers.
   - Solution: Use backtracking to include or exclude each element.
   - [LeetCode #78](https://leetcode.com/problems/subsets/)

## Tips and Tricks

1. **Identifying Backtracking Problems**:
   - Problems that ask for all possible solutions
   - Problems that involve exploring all possible combinations or permutations
   - Problems with constraints that eliminate certain paths
   - Problems that can be modeled as a search through a tree or graph

2. **Designing Backtracking Solutions**:
   - Define a clear state representation
   - Identify the constraints that must be satisfied
   - Determine how to generate the next states
   - Implement a mechanism to backtrack when a path leads to a dead end

3. **Optimizing Backtracking Algorithms**:
   - Use pruning to eliminate invalid paths early
   - Choose an efficient state representation
   - Consider using memoization to avoid redundant computations
   - Order the choices to explore more promising paths first

4. **Common Pitfalls**:
   - Forgetting to backtrack (undo changes) after exploring a path
   - Not properly checking constraints
   - Inefficient state representation leading to slow execution
   - Redundant exploration of the same states

5. **Debugging Backtracking Algorithms**:
   - Trace through small examples by hand
   - Add print statements to visualize the backtracking process
   - Check base cases and constraints
   - Verify that backtracking correctly undoes all changes 