# Greedy Algorithms Pattern

Greedy algorithms make locally optimal choices at each step with the hope of finding a global optimum. In other words, they choose the best option available at the moment without worrying about future consequences. While this approach doesn't always yield the optimal solution, for many problems, it does.

## Visual Representation

```
Coin Change Problem (Greedy Approach):
Amount: 63 cents
Available coins: 25¢, 10¢, 5¢, 1¢

Step 1: Choose the largest coin ≤ 63¢ → 25¢ (Remaining: 38¢)
Step 2: Choose the largest coin ≤ 38¢ → 25¢ (Remaining: 13¢)
Step 3: Choose the largest coin ≤ 13¢ → 10¢ (Remaining: 3¢)
Step 4: Choose the largest coin ≤ 3¢ → 1¢ (Remaining: 2¢)
Step 5: Choose the largest coin ≤ 2¢ → 1¢ (Remaining: 1¢)
Step 6: Choose the largest coin ≤ 1¢ → 1¢ (Remaining: 0¢)

Result: 2 quarters, 1 dime, 3 pennies = 6 coins
```

## When to Use Greedy Algorithms

- When the problem has the "greedy choice property" (a locally optimal choice leads to a globally optimal solution)
- When the problem has "optimal substructure" (an optimal solution can be constructed from optimal solutions of its subproblems)
- When you need a reasonably good solution quickly, even if it's not the absolute best
- When the problem involves optimization (maximizing or minimizing something)
- When making a series of choices to solve a problem

## Basic Greedy Algorithm Implementation

### Python Implementation (Activity Selection Problem)

```python
def activity_selection(start, finish):
    """
    Given start and finish times of activities, find the maximum number
    of activities that can be performed by a single person.
    """
    # Sort activities by finish time
    activities = sorted(zip(start, finish), key=lambda x: x[1])
    
    # Select the first activity
    selected = [activities[0]]
    last_finish_time = activities[0][1]
    
    # Consider the rest of the activities
    for activity in activities[1:]:
        start_time, finish_time = activity
        
        # If this activity starts after the finish time of the last selected activity
        if start_time >= last_finish_time:
            selected.append(activity)
            last_finish_time = finish_time
    
    return selected

# Example usage
start = [1, 3, 0, 5, 8, 5]
finish = [2, 4, 6, 7, 9, 9]
print(activity_selection(start, finish))
# Output: [(1, 2), (3, 4), (5, 7), (8, 9)]
```

### JavaScript Implementation (Activity Selection Problem)

```javascript
function activitySelection(start, finish) {
    // Create activities array with start and finish times
    const activities = [];
    for (let i = 0; i < start.length; i++) {
        activities.push([start[i], finish[i]]);
    }
    
    // Sort activities by finish time
    activities.sort((a, b) => a[1] - b[1]);
    
    // Select the first activity
    const selected = [activities[0]];
    let lastFinishTime = activities[0][1];
    
    // Consider the rest of the activities
    for (let i = 1; i < activities.length; i++) {
        const startTime = activities[i][0];
        const finishTime = activities[i][1];
        
        // If this activity starts after the finish time of the last selected activity
        if (startTime >= lastFinishTime) {
            selected.push(activities[i]);
            lastFinishTime = finishTime;
        }
    }
    
    return selected;
}

// Example usage
const start = [1, 3, 0, 5, 8, 5];
const finish = [2, 4, 6, 7, 9, 9];
console.log(activitySelection(start, finish));
// Output: [[1, 2], [3, 4], [5, 7], [8, 9]]
```

## Common Problems and Solutions

### 1. Fractional Knapsack

**Problem:** Given weights and values of n items, put these items in a knapsack of capacity W to get the maximum total value. In the fractional knapsack problem, we can break items for maximizing the total value of the knapsack.

**Python Solution:**
```python
def fractional_knapsack(values, weights, capacity):
    # Create a list of (value, weight, value/weight) tuples
    items = [(values[i], weights[i], values[i]/weights[i]) for i in range(len(values))]
    
    # Sort items by value-to-weight ratio in descending order
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

# Example usage
values = [60, 100, 120]
weights = [10, 20, 30]
capacity = 50
print(fractional_knapsack(values, weights, capacity))
# Output: 240.0
```

