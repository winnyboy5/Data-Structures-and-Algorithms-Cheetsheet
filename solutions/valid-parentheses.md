# Valid Parentheses

## Problem Statement

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

**Example 1:**
```
Input: s = "()"
Output: true
```

**Example 2:**
```
Input: s = "()[]{}"
Output: true
```

**Example 3:**
```
Input: s = "(]"
Output: false
```

**Example 4:**
```
Input: s = "([)]"
Output: false
```

**Example 5:**
```
Input: s = "{[]}"
Output: true
```

## Intuition

The key insight for this problem is to use a stack data structure to keep track of opening brackets. When we encounter a closing bracket, we check if it matches the most recent opening bracket (which should be at the top of the stack). If it does, we pop the opening bracket from the stack and continue. If not, the string is invalid.

This approach works because brackets must be closed in the correct order, which follows a Last-In-First-Out (LIFO) pattern, perfectly suited for a stack.

## Visual Explanation

Let's visualize the solution with Example 5: `s = "{[]}"`

```
Step 1: Initialize an empty stack: []
        Current character: '{'
        It's an opening bracket, so push it onto the stack: ['{']

Step 2: Current character: '['
        It's an opening bracket, so push it onto the stack: ['{', '[']

Step 3: Current character: ']'
        It's a closing bracket, so check if it matches the top of the stack
        Top of stack is '[', which matches ']', so pop from stack: ['{']

Step 4: Current character: '}'
        It's a closing bracket, so check if it matches the top of the stack
        Top of stack is '{', which matches '}', so pop from stack: []

Step 5: End of string, check if stack is empty
        Stack is empty, so the string is valid

Result: true
```

Now let's visualize Example 4: `s = "([)]"`

```
Step 1: Initialize an empty stack: []
        Current character: '('
        It's an opening bracket, so push it onto the stack: ['(']

Step 2: Current character: '['
        It's an opening bracket, so push it onto the stack: ['(', '[']

Step 3: Current character: ')'
        It's a closing bracket, so check if it matches the top of the stack
        Top of stack is '[', which does NOT match ')', so the string is invalid

Result: false
```

![Valid Parentheses Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize an empty stack.
2. Create a mapping of closing brackets to their corresponding opening brackets: `)` -> `(`, `}` -> `{`, `]` -> `[`.
3. Iterate through each character in the string:
   - If the character is an opening bracket (`(`, `{`, or `[`), push it onto the stack.
   - If the character is a closing bracket (`)`, `}`, or `]`):
     - If the stack is empty, return `false` (there's no matching opening bracket).
     - If the top of the stack doesn't match the corresponding opening bracket for the current closing bracket, return `false` (the brackets don't match).
     - Otherwise, pop the top element from the stack (the matching opening bracket).
4. After processing all characters, check if the stack is empty:
   - If it's empty, return `true` (all opening brackets have been matched).
   - If it's not empty, return `false` (there are unmatched opening brackets).

## Code Implementation

### Python

```python
def isValid(s):
    # Initialize an empty stack
    stack = []
    
    # Define a mapping of closing brackets to their corresponding opening brackets
    bracket_map = {
        ')': '(',
        '}': '{',
        ']': '['
    }
    
    # Iterate through each character in the string
    for char in s:
        # If the character is a closing bracket
        if char in bracket_map:
            # Pop the top element from the stack if it's not empty, otherwise use a dummy value
            top_element = stack.pop() if stack else '#'
            
            # Check if the popped element matches the corresponding opening bracket
            if bracket_map[char] != top_element:
                return False
        else:
            # If the character is an opening bracket, push it onto the stack
            stack.append(char)
    
    # If the stack is empty, all brackets have been matched
    return len(stack) == 0
```

### JavaScript

```javascript
function isValid(s) {
    // Initialize an empty stack
    const stack = [];
    
    // Define a mapping of closing brackets to their corresponding opening brackets
    const bracketMap = {
        ')': '(',
        '}': '{',
        ']': '['
    };
    
    // Iterate through each character in the string
    for (let i = 0; i < s.length; i++) {
        const char = s[i];
        
        // If the character is a closing bracket
        if (char in bracketMap) {
            // Pop the top element from the stack if it's not empty, otherwise use a dummy value
            const topElement = stack.length > 0 ? stack.pop() : '#';
            
            // Check if the popped element matches the corresponding opening bracket
            if (bracketMap[char] !== topElement) {
                return false;
            }
        } else {
            // If the character is an opening bracket, push it onto the stack
            stack.push(char);
        }
    }
    
    // If the stack is empty, all brackets have been matched
    return stack.length === 0;
}
```

## Time and Space Complexity

- **Time Complexity**: O(n), where n is the length of the string. We iterate through each character once.
- **Space Complexity**: O(n) in the worst case, where the string contains only opening brackets. In this case, all brackets would be pushed onto the stack.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2: `s = "()[]{}"`

```
Initialize stack = []
bracket_map = {')': '(', '}': '{', ']': '['}

Step 1: char = '('
        It's an opening bracket, so push it onto the stack
        stack = ['(']

Step 2: char = ')'
        It's a closing bracket, so check if it matches the top of the stack
        top_element = stack.pop() = '('
        bracket_map[')'] = '('
        '(' == '(' ✓
        stack = []

Step 3: char = '['
        It's an opening bracket, so push it onto the stack
        stack = ['[']

Step 4: char = ']'
        It's a closing bracket, so check if it matches the top of the stack
        top_element = stack.pop() = '['
        bracket_map[']'] = '['
        '[' == '[' ✓
        stack = []

Step 5: char = '{'
        It's an opening bracket, so push it onto the stack
        stack = ['{']

Step 6: char = '}'
        It's a closing bracket, so check if it matches the top of the stack
        top_element = stack.pop() = '{'
        bracket_map['}'] = '{'
        '{' == '{' ✓
        stack = []

End of string, check if stack is empty
stack = [] ✓

Result: true
```

## Edge Cases and Optimizations

- **Empty String**: An empty string is considered valid (stack remains empty).
- **Odd Length String**: If the string has an odd length, it cannot be valid (as brackets come in pairs), so we can return `false` immediately.
- **String with Only Opening or Only Closing Brackets**: If the string contains only opening brackets, the stack will not be empty at the end. If it contains only closing brackets, we'll try to pop from an empty stack.

## Common Mistakes

1. **Not checking if the stack is empty before popping**: Always check if the stack has elements before trying to pop.
2. **Not checking if the stack is empty at the end**: Even if all closing brackets match, there might be unmatched opening brackets left in the stack.
3. **Incorrect mapping**: Make sure the mapping of closing brackets to opening brackets is correct.

## Related Problems

1. **Generate Parentheses**: Generate all combinations of well-formed parentheses.
2. **Longest Valid Parentheses**: Find the length of the longest valid (well-formed) parentheses substring.
3. **Remove Invalid Parentheses**: Remove the minimum number of parentheses to make the input string valid.
4. **Minimum Add to Make Parentheses Valid**: Return the minimum number of parentheses we must add to make the string valid. 