# Longest Consecutive Sequence

## Problem Statement

Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in O(n) time.

**Example 1:**
```
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```

**Example 2:**
```
Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9
Explanation: The longest consecutive elements sequence is [0, 1, 2, 3, 4, 5, 6, 7, 8]. Therefore its length is 9.
```

## Intuition

The key insight for this problem is to use a hash set to efficiently check if a number exists in the array. We can then iterate through the array and for each number, check if it's the start of a sequence by verifying if the number-1 exists in the set. If it doesn't exist, then the current number is the start of a sequence, and we can count how long this sequence is by checking for consecutive numbers (number+1, number+2, etc.) in the set.

This approach allows us to find the longest consecutive sequence in O(n) time, as each number is only visited once.

## Visual Explanation

Let's visualize the solution with Example 1:

```
nums = [100, 4, 200, 1, 3, 2]

Step 1: Create a hash set from the array
set = {100, 4, 200, 1, 3, 2}

Step 2: Iterate through each number in the array
- 100: Check if 99 exists in the set? No, so 100 is the start of a sequence
  - Count consecutive numbers: 100, 101, 102, ...
  - Only 100 exists, so the sequence length is 1
  - Current max length = 1

- 4: Check if 3 exists in the set? Yes, so 4 is not the start of a sequence
  - Skip to the next number

- 200: Check if 199 exists in the set? No, so 200 is the start of a sequence
  - Count consecutive numbers: 200, 201, 202, ...
  - Only 200 exists, so the sequence length is 1
  - Current max length = 1

- 1: Check if 0 exists in the set? No, so 1 is the start of a sequence
  - Count consecutive numbers: 1, 2, 3, 4, 5, ...
  - 1, 2, 3, 4 exist, so the sequence length is 4
  - Current max length = 4

- 3: Check if 2 exists in the set? Yes, so 3 is not the start of a sequence
  - Skip to the next number

- 2: Check if 1 exists in the set? Yes, so 2 is not the start of a sequence
  - Skip to the next number

Step 3: Return the maximum sequence length
Result: 4
```

![Longest Consecutive Sequence Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Create a hash set from the input array to allow O(1) lookups.
2. Initialize a variable `max_length` to track the length of the longest consecutive sequence.
3. Iterate through each number in the array:
   - If the number-1 is not in the set, then the current number is the start of a sequence.
   - Count how many consecutive numbers (number+1, number+2, etc.) exist in the set.
   - Update `max_length` if the current sequence is longer.
4. Return `max_length`.

## Code Implementation

### Python

```python
def longestConsecutive(nums):
    if not nums:
        return 0
    
    # Create a hash set from the array
    num_set = set(nums)
    max_length = 0
    
    # Iterate through each number in the array
    for num in num_set:
        # Check if the current number is the start of a sequence
        if num - 1 not in num_set:
            current_num = num
            current_length = 1
            
            # Count consecutive numbers
            while current_num + 1 in num_set:
                current_num += 1
                current_length += 1
            
            # Update max_length
            max_length = max(max_length, current_length)
    
    return max_length
```

### JavaScript

```javascript
function longestConsecutive(nums) {
    if (!nums.length) {
        return 0;
    }
    
    // Create a hash set from the array
    const numSet = new Set(nums);
    let maxLength = 0;
    
    // Iterate through each number in the array
    for (const num of numSet) {
        // Check if the current number is the start of a sequence
        if (!numSet.has(num - 1)) {
            let currentNum = num;
            let currentLength = 1;
            
            // Count consecutive numbers
            while (numSet.has(currentNum + 1)) {
                currentNum++;
                currentLength++;
            }
            
            // Update maxLength
            maxLength = Math.max(maxLength, currentLength);
        }
    }
    
    return maxLength;
}
```

## Time and Space Complexity

- **Time Complexity**: O(n), where n is the length of the input array. Although we have a nested while loop, each number is only visited once because we only check for consecutive numbers if the current number is the start of a sequence.
- **Space Complexity**: O(n) for the hash set that stores all the numbers from the input array.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2:

```
nums = [0, 3, 7, 2, 5, 8, 4, 6, 0, 1]

Step 1: Create a hash set from the array
set = {0, 3, 7, 2, 5, 8, 4, 6, 1}

Step 2: Iterate through each number in the array
- 0: Check if -1 exists in the set? No, so 0 is the start of a sequence
  - Count consecutive numbers: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, ...
  - 0, 1, 2, 3, 4, 5, 6, 7, 8 exist, so the sequence length is 9
  - Current max length = 9

- 3: Check if 2 exists in the set? Yes, so 3 is not the start of a sequence
  - Skip to the next number

- 7: Check if 6 exists in the set? Yes, so 7 is not the start of a sequence
  - Skip to the next number

- 2: Check if 1 exists in the set? Yes, so 2 is not the start of a sequence
  - Skip to the next number

- 5: Check if 4 exists in the set? Yes, so 5 is not the start of a sequence
  - Skip to the next number

- 8: Check if 7 exists in the set? Yes, so 8 is not the start of a sequence
  - Skip to the next number

- 4: Check if 3 exists in the set? Yes, so 4 is not the start of a sequence
  - Skip to the next number

- 6: Check if 5 exists in the set? Yes, so 6 is not the start of a sequence
  - Skip to the next number

- 1: Check if 0 exists in the set? Yes, so 1 is not the start of a sequence
  - Skip to the next number

Step 3: Return the maximum sequence length
Result: 9
```

## Edge Cases and Optimizations

- **Empty Array**: If the input array is empty, return 0.
- **Duplicate Numbers**: The hash set automatically handles duplicates, so we don't need to worry about them.
- **Negative Numbers**: The algorithm works for negative numbers as well.
- **Optimization**: We can iterate through the hash set instead of the original array to avoid checking duplicates.

## Common Mistakes

1. **Not checking if the number is the start of a sequence**: It's important to only count sequences starting from their first element to avoid redundant counting.
2. **Using sorting**: Sorting the array would result in an O(n log n) time complexity, which doesn't meet the requirement of O(n).
3. **Not handling duplicates**: If we don't use a hash set, we need to handle duplicates explicitly.

## Related Problems

1. **Longest Increasing Subsequence**: Find the length of the longest subsequence where all elements are in increasing order.
2. **Maximum Gap**: Find the maximum gap between two successive elements in a sorted form of the array.
3. **Contains Duplicate III**: Check if there are two distinct indices i and j such that the absolute difference between nums[i] and nums[j] is at most t and the absolute difference between i and j is at most k.
4. **Missing Number**: Find the missing number in an array containing n distinct numbers from 0 to n. 