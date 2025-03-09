# Data Structures and Algorithms Cheatsheet

A comprehensive guide to common data structures and algorithms with implementation examples in Python and JavaScript.

## Table of Contents

1. [Big O Notation](./big-o-notation.md)
2. [Data Structures](./data-structures/README.md)
   - [Arrays/Lists](./data-structures/arrays.md)
   - [Linked Lists](./data-structures/linked-lists.md)
   - [Stacks](./data-structures/stacks.md)
   - [Queues](./data-structures/queues.md)
   - [Hash Tables/Maps](./data-structures/hash-tables.md)
   - [Trees](./data-structures/trees.md)
   - [Heaps](./data-structures/heaps.md)
   - [Graphs](./data-structures/graphs.md)
   - [Tries](./data-structures/tries.md)
3. [Algorithms](./algorithms/README.md)
   - [Sorting](./algorithms/sorting.md)
   - [Searching](./algorithms/searching.md)
   - [Recursion](./algorithms/recursion.md)
   - [Dynamic Programming](./algorithms/dynamic-programming.md)
   - [Greedy Algorithms](./algorithms/greedy.md)
   - [Backtracking](./algorithms/backtracking.md)
   - [Graph Algorithms](./algorithms/graph-algorithms.md)
   - [Bit Manipulation](./algorithms/bit-manipulation.md)
4. [Common Patterns](./patterns/README.md)
   - [Two Pointers](./patterns/two-pointers.md)
   - [Sliding Window](./patterns/sliding-window.md)
   - [Fast & Slow Pointers](./patterns/fast-slow-pointers.md)
   - [Merge Intervals](./patterns/merge-intervals.md)
   - [Cyclic Sort](./patterns/cyclic-sort.md)
   - [In-place Reversal of Linked List](./patterns/in-place-reversal-of-linked-list.md)
   - [Tree BFS/DFS](./patterns/tree-traversal.md)
   - [Topological Sort](./patterns/topological-sort.md)
   - [Binary Search](./patterns/binary-search.md)
   - [Subsets](./patterns/subsets.md)
   - [Modified Binary Search](./patterns/modified-binary-search.md)
   - [Top K Elements](./patterns/top-k-elements.md)
   - [K-way Merge](./patterns/k-way-merge.md)
   - [Greedy Algorithms](./patterns/greedy-algorithms.md)
   - [Backtracking](./patterns/backtracking.md)

## Important Interview Problems: Blind 75

This section includes the complete list of LeetCode Blind 75 problems, organized by category. Each problem includes a clear explanation of the underlying logic and visual representation of the solution approach.

### Array

1. **Two Sum** - [Solution](./solutions/two-sum.md)
   - **Logic**: Use a hash map to store each element and its index. For each element, check if the target minus the current element exists in the hash map.
   - **Visual**: `[2,7,11,15]`, target = 9 → Check if (9-2)=7 exists in map → Return [0,1]

2. **Best Time to Buy and Sell Stock** - [Solution](./solutions/buy-sell-stock.md)
   - **Logic**: Track the minimum price seen so far and calculate the maximum profit at each step.
   - **Visual**: `[7,1,5,3,6,4]` → Min=7 → Min=1 → Profit=4 → Profit=4 → Profit=5 → Profit=5

3. **Contains Duplicate** - [Solution](./solutions/contains-duplicate.md)
   - **Logic**: Use a hash set to track seen elements. If an element is already in the set, return true.
   - **Visual**: `[1,2,3,1]` → Set={1} → Set={1,2} → Set={1,2,3} → 1 already in set → Return true

4. **Product of Array Except Self** - [Solution](./solutions/product-except-self.md)
   - **Logic**: Create two arrays for prefix and suffix products, then multiply corresponding elements.
   - **Visual**: `[1,2,3,4]` → Prefix=[1,1,2,6] → Suffix=[24,12,4,1] → Result=[24,12,8,6]

5. **Maximum Subarray** - [Solution](./solutions/maximum-subarray.md)
   - **Logic**: Use Kadane's algorithm to track current sum and maximum sum seen so far.
   - **Visual**: `[-2,1,-3,4,-1,2,1,-5,4]` → Current=0 → Current=1 → Current=0 → Current=4 → Max=6

