# Bit Manipulation

Bit manipulation is the act of algorithmically manipulating bits or other pieces of data shorter than a byte. It's a powerful technique that can optimize space and time complexity in various algorithms.

## Table of Contents
- [Basic Bit Operations](#basic-bit-operations)
- [Common Bit Manipulation Techniques](#common-bit-manipulation-techniques)
- [Bit Manipulation Tricks](#bit-manipulation-tricks)
- [Applications](#applications)
- [Related Blind 75 & Grind 75 Problems](#related-blind-75--grind-75-problems)
- [Tips and Tricks](#tips-and-tricks)

## Basic Bit Operations

### Bitwise AND (&)
Returns 1 if both corresponding bits are 1, otherwise returns 0.

#### Visual Explanation
```
    1 0 1 1 0 1
AND 1 1 0 1 0 0
  = 1 0 0 1 0 0
```

#### Implementation
```python
result = a & b
```

### Bitwise OR (|)
Returns 1 if at least one of the corresponding bits is 1, otherwise returns 0.

#### Visual Explanation
```
   1 0 1 1 0 1
OR 1 1 0 1 0 0
 = 1 1 1 1 0 1
```

#### Implementation
```python
result = a | b
```

### Bitwise XOR (^)
Returns 1 if exactly one of the corresponding bits is 1, otherwise returns 0.

#### Visual Explanation
```
    1 0 1 1 0 1
XOR 1 1 0 1 0 0
  = 0 1 1 0 0 1
```

#### Implementation
```python
result = a ^ b
```

### Bitwise NOT (~)
Inverts all the bits (0 becomes 1, 1 becomes 0).

#### Visual Explanation
```
NOT 1 0 1 1 0 1
  = 0 1 0 0 1 0
```

#### Implementation
```python
result = ~a
```

### Left Shift (<<)
Shifts the bits to the left by a specified number of positions. Zeros are shifted in from the right.

#### Visual Explanation
```
5 << 2
5 in binary: 0 0 0 0 0 1 0 1
Shift left by 2: 0 0 0 1 0 1 0 0
Result: 20
```

#### Implementation
```python
result = a << b  # Shift a left by b bits
```

### Right Shift (>>)
Shifts the bits to the right by a specified number of positions. For signed numbers, the sign bit is preserved.

#### Visual Explanation
```
20 >> 2
20 in binary: 0 0 0 1 0 1 0 0
Shift right by 2: 0 0 0 0 0 1 0 1
Result: 5
```

#### Implementation
```python
result = a >> b  # Shift a right by b bits
```

## Common Bit Manipulation Techniques

### Check if a bit is set
Determine if the i-th bit of a number is 1.

#### Visual Explanation
```
Check if the 3rd bit of 42 is set:
42 in binary: 0 0 1 0 1 0 1 0
                    ^
                    3rd bit (0-indexed)

42 & (1 << 3) = 0 0 1 0 1 0 1 0
                & 0 0 0 0 1 0 0 0
                = 0 0 0 0 1 0 0 0 = 8 (non-zero, so the bit is set)
```

#### Implementation
```python
def is_bit_set(num, i):
    return (num & (1 << i)) != 0
```

### Set a bit
Set the i-th bit of a number to 1.

#### Visual Explanation
```
Set the 2nd bit of 42:
42 in binary: 0 0 1 0 1 0 1 0
                     ^
                     2nd bit (0-indexed)

42 | (1 << 2) = 0 0 1 0 1 0 1 0
                | 0 0 0 0 0 1 0 0
                = 0 0 1 0 1 1 1 0 = 46
```

#### Implementation
```python
def set_bit(num, i):
    return num | (1 << i)
```

### Clear a bit
Set the i-th bit of a number to 0.

#### Visual Explanation
```
Clear the 1st bit of 42:
42 in binary: 0 0 1 0 1 0 1 0
                      ^
                      1st bit (0-indexed)

42 & ~(1 << 1) = 0 0 1 0 1 0 1 0
                 & 1 1 1 1 1 1 0 1
                 = 0 0 1 0 1 0 0 0 = 40
```

#### Implementation
```python
def clear_bit(num, i):
    return num & ~(1 << i)
```

### Toggle a bit
Flip the i-th bit of a number (0 becomes 1, 1 becomes 0).

#### Visual Explanation
```
Toggle the 0th bit of 42:
42 in binary: 0 0 1 0 1 0 1 0
                        ^
                        0th bit (0-indexed)

42 ^ (1 << 0) = 0 0 1 0 1 0 1 0
                ^ 0 0 0 0 0 0 0 1
                = 0 0 1 0 1 0 1 1 = 43
```

#### Implementation
```python
def toggle_bit(num, i):
    return num ^ (1 << i)
```

### Count set bits (Hamming Weight)
Count the number of 1 bits in a number.

#### Visual Explanation
```
Count set bits in 42:
42 in binary: 0 0 1 0 1 0 1 0
Number of 1s: 3
```

#### Implementation
```python
def count_set_bits(num):
    count = 0
    while num:
        count += num & 1
        num >>= 1
    return count

# More efficient method (Brian Kernighan's Algorithm)
def count_set_bits_efficient(num):
    count = 0
    while num:
        num &= (num - 1)  # Clear the least significant set bit
        count += 1
    return count
```

### Check if a number is a power of 2
A number is a power of 2 if it has exactly one bit set.

#### Visual Explanation
```
Is 16 a power of 2?
16 in binary: 0 0 0 1 0 0 0 0
Number of 1s: 1
Yes, 16 is a power of 2.

Is 18 a power of 2?
18 in binary: 0 0 0 1 0 0 1 0
Number of 1s: 2
No, 18 is not a power of 2.
```

#### Implementation
```python
def is_power_of_two(num):
    return num > 0 and (num & (num - 1)) == 0
```

## Bit Manipulation Tricks

### Find the rightmost set bit
Find the position of the rightmost bit that is set to 1.

#### Visual Explanation
```
Find the rightmost set bit in 42:
42 in binary: 0 0 1 0 1 0 1 0
                        ^
                        Rightmost set bit is at position 1 (0-indexed)

42 & -42 = 0 0 1 0 1 0 1 0
         & 1 1 0 1 0 1 1 0 (2's complement of 42)
         = 0 0 0 0 0 0 1 0 = 2 (which is 1 << 1)
```

#### Implementation
```python
def rightmost_set_bit(num):
    return num & -num
```

### Clear all bits from LSB to i
Clear all bits from the least significant bit (LSB) to the i-th bit (inclusive).

#### Visual Explanation
```
Clear all bits from LSB to the 3rd bit in 42:
42 in binary: 0 0 1 0 1 0 1 0
                    ^
                    3rd bit (0-indexed)

42 & ~((1 << 4) - 1) = 0 0 1 0 1 0 1 0
                       & 1 1 1 1 0 0 0 0
                       = 0 0 1 0 0 0 0 0 = 32
```

#### Implementation
```python
def clear_bits_from_lsb_to_i(num, i):
    return num & ~((1 << (i + 1)) - 1)
```

### Clear all bits from i to MSB
Clear all bits from the i-th bit to the most significant bit (MSB).

#### Visual Explanation
```
Clear all bits from the 3rd bit to MSB in 42:
42 in binary: 0 0 1 0 1 0 1 0
                    ^
                    3rd bit (0-indexed)

42 & ((1 << 3) - 1) = 0 0 1 0 1 0 1 0
                      & 0 0 0 0 0 1 1 1
                      = 0 0 0 0 0 0 1 0 = 2
```

#### Implementation
```python
def clear_bits_from_i_to_msb(num, i):
    return num & ((1 << i) - 1)
```

### Swap two numbers without a temporary variable
Use XOR to swap two numbers without using a temporary variable.

#### Visual Explanation
```
Swap a = 5 and b = 9:

a = a ^ b = 5 ^ 9 = 12
b = a ^ b = 12 ^ 9 = 5
a = a ^ b = 12 ^ 5 = 9

Result: a = 9, b = 5
```

#### Implementation
```python
def swap(a, b):
    a = a ^ b
    b = a ^ b
    a = a ^ b
    return a, b
```

### Find the missing number
Find the missing number in an array containing n distinct numbers from 0 to n.

#### Visual Explanation
```
Find the missing number in [0, 1, 3, 4]:
XOR all numbers from 0 to n: 0 ^ 1 ^ 2 ^ 3 ^ 4 = 4
XOR all numbers in the array: 0 ^ 1 ^ 3 ^ 4 = 6
Missing number = 4 ^ 6 = 2
```

#### Implementation
```python
def find_missing_number(nums):
    n = len(nums)
    missing = n
    for i in range(n):
        missing ^= i ^ nums[i]
    return missing
```

## Applications

### Bitboards in Chess
Bitboards are used to represent the chess board, where each bit represents a square on the board.

#### Visual Explanation
```
Representing a chess board with white pawns on the second rank:
00000000
11111111  <- White pawns on the second rank
00000000
00000000
00000000
00000000
00000000
00000000

This can be represented as a 64-bit integer: 0x000000000000FF00
```

### Bloom Filters
Bloom filters use bit arrays to represent sets and test for membership.

#### Visual Explanation
```
A Bloom filter with 10 bits:
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0]

After adding "apple" (hash functions map to positions 1, 4, 7):
[0, 1, 0, 0, 1, 0, 0, 1, 0, 0]

After adding "banana" (hash functions map to positions 2, 5, 9):
[0, 1, 1, 0, 1, 1, 0, 1, 0, 1]

Testing if "cherry" is in the set (hash functions map to positions 0, 3, 8):
Bits at positions 0, 3, 8 are not all set, so "cherry" is definitely not in the set.
```

### Compression
Bit manipulation is used in various compression algorithms to reduce data size.

#### Example: Run-Length Encoding
```
Original data: AAAABBBCCDAA
Encoded data: 4A3B2C1D2A (each character is preceded by its count)
```

## Related Blind 75 & Grind 75 Problems

1. **Single Number** (Blind 75 #136)
   - Problem: Find the single element in an array where every other element appears twice.
   - Solution: XOR all elements together. The result is the single number.
   - [LeetCode #136](https://leetcode.com/problems/single-number/)

2. **Number of 1 Bits** (Blind 75 #191)
   - Problem: Count the number of '1' bits in an integer.
   - Solution: Use Brian Kernighan's algorithm or a simple loop.
   - [LeetCode #191](https://leetcode.com/problems/number-of-1-bits/)

3. **Counting Bits** (Blind 75 #338)
   - Problem: For every number from 0 to n, return the number of 1 bits.
   - Solution: Use dynamic programming with bit manipulation.
   - [LeetCode #338](https://leetcode.com/problems/counting-bits/)

4. **Reverse Bits** (Blind 75 #190)
   - Problem: Reverse the bits of a 32-bit unsigned integer.
   - Solution: Use bit manipulation to swap bits.
   - [LeetCode #190](https://leetcode.com/problems/reverse-bits/)

5. **Missing Number** (Blind 75 #268)
   - Problem: Find the missing number in an array containing n distinct numbers from 0 to n.
   - Solution: Use XOR to find the missing number.
   - [LeetCode #268](https://leetcode.com/problems/missing-number/)

6. **Sum of Two Integers** (Blind 75 #371)
   - Problem: Add two integers without using the + or - operators.
   - Solution: Use bit manipulation to simulate addition.
   - [LeetCode #371](https://leetcode.com/problems/sum-of-two-integers/)

## Tips and Tricks

1. **Understanding Binary Representation**:
   - Remember that the rightmost bit is the least significant bit (LSB) and has a value of 2^0 = 1.
   - Each bit to the left has a value of 2^i, where i is the position (0-indexed).
   - Negative numbers are typically represented using two's complement.

2. **Common Bit Manipulation Patterns**:
   - Use `n & (n - 1)` to clear the rightmost set bit.
   - Use `n & -n` to isolate the rightmost set bit.
   - Use `n | (1 << i)` to set the i-th bit.
   - Use `n & ~(1 << i)` to clear the i-th bit.
   - Use `n ^ (1 << i)` to toggle the i-th bit.

3. **Optimizing with Bit Manipulation**:
   - Use bit manipulation to avoid expensive operations like division and modulo.
   - For example, `n % 2` is equivalent to `n & 1`, and `n / 2` is equivalent to `n >> 1` for non-negative numbers.
   - Use bit manipulation to pack multiple values into a single integer for space optimization.

4. **Debugging Bit Manipulation**:
   - Convert numbers to binary representation for easier visualization.
   - Test with small, simple examples first.
   - Use a step-by-step approach to track bit changes.

5. **Language-Specific Considerations**:
   - In Python, integers have arbitrary precision, so be careful with operations that assume a fixed number of bits.
   - In languages like Java and C++, be aware of the sign bit and how it behaves with shift operations.
   - Use unsigned types when appropriate to avoid sign-related issues. 