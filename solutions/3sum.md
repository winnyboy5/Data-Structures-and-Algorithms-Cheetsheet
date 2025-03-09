# 3Sum

## Problem Statement

Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

**Example 1:**
```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
Explanation: 
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.
```

**Example 2:**
```
Input: nums = [0,1,1]
Output: []
Explanation: The only possible triplet does not sum up to 0.
```

**Example 3:**
```
Input: nums = [0,0,0]
Output: [[0,0,0]]
Explanation: The only possible triplet sums up to 0.
```

## Intuition

The brute force approach would be to use three nested loops to check all possible triplets, but that would be O(n³), which is too slow for large inputs. Instead, we can optimize by:

1. Sorting the array first (O(n log n))
2. Fixing one element and using the two-pointer technique to find pairs that sum to the negative of the fixed element (O(n²))

This reduces the overall time complexity to O(n²).

## Visual Explanation

Let's visualize the solution with Example 1: `nums = [-1,0,1,2,-1,-4]`

First, we sort the array: `nums = [-4,-1,-1,0,1,2]`

```
Step 1: Fix the first element nums[0] = -4
        Target sum = 0 - (-4) = 4
        Use two pointers to find pairs that sum to 4:
        - Left pointer at index 1 (value = -1)
        - Right pointer at index 5 (value = 2)
        - Sum = -1 + 2 = 1 < 4, so move left pointer right
        - Left pointer at index 2 (value = -1)
        - Sum = -1 + 2 = 1 < 4, so move left pointer right
        - Left pointer at index 3 (value = 0)
        - Sum = 0 + 2 = 2 < 4, so move left pointer right
        - Left pointer at index 4 (value = 1)
        - Sum = 1 + 2 = 3 < 4, so move left pointer right
        - Left pointer = right pointer, so no valid pairs found

Step 2: Fix the first element nums[1] = -1
        Target sum = 0 - (-1) = 1
        Use two pointers to find pairs that sum to 1:
        - Left pointer at index 2 (value = -1)
        - Right pointer at index 5 (value = 2)
        - Sum = -1 + 2 = 1 == 1, so add triplet [-1,-1,2]
        - Move left pointer right and right pointer left
        - Left pointer at index 3 (value = 0)
        - Right pointer at index 4 (value = 1)
        - Sum = 0 + 1 = 1 == 1, so add triplet [-1,0,1]
        - Move left pointer right and right pointer left
        - Left pointer = right pointer, so no more valid pairs

Step 3: Fix the first element nums[2] = -1
        This is a duplicate of nums[1], so skip to avoid duplicate triplets

Step 4: Fix the first element nums[3] = 0
        Target sum = 0 - 0 = 0
        Use two pointers to find pairs that sum to 0:
        - Left pointer at index 4 (value = 1)
        - Right pointer at index 5 (value = 2)
        - Sum = 1 + 2 = 3 > 0, so move right pointer left
        - Left pointer = right pointer, so no valid pairs found

Step 5: Fix the first element nums[4] = 1
        Target sum = 0 - 1 = -1
        Use two pointers to find pairs that sum to -1:
        - Left pointer at index 5 (value = 2)
        - Sum = 2 > -1, so move right pointer left
        - Left pointer = right pointer, so no valid pairs found

Result: [[-1,-1,2],[-1,0,1]]
```

