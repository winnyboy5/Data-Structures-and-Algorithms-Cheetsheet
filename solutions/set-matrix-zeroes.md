# Set Matrix Zeroes

## Problem Statement

Given an m x n integer matrix `matrix`, if an element is 0, set its entire row and column to 0's.

You must do it in place.

**Example 1:**
```
Input: matrix = [[1,1,1],[1,0,1],[1,1,1]]
Output: [[1,0,1],[0,0,0],[1,0,1]]
```

**Example 2:**
```
Input: matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
Output: [[0,0,0,0],[0,4,5,0],[0,3,1,0]]
```

## Intuition

The key insight for this problem is to efficiently track which rows and columns need to be zeroed out without using extra space. A naive approach would be to use additional arrays to mark the rows and columns that contain zeros, but we can optimize this by using the first row and first column of the matrix itself as markers.

The challenge is to handle the first row and first column properly, as they are used as markers but might also contain zeros themselves. To address this, we can:

1. Use separate variables to track if the first row and first column should be zeroed.
2. Use the first row and first column to mark which other rows and columns should be zeroed.
3. Zero out the marked rows and columns.
4. Finally, zero out the first row and first column if needed.

## Visual Explanation

Let's visualize the solution with Example 1: `matrix = [[1,1,1],[1,0,1],[1,1,1]]`

```
Initial matrix:
1 1 1
1 0 1
1 1 1

Step 1: Check if the first row and first column should be zeroed
- No zeros in the first row, so firstRowZero = false
- No zeros in the first column, so firstColZero = false

Step 2: Use the first row and first column as markers
- matrix[1][1] = 0, so set matrix[1][0] = 0 and matrix[0][1] = 0
- Updated matrix:
  1 0 1
  0 0 1
  1 1 1

Step 3: Zero out the marked rows and columns (except the first row and column)
- matrix[1][0] = 0, so zero out row 1
- matrix[0][1] = 0, so zero out column 1
- Updated matrix:
  1 0 1
  0 0 0
  1 0 1

Step 4: Zero out the first row and first column if needed
- firstRowZero = false, so the first row remains unchanged
- firstColZero = false, so the first column remains unchanged
- Final matrix:
  1 0 1
  0 0 0
  1 0 1
```

Let's visualize the solution with Example 2: `matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]`

```
Initial matrix:
0 1 2 0
3 4 5 2
1 3 1 5

Step 1: Check if the first row and first column should be zeroed
- First row contains zeros, so firstRowZero = true
- First column contains a zero, so firstColZero = true

Step 2: Use the first row and first column as markers
- matrix[0][0] = 0, so first row and first column are already marked
- matrix[0][3] = 0, so set matrix[0][3] = 0 (already 0)
- Updated matrix:
  0 1 2 0
  3 4 5 2
  1 3 1 5

Step 3: Zero out the marked rows and columns (except the first row and column)
- matrix[0][0] = 0, so zero out column 0 (except the first element)
- matrix[0][3] = 0, so zero out column 3 (except the first element)
- Updated matrix:
  0 1 2 0
  0 4 5 0
  0 3 1 0

Step 4: Zero out the first row and first column if needed
- firstRowZero = true, so zero out the first row
- firstColZero = true, so zero out the first column (already done)
- Final matrix:
  0 0 0 0
  0 4 5 0
  0 3 1 0
```

