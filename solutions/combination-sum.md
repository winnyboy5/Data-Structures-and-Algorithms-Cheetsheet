# Combination Sum

## Problem Statement

Given an array of distinct integers `candidates` and a target integer `target`, return a list of all unique combinations of `candidates` where the chosen numbers sum to `target`. You may return the combinations in any order.

The same number may be chosen from `candidates` an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

**Example 1:**
```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 + 2 + 3 = 7
7 = 7
```

**Example 2:**
```
Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
Explanation:
2 + 2 + 2 + 2 = 8
2 + 3 + 3 = 8
3 + 5 = 8
```

**Example 3:**
```
Input: candidates = [2], target = 1
Output: []
Explanation: There are no combinations that sum to 1.
```

## Intuition

The key insight for this problem is to use a backtracking approach. Backtracking is a general algorithm for finding all (or some) solutions to computational problems, particularly constraint satisfaction problems, that incrementally builds candidates to the solutions, and abandons a candidate ("backtracks") as soon as it determines that the candidate cannot possibly be completed to a valid solution.

For this problem, we can:
1. Start with an empty combination and a remaining target.
2. For each candidate, we can either include it in our combination or exclude it.
3. If we include a candidate, we reduce the remaining target by the value of the candidate.
4. We continue this process recursively until the remaining target becomes 0 (we found a valid combination) or negative (we need to backtrack).

## Visual Explanation

Let's visualize the solution with Example 1: `candidates = [2,3,6,7], target = 7`

```
Start with an empty combination [] and target = 7

For candidate 2:
  Include 2: [] -> [2], remaining target = 7 - 2 = 5
    For candidate 2:
      Include 2: [2] -> [2,2], remaining target = 5 - 2 = 3
        For candidate 2:
          Include 2: [2,2] -> [2,2,2], remaining target = 3 - 2 = 1
            For candidate 2:
              Include 2: [2,2,2] -> [2,2,2,2], remaining target = 1 - 2 = -1 (invalid, backtrack)
              Exclude 2, move to next candidate
            For candidate 3:
              Include 3: [2,2,2] -> [2,2,2,3], remaining target = 1 - 3 = -2 (invalid, backtrack)
              Exclude 3, move to next candidate
            For candidate 6:
              Include 6: [2,2,2] -> [2,2,2,6], remaining target = 1 - 6 = -5 (invalid, backtrack)
              Exclude 6, move to next candidate
            For candidate 7:
              Include 7: [2,2,2] -> [2,2,2,7], remaining target = 1 - 7 = -6 (invalid, backtrack)
              Exclude 7, no more candidates, backtrack
          Exclude 2, move to next candidate
        For candidate 3:
          Include 3: [2,2] -> [2,2,3], remaining target = 3 - 3 = 0 (valid combination: [2,2,3])
          Exclude 3, move to next candidate
        For candidate 6:
          Include 6: [2,2] -> [2,2,6], remaining target = 3 - 6 = -3 (invalid, backtrack)
          Exclude 6, move to next candidate
        For candidate 7:
          Include 7: [2,2] -> [2,2,7], remaining target = 3 - 7 = -4 (invalid, backtrack)
          Exclude 7, no more candidates, backtrack
      Exclude 2, move to next candidate
    ... (similar process for other candidates)

For candidate 7:
  Include 7: [] -> [7], remaining target = 7 - 7 = 0 (valid combination: [7])
  Exclude 7, no more candidates, backtrack

Result: [[2,2,3],[7]]
```

