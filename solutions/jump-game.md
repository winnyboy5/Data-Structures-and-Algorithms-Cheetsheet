# Jump Game

## Problem Statement

You are given an integer array `nums`. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return `true` if you can reach the last index, or `false` otherwise.

**Example 1:**
```
Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**Example 2:**
```
Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
```

## Intuition

The key insight for this problem is to track the furthest position we can reach as we iterate through the array. If at any point, our current position is beyond the furthest position we can reach, then we can't reach the end. Otherwise, if the furthest reachable position is at or beyond the last index, we can reach the end.

This is a greedy approach because at each step, we're making the locally optimal choice (updating the furthest reachable position) without reconsidering previous decisions.

## Visual Explanation

Let's visualize the solution with Example 1: `nums = [2,3,1,1,4]`

```
Index:  0  1  2  3  4
Value:  2  3  1  1  4
        ^
        |
     Current position = 0
     Max reach = 0 + 2 = 2

Index:  0  1  2  3  4
Value:  2  3  1  1  4
           ^
           |
     Current position = 1
     Max reach = max(2, 1 + 3) = 4

Index:  0  1  2  3  4
Value:  2  3  1  1  4
              ^
              |
     Current position = 2
     Max reach = max(4, 2 + 1) = 4

Index:  0  1  2  3  4
Value:  2  3  1  1  4
                 ^
                 |
     Current position = 3
     Max reach = max(4, 3 + 1) = 4

Index:  0  1  2  3  4
Value:  2  3  1  1  4
                    ^
                    |
     Current position = 4
     We've reached the last index!
     Return true
```

Now let's visualize Example 2: `nums = [3,2,1,0,4]`

```
Index:  0  1  2  3  4
Value:  3  2  1  0  4
        ^
        |
     Current position = 0
     Max reach = 0 + 3 = 3

Index:  0  1  2  3  4
Value:  3  2  1  0  4
           ^
           |
     Current position = 1
     Max reach = max(3, 1 + 2) = 3

Index:  0  1  2  3  4
Value:  3  2  1  0  4
              ^
              |
     Current position = 2
     Max reach = max(3, 2 + 1) = 3

Index:  0  1  2  3  4
Value:  3  2  1  0  4
                 ^
                 |
     Current position = 3
     Max reach = max(3, 3 + 0) = 3

Index:  0  1  2  3  4
Value:  3  2  1  0  4
                    ^
                    |
     Current position = 4
     But we can't reach position 4 because max reach is only 3!
     Return false
```

![Jump Game Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize a variable `max_reach` to 0, representing the furthest position we can reach.
2. Iterate through the array:
   - If the current position is beyond `max_reach`, return `false` (we can't reach this position).
   - Update `max_reach` to be the maximum of the current `max_reach` and the position we can reach from the current index (current index + jump length).
   - If `max_reach` is at or beyond the last index, return `true` (we can reach the end).
3. If we've iterated through the entire array without returning, return `true` (we can reach the end).

## Code Implementation

### Python

```python
def canJump(nums):
    max_reach = 0
    
    for i in range(len(nums)):
        # If current position is beyond max_reach, we can't reach here
        if i > max_reach:
            return False
        
        # Update max_reach
        max_reach = max(max_reach, i + nums[i])
        
        # If we can reach the last index, return True
        if max_reach >= len(nums) - 1:
            return True
    
    # If we've gone through the entire array, we can reach the end
    return True
```

### Python (Optimized)

```python
def canJump(nums):
    max_reach = 0
    
    for i in range(len(nums)):
        # If current position is beyond max_reach, we can't reach here
        if i > max_reach:
            return False
        
        # Update max_reach
        max_reach = max(max_reach, i + nums[i])
        
        # If we can reach the last index, return True
        if max_reach >= len(nums) - 1:
            return True
    
    # This line is actually unreachable because we either return True when max_reach >= len(nums) - 1,
    # or return False when i > max_reach. But we'll keep it for clarity.
    return True