**JavaScript Solution:**
```javascript
function fractionalKnapsack(values, weights, capacity) {
    // Create a list of [value, weight, value/weight] arrays
    const items = [];
    for (let i = 0; i < values.length; i++) {
        items.push([values[i], weights[i], values[i] / weights[i]]);
    }
    
    // Sort items by value-to-weight ratio in descending order
    items.sort((a, b) => b[2] - a[2]);
    
    let totalValue = 0;
    let remainingCapacity = capacity;
    
    for (const [value, weight, ratio] of items) {
        if (remainingCapacity >= weight) {
            // Take the whole item
            totalValue += value;
            remainingCapacity -= weight;
        } else {
            // Take a fraction of the item
            totalValue += value * (remainingCapacity / weight);
            break;  // Knapsack is full
        }
    }
    
    return totalValue;
}

// Example usage
const values = [60, 100, 120];
const weights = [10, 20, 30];
const capacity = 50;
console.log(fractionalKnapsack(values, weights, capacity));
// Output: 240.0
```

**Time Complexity:** O(n log n) where n is the number of items  
**Space Complexity:** O(n)

### 2. Minimum Number of Coins

**Problem:** Given a value V and an array of coin denominations, find the minimum number of coins required to make change for V.

**Python Solution:**
```python
def min_coins(coins, amount):
    # Sort coins in descending order
    coins.sort(reverse=True)
    
    count = 0
    remaining = amount
    result = []
    
    for coin in coins:
        # Use as many of the current coin as possible
        while remaining >= coin:
            remaining -= coin
            count += 1
            result.append(coin)
    
    # If we couldn't make the exact amount
    if remaining > 0:
        return -1, []
    
    return count, result

# Example usage
coins = [1, 5, 10, 25]
amount = 63
count, coins_used = min_coins(coins, amount)
print(f"Number of coins: {count}")
print(f"Coins used: {coins_used}")
# Output: 
# Number of coins: 6
# Coins used: [25, 25, 10, 1, 1, 1]
```

**JavaScript Solution:**
```javascript
function minCoins(coins, amount) {
    // Sort coins in descending order
    coins.sort((a, b) => b - a);
    
    let count = 0;
    let remaining = amount;
    const result = [];
    
    for (const coin of coins) {
        // Use as many of the current coin as possible
        while (remaining >= coin) {
            remaining -= coin;
            count++;
            result.push(coin);
        }
    }
    
    // If we couldn't make the exact amount
    if (remaining > 0) {
        return { count: -1, coinsUsed: [] };
    }
    
    return { count, coinsUsed: result };
}

// Example usage
const coins = [1, 5, 10, 25];
const amount = 63;
const { count, coinsUsed } = minCoins(coins, amount);
console.log(`Number of coins: ${count}`);
console.log(`Coins used: ${coinsUsed}`);
// Output: 
// Number of coins: 6
// Coins used: [25, 25, 10, 1, 1, 1]
```

**Time Complexity:** O(n log n + amount) where n is the number of coin denominations  
**Space Complexity:** O(amount) in the worst case

### 3. Huffman Coding

**Problem:** Given a set of characters and their frequencies, construct a Huffman tree and generate Huffman codes for each character.