![3Sum Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Sort the input array in ascending order.
2. Iterate through the array with a pointer `i` for the first element of the triplet:
   - If the current element is the same as the previous one, skip it to avoid duplicates.
   - Use two pointers, `left` and `right`, to find pairs that sum to the negative of `nums[i]`:
     - Initialize `left = i + 1` and `right = n - 1`.
     - While `left < right`:
       - Calculate the sum of `nums[left] + nums[right]`.
       - If the sum is less than the target, increment `left`.
       - If the sum is greater than the target, decrement `right`.
       - If the sum equals the target, add the triplet to the result and move both pointers.
       - Skip duplicate elements for both `left` and `right` pointers.
3. Return the result.

## Code Implementation

### Python

```python
def threeSum(nums):
    result = []
    nums.sort()
    n = len(nums)
    
    for i in range(n):
        # Skip duplicate elements for the first position
        if i > 0 and nums[i] == nums[i-1]:
            continue
        
        # Use two pointers to find pairs that sum to -nums[i]
        left, right = i + 1, n - 1
        target = -nums[i]
        
        while left < right:
            current_sum = nums[left] + nums[right]
            
            if current_sum < target:
                left += 1
            elif current_sum > target:
                right -= 1
            else:
                # Found a triplet
                result.append([nums[i], nums[left], nums[right]])
                
                # Skip duplicate elements for the second position
                while left < right and nums[left] == nums[left+1]:
                    left += 1
                
                # Skip duplicate elements for the third position
                while left < right and nums[right] == nums[right-1]:
                    right -= 1
                
                # Move both pointers
                left += 1
                right -= 1
    
    return result
```

### JavaScript

```javascript
function threeSum(nums) {
    const result = [];
    nums.sort((a, b) => a - b);
    const n = nums.length;
    
    for (let i = 0; i < n; i++) {
        // Skip duplicate elements for the first position
        if (i > 0 && nums[i] === nums[i-1]) {
            continue;
        }
        
        // Use two pointers to find pairs that sum to -nums[i]
        let left = i + 1;
        let right = n - 1;
        const target = -nums[i];
        
        while (left < right) {
            const currentSum = nums[left] + nums[right];
            
            if (currentSum < target) {
                left++;
            } else if (currentSum > target) {
                right--;
            } else {
                // Found a triplet
                result.push([nums[i], nums[left], nums[right]]);
                
                // Skip duplicate elements for the second position
                while (left < right && nums[left] === nums[left+1]) {
                    left++;
                }
                
                // Skip duplicate elements for the third position
                while (left < right && nums[right] === nums[right-1]) {
                    right--;
                }
                
                // Move both pointers
                left++;
                right--;
            }
        }
    }
    
    return result;
}
```

## Time and Space Complexity

- **Time Complexity**: O(n²), where n is the length of the input array. Sorting takes O(n log n), and the two-pointer approach takes O(n²).
- **Space Complexity**: O(1) excluding the output array. The space used for the result doesn't count towards the space complexity.

## Detailed Walkthrough with Example

Let's trace through the algorithm with a simpler example: `nums = [-1,0,1,2,-1,-4]`

After sorting: `nums = [-4,-1,-1,0,1,2]`

```
i = 0, nums[i] = -4
    target = -(-4) = 4
    left = 1, right = 5
    nums[left] + nums[right] = -1 + 2 = 1 < 4, so left++
    left = 2, right = 5
    nums[left] + nums[right] = -1 + 2 = 1 < 4, so left++
    left = 3, right = 5
    nums[left] + nums[right] = 0 + 2 = 2 < 4, so left++
    left = 4, right = 5
    nums[left] + nums[right] = 1 + 2 = 3 < 4, so left++
    left = 5, right = 5 (left == right), so break

i = 1, nums[i] = -1
    target = -(-1) = 1
    left = 2, right = 5
    nums[left] + nums[right] = -1 + 2 = 1 == 1, so add triplet [-1,-1,2]
    left = 3, right = 4 (after skipping duplicates)
    nums[left] + nums[right] = 0 + 1 = 1 == 1, so add triplet [-1,0,1]
    left = 4, right = 3 (left > right), so break

i = 2, nums[i] = -1
    Skip because nums[i] == nums[i-1]

i = 3, nums[i] = 0
    target = -(0) = 0
    left = 4, right = 5
    nums[left] + nums[right] = 1 + 2 = 3 > 0, so right--
    left = 4, right = 4 (left == right), so break

i = 4, nums[i] = 1
    target = -(1) = -1
    left = 5, right = 5 (left == right), so break

i = 5, nums[i] = 2
    left = 6, right = 5 (left > right), so break

Result: [[-1,-1,2],[-1,0,1]]
```

## Edge Cases and Optimizations

- **Empty Array**: If the array is empty or has fewer than 3 elements, the function will return an empty array.
- **All Zeros**: If the array contains all zeros, the function will return [[0,0,0]] (assuming there are at least 3 zeros).
- **Early Termination**: If the current element is greater than 0, we can break early since all subsequent elements will be greater than 0 (after sorting), and three positive numbers cannot sum to 0.

## Common Mistakes

1. **Not handling duplicates**: It's important to skip duplicate elements to avoid duplicate triplets in the result.
2. **Using a hash set approach without sorting**: While a hash set can be used to find pairs that sum to a target, sorting the array first makes it easier to handle duplicates.
3. **Not using the two-pointer technique**: The two-pointer technique is crucial for achieving O(n²) time complexity.

## Related Problems

1. **Two Sum**: Find all pairs of elements that sum to a target value.
2. **4Sum**: Find all quadruplets that sum to a target value.
3. **3Sum Closest**: Find a triplet that sums closest to a target value.
4. **3Sum Smaller**: Count triplets with sum less than a target value. 