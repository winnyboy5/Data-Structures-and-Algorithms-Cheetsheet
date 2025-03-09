# Stacks

A stack is a linear data structure that follows the Last In, First Out (LIFO) principle. Elements are added and removed from the same end, called the "top" of the stack.

## Visual Representation

```
   ┌────┐
   │ 30 │ ← Top (Last In, First Out)
   ├────┤
   │ 20 │
   ├────┤
   │ 10 │
   └────┘
```

## Key Operations

1. **Push**: Add an element to the top of the stack
2. **Pop**: Remove the top element from the stack
3. **Peek/Top**: View the top element without removing it
4. **isEmpty**: Check if the stack is empty

## Implementation in Python and JavaScript

### Python

#### Using a List

```python
class Stack:
    def __init__(self):
        self.items = []
    
    def push(self, item):
        self.items.append(item)
    
    def pop(self):
        if not self.is_empty():
            return self.items.pop()
        return None
    
    def peek(self):
        if not self.is_empty():
            return self.items[-1]
        return None
    
    def is_empty(self):
        return len(self.items) == 0
    
    def size(self):
        return len(self.items)

# Example usage
stack = Stack()
stack.push(10)
stack.push(20)
stack.push(30)
print(stack.peek())  # Output: 30
print(stack.pop())   # Output: 30
print(stack.pop())   # Output: 20
print(stack.size())  # Output: 1
```

#### Using a Linked List

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.next = None

class Stack:
    def __init__(self):
        self.top = None
        self.bottom = None
        self.length = 0
    
    def push(self, value):
        new_node = Node(value)
        if self.is_empty():
            self.top = new_node
            self.bottom = new_node
        else:
            new_node.next = self.top
            self.top = new_node
        self.length += 1
    
    def pop(self):
        if self.is_empty():
            return None
        
        popped_node = self.top
        if self.top == self.bottom:
            self.bottom = None
        
        self.top = self.top.next
        self.length -= 1
        return popped_node.value
    
    def peek(self):
        if self.is_empty():
            return None
        return self.top.value
    
    def is_empty(self):
        return self.length == 0
    
    def size(self):
        return self.length

# Example usage
stack = Stack()
stack.push(10)
stack.push(20)
stack.push(30)
print(stack.peek())  # Output: 30
print(stack.pop())   # Output: 30
print(stack.pop())   # Output: 20
print(stack.size())  # Output: 1
```

### JavaScript

#### Using an Array

```javascript
class Stack {
    constructor() {
        this.items = [];
    }
    
    push(item) {
        this.items.push(item);
    }
    
    pop() {
        if (!this.isEmpty()) {
            return this.items.pop();
        }
        return null;
    }
    
    peek() {
        if (!this.isEmpty()) {
            return this.items[this.items.length - 1];
        }
        return null;
    }
    
    isEmpty() {
        return this.items.length === 0;
    }
    
    size() {
        return this.items.length;
    }
}

// Example usage
const stack = new Stack();
stack.push(10);
stack.push(20);
stack.push(30);
console.log(stack.peek());  // Output: 30
console.log(stack.pop());   // Output: 30
console.log(stack.pop());   // Output: 20
console.log(stack.size());  // Output: 1
```

#### Using a Linked List

```javascript
class Node {
    constructor(value) {
        this.value = value;
        this.next = null;
    }
}

class Stack {
    constructor() {
        this.top = null;
        this.bottom = null;
        this.length = 0;
    }
    
    push(value) {
        const newNode = new Node(value);
        if (this.isEmpty()) {
            this.top = newNode;
            this.bottom = newNode;
        } else {
            newNode.next = this.top;
            this.top = newNode;
        }
        this.length++;
    }
    
    pop() {
        if (this.isEmpty()) {
            return null;
        }
        
        const poppedNode = this.top;
        if (this.top === this.bottom) {
            this.bottom = null;
        }
        
        this.top = this.top.next;
        this.length--;
        return poppedNode.value;
    }
    
    peek() {
        if (this.isEmpty()) {
            return null;
        }
        return this.top.value;
    }
    