**Python Solution:**
```python
import heapq
from collections import defaultdict

class Node:
    def __init__(self, char, freq):
        self.char = char
        self.freq = freq
        self.left = None
        self.right = None
    
    def __lt__(self, other):
        return self.freq < other.freq

def huffman_coding(data):
    if not data:
        return None, {}
    
    # Calculate frequency of each character
    frequency = defaultdict(int)
    for char in data:
        frequency[char] += 1
    
    # Create a priority queue (min heap)
    heap = [Node(char, freq) for char, freq in frequency.items()]
    heapq.heapify(heap)
    
    # Build the Huffman tree
    while len(heap) > 1:
        left = heapq.heappop(heap)
        right = heapq.heappop(heap)
        
        # Create a new internal node with these two nodes as children
        # and with frequency equal to the sum of the two nodes' frequencies
        internal = Node(None, left.freq + right.freq)
        internal.left = left
        internal.right = right
        
        heapq.heappush(heap, internal)
    
    # The remaining node is the root of the Huffman tree
    root = heap[0] if heap else None
    
    # Generate Huffman codes
    codes = {}
    
    def generate_codes(node, code):
        if node:
            if node.char:
                codes[node.char] = code
            generate_codes(node.left, code + "0")
            generate_codes(node.right, code + "1")
    
    generate_codes(root, "")
    
    return root, codes

# Example usage
data = "BCAADDDCCACACAC"
root, codes = huffman_coding(data)
print("Huffman Codes:")
for char, code in codes.items():
    print(f"{char}: {code}")

# Encode the data
encoded_data = ''.join(codes[char] for char in data)
print(f"Encoded data: {encoded_data}")

# Output:
# Huffman Codes:
# B: 110
# C: 0
# A: 10
# D: 111
# Encoded data: 1100101111111110000100100
```

**JavaScript Solution:**
```javascript
class Node {
    constructor(char, freq) {
        this.char = char;
        this.freq = freq;
        this.left = null;
        this.right = null;
    }
}

class MinHeap {
    constructor() {
        this.heap = [];
    }
    
    size() {
        return this.heap.length;
    }
    
    isEmpty() {
        return this.size() === 0;
    }
    
    push(node) {
        this.heap.push(node);
        this.heapifyUp(this.size() - 1);
    }
    
    pop() {
        if (this.isEmpty()) return null;
        
        const min = this.heap[0];
        const last = this.heap.pop();
        
        if (this.size() > 0) {
            this.heap[0] = last;
            this.heapifyDown(0);
        }
        
        return min;
    }
    
    heapifyUp(index) {
        const parent = Math.floor((index - 1) / 2);
        
        if (index > 0 && this.heap[parent].freq > this.heap[index].freq) {
            [this.heap[parent], this.heap[index]] = [this.heap[index], this.heap[parent]];
            this.heapifyUp(parent);
        }
    }
    
    heapifyDown(index) {
        const left = 2 * index + 1;
        const right = 2 * index + 2;
        let smallest = index;
        
        if (left < this.size() && this.heap[left].freq < this.heap[smallest].freq) {
            smallest = left;
        }
        
        if (right < this.size() && this.heap[right].freq < this.heap[smallest].freq) {
            smallest = right;
        }
        
        if (smallest !== index) {
            [this.heap[index], this.heap[smallest]] = [this.heap[smallest], this.heap[index]];
            this.heapifyDown(smallest);
        }
    }
}

function huffmanCoding(data) {
    if (!data) {
        return { root: null, codes: {} };
    }
    
    // Calculate frequency of each character
    const frequency = {};
    for (const char of data) {
        frequency[char] = (frequency[char] || 0) + 1;
    }
    
    // Create a priority queue (min heap)
    const heap = new MinHeap();
    for (const char in frequency) {
        heap.push(new Node(char, frequency[char]));
    }
    
    // Build the Huffman tree
    while (heap.size() > 1) {
        const left = heap.pop();
        const right = heap.pop();
        
        // Create a new internal node with these two nodes as children
        // and with frequency equal to the sum of the two nodes' frequencies
        const internal = new Node(null, left.freq + right.freq);
        internal.left = left;
        internal.right = right;
        
        heap.push(internal);
    }
    
    // The remaining node is the root of the Huffman tree
    const root = heap.size() > 0 ? heap.pop() : null;
    
    // Generate Huffman codes
    const codes = {};
    
    function generateCodes(node, code) {
        if (node) {
            if (node.char) {
                codes[node.char] = code;
            }
            generateCodes(node.left, code + "0");
            generateCodes(node.right, code + "1");
        }
    }
    
    generateCodes(root, "");
    
    return { root, codes };
}

// Example usage
const data = "BCAADDDCCACACAC";
const { root, codes } = huffmanCoding(data);
console.log("Huffman Codes:");
for (const char in codes) {
    console.log(`${char}: ${codes[char]}`);
}

// Encode the data
let encodedData = '';
for (const char of data) {
    encodedData += codes[char];
}
console.log(`Encoded data: ${encodedData}`);

// Output:
// Huffman Codes:
// B: 110
// C: 0
// A: 10
// D: 111
// Encoded data: 1100101111111110000100100
```

