# Spiral Matrix

## Problem Statement

Given an m x n matrix, return all elements of the matrix in spiral order.

**Example 1:**
```
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,2,3,6,9,8,7,4,5]
```

**Example 2:**
```
Input: matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```

## Intuition

The key insight for this problem is to traverse the matrix in a spiral pattern by defining four boundaries: top, right, bottom, and left. We start from the top-left corner and move right until we reach the right boundary, then move down until we reach the bottom boundary, then move left until we reach the left boundary, and finally move up until we reach the top boundary. After each traversal, we update the corresponding boundary to shrink the area we need to traverse.

This approach allows us to systematically visit all elements in the matrix in a spiral order.

## Visual Explanation

Let's visualize the solution with Example 1:

```
Matrix:
[1, 2, 3]
[4, 5, 6]
[7, 8, 9]

Step 1: Initialize boundaries
top = 0, right = 2, bottom = 2, left = 0
result = []

Step 2: Traverse top row (left to right)
top = 0, left = 0 to right = 2
Add elements: 1, 2, 3
result = [1, 2, 3]
Update top: top = 1

Step 3: Traverse right column (top to bottom)
right = 2, top = 1 to bottom = 2
Add elements: 6, 9
result = [1, 2, 3, 6, 9]
Update right: right = 1

Step 4: Traverse bottom row (right to left)
bottom = 2, right = 1 to left = 0
Add elements: 8, 7
result = [1, 2, 3, 6, 9, 8, 7]
Update bottom: bottom = 1

Step 5: Traverse left column (bottom to top)
left = 0, bottom = 1 to top = 1
Add elements: 4
result = [1, 2, 3, 6, 9, 8, 7, 4]
Update left: left = 1

Step 6: Traverse top row (left to right)
top = 1, left = 1 to right = 1
Add elements: 5
result = [1, 2, 3, 6, 9, 8, 7, 4, 5]
Update top: top = 2

Step 7: Check if we've traversed all elements
top > bottom or left > right, so we're done.

Final result: [1, 2, 3, 6, 9, 8, 7, 4, 5]
```

![Spiral Matrix Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize four boundaries: `top = 0`, `right = n-1`, `bottom = m-1`, and `left = 0`, where `m` is the number of rows and `n` is the number of columns.
2. Initialize an empty result array to store the elements in spiral order.
3. While `top <= bottom` and `left <= right`:
   - Traverse from left to right along the top row, then increment `top`.
   - Traverse from top to bottom along the right column, then decrement `right`.
   - If `top <= bottom`, traverse from right to left along the bottom row, then decrement `bottom`.
   - If `left <= right`, traverse from bottom to top along the left column, then increment `left`.
4. Return the result array.

## Code Implementation

### Python

```python
def spiralOrder(matrix):
    if not matrix:
        return []
    
    result = []
    top, right, bottom, left = 0, len(matrix[0]) - 1, len(matrix) - 1, 0
    
    while top <= bottom and left <= right:
        # Traverse from left to right along the top row
        for j in range(left, right + 1):
            result.append(matrix[top][j])
        top += 1
        
        # Traverse from top to bottom along the right column
        for i in range(top, bottom + 1):
            result.append(matrix[i][right])
        right -= 1
        
        # Traverse from right to left along the bottom row
        if top <= bottom:
            for j in range(right, left - 1, -1):
                result.append(matrix[bottom][j])
            bottom -= 1
        
        # Traverse from bottom to top along the left column
        if left <= right:
            for i in range(bottom, top - 1, -1):
                result.append(matrix[i][left])
            left += 1
    
    return result
```

### JavaScript

```javascript
function spiralOrder(matrix) {
    if (!matrix.length) {
        return [];
    }
    
    const result = [];
    let top = 0;
    let right = matrix[0].length - 1;
    let bottom = matrix.length - 1;
    let left = 0;
    
    while (top <= bottom && left <= right) {
        // Traverse from left to right along the top row
        for (let j = left; j <= right; j++) {
            result.push(matrix[top][j]);
        }
        top++;
        
        // Traverse from top to bottom along the right column
        for (let i = top; i <= bottom; i++) {
            result.push(matrix[i][right]);
        }
        right--;
        
        // Traverse from right to left along the bottom row
        if (top <= bottom) {
            for (let j = right; j >= left; j--) {
                result.push(matrix[bottom][j]);
            }
            bottom--;
        }
        
        // Traverse from bottom to top along the left column
        if (left <= right) {
            for (let i = bottom; i >= top; i--) {
                result.push(matrix[i][left]);
            }
            left++;
        }
    }
    
    return result;
}
```

## Time and Space Complexity

- **Time Complexity**: O(m × n), where m is the number of rows and n is the number of columns. We visit each element in the matrix exactly once.
- **Space Complexity**: O(1) excluding the output array. We only use a constant amount of extra space for the variables `top`, `right`, `bottom`, and `left`. If we include the output array, the space complexity is O(m × n).

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2:

```
Matrix:
[1,  2,  3,  4]
[5,  6,  7,  8]
[9, 10, 11, 12]

Initialize:
top = 0, right = 3, bottom = 2, left = 0
result = []

Iteration 1:
- Traverse top row: Add 1, 2, 3, 4
  result = [1, 2, 3, 4]
  Update top = 1
- Traverse right column: Add 8, 12
  result = [1, 2, 3, 4, 8, 12]
  Update right = 2
- Traverse bottom row: Add 11, 10, 9
  result = [1, 2, 3, 4, 8, 12, 11, 10, 9]
  Update bottom = 1
- Traverse left column: Add 5
  result = [1, 2, 3, 4, 8, 12, 11, 10, 9, 5]
  Update left = 1

Iteration 2:
- Traverse top row: Add 6, 7
  result = [1, 2, 3, 4, 8, 12, 11, 10, 9, 5, 6, 7]
  Update top = 2
- top > bottom, so we're done.

Final result: [1, 2, 3, 4, 8, 12, 11, 10, 9, 5, 6, 7]
```

## Edge Cases and Optimizations

- **Empty Matrix**: If the matrix is empty, we return an empty array.
- **Single Row Matrix**: If the matrix has only one row, we simply traverse from left to right.
- **Single Column Matrix**: If the matrix has only one column, we simply traverse from top to bottom.
- **Single Element Matrix**: If the matrix has only one element, we return that element.

## Common Mistakes

1. **Not checking boundaries**: Make sure to check if `top <= bottom` and `left <= right` before traversing the bottom row and left column, respectively.
2. **Off-by-one errors**: Be careful with the indices when traversing the matrix. Make sure to include the boundary elements.
3. **Updating boundaries incorrectly**: Make sure to update the boundaries after each traversal to shrink the area we need to traverse.

## Related Problems

1. **Spiral Matrix II**: Generate a matrix filled with elements from 1 to n² in spiral order.
2. **Rotate Image**: Rotate a matrix 90 degrees clockwise.
3. **Set Matrix Zeroes**: If an element in a matrix is 0, set its entire row and column to 0.
4. **Search a 2D Matrix**: Search for a target value in a sorted 2D matrix. 