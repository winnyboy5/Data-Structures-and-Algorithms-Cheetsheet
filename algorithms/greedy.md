# Greedy Algorithms

Greedy algorithms make locally optimal choices at each step with the hope of finding a global optimum. They are efficient and easy to implement but don't always yield the best solution.

## Table of Contents
- [Understanding Greedy Algorithms](#understanding-greedy-algorithms)
- [When to Use Greedy Algorithms](#when-to-use-greedy-algorithms)
- [Common Greedy Algorithm Problems](#common-greedy-algorithm-problems)
- [Greedy vs. Dynamic Programming](#greedy-vs-dynamic-programming)
- [Related Blind 75 & Grind 75 Problems](#related-blind-75--grind-75-problems)
- [Tips and Tricks](#tips-and-tricks)

## Understanding Greedy Algorithms

### Key Characteristics
1. **Makes locally optimal choices**: At each step, the algorithm makes the best choice at that moment.
2. **Never reconsiders**: Once a choice is made, it's never reconsidered.
3. **Simple and efficient**: Generally easier to implement and more efficient than other approaches.
4. **May not always yield optimal solutions**: For some problems, greedy algorithms don't guarantee the best solution.

### Components of a Greedy Algorithm
1. **Greedy choice property**: A globally optimal solution can be reached by making locally optimal choices.
2. **Optimal substructure**: An optimal solution to the problem contains optimal solutions to its subproblems.

### Visual Explanation

Consider the coin change problem with denominations [1, 5, 10, 25] and amount 36:

```
Step 1: Choose the largest coin ≤ 36: 25
        Remaining amount: 36 - 25 = 11
        Coins used: [25]

Step 2: Choose the largest coin ≤ 11: 10
        Remaining amount: 11 - 10 = 1
        Coins used: [25, 10]

Step 3: Choose the largest coin ≤ 1: 1
        Remaining amount: 1 - 1 = 0
        Coins used: [25, 10, 1]

Result: 3 coins [25, 10, 1]
```

## When to Use Greedy Algorithms

### Good Candidates for Greedy Approach
- **Activity selection problems**: Selecting the maximum number of activities that don't overlap.
- **Huffman coding**: Constructing optimal prefix codes for data compression.
- **Minimum spanning tree**: Finding a tree that connects all vertices with minimum total edge weight.
- **Interval scheduling**: Scheduling the maximum number of intervals without overlaps.
- **Fractional knapsack**: Maximizing value when items can be taken in fractions.

### When Not to Use Greedy Algorithms
- **0/1 Knapsack**: When items cannot be divided (use dynamic programming instead).
- **Shortest path in a graph with negative edges**: Greedy approaches like Dijkstra's algorithm fail.
- **Traveling Salesman Problem**: Finding the shortest route visiting all cities once.
- **Subset sum problem**: Finding a subset of numbers that sum to a target value.

## Common Greedy Algorithm Problems

### 1. Coin Change (with standard denominations)

#### Problem
Given a set of coin denominations and a target amount, find the minimum number of coins needed to make the amount.

#### Visual Explanation
```
Denominations: [1, 5, 10, 25]
Amount: 36

Step 1: Choose 25 (largest ≤ 36)
        Remaining: 11
Step 2: Choose 10 (largest ≤ 11)
        Remaining: 1
Step 3: Choose 1 (largest ≤ 1)
        Remaining: 0

Result: 3 coins [25, 10, 1]
```

#### Implementation

```python
def coin_change_greedy(coins, amount):
    # Sort coins in descending order
    coins.sort(reverse=True)
    
    count = 0
    result = []
    
    for coin in coins:
        # Use as many of the current coin as possible
        while amount >= coin:
            amount -= coin
            count += 1
            result.append(coin)
    
    if amount == 0:
        return count, result
    else:
        return -1, []  # Cannot make the amount with given coins
```

#### Note
The greedy approach works for standard US coin denominations [1, 5, 10, 25] but may not work for arbitrary denominations. For example, with denominations [1, 3, 4] and amount 6, the greedy approach gives [4, 1, 1] (3 coins) while the optimal solution is [3, 3] (2 coins).

### 2. Activity Selection

#### Problem
Given a set of activities with start and finish times, select the maximum number of activities that can be performed by a single person, assuming that a person can only work on a single activity at a time.

#### Visual Explanation
```
Activities: [(1,4), (3,5), (0,6), (5,7), (3,9), (5,9), (6,10), (8,11), (8,12), (2,14), (12,16)]
            (start_time, finish_time)

Sort by finish time:
[(0,6), (1,4), (2,14), (3,5), (3,9), (5,7), (5,9), (6,10), (8,11), (8,12), (12,16)]

Step 1: Select (1,4) (earliest finish)
Step 2: Next compatible activity: (5,7)
Step 3: Next compatible activity: (8,11)
Step 4: Next compatible activity: (12,16)

Result: 4 activities [(1,4), (5,7), (8,11), (12,16)]
```

#### Implementation

```python
def activity_selection(activities):
    # Sort activities by finish time
    activities.sort(key=lambda x: x[1])
    
    selected = [activities[0]]  # Select first activity
    last_finish_time = activities[0][1]
    
    for i in range(1, len(activities)):
        # If this activity starts after the last selected activity finishes
        if activities[i][0] >= last_finish_time:
            selected.append(activities[i])
            last_finish_time = activities[i][1]
    
    return selected
```

### 3. Fractional Knapsack

#### Problem
Given a set of items with weights and values, and a knapsack with a maximum capacity, determine the maximum value that can be obtained by taking fractions of items.

#### Visual Explanation
```
Items: [(value, weight)]
[(60, 10), (100, 20), (120, 30)]
Knapsack capacity: 50

Calculate value/weight ratio:
[(60/10=6, 10), (100/20=5, 20), (120/30=4, 30)]

Sort by ratio (descending):
[(6, 10), (5, 20), (4, 30)]

Step 1: Take all of item 1 (10 kg)
        Remaining capacity: 50 - 10 = 40
        Value: 60
Step 2: Take all of item 2 (20 kg)
        Remaining capacity: 40 - 20 = 20
        Value: 60 + 100 = 160
Step 3: Take 20/30 of item 3 (20 kg)
        Remaining capacity: 20 - 20 = 0
        Value: 160 + (120 * 20/30) = 160 + 80 = 240

Result: Maximum value = 240
```

#### Implementation

```python
def fractional_knapsack(items, capacity):
    # Calculate value/weight ratio for each item
    for item in items:
        item.append(item[0] / item[1])  # Add ratio as the third element
    
    # Sort items by value/weight ratio in descending order
    items.sort(key=lambda x: x[2], reverse=True)
    
    total_value = 0
    remaining_capacity = capacity
    
    for value, weight, ratio in items:
        if remaining_capacity >= weight:
            # Take the whole item
            total_value += value
            remaining_capacity -= weight
        else:
            # Take a fraction of the item
            total_value += value * (remaining_capacity / weight)
            break  # Knapsack is full
    
    return total_value
```

### 4. Huffman Coding

#### Problem
Given a set of characters and their frequencies, construct a binary tree that minimizes the weighted path length from the root to each character.

#### Visual Explanation
```
Characters and frequencies:
a: 5, b: 9, c: 12, d: 13, e: 16, f: 45

Step 1: Create a leaf node for each character and add them to a priority queue
        [a:5, b:9, c:12, d:13, e:16, f:45]

Step 2: While there is more than one node in the queue:
        - Remove the two nodes with the lowest frequencies
        - Create a new internal node with these two nodes as children
        - Add the new node to the queue

Step 3: The remaining node is the root of the Huffman tree

Final Huffman Tree:
                 100
                /    \
               /      \
              55       45(f)
             /  \
            /    \
           24     31
          /  \   /  \
         /    \ /    \
        9(b)  15 13(d) 18
             /  \      / \
            /    \    /   \
           5(a)  10(c) 16(e)

Huffman Codes:
a: 1000
b: 00
c: 1001
d: 101
e: 111
f: 0
```

#### Implementation

```python
import heapq

class Node:
    def __init__(self, freq, symbol, left=None, right=None):
        self.freq = freq
        self.symbol = symbol
        self.left = left
        self.right = right
        self.huff = ''
        
    def __lt__(self, other):
        return self.freq < other.freq

def print_nodes(node, val=''):
    # Huffman code for current node
    new_val = val + str(node.huff)
    
    # If node is not an edge node, traverse inside it
    if node.left:
        print_nodes(node.left, new_val)
    if node.right:
        print_nodes(node.right, new_val)
        
    # If node is edge node, print its huffman code
    if not node.left and not node.right:
        print(f"{node.symbol} -> {new_val}")

def huffman_coding(chars, freqs):
    nodes = []
    
    # Create a leaf node for each character and add it to the priority queue
    for i in range(len(chars)):
        heapq.heappush(nodes, Node(freqs[i], chars[i]))
    
    # While there is more than one node in the queue
    while len(nodes) > 1:
        # Remove the two nodes with the lowest frequencies
        left = heapq.heappop(nodes)
        right = heapq.heappop(nodes)
        
        # Assign directional value to these nodes
        left.huff = 0
        right.huff = 1
        
        # Create a new internal node with these two nodes as children
        # and with frequency equal to the sum of the two nodes' frequencies
        new_node = Node(left.freq + right.freq, left.symbol + right.symbol, left, right)
        
        # Add the new node to the priority queue
        heapq.heappush(nodes, new_node)
    
    # The remaining node is the root node and the tree is complete
    return nodes[0]
```

### 5. Minimum Spanning Tree (Kruskal's Algorithm)

#### Problem
Given a connected, undirected graph, find a spanning tree with the minimum total edge weight.

#### Visual Explanation
```
Graph:
A -- 7 -- B
|         |
2         8
|         |
C -- 3 -- D
|         |
4         9
|         |
E -- 5 -- F

Sort edges by weight:
(A,C): 2
(C,D): 3
(C,E): 4
(E,F): 5
(A,B): 7
(B,D): 8
(D,F): 9

Step 1: Add edge (A,C): 2
Step 2: Add edge (C,D): 3
Step 3: Add edge (C,E): 4
Step 4: Add edge (E,F): 5
Step 5: Add edge (A,B): 7

Result: MST with edges [(A,C), (C,D), (C,E), (E,F), (A,B)] and total weight 21
```

#### Implementation

```python
def kruskal(graph, vertices):
    # Create a list of all edges in the graph
    edges = []
    for u in range(vertices):
        for v, weight in graph[u]:
            edges.append((u, v, weight))
    
    # Sort edges by weight
    edges.sort(key=lambda x: x[2])
    
    # Initialize parent array for Union-Find
    parent = list(range(vertices))
    
    def find(i):
        if parent[i] != i:
            parent[i] = find(parent[i])
        return parent[i]
    
    def union(i, j):
        parent[find(i)] = find(j)
    
    mst = []
    total_weight = 0
    
    for u, v, weight in edges:
        if find(u) != find(v):  # Check if adding this edge creates a cycle
            union(u, v)
            mst.append((u, v, weight))
            total_weight += weight
            
            if len(mst) == vertices - 1:
                break  # MST is complete
    
    return mst, total_weight
```

## Greedy vs. Dynamic Programming

### Key Differences

| Aspect | Greedy Algorithm | Dynamic Programming |
|--------|------------------|---------------------|
| Decision Making | Makes locally optimal choices | Considers all possible choices |
| Optimization | Optimizes step by step | Optimizes over the entire problem |
| Efficiency | Generally more efficient | Can be more computationally intensive |
| Correctness | May not always yield optimal solutions | Always yields optimal solutions |
| Problem Types | Activity selection, Huffman coding | 0/1 Knapsack, Longest common subsequence |

### Example: Coin Change Problem

#### Greedy Approach (may not always be optimal)
```python
def coin_change_greedy(coins, amount):
    coins.sort(reverse=True)
    count = 0
    
    for coin in coins:
        while amount >= coin:
            amount -= coin
            count += 1
    
    return count if amount == 0 else -1
```

#### Dynamic Programming Approach (always optimal)
```python
def coin_change_dp(coins, amount):
    # Initialize dp array with amount + 1 (representing infinity)
    dp = [amount + 1] * (amount + 1)
    dp[0] = 0
    
    for coin in coins:
        for i in range(coin, amount + 1):
            dp[i] = min(dp[i], dp[i - coin] + 1)
    
    return dp[amount] if dp[amount] <= amount else -1
```

## Related Blind 75 & Grind 75 Problems

1. **Jump Game** (Blind 75 #55)
   - Problem: Determine if you can reach the last index of an array.
   - Solution: Use a greedy approach to track the maximum reachable position.
   - [LeetCode #55](https://leetcode.com/problems/jump-game/)

2. **Jump Game II** (Blind 75 #45)
   - Problem: Find the minimum number of jumps to reach the last index.
   - Solution: Use a greedy approach with BFS-like traversal.
   - [LeetCode #45](https://leetcode.com/problems/jump-game-ii/)

3. **Gas Station** (Grind 75)
   - Problem: Find the starting gas station that allows you to complete a circuit.
   - Solution: Use a greedy approach to find the valid starting point.
   - [LeetCode #134](https://leetcode.com/problems/gas-station/)

4. **Task Scheduler** (Blind 75 #621)
   - Problem: Schedule tasks with cooldown periods to minimize idle time.
   - Solution: Use a greedy approach with priority queue.
   - [LeetCode #621](https://leetcode.com/problems/task-scheduler/)

5. **Merge Intervals** (Blind 75 #56)
   - Problem: Merge overlapping intervals.
   - Solution: Sort intervals by start time and merge overlapping ones.
   - [LeetCode #56](https://leetcode.com/problems/merge-intervals/)

6. **Non-overlapping Intervals** (Grind 75)
   - Problem: Find the minimum number of intervals to remove to make the rest non-overlapping.
   - Solution: Sort intervals by end time and use a greedy approach.
   - [LeetCode #435](https://leetcode.com/problems/non-overlapping-intervals/)

## Tips and Tricks

1. **Identifying Greedy Problems**:
   - Look for problems where local optimization leads to global optimization
   - Problems involving maximizing or minimizing a value
   - Problems with a natural ordering (e.g., by time, weight, value)
   - Problems with optimal substructure

2. **Proving Greedy Algorithms**:
   - Greedy choice property: Prove that a locally optimal choice is part of the globally optimal solution
   - Optimal substructure: Prove that the problem can be broken down into subproblems
   - Induction: Prove that if the greedy choice works for n-1 elements, it works for n elements

3. **Common Greedy Strategies**:
   - Sort input data (by value, weight, ratio, start/end time, etc.)
   - Always pick the largest/smallest available option
   - Always pick the item with the best value/weight ratio
   - Process items in a specific order (earliest deadline first, shortest job first)

4. **When Greedy Fails**:
   - The problem requires considering future consequences of current decisions
   - The problem doesn't have optimal substructure
   - Local optimization doesn't lead to global optimization
   - In such cases, consider dynamic programming or other approaches

5. **Debugging Greedy Algorithms**:
   - Test with small examples where you can verify the optimal solution
   - Look for counterexamples where the greedy approach fails
   - Trace through the algorithm step by step to understand its behavior 