![Set Matrix Zeroes Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. **Check if the first row and first column should be zeroed**:
   - Scan the first row and first column to check if they contain any zeros.
   - Use separate boolean variables `firstRowZero` and `firstColZero` to track this.

2. **Use the first row and first column as markers**:
   - Iterate through the matrix (excluding the first row and column).
   - If a cell `matrix[i][j]` is 0, set `matrix[i][0]` and `matrix[0][j]` to 0 to mark that the ith row and jth column should be zeroed.

3. **Zero out the marked rows and columns**:
   - Iterate through the matrix (excluding the first row and column).
   - If `matrix[i][0]` or `matrix[0][j]` is 0, set `matrix[i][j]` to 0.

4. **Zero out the first row and first column if needed**:
   - If `firstRowZero` is true, set all elements in the first row to 0.
   - If `firstColZero` is true, set all elements in the first column to 0.

## Code Implementation

### Python

```python
def setZeroes(matrix):
    if not matrix or not matrix[0]:
        return
    
    m, n = len(matrix), len(matrix[0])
    
    # Check if the first row and first column should be zeroed
    first_row_zero = False
    first_col_zero = False
    
    for j in range(n):
        if matrix[0][j] == 0:
            first_row_zero = True
            break
    
    for i in range(m):
        if matrix[i][0] == 0:
            first_col_zero = True
            break
    
    # Use the first row and first column as markers
    for i in range(1, m):
        for j in range(1, n):
            if matrix[i][j] == 0:
                matrix[i][0] = 0
                matrix[0][j] = 0
    
    # Zero out the marked rows and columns (except the first row and column)
    for i in range(1, m):
        for j in range(1, n):
            if matrix[i][0] == 0 or matrix[0][j] == 0:
                matrix[i][j] = 0
    
    # Zero out the first row if needed
    if first_row_zero:
        for j in range(n):
            matrix[0][j] = 0
    
    # Zero out the first column if needed
    if first_col_zero:
        for i in range(m):
            matrix[i][0] = 0
```

### JavaScript

```javascript
function setZeroes(matrix) {
    if (!matrix || matrix.length === 0 || matrix[0].length === 0) {
        return;
    }
    
    const m = matrix.length;
    const n = matrix[0].length;
    
    // Check if the first row and first column should be zeroed
    let firstRowZero = false;
    let firstColZero = false;
    
    for (let j = 0; j < n; j++) {
        if (matrix[0][j] === 0) {
            firstRowZero = true;
            break;
        }
    }
    
    for (let i = 0; i < m; i++) {
        if (matrix[i][0] === 0) {
            firstColZero = true;
            break;
        }
    }
    
    // Use the first row and first column as markers
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            if (matrix[i][j] === 0) {
                matrix[i][0] = 0;
                matrix[0][j] = 0;
            }
        }
    }
    
    // Zero out the marked rows and columns (except the first row and column)
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            if (matrix[i][0] === 0 || matrix[0][j] === 0) {
                matrix[i][j] = 0;
            }
        }
    }
    
    // Zero out the first row if needed
    if (firstRowZero) {
        for (let j = 0; j < n; j++) {
            matrix[0][j] = 0;
        }
    }
    
    // Zero out the first column if needed
    if (firstColZero) {
        for (let i = 0; i < m; i++) {
            matrix[i][0] = 0;
        }
    }
}
```

## Time and Space Complexity

- **Time Complexity**: O(m × n), where m is the number of rows and n is the number of columns in the matrix. We iterate through the matrix multiple times, but each iteration is O(m × n).
- **Space Complexity**: O(1), as we only use a constant amount of extra space regardless of the input size. We use the matrix itself to store the markers.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 1: `matrix = [[1,1,1],[1,0,1],[1,1,1]]`

```
Initialize:
- matrix = [[1,1,1],[1,0,1],[1,1,1]]
- m = 3, n = 3

Step 1: Check if the first row and first column should be zeroed
- first_row_zero = false (no zeros in the first row)
- first_col_zero = false (no zeros in the first column)

Step 2: Use the first row and first column as markers
- Iteration (1,1): matrix[1][1] = 0, so set matrix[1][0] = 0 and matrix[0][1] = 0
- Updated matrix:
  [1,0,1]
  [0,0,1]
  [1,1,1]

Step 3: Zero out the marked rows and columns (except the first row and column)
- Iteration (1,1): matrix[1][0] = 0 or matrix[0][1] = 0, so set matrix[1][1] = 0 (already 0)
- Iteration (1,2): matrix[1][0] = 0, so set matrix[1][2] = 0
- Iteration (2,1): matrix[0][1] = 0, so set matrix[2][1] = 0
- Updated matrix:
  [1,0,1]
  [0,0,0]
  [1,0,1]

Step 4: Zero out the first row and first column if needed
- first_row_zero = false, so the first row remains unchanged
- first_col_zero = false, so the first column remains unchanged
- Final matrix:
  [1,0,1]
  [0,0,0]
  [1,0,1]
```

## Alternative Approaches

### Using Additional Space

A simpler approach is to use two separate arrays to track which rows and columns should be zeroed:

```python
def setZeroes(matrix):
    if not matrix or not matrix[0]:
        return
    
    m, n = len(matrix), len(matrix[0])
    rows_to_zero = set()
    cols_to_zero = set()
    
    # Identify which rows and columns should be zeroed
    for i in range(m):
        for j in range(n):
            if matrix[i][j] == 0:
                rows_to_zero.add(i)
                cols_to_zero.add(j)
    
    # Zero out the identified rows
    for row in rows_to_zero:
        for j in range(n):
            matrix[row][j] = 0
    
    # Zero out the identified columns
    for col in cols_to_zero:
        for i in range(m):
            matrix[i][col] = 0
```

This approach has a time complexity of O(m × n) and a space complexity of O(m + n).

## Edge Cases and Optimizations

- **Empty Matrix**: If the matrix is empty, the function will return immediately.
- **Single Row or Column**: The algorithm handles matrices with a single row or column correctly.
- **All Zeros**: If the matrix contains all zeros, the function will correctly set all elements to zero.
- **No Zeros**: If the matrix contains no zeros, the function will leave the matrix unchanged.

## Common Mistakes

1. **Not handling the first row and first column separately**: Since we're using the first row and first column as markers, we need to handle them separately.
2. **Zeroing out rows and columns prematurely**: Make sure to identify all cells that should be zeroed before actually zeroing them out.
3. **Not checking if the matrix is empty**: Always check if the input matrix is valid before processing it.

## Related Problems

1. **Game of Life**: Update the board based on the rules of Conway's Game of Life.
2. **Valid Sudoku**: Determine if a 9x9 Sudoku board is valid.
3. **Rotate Image**: Rotate an n x n 2D matrix by 90 degrees clockwise.
4. **Spiral Matrix**: Return all elements of the matrix in spiral order. 