# Sum of Two Integers

## Problem Statement

Given two integers `a` and `b`, return the sum of the two integers without using the operators `+` and `-`.

**Example 1:**
```
Input: a = 1, b = 2
Output: 3
```

**Example 2:**
```
Input: a = 2, b = 3
Output: 5
```

## Intuition

Since we can't use the `+` and `-` operators, we need to rely on bitwise operations to perform addition. The key insight is to understand how binary addition works and implement it using bitwise operations.

In binary addition, when we add two bits:
- 0 + 0 = 0
- 0 + 1 = 1
- 1 + 0 = 1
- 1 + 1 = 0 (with a carry of 1)

This behavior can be mimicked using the XOR (`^`) operation for the sum without carry, and the AND (`&`) operation with a left shift (`<<`) for the carry.

## Visual Explanation

Let's visualize the solution with Example 1: `a = 1, b = 2`

```
Step 1: Calculate sum without carry using XOR
a = 1 (01 in binary)
b = 2 (10 in binary)
a ^ b = 01 ^ 10 = 11 (3 in decimal)

Step 2: Calculate carry using AND and left shift
a & b = 01 & 10 = 00
(a & b) << 1 = 00 << 1 = 00 (0 in decimal)

Step 3: Update a and b
a = a ^ b = 3
b = (a & b) << 1 = 0

Step 4: Since b = 0, return a = 3
```

Let's try a more complex example: `a = 5, b = 3`

```
Step 1: Calculate sum without carry using XOR
a = 5 (101 in binary)
b = 3 (011 in binary)
a ^ b = 101 ^ 011 = 110 (6 in decimal)

Step 2: Calculate carry using AND and left shift
a & b = 101 & 011 = 001
(a & b) << 1 = 001 << 1 = 010 (2 in decimal)

Step 3: Update a and b
a = a ^ b = 6
b = (a & b) << 1 = 2

Step 4: Since b != 0, repeat the process
a = 6 (110 in binary)
b = 2 (010 in binary)
a ^ b = 110 ^ 010 = 100 (4 in decimal)
a & b = 110 & 010 = 010
(a & b) << 1 = 010 << 1 = 100 (4 in decimal)
a = 4, b = 4

Step 5: Since b != 0, repeat the process
a = 4 (100 in binary)
b = 4 (100 in binary)
a ^ b = 100 ^ 100 = 000 (0 in decimal)
a & b = 100 & 100 = 100
(a & b) << 1 = 100 << 1 = 1000 (8 in decimal)
a = 0, b = 8

Step 6: Since b != 0, repeat the process
a = 0 (000 in binary)
b = 8 (1000 in binary)
a ^ b = 000 ^ 1000 = 1000 (8 in decimal)
a & b = 000 & 1000 = 000
(a & b) << 1 = 000 << 1 = 000 (0 in decimal)
a = 8, b = 0

Step 7: Since b = 0, return a = 8
```

![Sum of Two Integers Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Use XOR (`^`) to calculate the sum without carry.
2. Use AND (`&`) with a left shift (`<<`) to calculate the carry.
3. Repeat steps 1 and 2 until there's no carry (i.e., `b` becomes 0).
4. Return the final sum.

## Code Implementation

### Python

```python
def getSum(a, b):
    # Python integers have unlimited precision, so we need to mask to simulate 32-bit integers
    mask = 0xFFFFFFFF
    
    # Loop until there's no carry
    while b != 0:
        # Calculate sum without carry
        sum_without_carry = (a ^ b) & mask
        
        # Calculate carry
        carry = ((a & b) << 1) & mask
        
        # Update a and b
        a = sum_without_carry
        b = carry
    
    # Handle negative numbers (if the 32nd bit is 1, it's a negative number)
    if a > 0x7FFFFFFF:
        a = ~(a ^ mask)
    
    return a
```

### JavaScript

```javascript
function getSum(a, b) {
    // Loop until there's no carry
    while (b !== 0) {
        // Calculate sum without carry
        const sumWithoutCarry = a ^ b;
        
        // Calculate carry
        const carry = (a & b) << 1;
        
        // Update a and b
        a = sumWithoutCarry;
        b = carry;
    }
    
    return a;
}
```

## Time and Space Complexity

- **Time Complexity**: O(log n), where n is the maximum of the absolute values of `a` and `b`. In the worst case, we need to perform the loop log(n) times, which is the number of bits in the larger number.
- **Space Complexity**: O(1), as we only use a constant amount of extra space regardless of the input size.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2: `a = 2, b = 3`

```
Step 1: Calculate sum without carry using XOR
a = 2 (10 in binary)
b = 3 (11 in binary)
a ^ b = 10 ^ 11 = 01 (1 in decimal)

Step 2: Calculate carry using AND and left shift
a & b = 10 & 11 = 10
(a & b) << 1 = 10 << 1 = 100 (4 in decimal)

Step 3: Update a and b
a = a ^ b = 1
b = (a & b) << 1 = 4

Step 4: Since b != 0, repeat the process
a = 1 (01 in binary)
b = 4 (100 in binary)
a ^ b = 01 ^ 100 = 101 (5 in decimal)
a & b = 01 & 100 = 000
(a & b) << 1 = 000 << 1 = 000 (0 in decimal)
a = 5, b = 0

Step 5: Since b = 0, return a = 5
```

## Edge Cases and Optimizations

- **Negative Numbers**: In languages like Python where integers have unlimited precision, we need to handle negative numbers carefully by masking to simulate 32-bit integers.
- **Zero**: If one of the numbers is zero, the function will return the other number directly.
- **Overflow**: In languages with fixed-size integers, we need to be careful about overflow. However, the bitwise operations naturally handle overflow in most languages.

## Common Mistakes

1. **Not handling negative numbers**: In languages like Python, we need to mask the result to simulate 32-bit integers and handle negative numbers correctly.
2. **Infinite loops**: If the carry calculation is not done correctly, the loop might never terminate.
3. **Using the `+` or `-` operators**: The problem specifically asks not to use these operators, so make sure to rely only on bitwise operations.

## Related Problems

1. **Add Binary**: Add two binary strings and return their sum as a binary string.
2. **Plus One**: Given a non-empty array of decimal digits representing a non-negative integer, increment one to the integer.
3. **Add Strings**: Given two non-negative integers represented as string, return their sum.
4. **Subtract Two Integers**: Implement subtraction without using the `-` operator. 