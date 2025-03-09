# Hash Tables

A hash table (also known as a hash map) is a data structure that implements an associative array abstract data type, a structure that can map keys to values. It uses a hash function to compute an index into an array of buckets or slots, from which the desired value can be found.

## Visual Representation

```
┌─────────────────────────────────────────────────────────────┐
│                         Hash Table                          │
├───────┬───────────────────────────────────────────────────┐ │
│ Index │                      Bucket                       │ │
├───────┼───────────────────────────────────────────────────┤ │
│   0   │                                                   │ │
├───────┼───────────────────────────────────────────────────┤ │
│   1   │ ┌─────────────┐                                   │ │
│       │ │ "John": 25  │                                   │ │
│       │ └─────────────┘                                   │ │
├───────┼───────────────────────────────────────────────────┤ │
│   2   │                                                   │ │
├───────┼───────────────────────────────────────────────────┤ │
│   3   │ ┌─────────────┐  ┌─────────────┐                  │ │
│       │ │ "Sarah": 30 │→ │ "Mike": 35  │                  │ │
│       │ └─────────────┘  └─────────────┘                  │ │
├───────┼───────────────────────────────────────────────────┤ │
│   4   │                                                   │ │
├───────┼───────────────────────────────────────────────────┤ │
│   5   │ ┌─────────────┐                                   │ │
│       │ │ "Lisa": 28  │                                   │ │
│       │ └─────────────┘                                   │ │
└───────┴───────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

## Key Components

1. **Hash Function**: Converts keys into array indices
2. **Array**: Stores the key-value pairs
3. **Collision Handling**: Manages situations when different keys hash to the same index

## Key Operations

1. **Insert**: Add a key-value pair to the hash table
2. **Search/Lookup**: Find a value associated with a key
3. **Delete**: Remove a key-value pair from the hash table

## Implementation in Python and JavaScript

### Python

#### Using Built-in Dictionary

```python
# Python dictionaries are implemented as hash tables
hash_table = {}

# Insert
hash_table["name"] = "John"
hash_table["age"] = 30
hash_table["city"] = "New York"

# Lookup
print(hash_table["name"])  # Output: John

# Check if key exists
if "age" in hash_table:
    print("Age exists in the hash table")

# Delete
del hash_table["city"]

# Iterate through keys and values
for key, value in hash_table.items():
    print(f"{key}: {value}")
```

#### Custom Implementation

```python
class HashTable:
    def __init__(self, size=7):
        self.size = size
        self.table = [[] for _ in range(size)]  # List of buckets (each bucket is a list)
    
    def _hash(self, key):
        # Simple hash function for strings
        if isinstance(key, str):
            total = 0
            for char in key:
                total += ord(char)
            return total % self.size
        # For other types, use the built-in hash function
        return hash(key) % self.size
    
    def set(self, key, value):
        index = self._hash(key)
        bucket = self.table[index]
        
        for i, (k, v) in enumerate(bucket):
            if k == key:
                bucket[i] = (key, value)  # Update existing key
                return
        
        bucket.append((key, value))  # Add new key-value pair
    
    def get(self, key):
        index = self._hash(key)
        bucket = self.table[index]
        
        for k, v in bucket:
            if k == key:
                return v
        
        return None  # Key not found
    
    def keys(self):
        all_keys = []
        for bucket in self.table:
            for key, _ in bucket:
                all_keys.append(key)
        return all_keys
    
    def delete(self, key):
        index = self._hash(key)
        bucket = self.table[index]
        
        for i, (k, v) in enumerate(bucket):
            if k == key:
                del bucket[i]
                return True
        
        return False  # Key not found

# Example usage
ht = HashTable()
ht.set("name", "John")
ht.set("age", 30)
ht.set("city", "New York")

print(ht.get("name"))  # Output: John
print(ht.keys())       # Output: ['name', 'age', 'city']
ht.delete("city")
print(ht.keys())       # Output: ['name', 'age']
```

### JavaScript

#### Using Built-in Map

```javascript
// JavaScript Map is similar to a hash table
const hashTable = new Map();

