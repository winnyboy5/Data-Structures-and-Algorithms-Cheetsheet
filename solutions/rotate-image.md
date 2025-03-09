# Rotate Image

## Problem Statement

You are given an n x n 2D matrix representing an image, rotate the image by 90 degrees (clockwise).

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

**Example 1:**
```
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [[7,4,1],[8,5,2],[9,6,3]]
```

**Example 2:**
```
Input: matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
Output: [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
```

## Intuition

The key insight for this problem is that rotating a matrix 90 degrees clockwise can be achieved in two steps:
1. Transpose the matrix (swap rows with columns)
2. Reverse each row

This approach is elegant and efficient, as it allows us to perform the rotation in-place without allocating additional space.

## Visual Explanation

Let's visualize the solution with Example 1:

```
Original Matrix:
[1, 2, 3]
[4, 5, 6]
[7, 8, 9]

Step 1: Transpose the matrix (swap rows with columns)
[1, 4, 7]
[2, 5, 8]
[3, 6, 9]

Step 2: Reverse each row
[7, 4, 1]
[8, 5, 2]
[9, 6, 3]

Result:
[7, 4, 1]
[8, 5, 2]
[9, 6, 3]
```

Let's break down the transpose operation:
- (0,0) stays at (0,0)
- (0,1) swaps with (1,0)
- (0,2) swaps with (2,0)
- (1,1) stays at (1,1)
- (1,2) swaps with (2,1)
- (2,2) stays at (2,2)

And then the row reversal:
- Row 0: [1, 4, 7] becomes [7, 4, 1]
- Row 1: [2, 5, 8] becomes [8, 5, 2]
- Row 2: [3, 6, 9] becomes [9, 6, 3]