**Time Complexity:** O(n log n) where n is the number of unique characters  
**Space Complexity:** O(n)

### 4. Job Sequencing with Deadlines

**Problem:** Given a set of jobs with deadlines and profits, find the maximum profit that can be earned by executing jobs within their deadlines.

**Python Solution:**
```python
def job_sequencing(jobs):
    # Sort jobs by profit in descending order
    jobs.sort(key=lambda x: x[2], reverse=True)
    
    # Find the maximum deadline
    max_deadline = max(job[1] for job in jobs)
    
    # Initialize the result array and slot array
    result = [-1] * max_deadline
    slot = [False] * max_deadline
    
    # Fill the slots with jobs
    for job in jobs:
        job_id, deadline, profit = job
        
        # Find a free slot for this job
        for j in range(min(max_deadline, deadline) - 1, -1, -1):
            if not slot[j]:
                result[j] = job_id
                slot[j] = True
                break
    
    # Collect the scheduled jobs
    scheduled_jobs = [result[i] for i in range(max_deadline) if slot[i]]
    
    return scheduled_jobs

# Example usage
# Each job is represented as (job_id, deadline, profit)
jobs = [(1, 4, 20), (2, 1, 10), (3, 1, 40), (4, 1, 30)]
print(job_sequencing(jobs))
# Output: [3, 1]
```

**JavaScript Solution:**
```javascript
function jobSequencing(jobs) {
    // Sort jobs by profit in descending order
    jobs.sort((a, b) => b[2] - a[2]);
    
    // Find the maximum deadline
    const maxDeadline = Math.max(...jobs.map(job => job[1]));
    
    // Initialize the result array and slot array
    const result = Array(maxDeadline).fill(-1);
    const slot = Array(maxDeadline).fill(false);
    
    // Fill the slots with jobs
    for (const job of jobs) {
        const [jobId, deadline, profit] = job;
        
        // Find a free slot for this job
        for (let j = Math.min(maxDeadline, deadline) - 1; j >= 0; j--) {
            if (!slot[j]) {
                result[j] = jobId;
                slot[j] = true;
                break;
            }
        }
    }
    
    // Collect the scheduled jobs
    const scheduledJobs = [];
    for (let i = 0; i < maxDeadline; i++) {
        if (slot[i]) {
            scheduledJobs.push(result[i]);
        }
    }
    
    return scheduledJobs;
}

// Example usage
// Each job is represented as [job_id, deadline, profit]
const jobs = [[1, 4, 20], [2, 1, 10], [3, 1, 40], [4, 1, 30]];
console.log(jobSequencing(jobs));
// Output: [3, 1]
```

**Time Complexity:** O(n log n + n * m) where n is the number of jobs and m is the maximum deadline  
**Space Complexity:** O(m)

### 5. Minimum Spanning Tree (Prim's Algorithm)

**Problem:** Given a connected, undirected graph, find a spanning tree with the minimum possible total edge weight.

**Python Solution:**
```python
import heapq

def prims_mst(graph, start):
    # Initialize variables
    n = len(graph)
    visited = [False] * n
    min_heap = [(0, start, -1)]  # (weight, vertex, parent)
    mst = []
    total_weight = 0
    
    while min_heap:
        weight, vertex, parent = heapq.heappop(min_heap)
        
        # If vertex is already visited, skip
        if visited[vertex]:
            continue
        
        # Mark vertex as visited
        visited[vertex] = True
        
        # Add edge to MST (except for the start vertex)
        if parent != -1:
            mst.append((parent, vertex, weight))
            total_weight += weight
        
        # Add all adjacent vertices to the min heap
        for neighbor, edge_weight in graph[vertex]:
            if not visited[neighbor]:
                heapq.heappush(min_heap, (edge_weight, neighbor, vertex))
    
    return mst, total_weight

# Example usage
# Graph represented as an adjacency list: [[(neighbor, weight), ...], ...]
graph = [
    [(1, 2), (3, 6)],             # Vertex 0
    [(0, 2), (2, 3), (3, 8), (4, 5)],  # Vertex 1
    [(1, 3), (4, 7)],             # Vertex 2
    [(0, 6), (1, 8), (4, 9)],     # Vertex 3
    [(1, 5), (2, 7), (3, 9)]      # Vertex 4
]

mst, total_weight = prims_mst(graph, 0)
print(f"Minimum Spanning Tree: {mst}")
print(f"Total Weight: {total_weight}")
# Output:
# Minimum Spanning Tree: [(0, 1, 2), (1, 2, 3), (1, 4, 5), (0, 3, 6)]
# Total Weight: 16
```

