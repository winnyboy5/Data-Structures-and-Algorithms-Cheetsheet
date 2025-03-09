# Reverse Bits

## Problem Statement

Reverse bits of a given 32 bits unsigned integer.

**Example 1:**
```
Input: n = 00000010100101000001111010011100
Output:    964176192 (00111001011110000010100101000000)
Explanation: The input binary string 00000010100101000001111010011100 represents the unsigned integer 43261596, so return 964176192 which its binary representation is 00111001011110000010100101000000.
```

**Example 2:**
```
Input: n = 11111111111111111111111111111101
Output:   3221225471 (10111111111111111111111111111111)
Explanation: The input binary string 11111111111111111111111111111101 represents the unsigned integer 4294967293, so return 3221225471 which its binary representation is 10111111111111111111111111111111.
```

## Intuition

The key insight for this problem is to understand that reversing the bits of a 32-bit integer means taking the bit at position i (from the right) and placing it at position (31 - i). For example, the bit at position 0 (the rightmost bit) should go to position 31 (the leftmost bit), the bit at position 1 should go to position 30, and so on.

We can approach this problem in several ways:

1. **Bit-by-Bit Reversal**: Iterate through each bit of the input number, extract it, and place it in the appropriate position in the result.

2. **Divide and Conquer**: Swap adjacent bits, then adjacent pairs of bits, then adjacent nibbles, and so on.

3. **Lookup Table**: Use a precomputed table to reverse the bits of each byte, then combine the results.

The bit-by-bit reversal approach is the most straightforward and intuitive, so we'll focus on that.

## Visual Explanation

Let's visualize the bit-by-bit reversal approach with a simplified 8-bit example:

Input: `n = 10101100` (decimal 172)

```
Initialize result = 00000000

Iteration 1 (rightmost bit of n):
  n & 1 = 10101100 & 00000001 = 00000000 (bit is 0)
  result = (result << 1) | (n & 1) = (00000000 << 1) | 0 = 00000000
  n = n >> 1 = 10101100 >> 1 = 01010110

Iteration 2:
  n & 1 = 01010110 & 00000001 = 00000000 (bit is 0)
  result = (result << 1) | (n & 1) = (00000000 << 1) | 0 = 00000000
  n = n >> 1 = 01010110 >> 1 = 00101011

Iteration 3:
  n & 1 = 00101011 & 00000001 = 00000001 (bit is 1)
  result = (result << 1) | (n & 1) = (00000000 << 1) | 1 = 00000001
  n = n >> 1 = 00101011 >> 1 = 00010101

Iteration 4:
  n & 1 = 00010101 & 00000001 = 00000001 (bit is 1)
  result = (result << 1) | (n & 1) = (00000001 << 1) | 1 = 00000011
  n = n >> 1 = 00010101 >> 1 = 00001010

Iteration 5:
  n & 1 = 00001010 & 00000001 = 00000000 (bit is 0)
  result = (result << 1) | (n & 1) = (00000011 << 1) | 0 = 00000110
  n = n >> 1 = 00001010 >> 1 = 00000101

Iteration 6:
  n & 1 = 00000101 & 00000001 = 00000001 (bit is 1)
  result = (result << 1) | (n & 1) = (00000110 << 1) | 1 = 00001101
  n = n >> 1 = 00000101 >> 1 = 00000010

Iteration 7:
  n & 1 = 00000010 & 00000001 = 00000000 (bit is 0)
  result = (result << 1) | (n & 1) = (00001101 << 1) | 0 = 00011010
  n = n >> 1 = 00000010 >> 1 = 00000001

Iteration 8:
  n & 1 = 00000001 & 00000001 = 00000001 (bit is 1)
  result = (result << 1) | (n & 1) = (00011010 << 1) | 1 = 00110101
  n = n >> 1 = 00000001 >> 1 = 00000000

Output: result = 00110101 (decimal 53)
```

Checking our work: The reverse of `10101100` is `00110101`, which is correct.

For a 32-bit integer, we would perform the same process 32 times.