// Insert
hashTable.set("name", "John");
hashTable.set("age", 30);
hashTable.set("city", "New York");

// Lookup
console.log(hashTable.get("name"));  // Output: John

// Check if key exists
if (hashTable.has("age")) {
    console.log("Age exists in the hash table");
}

// Delete
hashTable.delete("city");

// Iterate through keys and values
for (const [key, value] of hashTable) {
    console.log(`${key}: ${value}`);
}
```

#### Using Object (Simple Hash Table)

```javascript
// JavaScript objects can be used as simple hash tables
const hashTable = {};

// Insert
hashTable["name"] = "John";
hashTable["age"] = 30;
hashTable["city"] = "New York";

// Lookup
console.log(hashTable["name"]);  // Output: John

// Check if key exists
if ("age" in hashTable) {
    console.log("Age exists in the hash table");
}

// Delete
delete hashTable["city"];

// Iterate through keys and values
for (const key in hashTable) {
    console.log(`${key}: ${hashTable[key]}`);
}
```

#### Custom Implementation

```javascript
class HashTable {
    constructor(size = 7) {
        this.size = size;
        this.table = Array(size).fill().map(() => []);  // Array of buckets
    }
    
    _hash(key) {
        // Simple hash function for strings
        if (typeof key === 'string') {
            let total = 0;
            for (let i = 0; i < key.length; i++) {
                total += key.charCodeAt(i);
            }
            return total % this.size;
        }
        // For other types, convert to string and hash
        return String(key).length % this.size;
    }
    
    set(key, value) {
        const index = this._hash(key);
        const bucket = this.table[index];
        
        for (let i = 0; i < bucket.length; i++) {
            if (bucket[i][0] === key) {
                bucket[i] = [key, value];  // Update existing key
                return;
            }
        }
        
        bucket.push([key, value]);  // Add new key-value pair
    }
    
    get(key) {
        const index = this._hash(key);
        const bucket = this.table[index];
        
        for (const [k, v] of bucket) {
            if (k === key) {
                return v;
            }
        }
        
        return undefined;  // Key not found
    }
    
    keys() {
        const allKeys = [];
        for (const bucket of this.table) {
            for (const [key] of bucket) {
                allKeys.push(key);
            }
        }
        return allKeys;
    }
    
    delete(key) {
        const index = this._hash(key);
        const bucket = this.table[index];
        
        for (let i = 0; i < bucket.length; i++) {
            if (bucket[i][0] === key) {
                bucket.splice(i, 1);
                return true;
            }
        }
        
        return false;  // Key not found
    }
}

// Example usage
const ht = new HashTable();
ht.set("name", "John");
ht.set("age", 30);
ht.set("city", "New York");

