# Subsets Pattern

The Subsets pattern is a technique used to generate all possible subsets (the power set) of a given set. This pattern is particularly useful for problems that require exploring all possible combinations or subsets of elements.

## Visual Representation

```
Set: [1, 2, 3]

Power Set (all subsets):
[]
[1]
[2]
[1, 2]
[3]
[1, 3]
[2, 3]
[1, 2, 3]
```

## When to Use Subsets Pattern

- When you need to find all possible combinations or subsets of a set
- When the problem requires exploring all possible ways to select elements
- When dealing with permutations, combinations, or power sets
- When the problem involves selecting or not selecting elements from a set
- When you need to generate all possible states of a system

## Basic Subsets Implementation

### Python Implementation (Iterative)

```python
def find_subsets(nums):
    # Start with an empty subset
    subsets = [[]]
    
    for num in nums:
        # For each existing subset, create a new subset by adding the current number
        n = len(subsets)
        for i in range(n):
            # Create a new subset by adding the current number to an existing subset
            subsets.append(subsets[i] + [num])
    
    return subsets

# Example usage
nums = [1, 2, 3]
print(find_subsets(nums))
# Output: [[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]]
```

### Python Implementation (Recursive)

```python
def find_subsets_recursive(nums):
    result = []
    
    def backtrack(index, current):
        # Add the current subset to the result
        result.append(current[:])
        
        # Explore all possible subsets by including elements from index onwards
        for i in range(index, len(nums)):
            # Include the current element
            current.append(nums[i])
            
            # Recursively generate subsets with the current element included
            backtrack(i + 1, current)
            
            # Backtrack (exclude the current element)
            current.pop()
    
    backtrack(0, [])
    return result

# Example usage
nums = [1, 2, 3]
print(find_subsets_recursive(nums))
# Output: [[], [1], [1, 2], [1, 2, 3], [1, 3], [2], [2, 3], [3]]
```

### JavaScript Implementation (Iterative)

```javascript
function findSubsets(nums) {
    // Start with an empty subset
    const subsets = [[]];
    
    for (const num of nums) {
        // For each existing subset, create a new subset by adding the current number
        const n = subsets.length;
        for (let i = 0; i < n; i++) {
            // Create a new subset by adding the current number to an existing subset
            subsets.push([...subsets[i], num]);
        }
    }
    
    return subsets;
}

// Example usage
const nums = [1, 2, 3];
console.log(findSubsets(nums));
// Output: [[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]]
```

### JavaScript Implementation (Recursive)

```javascript
function findSubsetsRecursive(nums) {
    const result = [];
    
    function backtrack(index, current) {
        // Add the current subset to the result
        result.push([...current]);
        
        // Explore all possible subsets by including elements from index onwards
        for (let i = index; i < nums.length; i++) {
            // Include the current element
            current.push(nums[i]);
            
            // Recursively generate subsets with the current element included
            backtrack(i + 1, current);
            
            // Backtrack (exclude the current element)
            current.pop();
        }
    }
    
    backtrack(0, []);
    return result;
}

// Example usage
const nums = [1, 2, 3];
console.log(findSubsetsRecursive(nums));
// Output: [[], [1], [1, 2], [1, 2, 3], [1, 3], [2], [2, 3], [3]]
```

## Common Problems and Solutions

### 1. Subsets

**Problem:** Given an integer array nums of unique elements, return all possible subsets (the power set).

**Python Solution:**
```python
def subsets(nums):
    result = []
    
    def backtrack(index, current):
        result.append(current[:])
        
        for i in range(index, len(nums)):
            current.append(nums[i])
            backtrack(i + 1, current)
            current.pop()
    
    backtrack(0, [])
    return result

# Example usage
nums = [1, 2, 3]
print(subsets(nums))
# Output: [[], [1], [1, 2], [1, 2, 3], [1, 3], [2], [2, 3], [3]]
```

