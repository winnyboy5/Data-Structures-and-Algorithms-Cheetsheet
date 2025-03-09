# Two Sum

## Problem Statement

Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

**Example 1:**
```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

**Example 2:**
```
Input: nums = [3,2,4], target = 6
Output: [1,2]
Explanation: Because nums[1] + nums[2] == 6, we return [1, 2].
```

**Example 3:**
```
Input: nums = [3,3], target = 6
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 6, we return [0, 1].
```

## Difficulty Level: ★★☆☆☆ (Easy)

## Prerequisite Knowledge
- Arrays
- Hash Tables/Maps
- Basic time and space complexity analysis

## Intuition

The key insight for this problem is to use a hash map (dictionary) to store the elements we've seen so far and their indices. For each element in the array, we check if the complement (`target - current element`) exists in our hash map. If it does, we've found our pair. If not, we add the current element and its index to the hash map and continue.

This approach allows us to solve the problem in a single pass through the array with O(1) lookups in the hash map, resulting in an O(n) time complexity solution.

## Visual Explanation

Let's visualize the solution with Example 1: `nums = [2,7,11,15], target = 9`

```
Step 1: Initialize an empty hash map: {}
        Current element: nums[0] = 2
        Complement: target - nums[0] = 9 - 2 = 7
        Is 7 in the hash map? No
        Add 2 to the hash map: {2: 0}

Step 2: Current element: nums[1] = 7
        Complement: target - nums[1] = 9 - 7 = 2
        Is 2 in the hash map? Yes, at index 0
        Return [0, 1]
```

![Two Sum Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize an empty hash map to store elements and their indices.
2. Iterate through the array:
   - For each element, calculate its complement (`target - current element`).
   - Check if the complement exists in the hash map:
     - If it does, return the indices of the complement and the current element.
     - If it doesn't, add the current element and its index to the hash map.
3. If no solution is found after iterating through the array, return an empty array (though the problem guarantees a solution exists).

## Code Implementation

### Python

```python
def twoSum(nums, target):
    # Initialize a hash map to store elements and their indices
    num_map = {}
    
    # Iterate through the array
    for i, num in enumerate(nums):
        # Calculate the complement
        complement = target - num
        
        # Check if the complement exists in the hash map
        if complement in num_map:
            # Return the indices of the complement and the current element
            return [num_map[complement], i]
        
        # Add the current element and its index to the hash map
        num_map[num] = i
    
    # If no solution is found (though the problem guarantees one exists)
    return []
```

### JavaScript

```javascript
function twoSum(nums, target) {
    // Initialize a hash map to store elements and their indices
    const numMap = {};
    
    // Iterate through the array
    for (let i = 0; i < nums.length; i++) {
        // Calculate the complement
        const complement = target - nums[i];
        
        // Check if the complement exists in the hash map
        if (complement in numMap) {
            // Return the indices of the complement and the current element
            return [numMap[complement], i];
        }
        
        // Add the current element and its index to the hash map
        numMap[nums[i]] = i;
    }
    
    // If no solution is found (though the problem guarantees one exists)
    return [];
}
```

## Language-Specific Optimizations

### Python
```python
# Using defaultdict for cleaner code
from collections import defaultdict

def twoSum(nums, target):
    # defaultdict will return a default value (in this case -1) for non-existent keys
    num_map = defaultdict(lambda: -1)
    
    for i, num in enumerate(nums):
        complement = target - num
        if num_map[complement] != -1:
            return [num_map[complement], i]
        num_map[num] = i
    
    return []

# Using list comprehension for a more concise approach (though less efficient)
def twoSum_concise(nums, target):
    num_map = {}
    for i, num in enumerate(nums):
        if target - num in num_map:
            return [num_map[target - num], i]
        num_map[num] = i
    return []
```

### JavaScript
```javascript
// Using Map instead of object for better performance with non-string keys
function twoSum(nums, target) {
    const numMap = new Map();
    
    for (let i = 0; i < nums.length; i++) {
        const complement = target - nums[i];
        
        if (numMap.has(complement)) {
            return [numMap.get(complement), i];
        }
        
        numMap.set(nums[i], i);
    }
    
    return [];
}

// Using array methods for a more functional approach
function twoSum_functional(nums, target) {
    const numMap = new Map();
    
    const result = nums.findIndex((num, i) => {
        const complement = target - num;
        if (numMap.has(complement)) {
            // We found a match, but findIndex only returns the current index
            // So we store both indices for later
            result = [numMap.get(complement), i];
            return true;
        }
        numMap.set(num, i);
        return false;
    });
    
    return result !== -1 ? result : [];
}
```

## Time and Space Complexity

- **Time Complexity**: O(n), where n is the length of the array. We iterate through the array once, and hash map operations (lookup and insertion) are O(1) on average.
- **Space Complexity**: O(n) in the worst case, where we might need to store almost all elements in the hash map before finding a solution.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2: `nums = [3,2,4], target = 6`

```
Initialize num_map = {}

