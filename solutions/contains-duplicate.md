# Contains Duplicate

## Problem Statement

Given an integer array `nums`, return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.

**Example 1:**
```
Input: nums = [1,2,3,1]
Output: true
Explanation: The value 1 appears twice in the array.
```

**Example 2:**
```
Input: nums = [1,2,3,4]
Output: false
Explanation: All elements in the array are distinct.
```

**Example 3:**
```
Input: nums = [1,1,1,3,3,4,3,2,4,2]
Output: true
Explanation: Both values 1, 2, 3, and 4 appear multiple times in the array.
```

## Intuition

The key insight for this problem is to use a hash set (or hash table) to keep track of elements we've already seen. As we iterate through the array, we check if the current element is already in the set. If it is, we've found a duplicate and can return `true`. If not, we add the element to the set and continue.

This approach allows us to solve the problem in a single pass through the array with O(1) lookups in the hash set, resulting in an O(n) time complexity solution.

## Visual Explanation

Let's visualize the solution with Example 1: `nums = [1,2,3,1]`

```
Step 1: Initialize an empty set: {}
        Current element: nums[0] = 1
        Is 1 in the set? No
        Add 1 to the set: {1}

Step 2: Current element: nums[1] = 2
        Is 2 in the set? No
        Add 2 to the set: {1, 2}

Step 3: Current element: nums[2] = 3
        Is 3 in the set? No
        Add 3 to the set: {1, 2, 3}

Step 4: Current element: nums[3] = 1
        Is 1 in the set? Yes
        Return true
```

![Contains Duplicate Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize an empty hash set to store elements we've seen.
2. Iterate through the array:
   - For each element, check if it's already in the hash set:
     - If it is, return `true` (we've found a duplicate).
     - If it's not, add it to the hash set.
3. If we finish iterating through the array without finding any duplicates, return `false`.

## Code Implementation

### Python

```python
def containsDuplicate(nums):
    # Initialize a hash set to store elements we've seen
    seen = set()
    
    # Iterate through the array
    for num in nums:
        # Check if the element is already in the set
        if num in seen:
            return True
        
        # Add the element to the set
        seen.add(num)
    
    # If we've gone through the entire array without finding duplicates
    return False
```

### Python (Alternative)

```python
def containsDuplicate(nums):
    # Compare the length of the array with the length of its set
    # If they're different, there must be duplicates
    return len(nums) > len(set(nums))
```

### JavaScript

```javascript
function containsDuplicate(nums) {
    // Initialize a hash set to store elements we've seen
    const seen = new Set();
    
    // Iterate through the array
    for (let i = 0; i < nums.length; i++) {
        // Check if the element is already in the set
        if (seen.has(nums[i])) {
            return true;
        }
        
        // Add the element to the set
        seen.add(nums[i]);
    }
    
    // If we've gone through the entire array without finding duplicates
    return false;
}
```

### JavaScript (Alternative)

```javascript
function containsDuplicate(nums) {
    // Compare the length of the array with the length of its set
    // If they're different, there must be duplicates
    return nums.length > new Set(nums).size;
}
```

## Time and Space Complexity

- **Time Complexity**: O(n), where n is the length of the array. We iterate through the array once, and hash set operations (lookup and insertion) are O(1) on average.
- **Space Complexity**: O(n) in the worst case, where all elements are distinct and we need to store all of them in the hash set.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 3: `nums = [1,1,1,3,3,4,3,2,4,2]`

```
Initialize seen = {}

Step 1: num = 1
        Is 1 in seen? No
        Add 1 to seen: {1}

Step 2: num = 1
        Is 1 in seen? Yes
        Return true
```

The algorithm correctly identifies that the array contains duplicates and returns `true` after finding the second occurrence of 1.

## Alternative Approaches

### Sorting Approach

Another approach is to sort the array first and then check for adjacent duplicates:

```python
def containsDuplicate(nums):
    # Sort the array
    nums.sort()
    
    # Check for adjacent duplicates
    for i in range(1, len(nums)):
        if nums[i] == nums[i - 1]:
            return True
    
    return False
```

This approach has a time complexity of O(n log n) due to the sorting operation and a space complexity of O(1) if we're allowed to modify the input array.

### Brute Force Approach

A straightforward but inefficient approach is to check all pairs of elements:

```python
def containsDuplicate(nums):
    n = len(nums)
    for i in range(n):
        for j in range(i + 1, n):
            if nums[i] == nums[j]:
                return True
    
    return False
```

This approach has a time complexity of O(n²) and a space complexity of O(1).

## Edge Cases and Optimizations

- **Empty Array**: If the array is empty, there are no duplicates, so the function will return `false`.
- **Single Element**: If the array has only one element, there are no duplicates, so the function will return `false`.
- **All Duplicates**: If all elements in the array are the same, the function will return `true` after checking the second element.
- **Large Arrays**: For very large arrays, the hash set approach is still efficient, but memory usage could be a concern.

## Common Mistakes

1. **Not handling empty arrays**: Always check if the input array is empty.
2. **Using a list instead of a set for lookups**: Lists have O(n) lookup time, which would make the algorithm O(n²) overall. Sets have O(1) lookup time on average.
3. **Not considering the space complexity**: The hash set approach uses O(n) extra space, which might be a concern for very large arrays.

## Related Problems

1. **Contains Duplicate II**: Check if there are duplicates within a specific distance.
2. **Contains Duplicate III**: Check if there are duplicates within a specific range of values and distance.
3. **Find the Duplicate Number**: Find the duplicate in an array where each integer appears exactly once except for one integer which appears multiple times.
4. **Find All Duplicates in an Array**: Find all duplicates in an array where each integer appears once or twice. 