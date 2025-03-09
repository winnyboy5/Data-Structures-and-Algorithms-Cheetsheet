# Cyclic Sort Pattern

The Cyclic Sort pattern is an efficient sorting algorithm specifically designed for arrays containing a range of numbers. It's particularly useful when dealing with arrays where the values are in a given range (e.g., 1 to n).

## Visual Representation

```
Original array: [3, 1, 5, 4, 2]
Range: 1 to 5

Step 1: Check if element at index 0 (value 3) is at the correct position
        3 should be at index 2, so swap [3, 1, 5, 4, 2] → [5, 1, 3, 4, 2]

Step 2: Check if element at index 0 (value 5) is at the correct position
        5 should be at index 4, so swap [5, 1, 3, 4, 2] → [2, 1, 3, 4, 5]

Step 3: Check if element at index 0 (value 2) is at the correct position
        2 should be at index 1, so swap [2, 1, 3, 4, 5] → [1, 2, 3, 4, 5]

Step 4: Check if element at index 0 (value 1) is at the correct position
        1 should be at index 0, so it's already in the correct position

Step 5: Move to index 1 and continue the process
        All elements are now in their correct positions: [1, 2, 3, 4, 5]
```

## When to Use Cyclic Sort

- When dealing with arrays containing numbers in a given range (e.g., 1 to n)
- When asked to find missing numbers, duplicate numbers, or smallest missing positive numbers
- When the problem involves sorting an array with minimal space complexity
- When the problem hints at placing each number at its correct index

## Basic Cyclic Sort Implementation

### Python

```python
def cyclic_sort(nums):
    i = 0
    while i < len(nums):
        # Correct position for nums[i] is nums[i] - 1 (assuming 1-indexed values)
        correct_position = nums[i] - 1
        if nums[i] != nums[correct_position]:
            # Swap the elements
            nums[i], nums[correct_position] = nums[correct_position], nums[i]
        else:
            i += 1
    return nums

# Example usage
arr = [3, 1, 5, 4, 2]
print(cyclic_sort(arr))  # Output: [1, 2, 3, 4, 5]
```

### JavaScript

```javascript
function cyclicSort(nums) {
    let i = 0;
    while (i < nums.length) {
        // Correct position for nums[i] is nums[i] - 1 (assuming 1-indexed values)
        const correctPosition = nums[i] - 1;
        if (nums[i] !== nums[correctPosition]) {
            // Swap the elements
            [nums[i], nums[correctPosition]] = [nums[correctPosition], nums[i]];
        } else {
            i++;
        }
    }
    return nums;
}

// Example usage
const arr = [3, 1, 5, 4, 2];
console.log(cyclicSort(arr));  // Output: [1, 2, 3, 4, 5]
```

## Common Problems and Solutions

### 1. Find the Missing Number

**Problem:** Given an array containing n distinct numbers taken from 0 to n, find the missing number.

**Python Solution:**
```python
def find_missing_number(nums):
    i, n = 0, len(nums)
    
    while i < n:
        # Correct position for nums[i] is nums[i] (assuming 0-indexed values)
        correct_position = nums[i]
        
        # If the number is within range and not at its correct position
        if nums[i] < n and nums[i] != nums[correct_position]:
            # Swap the elements
            nums[i], nums[correct_position] = nums[correct_position], nums[i]
        else:
            i += 1
    
    # Find the missing number by checking which index doesn't have the correct value
    for i in range(n):
        if nums[i] != i:
            return i
    
    # If all numbers from 0 to n-1 are present, then n is missing
    return n

# Example usage
arr = [3, 0, 1]
print(find_missing_number(arr))  # Output: 2
```