Step 1: i = 0, num = 3
        complement = target - num = 6 - 3 = 3
        Is 3 in num_map? No
        Add 3 to num_map: {3: 0}

Step 2: i = 1, num = 2
        complement = target - num = 6 - 2 = 4
        Is 4 in num_map? No
        Add 2 to num_map: {3: 0, 2: 1}

Step 3: i = 2, num = 4
        complement = target - num = 6 - 4 = 2
        Is 2 in num_map? Yes, at index 1
        Return [1, 2]
```

## Alternative Approaches

### Brute Force Approach

A straightforward approach is to use nested loops to check all possible pairs:

```python
def twoSum(nums, target):
    n = len(nums)
    for i in range(n):
        for j in range(i + 1, n):
            if nums[i] + nums[j] == target:
                return [i, j]
    return []
```

This approach has a time complexity of O(n²) and a space complexity of O(1).

### Two-Pointer Approach (for sorted arrays)

If the array is sorted, we can use two pointers starting from both ends of the array:

```python
def twoSum(nums, target):
    # Note: This returns the values, not indices, and assumes the array is sorted
    left, right = 0, len(nums) - 1
    while left < right:
        current_sum = nums[left] + nums[right]
        if current_sum == target:
            return [left, right]
        elif current_sum < target:
            left += 1
        else:
            right -= 1
    return []
```

This approach has a time complexity of O(n) but requires the array to be sorted. If the array is not sorted, sorting it would take O(n log n) time, making the overall time complexity O(n log n).

## Approach Comparison

| Approach | Time Complexity | Space Complexity | Pros | Cons |
|----------|----------------|------------------|------|------|
| Hash Map | O(n) | O(n) | Fast, single pass | Uses extra space |
| Brute Force | O(n²) | O(1) | No extra space | Slow for large inputs |
| Two Pointers | O(n) + O(n log n) for sorting | O(1) | No extra space | Requires sorting, loses original indices |

## Common Bugs and Debugging Tips

### 1. Using the Same Element Twice

**Problem**: Accidentally using the same array element twice.

**Example**:
```python
# Incorrect implementation
def twoSum_buggy(nums, target):
    for i in range(len(nums)):
        complement = target - nums[i]
        if complement in nums:  # This might find the same element!
            j = nums.index(complement)
            return [i, j]
    return []
```

**Debugging Tips**:
- Make sure to check if the complement's index is different from the current index
- Use a hash map to store indices, not just presence
- Add elements to the hash map after checking for the complement

### 2. Returning Values Instead of Indices

**Problem**: Returning the values that sum to the target instead of their indices.

**Example**:
```javascript
// Incorrect implementation
function twoSum_buggy(nums, target) {
    for (let i = 0; i < nums.length; i++) {
        for (let j = i + 1; j < nums.length; j++) {
            if (nums[i] + nums[j] === target) {
                return [nums[i], nums[j]];  // Wrong! Should return indices
            }
        }
    }
    return [];
}
```

**Debugging Tips**:
- Carefully read the problem statement to understand what to return
- Add comments to clarify what each variable represents
- Test with examples where values and indices are different

### 3. Hash Map Key Type Issues

**Problem**: In JavaScript, object keys are always strings, which can cause issues.

**Example**:
```javascript
// Potentially buggy implementation
function twoSum_buggy(nums, target) {
    const numMap = {};
    
    for (let i = 0; i < nums.length; i++) {
        const complement = target - nums[i];
        
        if (numMap[complement] !== undefined) {  // Be careful with 0 indices!
            return [numMap[complement], i];
        }
        
        numMap[nums[i]] = i;  // nums[i] becomes a string key
    }
    
    return [];
}
```

**Debugging Tips**:
- Use `Map` instead of plain objects in JavaScript
- Check for existence with `hasOwnProperty` or explicit `!== undefined`
- Be careful with falsy values like 0 that might evaluate incorrectly in conditions

## Real-world Applications

The Two Sum problem demonstrates techniques that are used in various real-world applications:

### 1. Financial Systems
In trading systems, finding pairs of assets that sum to a particular value is common for arbitrage opportunities or portfolio balancing.

```python
def find_portfolio_pairs(assets, target_value):
    """Find pairs of assets that sum to a target value for portfolio construction."""
    value_map = {}
    pairs = []
    
    for asset_id, value in assets.items():
        complement = target_value - value
        if complement in value_map:
            pairs.append((asset_id, value_map[complement]))
        value_map[value] = asset_id
    
    return pairs
