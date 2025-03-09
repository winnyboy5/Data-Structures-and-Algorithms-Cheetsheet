# Meeting Rooms

## Problem Statement

Given an array of meeting time intervals where `intervals[i] = [start_i, end_i]`, determine if a person could attend all meetings.

In other words, determine if there are any overlapping intervals in the given array.

**Example 1:**
```
Input: intervals = [[0,30],[5,10],[15,20]]
Output: false
Explanation: The person cannot attend all meetings because [0,30] overlaps with [5,10] and [15,20].
```

**Example 2:**
```
Input: intervals = [[7,10],[2,4]]
Output: true
Explanation: The person can attend all meetings because [7,10] and [2,4] do not overlap.
```

## Intuition

The key insight for this problem is to recognize that a person can attend all meetings if and only if there are no overlapping intervals. Two intervals overlap if the start time of one interval is less than the end time of another interval.

To check for overlaps efficiently, we can sort the intervals by their start times. Then, we only need to check if the end time of each interval is greater than the start time of the next interval. If such a case exists, then there is an overlap, and the person cannot attend all meetings.

## Visual Explanation

Let's visualize the solution with Example 1: `intervals = [[0,30],[5,10],[15,20]]`

First, we sort the intervals by start time (which in this case is already sorted):
```
[0,30], [5,10], [15,20]
```

Now, we check for overlaps:
1. Compare [0,30] and [5,10]:
   - End time of [0,30] = 30
   - Start time of [5,10] = 5
   - Since 30 > 5, these intervals overlap
   - Return false

![Meeting Rooms Visualization](https://i.imgur.com/JUDTkUC.png)

Let's also visualize Example 2: `intervals = [[7,10],[2,4]]`

First, we sort the intervals by start time:
```
[2,4], [7,10]
```

Now, we check for overlaps:
1. Compare [2,4] and [7,10]:
   - End time of [2,4] = 4
   - Start time of [7,10] = 7
   - Since 4 < 7, these intervals do not overlap
   - Continue checking (no more intervals)
   - Return true

## Step-by-Step Approach

1. Sort the intervals by their start times.
2. Iterate through the sorted intervals from index 0 to n-2 (where n is the number of intervals).
3. For each interval at index i, check if its end time is greater than the start time of the interval at index i+1.
4. If such a case exists, return false (the person cannot attend all meetings).
5. If we complete the iteration without finding any overlaps, return true (the person can attend all meetings).

## Code Implementation

### Python

```python
def canAttendMeetings(intervals):
    # Sort intervals by start time
    intervals.sort(key=lambda x: x[0])
    
    # Check for overlaps
    for i in range(len(intervals) - 1):
        if intervals[i][1] > intervals[i + 1][0]:
            return False
    
    return True
```

### JavaScript

```javascript
function canAttendMeetings(intervals) {
    // Sort intervals by start time
    intervals.sort((a, b) => a[0] - b[0]);
    
    // Check for overlaps
    for (let i = 0; i < intervals.length - 1; i++) {
        if (intervals[i][1] > intervals[i + 1][0]) {
            return false;
        }
    }
    
    return true;
}
```

## Time and Space Complexity

- **Time Complexity**: O(n log n), where n is the number of intervals. This is dominated by the sorting operation.
- **Space Complexity**: O(1) if we sort in place, or O(n) if the sorting algorithm requires additional space.

## Detailed Walkthrough with Example

Let's trace through the algorithm with Example 1: `intervals = [[0,30],[5,10],[15,20]]`

1. Sort the intervals by start time:
   - The intervals are already sorted: `[[0,30],[5,10],[15,20]]`

2. Iterate through the sorted intervals and check for overlaps:
   - i = 0: Compare [0,30] and [5,10]
     - End time of [0,30] = 30
     - Start time of [5,10] = 5
     - Since 30 > 5, these intervals overlap
     - Return false

The algorithm correctly determines that the person cannot attend all meetings because there are overlapping intervals.

Now let's trace through Example 2: `intervals = [[7,10],[2,4]]`

1. Sort the intervals by start time:
   - After sorting: `[[2,4],[7,10]]`

2. Iterate through the sorted intervals and check for overlaps:
   - i = 0: Compare [2,4] and [7,10]
     - End time of [2,4] = 4
     - Start time of [7,10] = 7
     - Since 4 < 7, these intervals do not overlap
     - Continue checking (no more intervals)
     - Return true

The algorithm correctly determines that the person can attend all meetings because there are no overlapping intervals.

## Edge Cases and Optimizations

- **Empty Array**: If the input array is empty, the function will return true (no meetings to attend).
- **Single Interval**: If there is only one interval, the function will return true (only one meeting to attend).
- **All Overlapping Intervals**: The function will return false as soon as it finds the first overlap.
- **No Overlapping Intervals**: The function will iterate through all intervals and return true.

## Common Mistakes

1. **Not sorting the intervals**: It's crucial to sort the intervals by start time to efficiently check for overlaps.
2. **Incorrect overlap check**: Remember that two intervals overlap if the start time of one is less than the end time of the other.
3. **Off-by-one errors**: Be careful with the loop bounds when iterating through the intervals.

## Related Problems

1. **Meeting Rooms II**: Find the minimum number of conference rooms required to hold all meetings.
2. **Merge Intervals**: Merge all overlapping intervals.
3. **Insert Interval**: Insert a new interval into a sorted list of non-overlapping intervals.
4. **Non-overlapping Intervals**: Find the minimum number of intervals to remove to make the rest non-overlapping. 