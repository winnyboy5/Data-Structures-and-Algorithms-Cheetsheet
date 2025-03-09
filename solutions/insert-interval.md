# Insert Interval

## Problem Statement

You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [starti, endi]` represent the start and the end of the ith interval and `intervals` is sorted in ascending order by `starti`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` after the insertion.

**Example 1:**
```
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

**Example 2:**
```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
```

**Example 3:**
```
Input: intervals = [], newInterval = [5,7]
Output: [[5,7]]
```

**Example 4:**
```
Input: intervals = [[1,5]], newInterval = [2,3]
Output: [[1,5]]
```

**Example 5:**
```
Input: intervals = [[1,5]], newInterval = [2,7]
Output: [[1,7]]
```

## Intuition

The key insight for this problem is to handle three different cases:

1. **Non-overlapping intervals that come before the new interval**: These intervals can be added to the result directly.
2. **Overlapping intervals with the new interval**: These intervals need to be merged with the new interval.
3. **Non-overlapping intervals that come after the new interval**: These intervals can be added to the result directly after the merged interval.

Two intervals `[a, b]` and `[c, d]` overlap if and only if `a <= d && c <= b`. In other words, they overlap if one interval's start is less than or equal to the other interval's end, and vice versa.

## Visual Explanation

Let's visualize the solution with Example 1:

```
Intervals: [[1,3],[6,9]]
New Interval: [2,5]
```

1. Process intervals that come before the new interval:
   - [1,3] overlaps with [2,5] because 1 <= 5 and 2 <= 3, so we merge them into [1,5].

2. Process intervals that overlap with the new interval:
   - No more overlapping intervals.

3. Process intervals that come after the new interval:
   - [6,9] comes after [1,5], so we add it to the result.

4. Result: [[1,5],[6,9]]

Let's visualize this on a number line:

```
0---1---2---3---4---5---6---7---8---9---10
    |---|
        |-----|  <- New Interval
                |-----|
    
    |-----------|
                |-----|
```

Now let's visualize Example 2:

```
Intervals: [[1,2],[3,5],[6,7],[8,10],[12,16]]
New Interval: [4,8]
```

1. Process intervals that come before the new interval:
   - [1,2] comes before [4,8], so we add it to the result.
   - [3,5] overlaps with [4,8] because 3 <= 8 and 4 <= 5, so we merge them into [3,8].

2. Process intervals that overlap with the new interval:
   - [6,7] overlaps with [3,8] because 6 <= 8 and 3 <= 7, so we merge them into [3,8].
   - [8,10] overlaps with [3,8] because 8 <= 8 and 3 <= 10, so we merge them into [3,10].

3. Process intervals that come after the new interval:
   - [12,16] comes after [3,10], so we add it to the result.

4. Result: [[1,2],[3,10],[12,16]]

```
0---1---2---3---4---5---6---7---8---9---10---11---12---13---14---15---16
    |---|   |---|   |---|   |-----|           |-----------|
                |-------|  <- New Interval
    
    |---|   |-------------------|           |-----------|
```

![Insert Interval Visualization](https://i.imgur.com/JUDTkUC.png)

## Step-by-Step Approach

1. Initialize an empty result array.
2. Iterate through the intervals:
   - If the current interval ends before the new interval starts, add it to the result.
   - If the current interval starts after the new interval ends, it means we've processed all overlapping intervals. Add the merged new interval to the result, then add all remaining intervals.
   - If there's an overlap, merge the current interval with the new interval by updating the new interval's start to the minimum of both starts and the end to the maximum of both ends.
3. If we haven't added the new interval yet (because it overlaps with the last interval or comes after all intervals), add it to the result.
4. Return the result.

## Code Implementation

### Python

```python
def insert(intervals, newInterval):
    result = []
    i = 0
    n = len(intervals)
    
    # Add all intervals that come before newInterval
    while i < n and intervals[i][1] < newInterval[0]:
        result.append(intervals[i])
        i += 1
    
    # Merge overlapping intervals
    while i < n and intervals[i][0] <= newInterval[1]:
        newInterval[0] = min(newInterval[0], intervals[i][0])
        newInterval[1] = max(newInterval[1], intervals[i][1])
        i += 1
    
    # Add the merged interval
    result.append(newInterval)
    
    # Add all intervals that come after newInterval
    while i < n:
        result.append(intervals[i])
        i += 1
    
    return result
