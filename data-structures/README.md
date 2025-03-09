# Data Structures

Data structures are specialized formats for organizing, storing, and manipulating data. Choosing the right data structure is crucial for efficient algorithm design.

## Overview

This section covers the following data structures:

1. [Arrays/Lists](./arrays.md) - Contiguous memory locations storing elements of the same type
2. [Linked Lists](./linked-lists.md) - Linear collection of nodes, each pointing to the next node
3. [Stacks](./stacks.md) - LIFO (Last In, First Out) data structure
4. [Queues](./queues.md) - FIFO (First In, First Out) data structure
5. [Hash Tables/Maps](./hash-tables.md) - Key-value pairs with O(1) average lookup time
6. [Trees](./trees.md) - Hierarchical structure with a root node and child nodes
7. [Heaps](./heaps.md) - Specialized tree-based structure satisfying the heap property
8. [Graphs](./graphs.md) - Collection of nodes (vertices) connected by edges
9. [Tries](./tries.md) - Tree-like structure for efficient string operations

## How to Choose the Right Data Structure

| If you need to... | Consider using... |
|-------------------|-------------------|
| Store a collection of items with fast random access | Array/List |
| Efficiently insert/delete at the beginning/middle | Linked List |
| Process items in reverse order of arrival | Stack |
| Process items in order of arrival | Queue |
| Fast lookups by key | Hash Table/Map |
| Represent hierarchical relationships | Tree |
| Quick access to the min/max element | Heap |
| Represent connections between objects | Graph |
| Efficient prefix matching for strings | Trie |

## Time Complexity Comparison

| Data Structure | Access | Search | Insertion | Deletion |
|----------------|--------|--------|-----------|----------|
| Array          | O(1)   | O(n)   | O(n)      | O(n)     |
| Linked List    | O(n)   | O(n)   | O(1)      | O(1)     |
| Stack          | O(n)   | O(n)   | O(1)      | O(1)     |
| Queue          | O(n)   | O(n)   | O(1)      | O(1)     |
| Hash Table     | N/A    | O(1)*  | O(1)*     | O(1)*    |
| Binary Tree    | O(n)   | O(n)   | O(n)      | O(n)     |
| BST (balanced) | O(log n) | O(log n) | O(log n) | O(log n) |
| Heap           | O(1)** | O(n)   | O(log n)  | O(log n) |
| Trie           | O(m)   | O(m)   | O(m)      | O(m)     |

\* Average case, can be O(n) in worst case  
\** For the min/max element only  
m = length of the key/string 