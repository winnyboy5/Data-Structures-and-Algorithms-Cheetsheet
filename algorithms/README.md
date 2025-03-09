# Algorithms

Algorithms are step-by-step procedures for solving problems or performing tasks. This section covers common algorithms, their implementations, and analysis.

## Overview

This section covers the following algorithm categories:

1. [Sorting](./sorting.md) - Arranging elements in a specific order
2. [Searching](./searching.md) - Finding elements in a data structure
3. [Recursion](./recursion.md) - Solving problems by breaking them down into smaller instances
4. [Dynamic Programming](./dynamic-programming.md) - Breaking problems into overlapping subproblems
5. [Greedy Algorithms](./greedy.md) - Making locally optimal choices at each step
6. [Backtracking](./backtracking.md) - Building solutions incrementally and abandoning them when they fail
7. [Graph Algorithms](./graph-algorithms.md) - Solving problems related to graph structures
8. [Bit Manipulation](./bit-manipulation.md) - Manipulating data at the bit level

## Algorithm Selection Guide

| Problem Type | Recommended Algorithms |
|--------------|------------------------|
| Sorting small arrays | Insertion Sort, Bubble Sort |
| Sorting large arrays | Merge Sort, Quick Sort, Heap Sort |
| Searching sorted arrays | Binary Search |
| Searching unsorted arrays | Linear Search, Hash Tables |
| Optimization with overlapping subproblems | Dynamic Programming |
| Optimization with optimal substructure | Greedy Algorithms |
| Constraint satisfaction | Backtracking |
| Path finding | BFS, Dijkstra's, A* |
| Minimum spanning tree | Kruskal's, Prim's |
| Topological ordering | Topological Sort |

## Time Complexity Comparison

| Algorithm | Best Case | Average Case | Worst Case | Space Complexity |
|-----------|-----------|--------------|------------|------------------|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) |
| Linear Search | O(1) | O(n) | O(n) | O(1) |
| Binary Search | O(1) | O(log n) | O(log n) | O(1) |
| BFS/DFS | O(V + E) | O(V + E) | O(V + E) | O(V) |
| Dijkstra's | O(E + V log V) | O(E + V log V) | O(E + V log V) | O(V) |

V = number of vertices, E = number of edges

## Algorithm Design Techniques

1. **Divide and Conquer**: Break the problem into smaller subproblems, solve them recursively, and combine their solutions.
   - Examples: Merge Sort, Quick Sort, Binary Search

2. **Dynamic Programming**: Solve complex problems by breaking them down into simpler overlapping subproblems.
   - Examples: Fibonacci, Knapsack Problem, Longest Common Subsequence

3. **Greedy Approach**: Make locally optimal choices at each step with the hope of finding a global optimum.
   - Examples: Huffman Coding, Dijkstra's Algorithm, Fractional Knapsack

4. **Backtracking**: Build solutions incrementally and abandon a solution as soon as it's determined to be invalid.
   - Examples: N-Queens, Sudoku Solver, Hamiltonian Path

5. **Branch and Bound**: Systematically enumerate candidate solutions by means of state space search.
   - Examples: Traveling Salesman Problem, 0/1 Knapsack 