    isEmpty() {
        return this.length === 0;
    }
    
    size() {
        return this.length;
    }
}

// Example usage
const stack = new Stack();
stack.push(10);
stack.push(20);
stack.push(30);
console.log(stack.peek());  // Output: 30
console.log(stack.pop());   // Output: 30
console.log(stack.pop());   // Output: 20
console.log(stack.size());  // Output: 1
```

## Time and Space Complexity

| Operation | Time Complexity | Space Complexity |
|-----------|----------------|-----------------|
| Push      | O(1)           | O(1)            |
| Pop       | O(1)           | O(1)            |
| Peek      | O(1)           | O(1)            |
| isEmpty   | O(1)           | O(1)            |
| Size      | O(1)           | O(1)            |

## Common Stack Applications

1. **Function Call Stack**: Used by programming languages to manage function calls and returns
2. **Expression Evaluation**: Used to evaluate arithmetic expressions (infix, postfix, prefix)
3. **Undo Mechanism**: Used in editors and other applications to implement undo functionality
4. **Backtracking Algorithms**: Used in maze solving, puzzle solving, and other backtracking algorithms
5. **Browser History**: Used to implement forward and backward navigation
6. **Balanced Parentheses**: Used to check if parentheses in an expression are balanced
7. **Syntax Parsing**: Used in compilers and interpreters for syntax analysis

## Common Stack Algorithms

### 1. Balanced Parentheses

**Python:**
```python
def is_balanced(expression):
    stack = []
    opening_brackets = "({["
    closing_brackets = ")}]"
    bracket_pairs = {')': '(', '}': '{', ']': '['}
    
    for char in expression:
        if char in opening_brackets:
            stack.append(char)
        elif char in closing_brackets:
            if not stack or stack.pop() != bracket_pairs[char]:
                return False
    
    return len(stack) == 0

# Example usage
print(is_balanced("({[]}())"))  # Output: True
print(is_balanced("({[}])"))    # Output: False
```

**JavaScript:**
```javascript
function isBalanced(expression) {
    const stack = [];
    const openingBrackets = "({[";
    const closingBrackets = ")}]";
    const bracketPairs = {')': '(', '}': '{', ']': '['};
    
    for (let i = 0; i < expression.length; i++) {
        const char = expression[i];
        if (openingBrackets.includes(char)) {
            stack.push(char);
        } else if (closingBrackets.includes(char)) {
            if (stack.length === 0 || stack.pop() !== bracketPairs[char]) {
                return false;
            }
        }
    }
    
    return stack.length === 0;
}

// Example usage
console.log(isBalanced("({[]}())"));  // Output: true
console.log(isBalanced("({[}])"));    // Output: false
```

### 2. Infix to Postfix Conversion

**Python:**
```python
def infix_to_postfix(expression):
    precedence = {'+': 1, '-': 1, '*': 2, '/': 2, '^': 3}
    stack = []
    postfix = []
    
    for char in expression:
        if char.isalnum():  # Operand
            postfix.append(char)
        elif char == '(':
            stack.append(char)
        elif char == ')':
            while stack and stack[-1] != '(':
                postfix.append(stack.pop())
            stack.pop()  # Remove '('
        else:  # Operator
            while stack and stack[-1] != '(' and precedence.get(char, 0) <= precedence.get(stack[-1], 0):
                postfix.append(stack.pop())
            stack.append(char)
    
    while stack:
        postfix.append(stack.pop())
    
    return ''.join(postfix)

# Example usage
print(infix_to_postfix("a+b*(c^d-e)^(f+g*h)-i"))  # Output: "abcd^e-fg*h+^*+i-"
```

**JavaScript:**
```javascript
function infixToPostfix(expression) {
    const precedence = {'+': 1, '-': 1, '*': 2, '/': 2, '^': 3};
    const stack = [];
    let postfix = "";
    
    for (let i = 0; i < expression.length; i++) {
        const char = expression[i];
        if (/[a-zA-Z0-9]/.test(char)) {  // Operand
            postfix += char;
        } else if (char === '(') {
            stack.push(char);
        } else if (char === ')') {
            while (stack.length > 0 && stack[stack.length - 1] !== '(') {
                postfix += stack.pop();
            }
            stack.pop();  // Remove '('
        } else {  // Operator
            while (stack.length > 0 && stack[stack.length - 1] !== '(' && 
                   (precedence[char] || 0) <= (precedence[stack[stack.length - 1]] || 0)) {
                postfix += stack.pop();
            }
            stack.push(char);
        }
    }
    
    while (stack.length > 0) {
        postfix += stack.pop();
    }
    
    return postfix;
}