**JavaScript Solution:**
```javascript
class MinHeap {
    constructor() {
        this.heap = [];
    }
    
    size() {
        return this.heap.length;
    }
    
    isEmpty() {
        return this.size() === 0;
    }
    
    push(item) {
        this.heap.push(item);
        this.heapifyUp(this.size() - 1);
    }
    
    pop() {
        if (this.isEmpty()) return null;
        
        const min = this.heap[0];
        const last = this.heap.pop();
        
        if (this.size() > 0) {
            this.heap[0] = last;
            this.heapifyDown(0);
        }
        
        return min;
    }
    
    heapifyUp(index) {
        const parent = Math.floor((index - 1) / 2);
        
        if (index > 0 && this.heap[parent][0] > this.heap[index][0]) {
            [this.heap[parent], this.heap[index]] = [this.heap[index], this.heap[parent]];
            this.heapifyUp(parent);
        }
    }
    
    heapifyDown(index) {
        const left = 2 * index + 1;
        const right = 2 * index + 2;
        let smallest = index;
        
        if (left < this.size() && this.heap[left][0] < this.heap[smallest][0]) {
            smallest = left;
        }
        
        if (right < this.size() && this.heap[right][0] < this.heap[smallest][0]) {
            smallest = right;
        }
        
        if (smallest !== index) {
            [this.heap[index], this.heap[smallest]] = [this.heap[smallest], this.heap[index]];
            this.heapifyDown(smallest);
        }
    }
}

function primsMST(graph, start) {
    // Initialize variables
    const n = graph.length;
    const visited = Array(n).fill(false);
    const minHeap = new MinHeap();
    minHeap.push([0, start, -1]);  // [weight, vertex, parent]
    const mst = [];
    let totalWeight = 0;
    
    while (!minHeap.isEmpty()) {
        const [weight, vertex, parent] = minHeap.pop();
        
        // If vertex is already visited, skip
        if (visited[vertex]) {
            continue;
        }
        
        // Mark vertex as visited
        visited[vertex] = true;
        
        // Add edge to MST (except for the start vertex)
        if (parent !== -1) {
            mst.push([parent, vertex, weight]);
            totalWeight += weight;
        }
        
        // Add all adjacent vertices to the min heap
        for (const [neighbor, edgeWeight] of graph[vertex]) {
            if (!visited[neighbor]) {
                minHeap.push([edgeWeight, neighbor, vertex]);
            }
        }
    }
    
    return { mst, totalWeight };
}

// Example usage
// Graph represented as an adjacency list: [[[neighbor, weight], ...], ...]
const graph = [
    [[1, 2], [3, 6]],                 // Vertex 0
    [[0, 2], [2, 3], [3, 8], [4, 5]], // Vertex 1
    [[1, 3], [4, 7]],                 // Vertex 2
    [[0, 6], [1, 8], [4, 9]],         // Vertex 3
    [[1, 5], [2, 7], [3, 9]]          // Vertex 4
];

const { mst, totalWeight } = primsMST(graph, 0);
console.log(`Minimum Spanning Tree: ${JSON.stringify(mst)}`);
console.log(`Total Weight: ${totalWeight}`);
// Output:
// Minimum Spanning Tree: [[0, 1, 2], [1, 2, 3], [1, 4, 5], [0, 3, 6]]
// Total Weight: 16
```