6. **Maximum Product Subarray** - [Solution](./solutions/maximum-product-subarray.md)
   - **Logic**: Track both maximum and minimum products (for handling negative numbers).
   - **Visual**: `[2,3,-2,4]` → Max=2 → Max=6 → Max=6 → Max=24

7. **Find Minimum in Rotated Sorted Array** - [Solution](./solutions/find-minimum-rotated.md)
   - **Logic**: Use binary search to find the pivot point (smallest element).
   - **Visual**: `[4,5,6,7,0,1,2]` → mid=7 > right=2 → search left half → mid=0 < right=2 → search right half

8. **Search in Rotated Sorted Array** - [Solution](./solutions/search-rotated-array.md)
   - **Logic**: Use binary search with additional logic to handle the rotation.
   - **Visual**: `[4,5,6,7,0,1,2]`, target=0 → mid=7 → target < mid, but left half not sorted → search right half

9. **3Sum** - [Solution](./solutions/3sum.md)
   - **Logic**: Sort the array, then use two pointers technique for each element.
   - **Visual**: `[-1,0,1,2,-1,-4]` → Sort → `[-4,-1,-1,0,1,2]` → For each element, find pairs that sum to its negative

10. **Container With Most Water** - [Solution](./solutions/container-most-water.md)
    - **Logic**: Use two pointers at the ends, move the pointer with smaller height inward.
    - **Visual**: `[1,8,6,2,5,4,8,3,7]` → Area = min(1,7) * 8 = 8 → Move left → Area = min(8,7) * 7 = 49

### Binary

11. **Sum of Two Integers** - [Solution](./solutions/sum-two-integers.md)
    - **Logic**: Use bitwise XOR for addition without carry, and bitwise AND with left shift for carry.
    - **Visual**: 5 + 3 → (5 XOR 3) + ((5 AND 3) << 1) → 6 + 2 → (6 XOR 2) + ((6 AND 2) << 1) → 4 + 8 → 12

12. **Number of 1 Bits** - [Solution](./solutions/number-1-bits.md)
    - **Logic**: Use bit manipulation to count set bits (n & (n-1) clears the least significant set bit).
    - **Visual**: 11 (1011) → 1011 & 1010 = 1010 → 1010 & 1001 = 1000 → 1000 & 0111 = 0000 → Count = 3

13. **Counting Bits** - [Solution](./solutions/counting-bits.md)
    - **Logic**: Use dynamic programming with the pattern that i's set bits = (i >> 1)'s set bits + (i & 1).
    - **Visual**: For n=5 → [0,1,1,2,1,2] → dp[i] = dp[i>>1] + (i&1)

14. **Missing Number** - [Solution](./solutions/missing-number.md)
    - **Logic**: Use XOR of all numbers from 0 to n and all elements in the array.
    - **Visual**: `[3,0,1]` → XOR all indices (0,1,2) and values (3,0,1) → 0⊕1⊕2⊕3⊕0⊕1 = 2

15. **Reverse Bits** - [Solution](./solutions/reverse-bits.md)
    - **Logic**: Build the result by shifting and adding bits one by one.
    - **Visual**: 43261596 (00000010100101000001111010011100) → 964176192 (00111001011110000010100101000000)

### Dynamic Programming

16. **Climbing Stairs** - [Solution](./solutions/climbing-stairs.md)
    - **Logic**: Use Fibonacci sequence - ways to reach step n = ways to reach step n-1 + ways to reach step n-2.
    - **Visual**: For n=5 → dp=[1,1,2,3,5,8] → Answer = 8

17. **Coin Change** - [Solution](./solutions/coin-change.md)
    - **Logic**: Use dynamic programming to find minimum coins needed for each amount from 1 to target.
    - **Visual**: coins=[1,2,5], amount=11 → dp=[0,1,1,2,2,1,2,2,3,3,2,3] → Answer = 3 (5+5+1)

18. **Longest Increasing Subsequence** - [Solution](./solutions/longest-increasing-subsequence.md)
    - **Logic**: For each element, find the longest subsequence ending at previous elements that can be extended.
    - **Visual**: `[10,9,2,5,3,7,101,18]` → dp=[1,1,1,2,2,3,4,4] → Answer = 4 (2,3,7,101 or 2,3,7,18)