```

### JavaScript

```javascript
function canJump(nums) {
    let maxReach = 0;
    
    for (let i = 0; i < nums.length; i++) {
        // If current position is beyond maxReach, we can't reach here
        if (i > maxReach) {
            return false;
        }
        
        // Update maxReach
        maxReach = Math.max(maxReach, i + nums[i]);
        
        // If we can reach the last index, return true
        if (maxReach >= nums.length - 1) {
            return true;
        }
    }
    
    // If we've gone through the entire array, we can reach the end
    return true;
}
```

### JavaScript (Optimized)

```javascript
function canJump(nums) {
    let maxReach = 0;
    
    for (let i = 0; i < nums.length; i++) {
        // If current position is beyond maxReach, we can't reach here
        if (i > maxReach) {
            return false;
        }
        
        // Update maxReach
        maxReach = Math.max(maxReach, i + nums[i]);
        
        // If we can reach the last index, return true
        if (maxReach >= nums.length - 1) {
            return true;
        }
    }
    
    // This line is actually unreachable because we either return true when maxReach >= nums.length - 1,
    // or return false when i > maxReach. But we'll keep it for clarity.
    return true;
}
```

## Alternative Approach: Working Backwards

Another approach is to work backwards from the last index. We start from the second-to-last index and check if we can reach the last index from there. If we can, we update our target to be this index and continue working backwards. If we eventually reach the first index, we can reach the end.

### Python (Working Backwards)

```python
def canJump(nums):
    n = len(nums)
    target = n - 1
    
    for i in range(n - 2, -1, -1):
        if i + nums[i] >= target:
            target = i
    
    return target == 0
```

### JavaScript (Working Backwards)

```javascript
function canJump(nums) {
    const n = nums.length;
    let target = n - 1;
    
    for (let i = n - 2; i >= 0; i--) {
        if (i + nums[i] >= target) {
            target = i;
        }
    }
    
    return target === 0;
}
```

## Time and Space Complexity

### Time Complexity
- **O(n)**, where n is the length of the array. We iterate through the array once.

### Space Complexity
- **O(1)**, as we only use a constant amount of extra space.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 1: `nums = [2,3,1,1,4]`

```
Initialize max_reach = 0

i = 0, nums[0] = 2:
- i <= max_reach? Yes (0 <= 0)
- Update max_reach = max(0, 0 + 2) = 2
- max_reach >= len(nums) - 1? No (2 < 4)

i = 1, nums[1] = 3:
- i <= max_reach? Yes (1 <= 2)
- Update max_reach = max(2, 1 + 3) = 4
- max_reach >= len(nums) - 1? Yes (4 >= 4)
- Return True
```

Let's trace through the algorithm with Example 2: `nums = [3,2,1,0,4]`

```
Initialize max_reach = 0

i = 0, nums[0] = 3:
- i <= max_reach? Yes (0 <= 0)
- Update max_reach = max(0, 0 + 3) = 3
- max_reach >= len(nums) - 1? No (3 < 4)

i = 1, nums[1] = 2:
- i <= max_reach? Yes (1 <= 3)
- Update max_reach = max(3, 1 + 2) = 3
- max_reach >= len(nums) - 1? No (3 < 4)

i = 2, nums[2] = 1:
- i <= max_reach? Yes (2 <= 3)
- Update max_reach = max(3, 2 + 1) = 3
- max_reach >= len(nums) - 1? No (3 < 4)

i = 3, nums[3] = 0:
- i <= max_reach? Yes (3 <= 3)
- Update max_reach = max(3, 3 + 0) = 3
- max_reach >= len(nums) - 1? No (3 < 4)

i = 4, nums[4] = 4:
- i <= max_reach? No (4 > 3)
- Return False
```

## Edge Cases and Optimizations

- **Single Element**: If the array has only one element, we're already at the last index, so return `true`.
- **Zero at First Position**: If the first element is 0 and the array has more than one element, we can't move forward, so return `false`.
- **Early Termination**: We can return `true` as soon as `max_reach` is at or beyond the last index, which can save some iterations.

## Common Mistakes

1. **Not handling the case where the current position is beyond max_reach**: If we can't reach the current position, we should return `false`.
2. **Not updating max_reach correctly**: Make sure to take the maximum of the current `max_reach` and the position we can reach from the current index.
3. **Not checking if we can reach the last index**: We should return `true` as soon as `max_reach` is at or beyond the last index.

## Related Problems

1. **Jump Game II**: Find the minimum number of jumps to reach the last index.
2. **Jump Game III**: Given an array of non-negative integers, you are initially positioned at the first index of the array. Each element in the array represents your maximum jump length at that position. Your goal is to reach the last index in the minimum number of jumps.
3. **Jump Game IV**: Given an array of integers arr, you are initially positioned at the first index of the array. In one step you can jump from index i to index i + 1, i - 1, or to any index j such that arr[i] == arr[j] and i != j. Return the minimum number of steps to reach the last index of the array.
4. **Jump Game V**: Given an array of integers arr and an integer d. In one step you can jump from index i to index i + x where: i + x >= 0 and i + x < arr.length and 0 < x <= d and arr[i] > arr[i+x]. You can't jump outside of the array at any time. Return the maximum number of jumps you can make in the array. 