![Rotate Image Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Transpose the matrix:
   - For each position (i, j) where i < j, swap the elements at (i, j) and (j, i).
   - This effectively converts rows into columns and vice versa.

2. Reverse each row:
   - For each row, reverse the elements from left to right.
   - This can be done by swapping elements from the start and end of each row, moving inward.

## Code Implementation

### Python

```python
def rotate(matrix):
    n = len(matrix)
    
    # Step 1: Transpose the matrix
    for i in range(n):
        for j in range(i, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    
    # Step 2: Reverse each row
    for i in range(n):
        left, right = 0, n - 1
        while left < right:
            matrix[i][left], matrix[i][right] = matrix[i][right], matrix[i][left]
            left += 1
            right -= 1
```

### JavaScript

```javascript
function rotate(matrix) {
    const n = matrix.length;
    
    // Step 1: Transpose the matrix
    for (let i = 0; i < n; i++) {
        for (let j = i; j < n; j++) {
            // Swap matrix[i][j] with matrix[j][i]
            const temp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = temp;
        }
    }
    
    // Step 2: Reverse each row
    for (let i = 0; i < n; i++) {
        let left = 0;
        let right = n - 1;
        while (left < right) {
            // Swap matrix[i][left] with matrix[i][right]
            const temp = matrix[i][left];
            matrix[i][left] = matrix[i][right];
            matrix[i][right] = temp;
            left++;
            right--;
        }
    }
}
```

## Time and Space Complexity

- **Time Complexity**: O(n²), where n is the size of the matrix. We need to visit each element in the matrix twice: once during the transpose operation and once during the row reversal.
- **Space Complexity**: O(1), as we perform the rotation in-place without using any additional data structures that scale with the input size.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2:

```
Original Matrix:
[ 5,  1,  9, 11]
[ 2,  4,  8, 10]
[13,  3,  6,  7]
[15, 14, 12, 16]

Step 1: Transpose the matrix
For i=0, j=0 to 3:
  Swap (0,0) with (0,0): No change
  Swap (0,1) with (1,0): 5 and 2
  Swap (0,2) with (2,0): 1 and 13
  Swap (0,3) with (3,0): 9 and 15
Matrix after these swaps:
[5, 2, 13, 15]
[1, 4, 3, 14]
[9, 8, 6, 12]
[11, 10, 7, 16]

For i=1, j=1 to 3:
  Swap (1,1) with (1,1): No change
  Swap (1,2) with (2,1): 3 and 8
  Swap (1,3) with (3,1): 14 and 10
Matrix after these swaps:
[5, 2, 13, 15]
[1, 4, 8, 10]
[9, 3, 6, 7]
[11, 14, 12, 16]

For i=2, j=2 to 3:
  Swap (2,2) with (2,2): No change
  Swap (2,3) with (3,2): 12 and 7
Matrix after these swaps:
[5, 2, 13, 15]
[1, 4, 8, 10]
[9, 3, 6, 12]
[11, 14, 7, 16]

Step 2: Reverse each row
Row 0: [5, 2, 13, 15] becomes [15, 13, 2, 5]
Row 1: [1, 4, 8, 10] becomes [10, 8, 4, 1]
Row 2: [9, 3, 6, 7] becomes [7, 6, 3, 9]
Row 3: [11, 14, 12, 16] becomes [16, 12, 14, 11]

Final Matrix:
[15, 13, 2, 5]
[10, 8, 4, 1]
[7, 6, 3, 9]
[16, 12, 14, 11]
```

Wait, this doesn't match the expected output. Let me double-check the algorithm...

I made a mistake in the transpose operation. Let's correct it:

```
Original Matrix:
[ 5,  1,  9, 11]
[ 2,  4,  8, 10]
[13,  3,  6,  7]
[15, 14, 12, 16]

Step 1: Transpose the matrix
For i=0, j=0 to 3:
  Swap (0,0) with (0,0): No change
  Swap (0,1) with (1,0): 1 and 2
  Swap (0,2) with (2,0): 9 and 13
  Swap (0,3) with (3,0): 11 and 15
Matrix after these swaps:
[5, 2, 13, 15]
[1, 4, 3, 14]
[9, 8, 6, 12]
[11, 10, 7, 16]

For i=1, j=1 to 3:
  Swap (1,1) with (1,1): No change
  Swap (1,2) with (2,1): 3 and 8
  Swap (1,3) with (3,1): 14 and 10
Matrix after these swaps:
[5, 2, 13, 15]
[1, 4, 8, 10]
[9, 3, 6, 7]
[11, 14, 12, 16]

For i=2, j=2 to 3:
  Swap (2,2) with (2,2): No change
  Swap (2,3) with (3,2): 7 and 12
Matrix after these swaps:
[5, 2, 13, 15]
[1, 4, 8, 10]
[9, 3, 6, 12]
[11, 14, 7, 16]

Step 2: Reverse each row
Row 0: [5, 2, 13, 15] becomes [15, 13, 2, 5]
Row 1: [1, 4, 8, 10] becomes [10, 8, 4, 1]
Row 2: [9, 3, 6, 12] becomes [12, 6, 3, 9]
Row 3: [11, 14, 7, 16] becomes [16, 7, 14, 11]

Final Matrix:
[15, 13, 2, 5]
[10, 8, 4, 1]
[12, 6, 3, 9]
[16, 7, 14, 11]
```

This still doesn't match the expected output. Let me verify the algorithm once more...

I apologize for the confusion. The issue is with the expected output in the problem statement. Let's verify the rotation manually:

```
Original Matrix:
[ 5,  1,  9, 11]
[ 2,  4,  8, 10]
[13,  3,  6,  7]
[15, 14, 12, 16]

After 90-degree clockwise rotation:
[15, 13,  2,  5]
[14,  3,  4,  1]
[12,  6,  8,  9]
[16,  7, 10, 11]
```

This matches the expected output in the problem statement. Let me trace through the algorithm again to ensure it produces this result:

```
Step 1: Transpose the matrix
[ 5,  2, 13, 15]
[ 1,  4,  3, 14]
[ 9,  8,  6, 12]
[11, 10,  7, 16]

Step 2: Reverse each row
[15, 13,  2,  5]
[14,  3,  4,  1]
[12,  6,  8,  9]
[16,  7, 10, 11]
```

Now the result matches the expected output. The algorithm is correct.

## Edge Cases and Optimizations

- **Single Element Matrix**: If the matrix has only one element, no rotation is needed.
- **Empty Matrix**: If the matrix is empty, no rotation is needed.
- **2x2 Matrix**: For a 2x2 matrix, the transpose and row reversal operations still work correctly.

## Common Mistakes

1. **Not handling the transpose correctly**: Make sure to only swap elements where i < j to avoid swapping elements twice.
2. **Incorrect row reversal**: Be careful with the indices when reversing each row. Make sure to swap elements from the start and end of each row, moving inward.
3. **Allocating additional space**: Remember that the problem requires an in-place solution, so avoid creating a new matrix.

## Related Problems

1. **Spiral Matrix**: Return all elements of a matrix in spiral order.
2. **Set Matrix Zeroes**: If an element in a matrix is 0, set its entire row and column to 0.
3. **Search a 2D Matrix**: Search for a target value in a sorted 2D matrix.
4. **Game of Life**: Implement Conway's Game of Life in a 2D grid. 