19. **Longest Common Subsequence** - [Solution](./solutions/longest-common-subsequence.md)
    - **Logic**: Use 2D dynamic programming to build up the solution for each prefix of both strings.
    - **Visual**: "abcde", "ace" → dp[i][j] = 1 + dp[i-1][j-1] if chars match, else max(dp[i-1][j], dp[i][j-1])

20. **Word Break** - [Solution](./solutions/word-break.md)
    - **Logic**: Use dynamic programming to determine if substrings can be formed from dictionary words.
    - **Visual**: s="leetcode", wordDict=["leet","code"] → dp=[T,F,F,F,T,F,F,F,T] → Answer = True

21. **Combination Sum** - [Solution](./solutions/combination-sum.md)
    - **Logic**: Use backtracking to find all unique combinations that sum to the target.
    - **Visual**: candidates=[2,3,6,7], target=7 → [2,2,3] and [7]

22. **House Robber** - [Solution](./solutions/house-robber.md)
    - **Logic**: Use dynamic programming to decide whether to rob current house or skip it.
    - **Visual**: `[1,2,3,1]` → dp=[1,2,4,4] → Answer = 4 (rob houses 1 and 3)

23. **House Robber II** - [Solution](./solutions/house-robber-ii.md)
    - **Logic**: Run House Robber algorithm twice - once for houses 0 to n-2, and once for houses 1 to n-1.
    - **Visual**: `[2,3,2]` → Rob houses 0 to n-2: [2,3] → 3, Rob houses 1 to n-1: [3,2] → 3 → Answer = 3

24. **Decode Ways** - [Solution](./solutions/decode-ways.md)
    - **Logic**: Use dynamic programming to count ways to decode each prefix of the string.
    - **Visual**: "226" → dp=[1,1,2,3] → Answer = 3 ("BZ", "VF", "BBF")

25. **Unique Paths** - [Solution](./solutions/unique-paths.md)
    - **Logic**: Use dynamic programming to count paths to each cell based on paths to cells above and to the left.
    - **Visual**: m=3, n=2 → dp=[[1,1],[1,2],[1,3]] → Answer = 3

26. **Jump Game** - [Solution](./solutions/jump-game.md)
    - **Logic**: Use greedy approach to track the furthest reachable position.
    - **Visual**: `[2,3,1,1,4]` → Max reach = 2 → Max reach = 4 → Max reach = 4 → Max reach = 4 → Answer = True

### Graph

27. **Clone Graph** - [Solution](./solutions/clone-graph.md)
    - **Logic**: Use DFS/BFS with a hash map to track cloned nodes.
    - **Visual**: Original graph → Create map of original to clone → Traverse and connect cloned nodes

28. **Course Schedule** - [Solution](./solutions/course-schedule.md)
    - **Logic**: Use topological sort to detect cycles in the directed graph.
    - **Visual**: prerequisites=[[1,0],[0,1]] → Graph has cycle → Answer = False

29. **Pacific Atlantic Water Flow** - [Solution](./solutions/pacific-atlantic.md)
    - **Logic**: Use DFS/BFS from ocean boundaries to find cells reachable from both oceans.
    - **Visual**: Start DFS from Pacific and Atlantic edges → Find intersection of reachable cells

30. **Number of Islands** - [Solution](./solutions/number-of-islands.md)
    - **Logic**: Use DFS/BFS to explore and mark connected land cells.
    - **Visual**: For each '1', explore all connected '1's and mark as visited → Count distinct islands

31. **Longest Consecutive Sequence** - [Solution](./solutions/longest-consecutive-sequence.md)
    - **Logic**: Use a hash set to check if each number is the start of a sequence.
    - **Visual**: `[100,4,200,1,3,2]` → For each num, check if num-1 exists → If not, count consecutive nums

32. **Alien Dictionary** - [Solution](./solutions/alien-dictionary.md)
    - **Logic**: Build a graph of character dependencies, then perform topological sort.
    - **Visual**: ["wrt","wrf","er","ett","rftt"] → w→e→r→t→f → Answer = "wertf"

