# Container With Most Water

## Problem Statement

You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `i`th line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

Notice that you may not slant the container.

**Example 1:**
![Container With Most Water Example](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)
```
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
```

**Example 2:**
```
Input: height = [1,1]
Output: 1
```

## Intuition

The key insight for this problem is to use a two-pointer approach. We start with the widest possible container (using the first and last lines) and then try to find a better solution by moving the pointers.

The amount of water a container can hold is determined by the shorter of the two lines and the distance between them. Specifically, the area is calculated as:

```
Area = min(height[left], height[right]) * (right - left)
```

To maximize this area, we need to consider both the height and the width. Since we start with the maximum width, we can only increase the area by finding taller lines. Therefore, we should move the pointer pointing to the shorter line inward, as moving the taller line would only decrease the width without any potential increase in height.

## Visual Explanation

Let's visualize the solution with Example 1: `height = [1,8,6,2,5,4,8,3,7]`

```
Initial state:
height = [1,8,6,2,5,4,8,3,7]
          ^               ^
         left           right
         (0)             (8)

Calculate area:
Area = min(1, 7) * (8 - 0) = 1 * 8 = 8

Since height[left] < height[right], move left pointer:
height = [1,8,6,2,5,4,8,3,7]
            ^             ^
           left         right
           (1)           (8)

Calculate area:
Area = min(8, 7) * (8 - 1) = 7 * 7 = 49

Since height[left] > height[right], move right pointer:
height = [1,8,6,2,5,4,8,3,7]
            ^           ^
           left       right
           (1)         (7)

Calculate area:
Area = min(8, 3) * (7 - 1) = 3 * 6 = 18

Since height[left] > height[right], move right pointer:
height = [1,8,6,2,5,4,8,3,7]
            ^         ^
           left     right
           (1)       (6)

Calculate area:
Area = min(8, 8) * (6 - 1) = 8 * 5 = 40

Since height[left] == height[right], move either pointer (let's move right):
height = [1,8,6,2,5,4,8,3,7]
            ^       ^
           left   right
           (1)     (5)

Calculate area:
Area = min(8, 4) * (5 - 1) = 4 * 4 = 16

Continue this process...

The maximum area found is 49.
```

![Container With Most Water Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize two pointers, `left` and `right`, pointing to the start and end of the array.
2. Initialize a variable `max_area` to 0.
3. While `left < right`:
   - Calculate the current area as `min(height[left], height[right]) * (right - left)`.
   - Update `max_area` if the current area is larger.
   - Move the pointer pointing to the shorter line inward:
     - If `height[left] < height[right]`, increment `left`.
     - If `height[left] >= height[right]`, decrement `right`.
4. Return `max_area`.

## Code Implementation

### Python

```python
def maxArea(height):
    left, right = 0, len(height) - 1
    max_area = 0
    
    while left < right:
        # Calculate the current area
        current_area = min(height[left], height[right]) * (right - left)
        
        # Update max_area if the current area is larger
        max_area = max(max_area, current_area)
        
        # Move the pointer pointing to the shorter line
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1
    
    return max_area
```

### JavaScript

```javascript
function maxArea(height) {
    let left = 0;
    let right = height.length - 1;
    let maxArea = 0;
    
    while (left < right) {
        // Calculate the current area
        const currentArea = Math.min(height[left], height[right]) * (right - left);
        
        // Update maxArea if the current area is larger
        maxArea = Math.max(maxArea, currentArea);
        
        // Move the pointer pointing to the shorter line
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }
    
    return maxArea;
}
```

## Time and Space Complexity

- **Time Complexity**: O(n), where n is the length of the array. We process each element at most once.
- **Space Complexity**: O(1), as we only use a constant amount of extra space regardless of the input size.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2: `height = [1,1]`

```
Initial state:
height = [1,1]
          ^ ^
        left right
        (0)  (1)

Calculate area:
Area = min(1, 1) * (1 - 0) = 1 * 1 = 1

Since height[left] == height[right], move either pointer (let's move right):
height = [1,1]
          ^
       left,right
        (0)

Since left == right, the loop terminates.

The maximum area found is 1.
```

## Edge Cases and Optimizations

- **Empty Array**: If the array is empty, the function will return 0.
- **Single Element**: If the array has only one element, the function will return 0 (as we need at least two lines to form a container).
- **All Same Height**: If all lines have the same height, the maximum area will be the height multiplied by the maximum width.

## Common Mistakes

1. **Using a brute force approach**: A common mistake is to use a nested loop to check all possible pairs of lines, which has a time complexity of O(n²). The two-pointer approach is more efficient with a time complexity of O(n).
2. **Moving the wrong pointer**: Make sure to move the pointer pointing to the shorter line, as moving the taller line can only decrease the area.
3. **Not considering the width**: Remember that the area is calculated as the minimum height multiplied by the width. Both factors are important.

## Related Problems

1. **Trapping Rain Water**: Calculate how much water can be trapped after raining.
2. **Largest Rectangle in Histogram**: Find the area of the largest rectangle in a histogram.
3. **Container With Most Water II**: A 2D version of this problem.
4. **Two Sum**: Find two numbers in an array that add up to a specific target. 