**JavaScript Solution:**
```javascript
function subsets(nums) {
    const result = [];
    
    function backtrack(index, current) {
        result.push([...current]);
        
        for (let i = index; i < nums.length; i++) {
            current.push(nums[i]);
            backtrack(i + 1, current);
            current.pop();
        }
    }
    
    backtrack(0, []);
    return result;
}

// Example usage
const nums = [1, 2, 3];
console.log(subsets(nums));
// Output: [[], [1], [1, 2], [1, 2, 3], [1, 3], [2], [2, 3], [3]]
```

**Time Complexity:** O(2^n) where n is the length of the input array  
**Space Complexity:** O(2^n) for storing all subsets

### 2. Subsets II (with duplicates)

**Problem:** Given an integer array nums that may contain duplicates, return all possible subsets (the power set).

**Python Solution:**
```python
def subsets_with_dup(nums):
    # Sort the array to handle duplicates
    nums.sort()
    result = []
    
    def backtrack(index, current):
        result.append(current[:])
        
        for i in range(index, len(nums)):
            # Skip duplicates
            if i > index and nums[i] == nums[i - 1]:
                continue
            
            current.append(nums[i])
            backtrack(i + 1, current)
            current.pop()
    
    backtrack(0, [])
    return result

# Example usage
nums = [1, 2, 2]
print(subsets_with_dup(nums))
# Output: [[], [1], [1, 2], [1, 2, 2], [2], [2, 2]]
```

**JavaScript Solution:**
```javascript
function subsetsWithDup(nums) {
    // Sort the array to handle duplicates
    nums.sort((a, b) => a - b);
    const result = [];
    
    function backtrack(index, current) {
        result.push([...current]);
        
        for (let i = index; i < nums.length; i++) {
            // Skip duplicates
            if (i > index && nums[i] === nums[i - 1]) {
                continue;
            }
            
            current.push(nums[i]);
            backtrack(i + 1, current);
            current.pop();
        }
    }
    
    backtrack(0, []);
    return result;
}

// Example usage
const nums = [1, 2, 2];
console.log(subsetsWithDup(nums));
// Output: [[], [1], [1, 2], [1, 2, 2], [2], [2, 2]]
```

**Time Complexity:** O(2^n) where n is the length of the input array  
**Space Complexity:** O(2^n) for storing all subsets

### 3. Permutations

**Problem:** Given an array nums of distinct integers, return all the possible permutations.

**Python Solution:**
```python
def permute(nums):
    result = []
    
    def backtrack(current):
        # If the current permutation is complete
        if len(current) == len(nums):
            result.append(current[:])
            return
        
        for num in nums:
            # Skip if the number is already used
            if num in current:
                continue
            
            # Include the current number
            current.append(num)
            
            # Recursively generate permutations
            backtrack(current)
            
            # Backtrack
            current.pop()
    
    backtrack([])
    return result

# Example usage
nums = [1, 2, 3]
print(permute(nums))
# Output: [[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1]]
```

**JavaScript Solution:**
```javascript
function permute(nums) {
    const result = [];
    
    function backtrack(current) {
        // If the current permutation is complete
        if (current.length === nums.length) {
            result.push([...current]);
            return;
        }
        
        for (const num of nums) {
            // Skip if the number is already used
            if (current.includes(num)) {
                continue;
            }
            
            // Include the current number
            current.push(num);
            
            // Recursively generate permutations
            backtrack(current);
            
            // Backtrack
            current.pop();
        }
    }
    
    backtrack([]);
    return result;
}

// Example usage
const nums = [1, 2, 3];
console.log(permute(nums));
// Output: [[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1]]
```

**Time Complexity:** O(n!) where n is the length of the input array  
**Space Complexity:** O(n!) for storing all permutations

### 4. Combination Sum

**Problem:** Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations of candidates where the chosen numbers sum to target. You may return the combinations in any order.