33. **Graph Valid Tree** - [Solution](./solutions/graph-valid-tree.md)
    - **Logic**: Check if the graph has no cycles and all nodes are connected.
    - **Visual**: n=5, edges=[[0,1],[0,2],[0,3],[1,4]] → Use DFS/BFS to detect cycles → Answer = True

34. **Number of Connected Components in an Undirected Graph** - [Solution](./solutions/number-connected-components.md)
    - **Logic**: Use DFS/BFS or Union-Find to count connected components.
    - **Visual**: n=5, edges=[[0,1],[1,2],[3,4]] → Two connected components → Answer = 2

### Interval

35. **Insert Interval** - [Solution](./solutions/insert-interval.md)
    - **Logic**: Insert the new interval and merge overlapping intervals.
    - **Visual**: intervals=[[1,3],[6,9]], newInterval=[2,5] → Result=[[1,5],[6,9]]

36. **Merge Intervals** - [Solution](./solutions/merge-intervals.md)
    - **Logic**: Sort intervals by start time, then merge overlapping ones.
    - **Visual**: `[[1,3],[2,6],[8,10],[15,18]]` → Sort → Merge [1,3] and [2,6] → Result=[[1,6],[8,10],[15,18]]

37. **Non-overlapping Intervals** - [Solution](./solutions/non-overlapping-intervals.md)
    - **Logic**: Sort intervals by end time, greedily keep intervals that don't overlap.
    - **Visual**: `[[1,2],[2,3],[3,4],[1,3]]` → Sort by end → Remove [1,3] → Answer = 1

38. **Meeting Rooms** - [Solution](./solutions/meeting-rooms.md)
    - **Logic**: Sort intervals by start time, check if any meeting overlaps with the next.
    - **Visual**: `[[0,30],[5,10],[15,20]]` → Sort → [0,30] overlaps with [5,10] → Answer = False

39. **Meeting Rooms II** - [Solution](./solutions/meeting-rooms-ii.md)
    - **Logic**: Use a min heap to track end times of ongoing meetings.
    - **Visual**: `[[0,30],[5,10],[15,20]]` → Heap=[30] → Heap=[10,30] → Heap=[20,30] → Answer = 2

### Linked List

40. **Reverse Linked List** - [Solution](./solutions/reverse-linked-list.md)
    - **Logic**: Iterate through the list, reversing pointers as you go.
    - **Visual**: 1→2→3→4→5→NULL → NULL←1←2←3←4←5

