# Algorithm Patterns

Algorithm patterns are reusable approaches to solving specific types of problems. Recognizing these patterns can help you quickly identify and solve similar problems.

## Overview

This section covers the following algorithm patterns:

1. [Two Pointers](./two-pointers.md) - Using two pointers to traverse data
2. [Sliding Window](./sliding-window.md) - Efficiently processing contiguous subarrays
3. [Fast & Slow Pointers](./fast-slow-pointers.md) - Detecting cycles and finding middle elements
4. [Merge Intervals](./merge-intervals.md) - Dealing with overlapping intervals
5. [Cyclic Sort](./cyclic-sort.md) - Efficiently sorting numbers in a given range
6. [In-place Reversal of Linked List](./in-place-reversal-of-linked-list.md) - Reversing linked lists without extra space
7. [Tree BFS/DFS](./tree-traversal.md) - Breadth-first and depth-first tree traversals
8. [Topological Sort](./topological-sort.md) - Ordering tasks with dependencies
9. [Binary Search](./binary-search.md) - Efficiently searching sorted arrays
10. [Subsets](./subsets.md) - Generating all possible subsets, permutations, and combinations
11. [Modified Binary Search](./modified-binary-search.md) - Variations of binary search
12. [Top K Elements](./top-k-elements.md) - Finding the top/smallest K elements
13. [K-way Merge](./k-way-merge.md) - Merging K sorted arrays
14. [Greedy Algorithms](./greedy-algorithms.md) - Making locally optimal choices
15. [Backtracking](./backtracking.md) - Building solutions incrementally and undoing choices

## Pattern Recognition Guide

| If the problem involves... | Consider using... |
|----------------------------|-------------------|
| Sorted array, find a pair with target sum | Two Pointers |
| Find subarrays with a given condition | Sliding Window |
| Linked list with cycle or finding middle | Fast & Slow Pointers |
| Overlapping intervals | Merge Intervals |
| Array with numbers in range [1...n] | Cyclic Sort |
| Reversing a linked list | In-place Reversal |
| Level-by-level tree processing | Tree BFS |
| Exploring all paths in a tree | Tree DFS |
| Tasks with dependencies | Topological Sort |
| Sorted array, efficient search | Binary Search |
| Generate all combinations/permutations | Subsets |
| Find a specific element in a sorted array | Modified Binary Search |
| Find the K largest/smallest elements | Top K Elements |
| Merge K sorted arrays/lists | K-way Merge |
| Optimization problems with local choices | Greedy Algorithms |
| Finding all possible solutions | Backtracking |

## How to Identify the Right Pattern

1. **Analyze the input data structure**:
   - Is it an array, linked list, tree, or graph?
   - Is the data sorted or unsorted?

2. **Understand the problem constraints**:
   - Are there specific time/space complexity requirements?
   - Is in-place modification required?

3. **Look for key phrases**:
   - "Find a pair/triplet" → Two Pointers
   - "Subarray/substring with condition" → Sliding Window
   - "Detect cycle" → Fast & Slow Pointers
   - "Merge overlapping intervals" → Merge Intervals
   - "Numbers in range 1 to n" → Cyclic Sort
   - "Level order traversal" → Tree BFS
   - "All paths from root to leaf" → Tree DFS
   - "Task scheduling with dependencies" → Topological Sort
   - "Efficiently search in sorted array" → Binary Search
   - "All subsets/combinations/permutations" → Subsets
   - "K largest/smallest elements" → Top K Elements
   - "Locally optimal choices" → Greedy Algorithms
   - "All possible solutions" or "Generate all valid combinations" → Backtracking

4. **Consider the problem's objective**:
   - Finding elements vs. transforming data
   - Counting vs. optimizing
   - Checking existence vs. finding all instances 