**Python Solution:**
```python
def combination_sum(candidates, target):
    result = []
    
    def backtrack(index, current, remaining):
        # If the remaining sum is 0, we found a valid combination
        if remaining == 0:
            result.append(current[:])
            return
        
        # If the remaining sum is negative or we've used all candidates, stop
        if remaining < 0 or index >= len(candidates):
            return
        
        # Include the current candidate (multiple times if needed)
        current.append(candidates[index])
        backtrack(index, current, remaining - candidates[index])
        current.pop()
        
        # Skip the current candidate
        backtrack(index + 1, current, remaining)
    
    backtrack(0, [], target)
    return result

# Example usage
candidates = [2, 3, 6, 7]
target = 7
print(combination_sum(candidates, target))
# Output: [[2, 2, 3], [7]]
```

**JavaScript Solution:**
```javascript
function combinationSum(candidates, target) {
    const result = [];
    
    function backtrack(index, current, remaining) {
        // If the remaining sum is 0, we found a valid combination
        if (remaining === 0) {
            result.push([...current]);
            return;
        }
        
        // If the remaining sum is negative or we've used all candidates, stop
        if (remaining < 0 || index >= candidates.length) {
            return;
        }
        
        // Include the current candidate (multiple times if needed)
        current.push(candidates[index]);
        backtrack(index, current, remaining - candidates[index]);
        current.pop();
        
        // Skip the current candidate
        backtrack(index + 1, current, remaining);
    }
    
    backtrack(0, [], target);
    return result;
}

// Example usage
const candidates = [2, 3, 6, 7];
const target = 7;
console.log(combinationSum(candidates, target));
// Output: [[2, 2, 3], [7]]
```

**Time Complexity:** O(2^n) where n is the length of the candidates array  
**Space Complexity:** O(target/min) where min is the minimum value in candidates

### 5. Letter Combinations of a Phone Number

**Problem:** Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

**Python Solution:**
```python
def letter_combinations(digits):
    if not digits:
        return []
    
    # Mapping of digits to letters
    phone_map = {
        '2': 'abc',
        '3': 'def',
        '4': 'ghi',
        '5': 'jkl',
        '6': 'mno',
        '7': 'pqrs',
        '8': 'tuv',
        '9': 'wxyz'
    }
    
    result = []
    
    def backtrack(index, current):
        # If we've processed all digits, add the current combination
        if index == len(digits):
            result.append(''.join(current))
            return
        
        # Get the letters corresponding to the current digit
        letters = phone_map[digits[index]]
        
        # Try each letter
        for letter in letters:
            current.append(letter)
            backtrack(index + 1, current)
            current.pop()
    
    backtrack(0, [])
    return result

# Example usage
digits = "23"
print(letter_combinations(digits))
# Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]
```

**JavaScript Solution:**
```javascript
function letterCombinations(digits) {
    if (!digits) {
        return [];
    }
    
    // Mapping of digits to letters
    const phoneMap = {
        '2': 'abc',
        '3': 'def',
        '4': 'ghi',
        '5': 'jkl',
        '6': 'mno',
        '7': 'pqrs',
        '8': 'tuv',
        '9': 'wxyz'
    };
    
    const result = [];
    
    function backtrack(index, current) {
        // If we've processed all digits, add the current combination
        if (index === digits.length) {
            result.push(current.join(''));
            return;
        }
        
        // Get the letters corresponding to the current digit
        const letters = phoneMap[digits[index]];
        
        // Try each letter
        for (const letter of letters) {
            current.push(letter);
            backtrack(index + 1, current);
            current.pop();
        }
    }
    
    backtrack(0, []);
    return result;
}

// Example usage
const digits = "23";
console.log(letterCombinations(digits));
// Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]
```

**Time Complexity:** O(4^n) where n is the length of the input string (since some digits map to 4 letters)  
**Space Complexity:** O(n) for the recursion stack

## Time and Space Complexity

| Algorithm | Time Complexity | Space Complexity |
|-----------|----------------|-----------------|
| Subsets | O(2^n) | O(2^n) |
| Subsets II | O(2^n) | O(2^n) |
| Permutations | O(n!) | O(n!) |
| Combination Sum | O(2^n) | O(target/min) |
| Letter Combinations | O(4^n) | O(n) |