![Reverse Bits Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize `result` to 0.
2. Iterate 32 times (for each bit in the 32-bit integer):
   - Shift `result` to the left by 1 bit to make room for the next bit.
   - Extract the rightmost bit of `n` using `n & 1`.
   - Add the extracted bit to `result` using the OR operation (`|`).
   - Shift `n` to the right by 1 bit to process the next bit.
3. Return `result`.

## Code Implementation

### Python

```python
def reverseBits(n):
    result = 0
    
    # Iterate through all 32 bits
    for i in range(32):
        # Shift result to the left by 1 bit
        result <<= 1
        
        # Extract the rightmost bit of n and add it to result
        result |= (n & 1)
        
        # Shift n to the right by 1 bit
        n >>= 1
    
    return result
```

### JavaScript

```javascript
function reverseBits(n) {
    let result = 0;
    
    // Iterate through all 32 bits
    for (let i = 0; i < 32; i++) {
        // Shift result to the left by 1 bit
        result <<= 1;
        
        // Extract the rightmost bit of n and add it to result
        result |= (n & 1);
        
        // Shift n to the right by 1 bit
        n >>>= 1;  // Use unsigned right shift
    }
    
    // Convert to unsigned 32-bit integer
    return result >>> 0;
}
```

## Alternative Approach: Divide and Conquer

Another approach is to use a divide-and-conquer strategy, where we swap adjacent bits, then adjacent pairs of bits, and so on. This approach is more efficient for hardware implementation but less intuitive.

### Python (Divide and Conquer)

```python
def reverseBits(n):
    # Swap adjacent bits
    n = ((n & 0x55555555) << 1) | ((n & 0xAAAAAAAA) >> 1)
    # Swap adjacent pairs of bits
    n = ((n & 0x33333333) << 2) | ((n & 0xCCCCCCCC) >> 2)
    # Swap adjacent nibbles
    n = ((n & 0x0F0F0F0F) << 4) | ((n & 0xF0F0F0F0) >> 4)
    # Swap adjacent bytes
    n = ((n & 0x00FF00FF) << 8) | ((n & 0xFF00FF00) >> 8)
    # Swap adjacent 16-bit chunks
    n = ((n & 0x0000FFFF) << 16) | ((n & 0xFFFF0000) >> 16)
    
    return n
```

### JavaScript (Divide and Conquer)

```javascript
function reverseBits(n) {
    // Swap adjacent bits
    n = ((n & 0x55555555) << 1) | ((n & 0xAAAAAAAA) >>> 1);
    // Swap adjacent pairs of bits
    n = ((n & 0x33333333) << 2) | ((n & 0xCCCCCCCC) >>> 2);
    // Swap adjacent nibbles
    n = ((n & 0x0F0F0F0F) << 4) | ((n & 0xF0F0F0F0) >>> 4);
    // Swap adjacent bytes
    n = ((n & 0x00FF00FF) << 8) | ((n & 0xFF00FF00) >>> 8);
    // Swap adjacent 16-bit chunks
    n = ((n & 0x0000FFFF) << 16) | ((n & 0xFFFF0000) >>> 16);
    
    return n >>> 0;  // Convert to unsigned 32-bit integer
}
```

## Time and Space Complexity

### Bit-by-Bit Approach:
- **Time Complexity**: O(1), as we always perform exactly 32 iterations regardless of the input size.
- **Space Complexity**: O(1), as we only use a single variable regardless of the input size.

### Divide and Conquer Approach:
- **Time Complexity**: O(1), as we perform a fixed number of operations regardless of the input size.
- **Space Complexity**: O(1), as we only use a single variable regardless of the input size.

## Detailed Walkthrough with Example

Let's trace through the bit-by-bit approach with Example 1: `n = 43261596` (binary: `00000010100101000001111010011100`)

```
Initialize result = 0

Iteration 1 (rightmost bit of n):
  n & 1 = 00000010100101000001111010011100 & 00000000000000000000000000000001 = 0
  result = (result << 1) | (n & 1) = (0 << 1) | 0 = 0
  n = n >> 1 = 00000001010010100000111101001110

Iteration 2:
  n & 1 = 00000001010010100000111101001110 & 00000000000000000000000000000001 = 0
  result = (result << 1) | (n & 1) = (0 << 1) | 0 = 0
  n = n >> 1 = 00000000101001010000011110100111

... (continuing for all 32 bits) ...

Final result = 00111001011110000010100101000000 (decimal: 964176192)
```

## Edge Cases and Optimizations

- **Zero Input**: If the input is 0, the output will also be 0.
- **All Ones**: If the input is all ones (4294967295 in decimal), the output will also be all ones.
- **Palindromic Bit Patterns**: If the input bit pattern is a palindrome (reads the same forwards and backwards), the output will be the same as the input.

**Optimization**: The divide-and-conquer approach is more efficient for hardware implementation and can be faster in practice, especially for larger bit widths. However, for 32-bit integers, both approaches are fast enough for most purposes.

## Common Mistakes

1. **Not handling all 32 bits**: Make sure to process all 32 bits, even if the leading bits are zeros.
2. **Using signed right shift in JavaScript**: In JavaScript, use the unsigned right shift operator (`>>>`) to ensure correct behavior with the sign bit.
3. **Not converting the result to an unsigned integer in JavaScript**: In JavaScript, make sure to convert the final result to an unsigned 32-bit integer using `>>> 0`.

## Related Problems

1. **Reverse Integer**: Reverse the digits of an integer.
2. **Number of 1 Bits**: Count the number of 1 bits in an integer.
3. **Single Number**: Find the only number that appears once in an array where all other numbers appear twice.
4. **Bitwise AND of Numbers Range**: Find the bitwise AND of all numbers in a range. 