**JavaScript Solution:**
```javascript
function findMissingNumber(nums) {
    let i = 0;
    const n = nums.length;
    
    while (i < n) {
        // Correct position for nums[i] is nums[i] (assuming 0-indexed values)
        const correctPosition = nums[i];
        
        // If the number is within range and not at its correct position
        if (nums[i] < n && nums[i] !== nums[correctPosition]) {
            // Swap the elements
            [nums[i], nums[correctPosition]] = [nums[correctPosition], nums[i]];
        } else {
            i++;
        }
    }
    
    // Find the missing number by checking which index doesn't have the correct value
    for (i = 0; i < n; i++) {
        if (nums[i] !== i) {
            return i;
        }
    }
    
    // If all numbers from 0 to n-1 are present, then n is missing
    return n;
}

// Example usage
const arr = [3, 0, 1];
console.log(findMissingNumber(arr));  // Output: 2
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

### 2. Find All Missing Numbers

**Problem:** Given an array of integers where 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once. Find all the elements of [1, n] inclusive that do not appear in this array.

**Python Solution:**
```python
def find_all_missing_numbers(nums):
    i, n = 0, len(nums)
    
    while i < n:
        # Correct position for nums[i] is nums[i] - 1 (assuming 1-indexed values)
        correct_position = nums[i] - 1
        
        # If the number is not at its correct position
        if nums[i] != nums[correct_position]:
            # Swap the elements
            nums[i], nums[correct_position] = nums[correct_position], nums[i]
        else:
            i += 1
    
    missing_numbers = []
    
    # Find all missing numbers by checking which indices don't have the correct values
    for i in range(n):
        if nums[i] != i + 1:
            missing_numbers.append(i + 1)
    
    return missing_numbers

# Example usage
arr = [4, 3, 2, 7, 8, 2, 3, 1]
print(find_all_missing_numbers(arr))  # Output: [5, 6]
```

**JavaScript Solution:**
```javascript
function findAllMissingNumbers(nums) {
    let i = 0;
    const n = nums.length;
    
    while (i < n) {
        // Correct position for nums[i] is nums[i] - 1 (assuming 1-indexed values)
        const correctPosition = nums[i] - 1;
        
        // If the number is not at its correct position
        if (nums[i] !== nums[correctPosition]) {
            // Swap the elements
            [nums[i], nums[correctPosition]] = [nums[correctPosition], nums[i]];
        } else {
            i++;
        }
    }
    
    const missingNumbers = [];
    
    // Find all missing numbers by checking which indices don't have the correct values
    for (i = 0; i < n; i++) {
        if (nums[i] !== i + 1) {
            missingNumbers.push(i + 1);
        }
    }
    
    return missingNumbers;
}

// Example usage
const arr = [4, 3, 2, 7, 8, 2, 3, 1];
console.log(findAllMissingNumbers(arr));  // Output: [5, 6]
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1) (excluding the output array)

### 3. Find the Duplicate Number

**Problem:** Given an array of integers containing n + 1 integers where each integer is in the range [1, n] inclusive, there is only one repeated number. Find this repeated number.

**Python Solution:**
```python
def find_duplicate(nums):
    i = 0
    
    while i < len(nums):
        # If the current number is not at the correct index
        if nums[i] != i + 1:
            # Correct position for nums[i] is nums[i] - 1
            correct_position = nums[i] - 1
            
            # If the number is already at its correct position, we found the duplicate
            if nums[i] == nums[correct_position]:
                return nums[i]
            
            # Otherwise, swap the elements
            nums[i], nums[correct_position] = nums[correct_position], nums[i]
        else:
            i += 1
    
    return -1  # No duplicate found

# Example usage
arr = [1, 3, 4, 2, 2]
print(find_duplicate(arr))  # Output: 2
```