41. **Linked List Cycle** - [Solution](./solutions/linked-list-cycle.md)
    - **Logic**: Use fast and slow pointers (Floyd's Cycle-Finding Algorithm).
    - **Visual**: Fast pointer moves 2 steps, slow pointer moves 1 step → If they meet, cycle exists

42. **Merge Two Sorted Lists** - [Solution](./solutions/merge-two-sorted-lists.md)
    - **Logic**: Compare heads of both lists and build a new sorted list.
    - **Visual**: 1→2→4, 1→3→4 → Compare heads → Build result: 1→1→2→3→4→4

43. **Merge K Sorted Lists** - [Solution](./solutions/merge-k-sorted-lists.md)
    - **Logic**: Use a min heap to efficiently find the smallest element among all lists.
    - **Visual**: Add first element of each list to heap → Extract min → Add next element from that list

44. **Remove Nth Node From End of List** - [Solution](./solutions/remove-nth-node.md)
    - **Logic**: Use two pointers with a gap of n nodes.
    - **Visual**: 1→2→3→4→5, n=2 → First pointer at 3, second at 1 → Remove node 4

45. **Reorder List** - [Solution](./solutions/reorder-list.md)
    - **Logic**: Find middle, reverse second half, then merge the two halves.
    - **Visual**: 1→2→3→4→5 → Find middle → Reverse 4→5 → Merge: 1→5→2→4→3

### Matrix

46. **Set Matrix Zeroes** - [Solution](./solutions/set-matrix-zeroes.md)
    - **Logic**: Use first row and column as markers for which rows/columns to zero out.
    - **Visual**: Mark first row/column → Zero out marked rows/columns

47. **Spiral Matrix** - [Solution](./solutions/spiral-matrix.md)
    - **Logic**: Traverse the matrix in spiral order using four boundaries.
    - **Visual**: Define top, bottom, left, right boundaries → Traverse and update boundaries

48. **Rotate Image** - [Solution](./solutions/rotate-image.md)
    - **Logic**: Transpose the matrix, then reverse each row.
    - **Visual**: Original → Transpose (rows become columns) → Reverse each row

49. **Word Search** - [Solution](./solutions/word-search.md)
    - **Logic**: Use backtracking to explore all possible paths from each cell.
    - **Visual**: For each cell, try to match the word starting from that cell using DFS

### String

50. **Longest Substring Without Repeating Characters** - [Solution](./solutions/longest-substring.md)
    - **Logic**: Use sliding window with a hash set to track characters in the current window.
    - **Visual**: "abcabcbb" → Window expands until duplicate → Window contracts to remove duplicate

51. **Longest Repeating Character Replacement** - [Solution](./solutions/character-replacement.md)
    - **Logic**: Use sliding window to find longest substring where (length - count of most frequent char) ≤ k.
    - **Visual**: "AABABBA", k=1 → Window with most frequent char + k replacements

52. **Minimum Window Substring** - [Solution](./solutions/minimum-window-substring.md)
    - **Logic**: Use sliding window to find smallest window containing all characters from target.
    - **Visual**: "ADOBECODEBANC", t="ABC" → Expand until all chars found → Contract to minimize

53. **Valid Anagram** - [Solution](./solutions/valid-anagram.md)
    - **Logic**: Count frequency of characters in both strings and compare.
    - **Visual**: "anagram", "nagaram" → Count chars → Compare counts → Answer = True

54. **Group Anagrams** - [Solution](./solutions/group-anagrams.md)
    - **Logic**: Use sorted string or character count as key to group anagrams.
    - **Visual**: ["eat","tea","tan","ate","nat","bat"] → Group by sorted string → [["eat","tea","ate"],["tan","nat"],["bat"]]

55. **Valid Parentheses** - [Solution](./solutions/valid-parentheses.md)
    - **Logic**: Use a stack to ensure each opening bracket has a matching closing bracket.
    - **Visual**: "({[]})" → Push opening brackets → Pop when matching closing bracket found

56. **Valid Palindrome** - [Solution](./solutions/valid-palindrome.md)
    - **Logic**: Use two pointers from both ends, ignoring non-alphanumeric characters.
    - **Visual**: "A man, a plan, a canal: Panama" → Compare alphanumeric chars from both ends

57. **Longest Palindromic Substring** - [Solution](./solutions/longest-palindromic-substring.md)
    - **Logic**: Expand around center for each possible center (character or between characters).
    - **Visual**: "babad" → For each position, expand outward while chars match → Answer = "bab" or "aba"

58. **Palindromic Substrings** - [Solution](./solutions/palindromic-substrings.md)
    - **Logic**: Count palindromes by expanding around each center.
    - **Visual**: "abc" → Single chars (3) + Expand around centers (0) → Answer = 3

59. **Encode and Decode Strings** - [Solution](./solutions/encode-decode-strings.md)
    - **Logic**: Use length-based encoding (e.g., "4#word" for "word").
    - **Visual**: ["lint","code","love","you"] → "4#lint4#code4#love3#you"

### Tree

60. **Maximum Depth of Binary Tree** - [Solution](./solutions/maximum-depth.md)
    - **Logic**: Use recursion to find the maximum depth of left and right subtrees + 1.
    - **Visual**: Root depth = max(left depth, right depth) + 1

61. **Same Tree** - [Solution](./solutions/same-tree.md)
    - **Logic**: Recursively check if current nodes and their subtrees are identical.
    - **Visual**: Compare current nodes → Recursively compare left and right subtrees

62. **Invert Binary Tree** - [Solution](./solutions/invert-binary-tree.md)
    - **Logic**: Swap left and right children for each node recursively.
    - **Visual**: Original tree → Swap children at each node → Inverted tree

63. **Binary Tree Maximum Path Sum** - [Solution](./solutions/binary-tree-maximum-path-sum.md)
    - **Logic**: Use recursion to find maximum path sum passing through each node.
    - **Visual**: For each node, calculate max path through it → Update global maximum

64. **Binary Tree Level Order Traversal** - [Solution](./solutions/level-order-traversal.md)
    - **Logic**: Use BFS with a queue to process nodes level by level.
    - **Visual**: Process root → Process all children → Process all grandchildren

65. **Serialize and Deserialize Binary Tree** - [Solution](./solutions/serialize-deserialize-binary-tree.md)
    - **Logic**: Use preorder traversal with null markers for serialization.
    - **Visual**: Tree → "1,2,null,null,3,4,null,null,5,null,null" → Tree

66. **Subtree of Another Tree** - [Solution](./solutions/subtree-of-another-tree.md)
    - **Logic**: For each node in the main tree, check if its subtree is identical to the second tree.
    - **Visual**: For each node → Check if identical to second tree

67. **Construct Binary Tree from Preorder and Inorder Traversal** - [Solution](./solutions/construct-binary-tree.md)
    - **Logic**: Use preorder to identify root, inorder to identify left and right subtrees.
    - **Visual**: Preorder=[3,9,20,15,7], Inorder=[9,3,15,20,7] → Root=3, Left=[9], Right=[15,20,7]

68. **Validate Binary Search Tree** - [Solution](./solutions/validate-bst.md)
    - **Logic**: Use recursion with min and max bounds for each subtree.
    - **Visual**: Each node must be > all nodes in left subtree and < all nodes in right subtree

69. **Kth Smallest Element in a BST** - [Solution](./solutions/kth-smallest-element-bst.md)
    - **Logic**: Use inorder traversal to visit nodes in ascending order.
    - **Visual**: Inorder traversal → Take kth element

70. **Lowest Common Ancestor of a Binary Search Tree** - [Solution](./solutions/lca-bst.md)
    - **Logic**: Traverse from root, go left if both nodes are smaller, right if both larger.
    - **Visual**: If p < root < q → root is LCA, else traverse left or right

71. **Implement Trie (Prefix Tree)** - [Solution](./solutions/implement-trie.md)
    - **Logic**: Use a tree structure where each node represents a character.
    - **Visual**: Root → Children for each first letter → Children for each second letter

72. **Add and Search Word - Data structure design** - [Solution](./solutions/add-search-word.md)
    - **Logic**: Extend Trie with wildcard search capability.
    - **Visual**: For '.' character, try all possible children

73. **Word Search II** - [Solution](./solutions/word-search-ii.md)
    - **Logic**: Use Trie to efficiently search for all words in the board.
    - **Visual**: Build Trie from words → DFS on board with Trie

### Heap

74. **Merge K Sorted Lists** - [Solution](./solutions/merge-k-sorted-lists.md)
    - **Logic**: Use a min heap to efficiently find the smallest element among all lists.
    - **Visual**: Add first element of each list to heap → Extract min → Add next element from that list

75. **Top K Frequent Elements** - [Solution](./solutions/top-k-frequent-elements.md)
    - **Logic**: Count frequencies, then use a heap or bucket sort to find top K.
    - **Visual**: `[1,1,1,2,2,3]`, k=2 → Counts: {1:3, 2:2, 3:1} → Answer = [1,2]

76. **Find Median from Data Stream** - [Solution](./solutions/find-median-from-data-stream.md)
    - **Logic**: Use two heaps (max heap for lower half, min heap for upper half).
    - **Visual**: Max heap contains smaller half, min heap contains larger half → Median is top of heaps

Each solution file will contain:
1. Problem statement and constraints
2. Visual explanation with diagrams
3. Intuition and approach
4. Step-by-step solution walkthrough
5. Code implementation in Python and JavaScript
6. Time and space complexity analysis
7. Edge cases and optimizations

## How to Use This Cheatsheet

Each topic includes:
- Conceptual explanations
- Visual representations
- Implementation in Python and JavaScript
- Common patterns and techniques
- Problem identification tips
- Time and space complexity analysis
- Common pitfalls and how to avoid them

## Contributing

Feel free to contribute to this cheatsheet by submitting pull requests or opening issues for improvements.

## License

This project is licensed under the MIT License - see the LICENSE file for details. 