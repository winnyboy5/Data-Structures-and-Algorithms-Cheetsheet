# Counting Bits

## Problem Statement

Given an integer `n`, return an array `ans` of length `n + 1` such that for each `i` (0 <= i <= n), `ans[i]` is the number of 1's in the binary representation of `i`.

**Example 1:**
```
Input: n = 2
Output: [0,1,1]
Explanation:
0 --> 0 (0 '1' bits)
1 --> 1 (1 '1' bit)
2 --> 10 (1 '1' bit)
```

**Example 2:**
```
Input: n = 5
Output: [0,1,1,2,1,2]
Explanation:
0 --> 0 (0 '1' bits)
1 --> 1 (1 '1' bit)
2 --> 10 (1 '1' bit)
3 --> 11 (2 '1' bits)
4 --> 100 (1 '1' bit)
5 --> 101 (2 '1' bits)
```

## Intuition

The key insight for this problem is to recognize that the number of 1 bits in a number can be calculated using previously computed results. There are several patterns we can exploit:

1. **Least Significant Bit (LSB) Pattern**: The number of 1 bits in `i` equals the number of 1 bits in `i >> 1` (i.e., `i` divided by 2) plus the value of the least significant bit of `i` (i.e., `i & 1`).

2. **Power of Two Pattern**: For a power of 2, there is exactly one 1 bit. For numbers between consecutive powers of 2, we can use previously computed results.

3. **Offset Pattern**: The number of 1 bits in `i` equals the number of 1 bits in `i - offset` plus 1, where `offset` is the largest power of 2 less than or equal to `i`.

We'll use the first pattern (LSB) for our solution as it's the most intuitive and efficient.

## Visual Explanation

Let's visualize the LSB pattern for n = 5:

```
i = 0: Binary = 0
   i >> 1 = 0, Binary = 0
   LSB = 0 & 1 = 0
   Count = count[0 >> 1] + (0 & 1) = count[0] + 0 = 0 + 0 = 0

i = 1: Binary = 1
   i >> 1 = 0, Binary = 0
   LSB = 1 & 1 = 1
   Count = count[1 >> 1] + (1 & 1) = count[0] + 1 = 0 + 1 = 1

i = 2: Binary = 10
   i >> 1 = 1, Binary = 1
   LSB = 2 & 1 = 0
   Count = count[2 >> 1] + (2 & 1) = count[1] + 0 = 1 + 0 = 1

i = 3: Binary = 11
   i >> 1 = 1, Binary = 1
   LSB = 3 & 1 = 1
   Count = count[3 >> 1] + (3 & 1) = count[1] + 1 = 1 + 1 = 2

i = 4: Binary = 100
   i >> 1 = 2, Binary = 10
   LSB = 4 & 1 = 0
   Count = count[4 >> 1] + (4 & 1) = count[2] + 0 = 1 + 0 = 1

i = 5: Binary = 101
   i >> 1 = 2, Binary = 10
   LSB = 5 & 1 = 1
   Count = count[5 >> 1] + (5 & 1) = count[2] + 1 = 1 + 1 = 2
```

![Counting Bits Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize an array `ans` of size `n + 1` with all zeros.
2. Set `ans[0] = 0` since 0 has no 1 bits.
3. For each number `i` from 1 to `n`:
   - Calculate the number of 1 bits using the formula: `ans[i] = ans[i >> 1] + (i & 1)`.
4. Return the `ans` array.

## Code Implementation

### Python

```python
def countBits(n):
    # Initialize the result array with 0s
    ans = [0] * (n + 1)
    
    # For each number from 1 to n
    for i in range(1, n + 1):
        # Number of 1 bits = number of 1 bits in i//2 + least significant bit
        ans[i] = ans[i >> 1] + (i & 1)
    
    return ans
```

### JavaScript

```javascript
function countBits(n) {
    // Initialize the result array with 0s
    const ans = new Array(n + 1).fill(0);
    
    // For each number from 1 to n
    for (let i = 1; i <= n; i++) {
        // Number of 1 bits = number of 1 bits in i//2 + least significant bit
        ans[i] = ans[i >> 1] + (i & 1);
    }
    
    return ans;
}
```

## Alternative Approach: Brian Kernighan's Algorithm

Another approach is to use Brian Kernighan's algorithm, which counts the number of set bits by repeatedly removing the rightmost set bit.

### Python (Brian Kernighan's Algorithm)

```python
def countBits(n):
    ans = [0] * (n + 1)
    
    for i in range(1, n + 1):
        count = 0
        num = i
        while num:
            num &= (num - 1)  # Clear the least significant set bit
            count += 1
        ans[i] = count
    
    return ans
```

### JavaScript (Brian Kernighan's Algorithm)

```javascript
function countBits(n) {
    const ans = new Array(n + 1).fill(0);
    
    for (let i = 1; i <= n; i++) {
        let count = 0;
        let num = i;
        while (num) {
            num &= (num - 1);  // Clear the least significant set bit
            count++;
        }
        ans[i] = count;
    }
    
    return ans;
}
```

## Time and Space Complexity

### Dynamic Programming Approach:
- **Time Complexity**: O(n), where n is the input integer. We iterate through each number from 0 to n once.
- **Space Complexity**: O(n) for the result array.

### Brian Kernighan's Algorithm:
- **Time Complexity**: O(n * log n), where n is the input integer. For each number, we perform at most log(n) operations to count the set bits.
- **Space Complexity**: O(n) for the result array.

## Detailed Walkthrough with Example

Let's trace through the dynamic programming approach with n = 5:

```
Initialize ans = [0, 0, 0, 0, 0, 0]

i = 1:
  ans[1] = ans[1 >> 1] + (1 & 1) = ans[0] + 1 = 0 + 1 = 1
  ans = [0, 1, 0, 0, 0, 0]

i = 2:
  ans[2] = ans[2 >> 1] + (2 & 1) = ans[1] + 0 = 1 + 0 = 1
  ans = [0, 1, 1, 0, 0, 0]

i = 3:
  ans[3] = ans[3 >> 1] + (3 & 1) = ans[1] + 1 = 1 + 1 = 2
  ans = [0, 1, 1, 2, 0, 0]

i = 4:
  ans[4] = ans[4 >> 1] + (4 & 1) = ans[2] + 0 = 1 + 0 = 1
  ans = [0, 1, 1, 2, 1, 0]

i = 5:
  ans[5] = ans[5 >> 1] + (5 & 1) = ans[2] + 1 = 1 + 1 = 2
  ans = [0, 1, 1, 2, 1, 2]

Return ans = [0, 1, 1, 2, 1, 2]
```

## Edge Cases and Optimizations

- **n = 0**: The result is [0], which is handled correctly by our implementation.
- **Large n**: The solution scales well for large n, with O(n) time complexity.

**Optimization**: We can use bit manipulation to optimize the solution further. The formula `ans[i] = ans[i >> 1] + (i & 1)` already uses bit manipulation for efficiency.

## Common Mistakes

1. **Not handling the base case**: Make sure to initialize ans[0] = 0.
2. **Using a less efficient algorithm**: Using a naive approach to count bits for each number would result in O(n * log n) time complexity.
3. **Off-by-one errors**: Be careful with the array size and loop bounds.

## Related Problems

1. **Number of 1 Bits**: Count the number of 1 bits in a single integer.
2. **Power of Two**: Determine if a number is a power of two.
3. **Bitwise AND of Numbers Range**: Find the bitwise AND of all numbers in a range.
4. **Single Number**: Find the number that appears only once in an array where all other numbers appear twice. 