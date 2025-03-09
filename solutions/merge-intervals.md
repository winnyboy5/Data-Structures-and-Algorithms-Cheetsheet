# Merge Intervals

## Problem Statement

Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

**Example 1:**
```
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlap, merge them into [1,6].
```

**Example 2:**
```
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

## Intuition

The key insight for this problem is to sort the intervals by their start times. Once sorted, we can merge overlapping intervals by comparing the end time of the current interval with the start time of the next interval.

If the end time of the current interval is greater than or equal to the start time of the next interval, they overlap and can be merged. Otherwise, they don't overlap and should remain separate.

## Visual Explanation

Let's visualize the solution with Example 1: `intervals = [[1,3],[2,6],[8,10],[15,18]]`

First, we sort the intervals by their start times (in this case, they're already sorted):

```
Sorted intervals: [[1,3],[2,6],[8,10],[15,18]]

Step 1: Initialize result with the first interval
        result = [[1,3]]

Step 2: Process interval [2,6]
        Last interval in result is [1,3]
        Does [1,3] overlap with [2,6]? Yes, because 3 >= 2
        Merge them: [1,max(3,6)] = [1,6]
        result = [[1,6]]

Step 3: Process interval [8,10]
        Last interval in result is [1,6]
        Does [1,6] overlap with [8,10]? No, because 6 < 8
        Add [8,10] to result
        result = [[1,6],[8,10]]

Step 4: Process interval [15,18]
        Last interval in result is [8,10]
        Does [8,10] overlap with [15,18]? No, because 10 < 15
        Add [15,18] to result
        result = [[1,6],[8,10],[15,18]]

Result: [[1,6],[8,10],[15,18]]
```

![Merge Intervals Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Sort the intervals based on the start time.
2. Initialize the result list with the first interval.
3. Iterate through the remaining intervals:
   - If the current interval overlaps with the last interval in the result (i.e., the end time of the last interval is greater than or equal to the start time of the current interval), merge them by updating the end time of the last interval to the maximum of the two end times.
   - If they don't overlap, add the current interval to the result.
4. Return the result.

## Code Implementation

### Python

```python
def merge(intervals):
    if not intervals:
        return []
    
    # Sort intervals based on start time
    intervals.sort(key=lambda x: x[0])
    
    result = [intervals[0]]
    
    for i in range(1, len(intervals)):
        current = intervals[i]
        last = result[-1]
        
        # If current interval overlaps with the last interval in result
        if current[0] <= last[1]:
            # Merge them by updating the end time of the last interval
            last[1] = max(last[1], current[1])
        else:
            # If they don't overlap, add the current interval to result
            result.append(current)
    
    return result
```

### JavaScript

```javascript
function merge(intervals) {
    if (!intervals || intervals.length === 0) {
        return [];
    }
    
    // Sort intervals based on start time
    intervals.sort((a, b) => a[0] - b[0]);
    
    const result = [intervals[0]];
    
    for (let i = 1; i < intervals.length; i++) {
        const current = intervals[i];
        const last = result[result.length - 1];
        
        // If current interval overlaps with the last interval in result
        if (current[0] <= last[1]) {
            // Merge them by updating the end time of the last interval
            last[1] = Math.max(last[1], current[1]);
        } else {
            // If they don't overlap, add the current interval to result
            result.push(current);
        }
    }
    
    return result;
}
```

## Time and Space Complexity

- **Time Complexity**: O(n log n), where n is the number of intervals. Sorting takes O(n log n), and the merging process takes O(n).
- **Space Complexity**: O(n) for the result array. If we consider the space used for sorting, it could be O(n) or O(log n) depending on the sorting algorithm.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2: `intervals = [[1,4],[4,5]]`

```
Sorted intervals: [[1,4],[4,5]]

Step 1: Initialize result with the first interval
        result = [[1,4]]

Step 2: Process interval [4,5]
        Last interval in result is [1,4]
        Does [1,4] overlap with [4,5]? Yes, because 4 >= 4
        Merge them: [1,max(4,5)] = [1,5]
        result = [[1,5]]

Result: [[1,5]]
```

## Edge Cases and Optimizations

- **Empty Input**: If the input array is empty, the function will return an empty array.
- **Single Interval**: If there's only one interval, it will be returned as is.
- **All Overlapping Intervals**: If all intervals overlap, they will be merged into a single interval.
- **No Overlapping Intervals**: If no intervals overlap, the result will be the same as the input (after sorting).

## Common Mistakes

1. **Not sorting the intervals**: It's crucial to sort the intervals by their start times to efficiently merge overlapping intervals.
2. **Incorrect overlap check**: The overlap condition is `current[0] <= last[1]`, not `current[0] < last[1]`. This handles the case where intervals touch (like `[1,4]` and `[4,5]`).
3. **Not updating the end time correctly**: When merging intervals, the end time should be the maximum of the two end times, not just the end time of the current interval.

## Related Problems

1. **Insert Interval**: Insert a new interval into a sorted list of non-overlapping intervals and merge if necessary.
2. **Non-overlapping Intervals**: Find the minimum number of intervals to remove to make the rest non-overlapping.
3. **Meeting Rooms**: Determine if a person can attend all meetings.
4. **Meeting Rooms II**: Find the minimum number of conference rooms required. 