```

### 2. Database Query Optimization
Hash-based joins in databases use similar techniques to efficiently match records from different tables.

### 3. Network Routing
In network routing algorithms, finding pairs of nodes that satisfy certain distance constraints uses similar hash-based lookups.

### 4. Cryptographic Applications
In some cryptographic protocols, finding pairs of values with specific properties (like hash collisions) uses similar techniques.

## Interactive Learning Resources

### Practice Similar Problems
- [LeetCode - Two Sum II (Sorted Array)](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
- [LeetCode - 3Sum](https://leetcode.com/problems/3sum/)
- [HackerRank - Pairs](https://www.hackerrank.com/challenges/pairs/problem)

### Visualizers
- [VisuAlgo - Two Sum Visualization](https://visualgo.net/en)
- [Algorithm Visualizer - Hash Table Operations](https://algorithm-visualizer.org/)

### Interactive Code Playground
Try this problem in an interactive environment:
- [Python Tutor - Two Sum](http://pythontutor.com/visualize.html#code=def%20two_sum%28nums,%20target%29%3A%0A%20%20%20%20num_map%20%3D%20%7B%7D%0A%20%20%20%20%0A%20%20%20%20for%20i,%20num%20in%20enumerate%28nums%29%3A%0A%20%20%20%20%20%20%20%20complement%20%3D%20target%20-%20num%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20if%20complement%20in%20num_map%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20return%20%5Bnum_map%5Bcomplement%5D,%20i%5D%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20num_map%5Bnum%5D%20%3D%20i%0A%20%20%20%20%0A%20%20%20%20return%20%5B%5D%0A%0A%23%20Test%20with%20example%0Anums%20%3D%20%5B2,%207,%2011,%2015%5D%0Atarget%20%3D%209%0Aprint%28two_sum%28nums,%20target%29%29&cumulative=false&curInstr=0&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=3&rawInputLstJSON=%5B%5D&textReferences=false)

## Edge Cases and Optimizations

- **Empty Array**: The problem guarantees a solution, so we don't need to handle this case.
- **Array with Only One Element**: The problem requires two distinct elements, so an array with only one element would not have a valid solution.
- **Multiple Solutions**: The problem guarantees exactly one solution, so we don't need to handle multiple solutions.
- **No Solution**: The problem guarantees a solution, so we don't need to handle this case.
- **Large Arrays**: For very large arrays, consider using more memory-efficient data structures or streaming approaches.

## Testing Strategy

Here's how to thoroughly test your Two Sum implementation:

```python
def test_two_sum():
    # Test cases with different array sizes
    assert sorted(twoSum([2, 7, 11, 15], 9)) == [0, 1]  # Basic case
    assert sorted(twoSum([3, 2, 4], 6)) == [1, 2]  # Multiple possible pairs
    assert sorted(twoSum([3, 3], 6)) == [0, 1]  # Duplicate numbers
    
    # Edge cases
    assert sorted(twoSum([1, 5, 8, -3], -2)) == [0, 3]  # Negative numbers
    assert sorted(twoSum([0, 0], 0)) == [0, 1]  # Zeros
    assert sorted(twoSum([1000000, 1000000], 2000000)) == [0, 1]  # Large numbers
    
    # Performance test for large arrays
    large_array = list(range(100000)) + [100000, 200000]
    assert sorted(twoSum(large_array, 300000)) == [100000, 100001]
    
    print("All tests passed!")

# Run tests
test_two_sum()
```

## Common Mistakes

1. **Using the same element twice**: Be careful not to use the same element twice. The problem requires two distinct elements.
2. **Returning values instead of indices**: Make sure to return the indices of the elements, not the elements themselves.
3. **Not checking if the complement exists before adding the current element**: This could lead to using the same element twice.
4. **Forgetting to handle edge cases**: While this problem has guarantees, real-world problems might require handling empty arrays, no solutions, etc.

## Related Problems

1. **Two Sum II - Input Array Is Sorted**: Similar to Two Sum, but the input array is sorted.
2. **3Sum**: Find all unique triplets in the array that give the sum of zero.
3. **4Sum**: Find all unique quadruplets in the array that give the sum of a target value.
4. **Two Sum Less Than K**: Find the maximum sum of any two elements less than or equal to a target value.
5. **Subarray Sum Equals K**: Find the number of subarrays that sum to a target value.

## Discussion Questions

1. How would you modify the solution to return all pairs that sum to the target, not just one?
2. What if the array is sorted? How would you optimize the solution?
3. How would you solve this problem with limited memory constraints?
4. Can you think of a way to solve this problem in a single pass without using extra space?
5. How would you extend this solution to find triplets that sum to a target (3Sum problem)? 