## Tips and Tricks for Subsets Pattern

1. **Backtracking Approach**: Use backtracking to generate all possible combinations by making choices and undoing them.

2. **Iterative Approach**: For simple subset generation, the iterative approach is often cleaner and easier to understand.

3. **Handling Duplicates**: Sort the input array first to easily skip duplicates during backtracking.

4. **Decision Tree**: Visualize the problem as a decision tree where at each step you decide whether to include an element or not.

5. **Base Case**: Always define a clear base case for your recursive function to avoid infinite recursion.

6. **Pruning**: Use pruning techniques to avoid exploring paths that won't lead to valid solutions.

7. **Memoization**: For problems with overlapping subproblems, consider using memoization to avoid redundant calculations.

## Common Pitfalls

1. **Modifying the Original Array**: Be careful not to modify the original array during backtracking.

2. **Deep Copy vs. Shallow Copy**: Make sure to create a deep copy of the current combination before adding it to the result.

3. **Infinite Recursion**: Ensure that your recursive function has a proper base case to avoid infinite recursion.

4. **Duplicate Solutions**: When dealing with duplicates, make sure to handle them correctly to avoid duplicate solutions.

5. **Memory Overflow**: For large inputs, the number of subsets can grow exponentially, potentially causing memory issues.

## How to Identify Subsets Pattern Problems

Look for these clues in the problem statement:

1. The problem asks for all possible combinations, subsets, or permutations.
2. The problem involves selecting elements from a set in different ways.
3. Keywords like "all possible ways," "combinations," "subsets," or "permutations."
4. The problem requires exploring all possible states or configurations.
5. The problem involves making choices at each step (include or exclude an element).

## Common Subsets Pattern Problems from Blind 75 and Grind 75

1. **Subsets** (Medium): Generate all possible subsets of a set of distinct integers.
2. **Subsets II** (Medium): Generate all possible subsets of a set that may contain duplicates.
3. **Permutations** (Medium): Generate all possible permutations of a set of distinct integers.
4. **Permutations II** (Medium): Generate all possible permutations of a set that may contain duplicates.
5. **Combination Sum** (Medium): Find all unique combinations of candidates that sum to a target.
6. **Combination Sum II** (Medium): Find all unique combinations of candidates that sum to a target, where each number may only be used once.
7. **Combination Sum III** (Medium): Find all possible combinations of k numbers that add up to a number n.
8. **Letter Combinations of a Phone Number** (Medium): Return all possible letter combinations that the number could represent.
9. **Palindrome Partitioning** (Medium): Partition a string such that every substring is a palindrome.
10. **Generate Parentheses** (Medium): Generate all combinations of well-formed parentheses.

## Subsets Pattern Template

Here's a general template for solving subsets pattern problems:

```python
def subsets_template(nums):
    result = []
    
    def backtrack(index, current):
        # Base case or processing of current state
        # (e.g., add current subset to result)
        process_current_state(current)
        
        # Explore all possible choices from the current state
        for i in range(index, len(nums)):
            # Make a choice
            current.append(nums[i])
            
            # Recursively explore with the current choice
            backtrack(i + 1, current)
            
            # Undo the choice (backtrack)
            current.pop()
    
    backtrack(0, [])
    return result
```

## Real-World Applications

1. **Feature Selection**: Selecting the best subset of features for a machine learning model.
2. **Menu Combinations**: Generating all possible combinations of items from a menu.
3. **Task Scheduling**: Finding all possible ways to schedule a set of tasks.
4. **Game States**: Exploring all possible states in a game like chess or tic-tac-toe.
5. **Circuit Design**: Analyzing all possible configurations of a circuit.
6. **Resource Allocation**: Finding all possible ways to allocate resources to different projects.
7. **Cryptography**: Generating all possible keys or combinations for encryption/decryption. 