# Meeting Rooms II

## Problem Statement

Given an array of meeting time intervals where `intervals[i] = [start_i, end_i]`, find the minimum number of conference rooms required to hold all meetings.

**Example 1:**
```
Input: intervals = [[0,30],[5,10],[15,20]]
Output: 2
Explanation: We need two meeting rooms.
Meeting room 1: [0,30]
Meeting room 2: [5,10], [15,20]
```

**Example 2:**
```
Input: intervals = [[7,10],[2,4]]
Output: 1
Explanation: We need only one meeting room since the meetings don't overlap.
```

## Intuition

The key insight for this problem is to track the number of ongoing meetings at any point in time. The minimum number of conference rooms required is equal to the maximum number of concurrent meetings.

There are two main approaches to solve this problem:

1. **Chronological Ordering**: We can separate the start and end times, sort them, and then simulate the meetings in chronological order. When we encounter a start time, we increment the count of rooms; when we encounter an end time, we decrement the count. The maximum count at any point represents the minimum number of rooms required.

2. **Min Heap**: We can sort the intervals by start time and use a min heap to track the end times of ongoing meetings. The size of the heap at any point represents the number of rooms currently in use.

Both approaches are efficient, but we'll focus on the min heap approach as it's more intuitive.

## Visual Explanation

Let's visualize the min heap approach with Example 1: `intervals = [[0,30],[5,10],[15,20]]`

First, we sort the intervals by start time (which in this case is already sorted):
```
[0,30], [5,10], [15,20]
```

Now, we process each interval:

1. Process [0,30]:
   - No ongoing meetings, so allocate a new room
   - Add end time 30 to the heap: [30]
   - Rooms needed: 1

2. Process [5,10]:
   - Check if any meeting has ended (min end time in heap <= current start time)
   - Min end time is 30, which is > 5, so no meeting has ended
   - Allocate a new room
   - Add end time 10 to the heap: [10, 30]
   - Rooms needed: 2

3. Process [15,20]:
   - Check if any meeting has ended (min end time in heap <= current start time)
   - Min end time is 10, which is < 15, so a meeting has ended
   - Reuse the room (remove 10 from heap)
   - Add end time 20 to the heap: [20, 30]
   - Rooms needed: 2

The maximum number of rooms needed at any point was 2.

