# Number of 1 Bits

## Problem Statement

Write a function that takes an unsigned integer and returns the number of '1' bits it has (also known as the Hamming weight).

**Note:**
- Note that in some languages, such as Java, there is no unsigned integer type. In this case, the input will be given as a signed integer type. It should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
- In Java, the compiler represents the signed integers using 2's complement notation. Therefore, in Example 3, the input represents the signed integer `-3`.

**Example 1:**
```
Input: n = 00000000000000000000000000001011
Output: 3
Explanation: The input binary string 00000000000000000000000000001011 has a total of three '1' bits.
```

**Example 2:**
```
Input: n = 00000000000000000000000010000000
Output: 1
Explanation: The input binary string 00000000000000000000000010000000 has a total of one '1' bit.
```

**Example 3:**
```
Input: n = 11111111111111111111111111111101
Output: 31
Explanation: The input binary string 11111111111111111111111111111101 has a total of thirty one '1' bits.
```

## Intuition

The key insight for this problem is to count the number of '1' bits in the binary representation of the given integer. There are several approaches to solve this problem:

1. **Bit Manipulation with Loop**: Iterate through each bit of the integer and count the '1' bits.
2. **Bit Manipulation with n & (n-1)**: Use the trick that n & (n-1) removes the rightmost '1' bit from n.
3. **Built-in Functions**: Some languages provide built-in functions to count the number of '1' bits.

The most efficient approach is the second one, which uses the property that n & (n-1) removes the rightmost '1' bit from n. By repeatedly applying this operation until n becomes 0, we can count the number of '1' bits.

## Visual Explanation

Let's visualize the solution with Example 1: `n = 00000000000000000000000000001011` (11 in decimal)

Using the n & (n-1) approach:

```
Step 1: n = 11 (1011 in binary)
n - 1 = 10 (1010 in binary)
n & (n-1) = 1011 & 1010 = 1010 (10 in decimal)
Count = 1

Step 2: n = 10 (1010 in binary)
n - 1 = 9 (1001 in binary)
n & (n-1) = 1010 & 1001 = 1000 (8 in decimal)
Count = 2

Step 3: n = 8 (1000 in binary)
n - 1 = 7 (0111 in binary)
n & (n-1) = 1000 & 0111 = 0000 (0 in decimal)
Count = 3

Since n = 0, we stop and return Count = 3.
```

![Number of 1 Bits Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

### Approach 1: Bit Manipulation with Loop

1. Initialize a counter to 0.
2. Iterate through each bit of the integer (usually 32 bits for a standard integer).
3. For each bit, check if it's a '1' by using the bitwise AND operation with a mask (1 << i).
4. If the result is non-zero, increment the counter.
5. Return the counter.

### Approach 2: Bit Manipulation with n & (n-1)

1. Initialize a counter to 0.
2. While n is not 0:
   - Apply the operation n = n & (n-1) to remove the rightmost '1' bit.
   - Increment the counter.
3. Return the counter.

## Code Implementation

### Python

```python
def hammingWeight(n):
    """
    :type n: int
    :rtype: int
    """
    count = 0
    while n:
        n &= (n - 1)  # Remove the rightmost '1' bit
        count += 1
    return count
```

### JavaScript

```javascript
function hammingWeight(n) {
    let count = 0;
    while (n !== 0) {
        n &= (n - 1);  // Remove the rightmost '1' bit
        count++;
    }
    return count;
}
```

## Alternative Approaches

### Approach 1: Bit Manipulation with Loop

```python
def hammingWeight(n):
    """
    :type n: int
    :rtype: int
    """
    count = 0
    for i in range(32):  # Assuming 32-bit integer
        if (n & (1 << i)) != 0:
            count += 1
    return count
```

```javascript
function hammingWeight(n) {
    let count = 0;
    for (let i = 0; i < 32; i++) {  // Assuming 32-bit integer
        if ((n & (1 << i)) !== 0) {
            count++;
        }
    }
    return count;
}
```

### Approach 3: Using Built-in Functions

```python
def hammingWeight(n):
    """
    :type n: int
    :rtype: int
    """
    return bin(n).count('1')
```

```javascript
function hammingWeight(n) {
    return n.toString(2).split('0').join('').length;
}
```

## Time and Space Complexity

### Approach 2: Bit Manipulation with n & (n-1)

- **Time Complexity**: O(k), where k is the number of '1' bits in the binary representation of n. In the worst case, all bits are '1', so the time complexity is O(log n) for an n-bit integer.
- **Space Complexity**: O(1), as we only use a constant amount of extra space regardless of the input size.

### Approach 1: Bit Manipulation with Loop

- **Time Complexity**: O(1), as we always iterate through a fixed number of bits (32 for a standard integer).
- **Space Complexity**: O(1), as we only use a constant amount of extra space regardless of the input size.

### Approach 3: Using Built-in Functions

- **Time Complexity**: O(log n), as converting to binary and counting '1's takes time proportional to the number of bits.
- **Space Complexity**: O(log n), as we need to store the binary representation of the integer.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2: `n = 00000000000000000000000010000000` (128 in decimal)

Using the n & (n-1) approach:

```
Step 1: n = 128 (10000000 in binary)
n - 1 = 127 (01111111 in binary)
n & (n-1) = 10000000 & 01111111 = 00000000 (0 in decimal)
Count = 1

Since n = 0, we stop and return Count = 1.
```

## Edge Cases and Optimizations

- **Zero**: If the input is 0, the function will return 0 as there are no '1' bits.
- **All Ones**: If all bits are '1', the function will return the number of bits in the integer (32 for a standard integer).
- **Negative Numbers**: In languages that use 2's complement for negative numbers, the function will work correctly for negative numbers as well.

## Common Mistakes

1. **Not handling negative numbers correctly**: In languages that use 2's complement for negative numbers, the leftmost bit is 1 for negative numbers. Make sure your solution handles this correctly.
2. **Using inefficient approaches**: The n & (n-1) approach is more efficient than iterating through each bit, especially for integers with few '1' bits.
3. **Not considering the number of bits**: Different languages may have different integer sizes. Make sure your solution works for the specific integer size in your language.

## Related Problems

1. **Reverse Bits**: Reverse the bits of a given 32-bit unsigned integer.
2. **Power of Two**: Determine if a given integer is a power of two.
3. **Counting Bits**: Given an integer n, return an array where ans[i] is the number of '1' bits in the binary representation of i.
4. **Single Number**: Find the single element in an array where every other element appears twice. 