**Time Complexity:** O(E log V) where E is the number of edges and V is the number of vertices  
**Space Complexity:** O(V + E)

## Time and Space Complexity

| Algorithm | Time Complexity | Space Complexity |
|-----------|----------------|-----------------|
| Activity Selection | O(n log n) | O(n) |
| Fractional Knapsack | O(n log n) | O(n) |
| Minimum Coins | O(n log n + amount) | O(amount) |
| Huffman Coding | O(n log n) | O(n) |
| Job Sequencing | O(n log n + n * m) | O(m) |
| Prim's MST | O(E log V) | O(V + E) |

## Tips and Tricks for Greedy Algorithms

1. **Sort First**: Many greedy algorithms start by sorting the input data based on some criteria (e.g., finish time, value-to-weight ratio).

2. **Identify the Greedy Choice**: Determine what the locally optimal choice is at each step.

3. **Prove Correctness**: Ensure that making the greedy choice at each step leads to a globally optimal solution.

4. **Consider Edge Cases**: Test your algorithm with edge cases like empty inputs, single elements, or extreme values.

5. **Optimize**: Look for ways to optimize your algorithm, such as using more efficient data structures (e.g., priority queues).

6. **Verify Optimality**: Check if the greedy approach actually gives the optimal solution for your problem.

7. **Fallback to Dynamic Programming**: If the greedy approach doesn't work, consider using dynamic programming instead.

## Common Pitfalls

1. **Assuming Greedy Always Works**: Not all problems can be solved optimally using a greedy approach.

2. **Incorrect Greedy Choice**: Choosing the wrong criteria for making the greedy choice.

3. **Not Considering All Constraints**: Forgetting to account for all the constraints of the problem.

4. **Overlooking Edge Cases**: Not handling edge cases properly.

5. **Inefficient Implementation**: Using inefficient data structures or algorithms.

## How to Identify Greedy Algorithm Problems

Look for these clues in the problem statement:

1. The problem asks for optimization (maximizing or minimizing something).
2. The problem can be solved by making a series of choices.
3. The locally optimal choice at each step leads to a globally optimal solution.
4. The problem has optimal substructure.
5. Keywords like "maximum," "minimum," "optimize," or "best."

## Common Greedy Algorithm Problems from Blind 75 and Grind 75

1. **Jump Game** (Medium): Determine if you can reach the last index of an array.
2. **Jump Game II** (Medium): Find the minimum number of jumps to reach the last index.
3. **Gas Station** (Medium): Find the starting gas station that allows you to complete the circuit.
4. **Task Scheduler** (Medium): Find the least number of intervals to finish all tasks.
5. **Minimum Number of Arrows to Burst Balloons** (Medium): Find the minimum number of arrows needed to burst all balloons.
6. **Non-overlapping Intervals** (Medium): Find the minimum number of intervals to remove to make the rest non-overlapping.
7. **Partition Labels** (Medium): Partition a string into as many parts as possible so that each letter appears in at most one part.
8. **Lemonade Change** (Easy): Determine if you can provide change for each customer.
9. **Queue Reconstruction by Height** (Medium): Reconstruct a queue based on people's heights and positions.
10. **Minimum Cost to Connect Sticks** (Medium): Find the minimum cost to connect all sticks.

## Greedy Algorithm Template

Here's a general template for solving greedy algorithm problems:

```python
def greedy_algorithm(input_data):
    # Preprocess the input (e.g., sort)
    processed_data = preprocess(input_data)
    
    result = initialize_result()
    
    for item in processed_data:
        # Make a greedy choice
        if is_valid_choice(item, result):
            result = update_result(result, item)
    
    return result
```

## Real-World Applications

1. **Scheduling**: Allocating resources to tasks in a way that minimizes completion time.
2. **Routing**: Finding the shortest path in a network.
3. **Data Compression**: Huffman coding for efficient data compression.
4. **Load Balancing**: Distributing workload across multiple resources.
5. **Currency Systems**: Designing coin denominations for efficient change-making.
6. **Network Design**: Constructing minimum spanning trees for network infrastructure.
7. **Resource Allocation**: Allocating limited resources to maximize utility. 