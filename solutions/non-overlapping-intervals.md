# Non-overlapping Intervals

## Problem Statement

Given an array of intervals `intervals` where `intervals[i] = [starti, endi]`, return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

**Example 1:**
```
Input: intervals = [[1,2],[2,3],[3,4],[1,3]]
Output: 1
Explanation: [1,3] can be removed and the rest of the intervals are non-overlapping.
```

**Example 2:**
```
Input: intervals = [[1,2],[1,2],[1,2]]
Output: 2
Explanation: You need to remove two [1,2] to make the rest of the intervals non-overlapping.
```

**Example 3:**
```
Input: intervals = [[1,2],[2,3]]
Output: 0
Explanation: You don't need to remove any of the intervals since they're already non-overlapping.
```

## Intuition

The key insight for this problem is to use a greedy approach. We want to keep as many intervals as possible, which means we want to remove the minimum number of intervals.

To achieve this, we can:
1. Sort the intervals by their end times.
2. Always keep the interval with the earliest end time, as this gives us the most flexibility for accommodating future intervals.
3. Remove any interval that overlaps with the current interval we've decided to keep.

By sorting by end time, we ensure that we're always keeping the interval that "finishes" earliest, which maximizes our chances of fitting in more intervals later.

## Visual Explanation

Let's visualize the solution with Example 1:

```
Intervals: [[1,2], [2,3], [3,4], [1,3]]
```

1. Sort the intervals by end time:
   ```
   [[1,2], [2,3], [1,3], [3,4]]
   ```

2. Process the intervals:
   - Keep [1,2] (earliest end time)
   - Keep [2,3] (doesn't overlap with [1,2])
   - Remove [1,3] (overlaps with [2,3] because 1 < 3 and 3 > 2)
   - Keep [3,4] (doesn't overlap with [2,3])

3. Result: We need to remove 1 interval.

Let's visualize this on a number line:

```
0---1---2---3---4---5
    |---|
        |---|
    |-------|  <- Remove this one
            |---|
```

For Example 2:
```
Intervals: [[1,2], [1,2], [1,2]]
```

1. Sort the intervals by end time (already sorted):
   ```
   [[1,2], [1,2], [1,2]]
   ```

2. Process the intervals:
   - Keep the first [1,2]
   - Remove the second [1,2] (overlaps with the first)
   - Remove the third [1,2] (overlaps with the first)

3. Result: We need to remove 2 intervals.

```
0---1---2---3---4---5
    |---|
    |---| <- Remove this one
    |---| <- Remove this one
```

![Non-overlapping Intervals Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Sort the intervals by their end times.
2. Initialize a variable `count` to 0 to track the number of intervals to remove.
3. Initialize a variable `end` to negative infinity to track the end time of the last kept interval.
4. Iterate through the sorted intervals:
   - If the current interval's start time is greater than or equal to `end`, it doesn't overlap with the last kept interval, so we keep it and update `end` to the current interval's end time.
   - If the current interval's start time is less than `end`, it overlaps with the last kept interval, so we remove it and increment `count`.
5. Return `count`.

## Code Implementation

### Python

```python
def eraseOverlapIntervals(intervals):
    if not intervals:
        return 0
    
    # Sort intervals by end time
    intervals.sort(key=lambda x: x[1])
    
    count = 0
    end = float('-inf')
    
    for interval in intervals:
        # If the current interval doesn't overlap with the previous one
        if interval[0] >= end:
            end = interval[1]
        else:
            # If it overlaps, remove the current interval
            count += 1
    
    return count
```

### JavaScript

```javascript
function eraseOverlapIntervals(intervals) {
    if (intervals.length === 0) {
        return 0;
    }
    
    // Sort intervals by end time
    intervals.sort((a, b) => a[1] - b[1]);
    
    let count = 0;
    let end = -Infinity;
    
    for (const interval of intervals) {
        // If the current interval doesn't overlap with the previous one
        if (interval[0] >= end) {
            end = interval[1];
        } else {
            // If it overlaps, remove the current interval
            count++;
        }
    }
    
    return count;
}
```

## Time and Space Complexity

### Time Complexity
- **O(n log n)**, where n is the number of intervals.
- Sorting the intervals takes O(n log n) time.
- Iterating through the sorted intervals takes O(n) time.

### Space Complexity
- **O(1)** or **O(n)** depending on the sorting implementation.
- We only use a constant amount of extra space for our variables.
- However, some sorting implementations might require O(n) space.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 1:

```
Intervals: [[1,2], [2,3], [3,4], [1,3]]
```

1. Sort the intervals by end time:
   ```
   [[1,2], [2,3], [1,3], [3,4]]
   ```

2. Initialize `count = 0` and `end = -Infinity`.

3. Process the intervals:
   - Interval [1,2]:
     - 1 >= -Infinity, so we keep it.
     - Update `end = 2`.
   - Interval [2,3]:
     - 2 >= 2, so we keep it.
     - Update `end = 3`.
   - Interval [1,3]:
     - 1 < 3, so it overlaps with the previous interval.
     - Increment `count = 1`.
   - Interval [3,4]:
     - 3 >= 3, so we keep it.
     - Update `end = 4`.

4. Return `count = 1`.

## Edge Cases and Optimizations

- **Empty Array**: If the input array is empty, return 0.
- **Single Interval**: If there's only one interval, return 0 as there's nothing to remove.
- **All Overlapping**: If all intervals overlap, we would need to remove all but one.
- **No Overlapping**: If no intervals overlap, return 0.

**Optimization**: Instead of explicitly removing intervals, we count the number of non-overlapping intervals we can keep and subtract from the total number of intervals. This gives us the number of intervals to remove.

## Common Mistakes

1. **Sorting by start time instead of end time**: Sorting by end time is crucial for the greedy approach to work correctly.
2. **Not handling empty arrays**: Make sure to check if the input array is empty before processing.
3. **Incorrect overlap check**: Two intervals [a,b] and [c,d] overlap if a < d and c < b.

## Related Problems

1. **Merge Intervals**: Merge all overlapping intervals.
2. **Insert Interval**: Insert a new interval into a set of non-overlapping intervals and merge if necessary.
3. **Meeting Rooms**: Determine if a person can attend all meetings.
4. **Meeting Rooms II**: Find the minimum number of conference rooms required. 