console.log(ht.get("name"));  // Output: John
console.log(ht.keys());       // Output: ["name", "age", "city"]
ht.delete("city");
console.log(ht.keys());       // Output: ["name", "age"]
```

## Hash Functions

A good hash function should:
1. Be deterministic: the same key should always produce the same hash value
2. Distribute keys uniformly across the hash table
3. Be efficient to compute

### Common Hash Functions

#### Division Method
```
hash(key) = key % table_size
```

#### Multiplication Method
```
hash(key) = floor(table_size * (key * A % 1))
```
where A is a constant between 0 and 1, often chosen as (√5 - 1)/2 ≈ 0.618033988749895

#### Universal Hashing
```
hash(key) = ((a * key + b) % p) % table_size
```
where p is a prime number larger than the key space, and a and b are random integers between 1 and p-1

## Collision Resolution Techniques

### 1. Chaining

Store multiple key-value pairs that hash to the same index in a linked list or array.

```
┌───────┐
│   0   │ → null
├───────┤
│   1   │ → (key1, value1) → (key2, value2)
├───────┤
│   2   │ → (key3, value3)
├───────┤
│   3   │ → null
└───────┘
```

### 2. Open Addressing

Find another empty slot in the hash table when a collision occurs.

#### Linear Probing
```
hash(key, i) = (hash(key) + i) % table_size
```

#### Quadratic Probing
```
hash(key, i) = (hash(key) + i + i²) % table_size
```

#### Double Hashing
```
hash(key, i) = (hash1(key) + i * hash2(key)) % table_size
```

## Time and Space Complexity

| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| Insert    | O(1)         | O(n)       |
| Search    | O(1)         | O(n)       |
| Delete    | O(1)         | O(n)       |

Space Complexity: O(n) where n is the number of key-value pairs

## Common Hash Table Applications

1. **Database Indexing**: Hash tables are used to create indexes for faster data retrieval
2. **Caching**: Storing frequently accessed data for quick access
3. **Symbol Tables**: Used by compilers and interpreters to store variable names and their attributes
4. **Associative Arrays**: Implementing dictionaries and maps
5. **Counting Frequencies**: Counting occurrences of elements in a collection
6. **De-duplication**: Removing duplicate elements from a collection
7. **Implementing Sets**: Storing unique elements

## Common Hash Table Algorithms

### 1. Two Sum

**Problem**: Given an array of integers, find two numbers such that they add up to a specific target.

**Python:**
```python
def two_sum(nums, target):
    hash_map = {}
    
    for i, num in enumerate(nums):
        complement = target - num
        if complement in hash_map:
            return [hash_map[complement], i]
        hash_map[num] = i
    
    return []  # No solution found

# Example usage
nums = [2, 7, 11, 15]
target = 9
print(two_sum(nums, target))  # Output: [0, 1]
```

**JavaScript:**
```javascript
function twoSum(nums, target) {
    const hashMap = {};
    
    for (let i = 0; i < nums.length; i++) {
        const complement = target - nums[i];
        if (complement in hashMap) {
            return [hashMap[complement], i];
        }
        hashMap[nums[i]] = i;
    }
    
    return [];  // No solution found
}

// Example usage
const nums = [2, 7, 11, 15];
const target = 9;
console.log(twoSum(nums, target));  // Output: [0, 1]
```

### 2. Group Anagrams

**Problem**: Given an array of strings, group anagrams together.

**Python:**
```python
def group_anagrams(strs):
    anagram_groups = {}
    
    for s in strs:
        # Sort the string to use as a key
        sorted_s = ''.join(sorted(s))
        
        if sorted_s in anagram_groups:
            anagram_groups[sorted_s].append(s)
        else:
            anagram_groups[sorted_s] = [s]
    
    return list(anagram_groups.values())

# Example usage
strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
print(group_anagrams(strs))  # Output: [["eat", "tea", "ate"], ["tan", "nat"], ["bat"]]
```

**JavaScript:**
```javascript
function groupAnagrams(strs) {
    const anagramGroups = {};
    
    for (const s of strs) {
        // Sort the string to use as a key
        const sortedS = s.split('').sort().join('');
        
        if (sortedS in anagramGroups) {
            anagramGroups[sortedS].push(s);
        } else {
            anagramGroups[sortedS] = [s];
        }
    }
    
    return Object.values(anagramGroups);
}

// Example usage
const strs = ["eat", "tea", "tan", "ate", "nat", "bat"];
console.log(groupAnagrams(strs));  // Output: [["eat", "tea", "ate"], ["tan", "nat"], ["bat"]]
```

### 3. LRU Cache

**Problem**: Design and implement a data structure for Least Recently Used (LRU) cache.

**Python:**
```python
class LRUCache:
    def __init__(self, capacity):
        self.capacity = capacity
        self.cache = {}  # key -> value
        self.usage = {}  # key -> timestamp
        self.time = 0
    
    def get(self, key):
        if key in self.cache:
            self.usage[key] = self.time
            self.time += 1
            return self.cache[key]
        return -1
    
    def put(self, key, value):
        if key in self.cache:
            self.cache[key] = value
            self.usage[key] = self.time
            self.time += 1
            return
        
        if len(self.cache) >= self.capacity:
            # Find least recently used key
            lru_key = min(self.usage.items(), key=lambda x: x[1])[0]
            del self.cache[lru_key]
            del self.usage[lru_key]
        
        self.cache[key] = value
        self.usage[key] = self.time
        self.time += 1