**JavaScript Solution:**
```javascript
function findDuplicate(nums) {
    let i = 0;
    
    while (i < nums.length) {
        // If the current number is not at the correct index
        if (nums[i] !== i + 1) {
            // Correct position for nums[i] is nums[i] - 1
            const correctPosition = nums[i] - 1;
            
            // If the number is already at its correct position, we found the duplicate
            if (nums[i] === nums[correctPosition]) {
                return nums[i];
            }
            
            // Otherwise, swap the elements
            [nums[i], nums[correctPosition]] = [nums[correctPosition], nums[i]];
        } else {
            i++;
        }
    }
    
    return -1;  // No duplicate found
}

// Example usage
const arr = [1, 3, 4, 2, 2];
console.log(findDuplicate(arr));  // Output: 2
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

### 4. Find All Duplicate Numbers

**Problem:** Given an array of integers where 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once. Find all the elements that appear twice in this array.

**Python Solution:**
```python
def find_all_duplicates(nums):
    i = 0
    duplicates = []
    
    while i < len(nums):
        # Correct position for nums[i] is nums[i] - 1
        correct_position = nums[i] - 1
        
        # If the number is not at its correct position
        if nums[i] != nums[correct_position]:
            # Swap the elements
            nums[i], nums[correct_position] = nums[correct_position], nums[i]
        else:
            i += 1
    
    # Find all duplicates by checking which indices don't have the correct values
    for i in range(len(nums)):
        if nums[i] != i + 1:
            duplicates.append(nums[i])
    
    return duplicates

# Example usage
arr = [4, 3, 2, 7, 8, 2, 3, 1]
print(find_all_duplicates(arr))  # Output: [2, 3]
```

**JavaScript Solution:**
```javascript
function findAllDuplicates(nums) {
    let i = 0;
    const duplicates = [];
    
    while (i < nums.length) {
        // Correct position for nums[i] is nums[i] - 1
        const correctPosition = nums[i] - 1;
        
        // If the number is not at its correct position
        if (nums[i] !== nums[correctPosition]) {
            // Swap the elements
            [nums[i], nums[correctPosition]] = [nums[correctPosition], nums[i]];
        } else {
            i++;
        }
    }
    
    // Find all duplicates by checking which indices don't have the correct values
    for (i = 0; i < nums.length; i++) {
        if (nums[i] !== i + 1) {
            duplicates.push(nums[i]);
        }
    }
    
    return duplicates;
}

// Example usage
const arr = [4, 3, 2, 7, 8, 2, 3, 1];
console.log(findAllDuplicates(arr));  // Output: [2, 3]
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1) (excluding the output array)

### 5. Find the First Missing Positive

**Problem:** Given an unsorted integer array, find the smallest missing positive integer.

**Python Solution:**
```python
def first_missing_positive(nums):
    i, n = 0, len(nums)
    
    while i < n:
        # Correct position for nums[i] is nums[i] - 1
        correct_position = nums[i] - 1
        
        # If the number is positive, within range, and not at its correct position
        if 0 < nums[i] <= n and nums[i] != nums[correct_position]:
            # Swap the elements
            nums[i], nums[correct_position] = nums[correct_position], nums[i]
        else:
            i += 1
    
    # Find the first missing positive by checking which index doesn't have the correct value
    for i in range(n):
        if nums[i] != i + 1:
            return i + 1
    
    # If all numbers from 1 to n are present, then n+1 is the first missing positive
    return n + 1

# Example usage
arr = [3, 4, -1, 1]
print(first_missing_positive(arr))  # Output: 2
```