![Meeting Rooms II Visualization](https://i.imgur.com/JUDTkUC.png)

Let's also visualize Example 2: `intervals = [[7,10],[2,4]]`

First, we sort the intervals by start time:
```
[2,4], [7,10]
```

Now, we process each interval:

1. Process [2,4]:
   - No ongoing meetings, so allocate a new room
   - Add end time 4 to the heap: [4]
   - Rooms needed: 1

2. Process [7,10]:
   - Check if any meeting has ended (min end time in heap <= current start time)
   - Min end time is 4, which is < 7, so a meeting has ended
   - Reuse the room (remove 4 from heap)
   - Add end time 10 to the heap: [10]
   - Rooms needed: 1

The maximum number of rooms needed at any point was 1.

## Step-by-Step Approach

1. Sort the intervals by their start times.
2. Initialize a min heap to track the end times of ongoing meetings.
3. Iterate through the sorted intervals:
   - While there are meetings in the heap that have ended (end time <= current start time), remove them from the heap.
   - Add the current meeting's end time to the heap.
   - The size of the heap at this point represents the number of rooms currently in use.
4. The maximum size of the heap during the iteration represents the minimum number of rooms required.

## Code Implementation

### Python

```python
import heapq

def minMeetingRooms(intervals):
    if not intervals:
        return 0
    
    # Sort intervals by start time
    intervals.sort(key=lambda x: x[0])
    
    # Initialize a min heap with the end time of the first meeting
    heap = [intervals[0][1]]
    
    # Process the rest of the meetings
    for i in range(1, len(intervals)):
        # If the current meeting starts after the earliest ending meeting,
        # we can reuse a room
        if intervals[i][0] >= heap[0]:
            heapq.heappop(heap)
        
        # Add the end time of the current meeting to the heap
        heapq.heappush(heap, intervals[i][1])
    
    # The size of the heap is the minimum number of rooms required
    return len(heap)
```

### JavaScript

```javascript
function minMeetingRooms(intervals) {
    if (!intervals || intervals.length === 0) {
        return 0;
    }
    
    // Sort intervals by start time
    intervals.sort((a, b) => a[0] - b[0]);
    
    // Initialize a min heap with the end time of the first meeting
    const heap = [intervals[0][1]];
    
    // Process the rest of the meetings
    for (let i = 1; i < intervals.length; i++) {
        // If the current meeting starts after the earliest ending meeting,
        // we can reuse a room
        if (intervals[i][0] >= heap[0]) {
            heap.shift();
        }
        
        // Add the end time of the current meeting to the heap
        heap.push(intervals[i][1]);
        heap.sort((a, b) => a - b);
    }
    
    // The size of the heap is the minimum number of rooms required
    return heap.length;
}
```

## Chronological Ordering Approach

Another approach is to use chronological ordering:

### Python (Chronological Ordering)

```python
def minMeetingRooms(intervals):
    if not intervals:
        return 0
    
    # Separate start and end times
    start_times = sorted([interval[0] for interval in intervals])
    end_times = sorted([interval[1] for interval in intervals])
    
    # Initialize pointers and room count
    s_ptr = 0
    e_ptr = 0
    rooms = 0
    max_rooms = 0
    
    # Process events in chronological order
    while s_ptr < len(intervals):
        if start_times[s_ptr] < end_times[e_ptr]:
            # A new meeting starts
            rooms += 1
            s_ptr += 1
        else:
            # A meeting ends
            rooms -= 1
            e_ptr += 1
        
        # Update the maximum number of rooms needed
        max_rooms = max(max_rooms, rooms)
    
    return max_rooms
```

### JavaScript (Chronological Ordering)

```javascript
function minMeetingRooms(intervals) {
    if (!intervals || intervals.length === 0) {
        return 0;
    }
    
    // Separate start and end times
    const startTimes = intervals.map(interval => interval[0]).sort((a, b) => a - b);
    const endTimes = intervals.map(interval => interval[1]).sort((a, b) => a - b);
    
    // Initialize pointers and room count
    let sPtr = 0;
    let ePtr = 0;
    let rooms = 0;
    let maxRooms = 0;
    
    // Process events in chronological order
    while (sPtr < intervals.length) {
        if (startTimes[sPtr] < endTimes[ePtr]) {
            // A new meeting starts
            rooms++;
            sPtr++;
        } else {
            // A meeting ends
            rooms--;
            ePtr++;
        }
        
        // Update the maximum number of rooms needed
        maxRooms = Math.max(maxRooms, rooms);
    }
    
    return maxRooms;
}
```

## Time and Space Complexity

### Min Heap Approach:
- **Time Complexity**: O(n log n), where n is the number of intervals. Sorting takes O(n log n) time, and each heap operation takes O(log n) time, which we do at most 2n times.
- **Space Complexity**: O(n) for the heap, which in the worst case will contain all n intervals.

### Chronological Ordering Approach:
- **Time Complexity**: O(n log n), where n is the number of intervals. Sorting the start and end times takes O(n log n) time, and the linear scan takes O(n) time.
- **Space Complexity**: O(n) for storing the separate arrays of start and end times.

## Detailed Walkthrough with Example

Let's trace through the min heap algorithm with Example 1: `intervals = [[0,30],[5,10],[15,20]]`

1. Sort the intervals by start time:
   - The intervals are already sorted: `[[0,30],[5,10],[15,20]]`

2. Initialize the heap with the end time of the first meeting:
   - Heap: [30]

3. Process the second meeting [5,10]:
   - Check if it can reuse a room: 5 < 30, so it cannot
   - Add its end time to the heap: [10, 30]

4. Process the third meeting [15,20]:
   - Check if it can reuse a room: 15 > 10, so it can
   - Remove 10 from the heap and add 20: [20, 30]

5. The maximum size of the heap was 2, so we need 2 rooms.

Now let's trace through the chronological ordering algorithm with the same example:

1. Separate and sort start and end times:
   - Start times: [0, 5, 15]
   - End times: [10, 20, 30]

2. Process events in chronological order:
   - Start at 0: rooms = 1, max_rooms = 1
   - Start at 5: rooms = 2, max_rooms = 2
   - End at 10: rooms = 1, max_rooms = 2
   - Start at 15: rooms = 2, max_rooms = 2
   - End at 20: rooms = 1, max_rooms = 2
   - End at 30: rooms = 0, max_rooms = 2

3. The maximum number of rooms needed was 2.

## Edge Cases and Optimizations

- **Empty Array**: If the input array is empty, the function will return 0 (no meetings to schedule).
- **Single Interval**: If there is only one interval, the function will return 1 (only one room needed).
- **All Overlapping Intervals**: In the worst case, all meetings overlap, and we need n rooms.
- **No Overlapping Intervals**: If no meetings overlap, we need only one room.

**Optimization**: In the min heap approach, we can optimize by only keeping track of the end times in the heap, rather than the entire intervals. This reduces the space complexity of the heap operations.

## Common Mistakes

1. **Not sorting the intervals**: It's crucial to sort the intervals by start time to process them in chronological order.
2. **Incorrect heap operations**: Be careful with the heap operations, especially when deciding whether to reuse a room.
3. **Not handling edge cases**: Remember to handle empty input arrays and other edge cases.

## Related Problems

1. **Meeting Rooms**: Determine if a person can attend all meetings (no overlapping intervals).
2. **Merge Intervals**: Merge all overlapping intervals.
3. **Insert Interval**: Insert a new interval into a sorted list of non-overlapping intervals.
4. **Non-overlapping Intervals**: Find the minimum number of intervals to remove to make the rest non-overlapping. 