# Example usage
cache = LRUCache(2)
cache.put(1, 1)
cache.put(2, 2)
print(cache.get(1))    # Output: 1
cache.put(3, 3)        # Evicts key 2
print(cache.get(2))    # Output: -1 (not found)
```

**JavaScript:**
```javascript
class LRUCache {
    constructor(capacity) {
        this.capacity = capacity;
        this.cache = new Map();  // Using Map to maintain insertion order
    }
    
    get(key) {
        if (!this.cache.has(key)) {
            return -1;
        }
        
        // Update the order by removing and re-adding the key
        const value = this.cache.get(key);
        this.cache.delete(key);
        this.cache.set(key, value);
        
        return value;
    }
    
    put(key, value) {
        // If key exists, remove it first
        if (this.cache.has(key)) {
            this.cache.delete(key);
        }
        // If cache is full, remove the least recently used item (first item)
        else if (this.cache.size >= this.capacity) {
            const firstKey = this.cache.keys().next().value;
            this.cache.delete(firstKey);
        }
        
        // Add the new key-value pair
        this.cache.set(key, value);
    }
}

// Example usage
const cache = new LRUCache(2);
cache.put(1, 1);
cache.put(2, 2);
console.log(cache.get(1));    // Output: 1
cache.put(3, 3);              // Evicts key 2
console.log(cache.get(2));    // Output: -1 (not found)
```

## Advantages of Hash Tables

1. **Fast Access**: O(1) average time complexity for lookups, insertions, and deletions
2. **Flexible Keys**: Can use various data types as keys (strings, numbers, objects)
3. **Dynamic Size**: Can grow or shrink as needed (in most implementations)
4. **Efficient for Large Datasets**: Performance remains consistent regardless of size

## Disadvantages of Hash Tables

1. **Unordered**: Elements are not stored in a specific order
2. **Collisions**: Different keys may hash to the same index, requiring collision resolution
3. **Memory Overhead**: May use more memory than arrays for small datasets
4. **Worst-case Performance**: O(n) in the worst case if many collisions occur

## When to Use Hash Tables

- When you need fast lookups, insertions, and deletions
- When the order of elements doesn't matter
- When you need to map keys to values
- When you need to check for duplicates
- When implementing caching mechanisms
- When counting frequencies of elements

## Common Hash Table Problems from Blind 75 and Grind 75

1. **Two Sum** (Easy): Find two numbers in an array that add up to a target.
2. **Group Anagrams** (Medium): Group strings that are anagrams of each other.
3. **Valid Anagram** (Easy): Determine if one string is an anagram of another.
4. **Contains Duplicate** (Easy): Check if an array contains any duplicates.
5. **Longest Substring Without Repeating Characters** (Medium): Find the length of the longest substring without repeating characters.
6. **LRU Cache** (Medium): Design and implement a data structure for Least Recently Used (LRU) cache.
7. **Subarray Sum Equals K** (Medium): Find the number of subarrays that sum to k.
8. **Top K Frequent Elements** (Medium): Find the k most frequent elements in an array.

## How to Identify Hash Table Problems

Look for these clues in the problem statement:

1. The problem involves looking up values based on keys.
2. The problem requires checking for duplicates or counting frequencies.
3. The problem involves finding pairs or groups with specific properties.
4. The problem requires caching or memoization.
5. The problem involves tracking elements that have been seen before.
6. Keywords like "lookup," "dictionary," "map," "frequency," or "count." 