**JavaScript Solution:**
```javascript
function firstMissingPositive(nums) {
    let i = 0;
    const n = nums.length;
    
    while (i < n) {
        // Correct position for nums[i] is nums[i] - 1
        const correctPosition = nums[i] - 1;
        
        // If the number is positive, within range, and not at its correct position
        if (nums[i] > 0 && nums[i] <= n && nums[i] !== nums[correctPosition]) {
            // Swap the elements
            [nums[i], nums[correctPosition]] = [nums[correctPosition], nums[i]];
        } else {
            i++;
        }
    }
    
    // Find the first missing positive by checking which index doesn't have the correct value
    for (i = 0; i < n; i++) {
        if (nums[i] !== i + 1) {
            return i + 1;
        }
    }
    
    // If all numbers from 1 to n are present, then n+1 is the first missing positive
    return n + 1;
}

// Example usage
const arr = [3, 4, -1, 1];
console.log(firstMissingPositive(arr));  // Output: 2
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

## Time and Space Complexity

| Algorithm | Time Complexity | Space Complexity |
|-----------|----------------|-----------------|
| Cyclic Sort | O(n) | O(1) |

## Tips and Tricks for Cyclic Sort

1. **Identify the Range**: Determine the range of values in the array (e.g., 1 to n, 0 to n).

2. **Calculate Correct Position**: For 1-indexed values, the correct position is `value - 1`. For 0-indexed values, the correct position is `value`.

3. **Handle Out-of-Range Values**: When dealing with arrays that might contain values outside the expected range, add conditions to skip those values.

4. **Swap, Don't Move**: Always swap elements to their correct positions rather than moving them directly.

5. **Check for Duplicates**: When swapping, check if the element at the target position is already the same as the current element to avoid infinite loops with duplicates.

6. **Post-Processing**: After sorting, scan the array to find missing or duplicate numbers based on the problem requirements.

## Common Pitfalls

1. **Off-by-One Errors**: Be careful with the mapping between values and indices, especially when dealing with 0-indexed or 1-indexed values.

2. **Infinite Loops**: Without proper handling of duplicates, the algorithm can get stuck in an infinite loop.

3. **Out-of-Range Values**: Forgetting to handle values outside the expected range can lead to index out of bounds errors.

4. **Modifying the Input**: Some problems may require preserving the original array, so make sure to create a copy if needed.

5. **Overlooking Edge Cases**: Don't forget to handle empty arrays or arrays with a single element.

## How to Identify Cyclic Sort Problems

Look for these clues in the problem statement:

1. The problem involves an array containing numbers in a given range (e.g., 1 to n).
2. The problem asks to find missing numbers, duplicate numbers, or the smallest missing positive.
3. The problem requires sorting the array with O(n) time complexity and O(1) space complexity.
4. The problem hints at placing each number at its correct index.
5. Keywords like "missing number," "duplicate number," or "first missing positive."

## Common Cyclic Sort Problems from Blind 75 and Grind 75

1. **Find the Missing Number** (Easy): Find the missing number in an array containing n distinct numbers from 0 to n.
2. **Find All Missing Numbers** (Easy): Find all numbers that are missing from an array of integers where 1 ≤ a[i] ≤ n.
3. **Find the Duplicate Number** (Medium): Find the duplicate number in an array of integers where each integer appears exactly once except for one.
4. **Find All Duplicates in an Array** (Medium): Find all duplicates in an array where each element appears once or twice.
5. **First Missing Positive** (Hard): Find the smallest missing positive integer in an unsorted array.
6. **Kth Missing Positive Number** (Easy): Find the kth positive integer that is missing from an array.
7. **Set Mismatch** (Easy): Find the number that occurs twice and the number that is missing in an array.

## Cyclic Sort Template

Here's a general template for solving cyclic sort problems:

```python
def cyclic_sort_template(nums):
    i = 0
    while i < len(nums):
        # Calculate the correct position based on the problem
        correct_position = get_correct_position(nums[i])
        
        # Check if the element needs to be swapped
        if should_swap(nums[i], nums[correct_position], correct_position):
            # Swap the elements
            nums[i], nums[correct_position] = nums[correct_position], nums[i]
        else:
            i += 1
    
    # Post-processing based on the problem requirements
    return post_process(nums)
```

## Real-World Applications

1. **Data Deduplication**: Finding and removing duplicate records in a database.
2. **Error Detection**: Identifying missing or duplicate packets in network communications.
3. **Data Integrity Checks**: Verifying that a sequence of numbers is complete without gaps.
4. **Resource Allocation**: Ensuring that each resource is assigned a unique identifier.
5. **Inventory Management**: Tracking missing or duplicate items in an inventory system.
6. **Sequence Verification**: Checking if a sequence of events or transactions is complete. 