// Example usage
console.log(infixToPostfix("a+b*(c^d-e)^(f+g*h)-i"));  // Output: "abcd^e-fg*h+^*+i-"
```

### 3. Evaluate Postfix Expression

**Python:**
```python
def evaluate_postfix(expression):
    stack = []
    
    for char in expression:
        if char.isdigit():
            stack.append(int(char))
        else:
            b = stack.pop()
            a = stack.pop()
            
            if char == '+':
                stack.append(a + b)
            elif char == '-':
                stack.append(a - b)
            elif char == '*':
                stack.append(a * b)
            elif char == '/':
                stack.append(a / b)
            elif char == '^':
                stack.append(a ** b)
    
    return stack.pop()

# Example usage
print(evaluate_postfix("23*5+"))  # Output: 11 (2*3+5)
```

**JavaScript:**
```javascript
function evaluatePostfix(expression) {
    const stack = [];
    
    for (let i = 0; i < expression.length; i++) {
        const char = expression[i];
        if (/\d/.test(char)) {
            stack.push(parseInt(char));
        } else {
            const b = stack.pop();
            const a = stack.pop();
            
            switch (char) {
                case '+':
                    stack.push(a + b);
                    break;
                case '-':
                    stack.push(a - b);
                    break;
                case '*':
                    stack.push(a * b);
                    break;
                case '/':
                    stack.push(a / b);
                    break;
                case '^':
                    stack.push(Math.pow(a, b));
                    break;
            }
        }
    }
    
    return stack.pop();
}

// Example usage
console.log(evaluatePostfix("23*5+"));  // Output: 11 (2*3+5)
```

## Advantages of Stacks

1. **Simple Implementation**: Easy to implement using arrays or linked lists
2. **Efficient Operations**: All operations (push, pop, peek) are O(1)
3. **Memory Management**: Efficient memory usage with dynamic size
4. **Recursive Algorithms**: Natural choice for implementing recursive algorithms iteratively

## Disadvantages of Stacks

1. **Limited Access**: Only the top element is accessible
2. **No Random Access**: Cannot access elements in the middle without removing elements above
3. **Fixed Size**: If implemented using arrays with fixed size, may lead to stack overflow
4. **Underflow**: Attempting to pop from an empty stack causes underflow

## When to Use Stacks

- When you need to access elements in reverse order (LIFO)
- When you need to check for balanced expressions
- When implementing undo/redo functionality
- When converting between different expression notations
- When implementing recursive algorithms iteratively
- When you need to keep track of function calls

## Common Stack Problems from Blind 75 and Grind 75

1. **Valid Parentheses** (Easy): Determine if the input string has valid parentheses.
2. **Min Stack** (Easy): Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
3. **Evaluate Reverse Polish Notation** (Medium): Evaluate the value of an arithmetic expression in Reverse Polish Notation.
4. **Daily Temperatures** (Medium): Given a list of daily temperatures, return a list such that, for each day, shows how many days you would have to wait until a warmer temperature.
5. **Largest Rectangle in Histogram** (Hard): Find the largest rectangular area possible in a given histogram.
6. **Trapping Rain Water** (Hard): Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

## How to Identify Stack Problems

Look for these clues in the problem statement:

1. The problem involves processing elements in a Last In, First Out (LIFO) order.
2. The problem involves matching pairs or checking for balanced expressions.
3. The problem requires backtracking or undoing operations.
4. The problem involves parsing or evaluating expressions.
5. The problem requires keeping track of previous states or operations.
6. Keywords like "nested," "balanced," "matching," "last," or "most recent." 