```

### JavaScript

```javascript
function insert(intervals, newInterval) {
    const result = [];
    let i = 0;
    const n = intervals.length;
    
    // Add all intervals that come before newInterval
    while (i < n && intervals[i][1] < newInterval[0]) {
        result.push(intervals[i]);
        i++;
    }
    
    // Merge overlapping intervals
    while (i < n && intervals[i][0] <= newInterval[1]) {
        newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
        newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
        i++;
    }
    
    // Add the merged interval
    result.push(newInterval);
    
    // Add all intervals that come after newInterval
    while (i < n) {
        result.push(intervals[i]);
        i++;
    }
    
    return result;
}
```

## Time and Space Complexity

### Time Complexity
- **O(n)**, where n is the number of intervals.
- We iterate through each interval exactly once.

### Space Complexity
- **O(n)** for the result array.
- In the worst case, we might need to store all original intervals plus the new interval.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 2:

```
Intervals: [[1,2],[3,5],[6,7],[8,10],[12,16]]
New Interval: [4,8]
```

1. Initialize `result = []`, `i = 0`.

2. Process intervals that come before the new interval:
   - Interval [1,2]: 2 < 4, so add it to result.
   - Update `result = [[1,2]]`, `i = 1`.

3. Process overlapping intervals:
   - Interval [3,5]: 3 <= 8 and 4 <= 5, so merge with new interval.
   - Update `newInterval = [3,8]`, `i = 2`.
   - Interval [6,7]: 6 <= 8 and 3 <= 7, so merge with new interval.
   - Update `newInterval = [3,8]`, `i = 3`.
   - Interval [8,10]: 8 <= 8 and 3 <= 10, so merge with new interval.
   - Update `newInterval = [3,10]`, `i = 4`.

4. Add the merged interval to the result:
   - Update `result = [[1,2],[3,10]]`.

5. Process intervals that come after the new interval:
   - Interval [12,16]: 12 > 10, so add it to result.
   - Update `result = [[1,2],[3,10],[12,16]]`, `i = 5`.

6. Return `result = [[1,2],[3,10],[12,16]]`.

## Edge Cases and Optimizations

- **Empty Intervals Array**: If the input array is empty, return an array containing only the new interval.
- **New Interval Comes Before All Intervals**: Add the new interval first, then add all original intervals.
- **New Interval Comes After All Intervals**: Add all original intervals, then add the new interval.
- **New Interval Is Contained Within an Existing Interval**: The merged interval will be the same as the existing interval.
- **New Interval Contains Multiple Existing Intervals**: The merged interval will span from the start of the first overlapping interval to the end of the last overlapping interval.

**Optimization**: We can avoid creating a new array for the result by modifying the input array in-place, but this would change the input which might not be desirable.

## Common Mistakes

1. **Not handling empty input**: Make sure to check if the input array is empty.
2. **Incorrect overlap check**: Two intervals [a,b] and [c,d] overlap if and only if a <= d and c <= b.
3. **Not merging all overlapping intervals**: Make sure to continue merging as long as there are overlapping intervals.
4. **Not adding the merged interval**: Don't forget to add the merged interval to the result after processing all overlapping intervals.

## Related Problems

1. **Merge Intervals**: Merge all overlapping intervals.
2. **Non-overlapping Intervals**: Find the minimum number of intervals to remove to make the rest non-overlapping.
3. **Meeting Rooms**: Determine if a person can attend all meetings.
4. **Meeting Rooms II**: Find the minimum number of conference rooms required. 