![Combination Sum Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Sort the candidates array (optional, but can help with pruning).
2. Use a recursive backtracking function that takes:
   - The current combination built so far
   - The remaining target
   - The starting index in the candidates array
3. Base cases:
   - If the remaining target is 0, we've found a valid combination, add it to the result.
   - If the remaining target is negative, the combination is invalid, return.
4. Recursive case:
   - For each candidate starting from the given index:
     - Include the candidate in the current combination.
     - Recursively call the function with the updated combination, reduced target, and the same starting index (since we can reuse the same candidate).
     - Backtrack by removing the candidate from the combination.
5. Return all valid combinations found.

## Code Implementation

### Python

```python
def combinationSum(candidates, target):
    result = []
    
    def backtrack(curr_combination, remaining_target, start_index):
        # Base cases
        if remaining_target == 0:
            result.append(curr_combination[:])  # Make a copy of the current combination
            return
        if remaining_target < 0:
            return
        
        # Recursive case
        for i in range(start_index, len(candidates)):
            curr_combination.append(candidates[i])
            # We can reuse the same element, so we pass i as the start_index
            backtrack(curr_combination, remaining_target - candidates[i], i)
            curr_combination.pop()  # Backtrack
    
    backtrack([], target, 0)
    return result
```

### JavaScript

```javascript
function combinationSum(candidates, target) {
    const result = [];
    
    function backtrack(currCombination, remainingTarget, startIndex) {
        // Base cases
        if (remainingTarget === 0) {
            result.push([...currCombination]);  // Make a copy of the current combination
            return;
        }
        if (remainingTarget < 0) {
            return;
        }
        
        // Recursive case
        for (let i = startIndex; i < candidates.length; i++) {
            currCombination.push(candidates[i]);
            // We can reuse the same element, so we pass i as the startIndex
            backtrack(currCombination, remainingTarget - candidates[i], i);
            currCombination.pop();  // Backtrack
        }
    }
    
    backtrack([], target, 0);
    return result;
}
```

## Time and Space Complexity

- **Time Complexity**: O(N^(T/M)), where N is the number of candidates, T is the target value, and M is the minimum value among the candidates. In the worst case, the algorithm explores all possible combinations, which can be exponential.
- **Space Complexity**: O(T/M) for the recursion stack, where T is the target value and M is the minimum value among the candidates. This represents the maximum depth of the recursion tree.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2: `candidates = [2,3,5], target = 8`

```
Start with backtrack([], 8, 0)

For i = 0 (candidate = 2):
  Include 2: [] -> [2], remaining target = 8 - 2 = 6
  Call backtrack([2], 6, 0)
  
    For i = 0 (candidate = 2):
      Include 2: [2] -> [2,2], remaining target = 6 - 2 = 4
      Call backtrack([2,2], 4, 0)
      
        For i = 0 (candidate = 2):
          Include 2: [2,2] -> [2,2,2], remaining target = 4 - 2 = 2
          Call backtrack([2,2,2], 2, 0)
          
            For i = 0 (candidate = 2):
              Include 2: [2,2,2] -> [2,2,2,2], remaining target = 2 - 2 = 0
              We found a valid combination: [2,2,2,2]
              Backtrack: [2,2,2,2] -> [2,2,2]
            
            For i = 1 (candidate = 3):
              Include 3: [2,2,2] -> [2,2,2,3], remaining target = 2 - 3 = -1 (invalid)
              Backtrack: [2,2,2,3] -> [2,2,2]
            
            For i = 2 (candidate = 5):
              Include 5: [2,2,2] -> [2,2,2,5], remaining target = 2 - 5 = -3 (invalid)
              Backtrack: [2,2,2,5] -> [2,2,2]
            
          Backtrack: [2,2,2] -> [2,2]
        
        For i = 1 (candidate = 3):
          Include 3: [2,2] -> [2,2,3], remaining target = 4 - 3 = 1
          Call backtrack([2,2,3], 1, 1)
          
            For i = 1 (candidate = 3):
              Include 3: [2,2,3] -> [2,2,3,3], remaining target = 1 - 3 = -2 (invalid)
              Backtrack: [2,2,3,3] -> [2,2,3]
            
            For i = 2 (candidate = 5):
              Include 5: [2,2,3] -> [2,2,3,5], remaining target = 1 - 5 = -4 (invalid)
              Backtrack: [2,2,3,5] -> [2,2,3]
            
          Backtrack: [2,2,3] -> [2,2]
        
        For i = 2 (candidate = 5):
          Include 5: [2,2] -> [2,2,5], remaining target = 4 - 5 = -1 (invalid)
          Backtrack: [2,2,5] -> [2,2]
        
      Backtrack: [2,2] -> [2]
    
    For i = 1 (candidate = 3):
      Include 3: [2] -> [2,3], remaining target = 6 - 3 = 3
      Call backtrack([2,3], 3, 1)
      
        For i = 1 (candidate = 3):
          Include 3: [2,3] -> [2,3,3], remaining target = 3 - 3 = 0
          We found a valid combination: [2,3,3]
          Backtrack: [2,3,3] -> [2,3]
        
        For i = 2 (candidate = 5):
          Include 5: [2,3] -> [2,3,5], remaining target = 3 - 5 = -2 (invalid)
          Backtrack: [2,3,5] -> [2,3]
        
      Backtrack: [2,3] -> [2]
    
    For i = 2 (candidate = 5):
      Include 5: [2] -> [2,5], remaining target = 6 - 5 = 1
      Call backtrack([2,5], 1, 2)
      
        For i = 2 (candidate = 5):
          Include 5: [2,5] -> [2,5,5], remaining target = 1 - 5 = -4 (invalid)
          Backtrack: [2,5,5] -> [2,5]
        
      Backtrack: [2,5] -> [2]
    
  Backtrack: [2] -> []

For i = 1 (candidate = 3):
  Include 3: [] -> [3], remaining target = 8 - 3 = 5
  Call backtrack([3], 5, 1)
  
    For i = 1 (candidate = 3):
      Include 3: [3] -> [3,3], remaining target = 5 - 3 = 2
      Call backtrack([3,3], 2, 1)
      
        For i = 1 (candidate = 3):
          Include 3: [3,3] -> [3,3,3], remaining target = 2 - 3 = -1 (invalid)
          Backtrack: [3,3,3] -> [3,3]
        
        For i = 2 (candidate = 5):
          Include 5: [3,3] -> [3,3,5], remaining target = 2 - 5 = -3 (invalid)
          Backtrack: [3,3,5] -> [3,3]
        
      Backtrack: [3,3] -> [3]
    
    For i = 2 (candidate = 5):
      Include 5: [3] -> [3,5], remaining target = 5 - 5 = 0
      We found a valid combination: [3,5]
      Backtrack: [3,5] -> [3]
    
  Backtrack: [3] -> []

For i = 2 (candidate = 5):
  Include 5: [] -> [5], remaining target = 8 - 5 = 3
  Call backtrack([5], 3, 2)
  
    For i = 2 (candidate = 5):
      Include 5: [5] -> [5,5], remaining target = 3 - 5 = -2 (invalid)
      Backtrack: [5,5] -> [5]
    
  Backtrack: [5] -> []

Result: [[2,2,2,2], [2,3,3], [3,5]]
```

## Edge Cases and Optimizations

- **Empty Candidates Array**: If the candidates array is empty, there can be no combinations that sum to the target, so the result is an empty array.
- **Target is 0**: If the target is 0, the only valid combination is an empty array. However, the problem statement specifies that we need to find combinations of candidates, so we should return an empty result.
- **No Valid Combinations**: If there are no combinations that sum to the target, the result is an empty array.

**Optimizations**:
1. **Sorting the Candidates**: Sorting the candidates array can help with pruning. If we encounter a candidate that is greater than the remaining target, we can break the loop since all subsequent candidates will also be greater.
2. **Early Termination**: If the remaining target becomes negative, we can immediately return without exploring further.

## Common Mistakes

1. **Not handling duplicates correctly**: The problem states that the same number can be chosen multiple times. Make sure to pass the current index as the start index in the recursive call, not the next index.
2. **Forgetting to backtrack**: After exploring a path, we need to remove the last added candidate to backtrack and explore other paths.
3. **Not making a copy of the current combination**: When adding a valid combination to the result, we need to make a copy of the current combination, not just add a reference to it.

## Related Problems

1. **Combination Sum II**: Find all unique combinations in candidates where the candidate numbers sum to target. Each number in candidates may only be used once in the combination.
2. **Combination Sum III**: Find all possible combinations of k numbers that add up to a number n, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.
3. **Subsets**: Given an integer array nums of unique elements, return all possible subsets (the power set).
4. **Permutations**: Given an array nums of distinct integers, return all the possible permutations. 