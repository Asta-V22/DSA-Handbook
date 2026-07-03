<!--
═══════════════════════════════════════════════════════════════════════════════
  DSA PATTERN HANDBOOK — HEAP & INTERVALS
  Chapter 07
  
  Patterns in this chapter:
    Pattern 19: Heap / Priority Queue
    Pattern 20: Interval Problems
    Pattern 21: Merge Intervals
    Pattern 22: Sweep Line
  
  Formatting Rules (internal reference for consistency):
  - Heading hierarchy: # (chapter) → ## (pattern) → ### (section)
  - 20 sections per pattern
  - Complexity notation: O(n), O(n log n), O(n²)
  - Star rating: ★ (filled), ☆ (empty), scale 1–5
  - Code style: C++17, 4-space indent, STL preferred
  - Tables: GitHub-flavored markdown pipe tables
  - Diagrams: ASCII art in code blocks
  - Problem references: "LeetCode #ID — Name" format
═══════════════════════════════════════════════════════════════════════════════
-->

# Chapter 7: Heap & Intervals

This chapter covers four patterns centered around **dynamic ordering** and **range-based problems**. Heaps efficiently track the best/worst element in a changing collection. Interval patterns handle overlapping, merging, and counting events over ranges.

---
---

# Pattern 19: Heap / Priority Queue

```
═══════════════════════════════════════════════════════════════
  HEAP / PRIORITY QUEUE
  OA Frequency: ★★★★★
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Uber | Flipkart
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Efficiently find the minimum or maximum element from a **changing collection**." You're repeatedly adding elements, removing elements, and querying for the best one.

A sorted array can find min/max in O(1), but insertion is O(n). An unsorted array has O(1) insertion but O(n) search. A **heap** gives O(log n) for both operations — the sweet spot.

**Why was this pattern invented?**

Many algorithms need to repeatedly pick the "best" item from a pool — Dijkstra's algorithm picks the closest unvisited node, task scheduling picks the highest-priority task, median maintenance picks from two halves. Heaps enable all of these efficiently.

---

## 2. Pattern Identification Checklist

- [ ] Do you need to repeatedly find the **min or max** from a dynamic collection?
- [ ] Does the problem mention "kth largest/smallest"?
- [ ] Does the problem involve **merging k sorted lists**?
- [ ] Does the problem mention **median of a stream**?
- [ ] Are you assigning resources to the **most/least** demanding tasks?
- [ ] Does the problem require a "greedy" choice of the current best?

**If you checked 2+ of these → Heap / Priority Queue.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points to Heap |
|---|---|
| "Kth largest / smallest" | Classic heap application |
| "Top K elements" | Min-heap of size k |
| "Merge K sorted" | K-way merge with min-heap |
| "Median from data stream" | Two heaps (max + min) |
| "Meeting rooms needed" | Min-heap of end times |
| "Task with highest priority" | Max-heap / priority queue |
| "Reorganize / rearrange by frequency" | Max-heap of frequencies |
| "Closest points" | Min-heap by distance |

---

## 4. Constraint Analysis

| Operation | Sorted Array | Unsorted Array | Heap |
|---|---|---|---|
| Insert | O(n) | O(1) | **O(log n)** |
| Find min/max | O(1) | O(n) | **O(1)** |
| Remove min/max | O(1) or O(n) | O(n) | **O(log n)** |
| Build from n elements | O(n log n) | O(n) | **O(n)** |

Use heap when you have **frequent insertions AND min/max queries**.

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "Sort the collection every time I need the max." → O(n log n) per query.

### Step 2: Key Observation
> "I don't need the entire sorted order — just the extreme (min or max). A heap maintains just enough structure to find the extreme efficiently."

### Step 3: Optimization
> "Use a priority queue. Insert in O(log n), extract max/min in O(log n)."

### Step 4: Advanced Uses
> "For kth largest: maintain a min-heap of size k — the top is the kth largest."
> "For median: maintain a max-heap (left half) and min-heap (right half) — the tops are the two middle elements."

---

## 6. Generic Algorithm

```
TOP_K_ELEMENTS(stream, k):
    min_heap = empty min-heap (size limited to k)
    
    for each element in stream:
        if heap.size() < k:
            heap.push(element)
        else if element > heap.top():
            heap.pop()
            heap.push(element)
    
    return heap    // contains top k elements; heap.top() = kth largest

MERGE_K_SORTED(lists):
    min_heap = empty min-heap
    
    // Initialize with first element of each list
    for each list:
        if list not empty:
            heap.push(list[0], list_index, 0)
    
    result = []
    while heap not empty:
        (value, list_idx, elem_idx) = heap.pop()
        result.append(value)
        if elem_idx + 1 < length(lists[list_idx]):
            heap.push(lists[list_idx][elem_idx + 1], list_idx, elem_idx + 1)
    
    return result
```

---

## 7. Reusable C++ Template

```cpp
// Template: Priority Queue — Common Patterns
// C++ priority_queue is a MAX-HEAP by default

#include <vector>
#include <queue>
using namespace std;

// === Max-Heap (default) ===
priority_queue<int> maxHeap;
maxHeap.push(5);    // insert
maxHeap.top();      // peek max (5)
maxHeap.pop();      // remove max

// === Min-Heap ===
priority_queue<int, vector<int>, greater<int>> minHeap;
minHeap.push(5);    // insert
minHeap.top();      // peek min

// === Custom comparator (for pairs, etc.) ===
// Min-heap by first element of pair
auto cmp = [](const pair<int,int>& a, const pair<int,int>& b) {
    return a.first > b.first;  // greater = min-heap
};
priority_queue<pair<int,int>, vector<pair<int,int>>, decltype(cmp)> pq(cmp);

// Template: Kth Largest Element
// Time: O(n log k), Space: O(k)
int kthLargest(vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int>> minHeap;  // min-heap of size k

    for (int num : nums) {
        minHeap.push(num);
        if ((int)minHeap.size() > k) {
            minHeap.pop();  // remove smallest; keep top k
        }
    }

    return minHeap.top();  // kth largest
}
```

---

## 8. Complexity Analysis

| Operation | Time | Why |
|---|---|---|
| push | O(log n) | Bubble up in the heap |
| pop | O(log n) | Bubble down after removing root |
| top | O(1) | Root is always the extreme |
| Build heap | O(n) | Heapify from bottom up |
| Top K elements | O(n log k) | n pushes, each O(log k) |
| Merge K sorted | O(N log k) | N total elements, heap size k |

---

## 9. Dry Run

**Problem**: Find the 2nd largest in `[3, 1, 4, 1, 5, 9]` using a min-heap of size k=2.

```
Min-heap (size ≤ 2):

num=3: push → heap: [3]           size=1 ≤ 2
num=1: push → heap: [1, 3]        size=2 ≤ 2
num=4: push → heap: [1, 3, 4]     size=3 > 2 → pop min (1)
       heap: [3, 4]
num=1: 1 < top(3) → skip (not in top k)
num=5: push → heap: [3, 4, 5]     size=3 > 2 → pop min (3)
       heap: [4, 5]
num=9: push → heap: [4, 5, 9]     size=3 > 2 → pop min (4)
       heap: [5, 9]

Answer: heap.top() = 5 (2nd largest) ✓
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Forgetting C++ `priority_queue` is MAX-HEAP by default | For min-heap: `priority_queue<int, vector<int>, greater<int>>` |
| Using `greater<>` for max-heap (wrong!) | `greater<>` = min-heap. Default (no argument) = max-heap |
| Pushing then popping when heap size = k (instead of checking first) | For efficiency, check if new element is better than top before pushing |
| Not handling empty heap | Always check `pq.empty()` before `pq.top()` or `pq.pop()` |
| Lazy deletion without tracking | When using lazy deletion, maintain a separate count/set of deleted elements |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Need sorted order (not just extreme) | Heap only gives max/min | Sort the array or use ordered set |
| Static data, many queries | Build once and query | Sorting, Segment Tree |
| Need kth element in O(n) worst case | Heap gives O(n log k) | QuickSelect gives O(n) average |
| Range min/max queries | Heap handles global min/max, not range | Segment Tree, Sparse Table |

---

## 12. Medium-Level Example Problem

**LeetCode #215 — Kth Largest Element in an Array**

> Given an integer array `nums` and an integer `k`, return the kth largest element.

**Why this represents the pattern well**: Clean application of min-heap of size k. Demonstrates the core heap technique for "top K" problems.

---

## 13. Solution Walkthrough

1. "Maintain a min-heap of size k."
2. "For each number, push it. If size > k, pop the smallest."
3. "After processing all numbers, the heap contains the top k largest. The top of the min-heap is the kth largest."

---

## 14. C++ Implementation

```cpp
#include <vector>
#include <queue>
using namespace std;

// Kth Largest Element
// Pattern: Min-Heap of size k
// Time: O(n log k), Space: O(k)
int findKthLargest(vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int>> minHeap;

    for (int num : nums) {
        minHeap.push(num);
        if ((int)minHeap.size() > k) {
            minHeap.pop();
        }
    }

    return minHeap.top();
}
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "Can you do it in O(n) average?" | Use QuickSelect (partition-based) — LeetCode standard |
| "What if elements stream in continuously?" | Keep heap of size k; answer is always top |
| "Find median of a stream?" | Two heaps: max-heap (left), min-heap (right). Balance sizes. |
| "Merge K sorted lists?" | Min-heap of k list heads; pop smallest, push its next |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Top K Frequent** | LeetCode #347. Count frequency first, then heap on frequencies |
| **Merge K Sorted Lists** | LeetCode #23. Min-heap of list node pointers |
| **Find Median from Stream** | LeetCode #295. Two heaps maintaining halves |
| **K Closest Points** | LeetCode #973. Max-heap of size k by distance |
| **Ugly Number II** | LeetCode #264. Min-heap to generate numbers in order |
| **Reorganize String** | LeetCode #767. Max-heap of (frequency, char) |

---

## 17. Related Patterns

| Pattern | Relationship |
|---|---|
| **Sorting** | Sort gives full order; heap gives only extreme — but cheaper for dynamic data |
| **Monotonic Deque** | Deque is O(1) for sliding window max; heap is O(log n) — deque is better for fixed windows |
| **Binary Search** | For kth element in static data, binary search may work |
| **Greedy** | Heap enables greedy algorithms that always pick "the best available" |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #703 — Kth Largest Element in Stream | Easy | Min-heap maintenance |
| 2 | LeetCode #215 — Kth Largest Element in Array | Medium | Core heap technique |
| 3 | LeetCode #23 — Merge K Sorted Lists | Hard | K-way merge |
| 4 | LeetCode #295 — Find Median from Data Stream | Hard | Two-heap technique |
| 5 | LeetCode #767 — Reorganize String | Medium | Frequency-based greedy with heap |

---

## 19. Memory Trick

> 🧠 **"The Talent Show"**
>
> A max-heap is like a talent show where the best performer is always on stage (at the top). New contestants enter (push), and they bubble up if they're better. When the star finishes (pop), the next best takes their place. You always know who the best is — just look at the stage.
>
> For kth largest with a min-heap: "Keep only the top k performers in the waiting room. The weakest of the top k (min of the heap) is at the door. If someone better shows up, the weakest gets kicked out."

---
---

# Pattern 20: Interval Problems

```
═══════════════════════════════════════════════════════════════
  INTERVAL PROBLEMS (GENERAL)
  OA Frequency: ★★★★☆
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Uber
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Problems involving ranges/intervals: `[start, end]`. Common questions: Do intervals overlap? How many overlap at a point? What's the minimum number of resources to handle all intervals? Can we fit all events?

**Why was this pattern invented?**

Real-world scheduling — meetings, flights, events — involves time intervals. The general pattern is: **sort the intervals** by some criterion, then process them in order to answer the question.

---

## 2. Pattern Identification Checklist

- [ ] Is the input a list of **intervals/ranges** `[start, end]`?
- [ ] Does the problem ask about **overlap, conflict, or scheduling**?
- [ ] Does it ask for the **minimum number of resources** (rooms, platforms)?
- [ ] Does it ask whether you can **attend all events**?

---

## 3. Recognition Keywords

| Keyword / Phrase | Sub-Pattern |
|---|---|
| "Merge overlapping intervals" | Merge Intervals (Pattern 21) |
| "Minimum meeting rooms / platforms" | Sweep Line or Heap |
| "Can attend all meetings?" | Sort by start, check overlap |
| "Insert interval" | Binary search or linear merge |
| "Non-overlapping intervals" | Sorting + Greedy (Ch. 5) |
| "Interval intersection" | Two pointers on sorted intervals |

---

## 4. The Three Interval Sub-Patterns

| Sub-Pattern | Sort By | Technique | Purpose |
|---|---|---|---|
| **Merge Intervals** | Start time | Linear merge | Combine overlapping |
| **Interval Scheduling** | End time | Greedy select | Max non-overlapping |
| **Sweep Line** | Event times | +1/-1 counting | Count overlaps at each point |

The general approach for almost all interval problems starts with **sorting**.

---

## 5–19. (Specific sub-patterns are covered in Patterns 21 and 22 below)

Interval problems are a category — the specific techniques are Merge Intervals and Sweep Line. For interval scheduling (non-overlapping maximization), see Chapter 5: Sorting + Greedy.

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #252 — Meeting Rooms | Easy | Sort + check overlap |
| 2 | LeetCode #56 — Merge Intervals | Medium | Classic merge |
| 3 | LeetCode #253 — Meeting Rooms II | Medium | Min rooms (heap or sweep) |
| 4 | LeetCode #435 — Non-overlapping Intervals | Medium | Max non-overlapping |
| 5 | LeetCode #759 — Employee Free Time | Hard | Merge + complement |

**Memory Trick:**

> 🧠 **"Sort First, Think Later"** — 90% of interval problems start with sorting. The only question is: sort by start or end? Start for merging, end for scheduling.

---
---

# Pattern 21: Merge Intervals

```
═══════════════════════════════════════════════════════════════
  MERGE INTERVALS
  OA Frequency: ★★★★★
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Flipkart | Goldman Sachs
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Given a collection of intervals, **combine overlapping intervals** into the smallest set of non-overlapping intervals that cover the same range.

Example: `[1,3], [2,6], [8,10]` → `[1,6], [8,10]`.

**Why was this pattern invented?**

In scheduling, resource allocation, and computational geometry, you often need to simplify overlapping ranges. Sorting by start time ensures you process intervals in order, and overlapping intervals become adjacent in the sorted list.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem give a list of **intervals to combine/merge**?
- [ ] Does it ask for the **number of merged intervals**?
- [ ] Does it involve **inserting an interval** into an existing set?
- [ ] Does the problem ask for the **union of ranges**?

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Merge overlapping intervals" | Direct application |
| "Insert interval" | Insert + re-merge |
| "Total covered length" | Merge first, then sum lengths |
| "Simplify schedule" | Merge overlapping time slots |

---

## 4. Constraint Analysis

| Constraint | Approach | Time |
|---|---|---|
| n ≤ 10⁵ | Sort + linear merge | O(n log n) ✅ |
| Already sorted | Linear merge only | O(n) ✅ |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each pair of intervals, check if they overlap and merge." → O(n²).

### Step 2: Key Observation
> "If intervals are sorted by start time, overlapping intervals are adjacent. I only need to compare each interval with the last merged one."

### Step 3: Optimal
> "Sort by start. Walk through: if current overlaps with last merged interval, extend the end. Otherwise, start a new merged interval." → O(n log n).

---

## 6. Generic Algorithm

```
MERGE_INTERVALS(intervals):
    sort intervals by start time
    
    merged = [intervals[0]]
    
    for i from 1 to n-1:
        if intervals[i].start <= merged.last().end:
            // Overlap → extend
            merged.last().end = max(merged.last().end, intervals[i].end)
        else:
            // No overlap → new interval
            merged.append(intervals[i])
    
    return merged
```

---

## 7. Reusable C++ Template

```cpp
// Template: Merge Intervals
// Time: O(n log n), Space: O(n)

#include <vector>
#include <algorithm>
using namespace std;

vector<vector<int>> mergeIntervals(vector<vector<int>>& intervals) {
    sort(intervals.begin(), intervals.end());  // sort by start time

    vector<vector<int>> merged;

    for (auto& interval : intervals) {
        if (merged.empty() || merged.back()[1] < interval[0]) {
            // No overlap → add new interval
            merged.push_back(interval);
        } else {
            // Overlap → extend end
            merged.back()[1] = max(merged.back()[1], interval[1]);
        }
    }

    return merged;
}
```

---

## 8. Complexity Analysis

| Metric | Value | Why |
|---|---|---|
| Time | O(n log n) | Sorting dominates |
| Space | O(n) | For the merged result |

---

## 9. Dry Run

**Input**: `[[1,3], [2,6], [8,10], [15,18]]`

```
Sorted: [[1,3], [2,6], [8,10], [15,18]]   (already sorted)

merged = [[1,3]]

[2,6]: start=2 ≤ merged.back().end=3 → overlap
  merged.back().end = max(3, 6) = 6
  merged = [[1,6]]

[8,10]: start=8 > merged.back().end=6 → no overlap
  merged = [[1,6], [8,10]]

[15,18]: start=15 > merged.back().end=10 → no overlap
  merged = [[1,6], [8,10], [15,18]]

  Timeline:
  [1====6]    [8==10]      [15==18]
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Not sorting first | Always sort by start time |
| Using `interval[1] < next[0]` instead of `>=` for overlap | Overlap when `current.end >= next.start` |
| Not using `max` when extending | The new interval's end might be shorter than the existing merged end |
| Modifying input array while iterating | Use a separate output array |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Count max overlapping intervals at any point | Merge removes overlap info | Sweep Line |
| Find minimum rooms/resources | Need concurrent count | Sweep Line or Heap |
| Weighted intervals (max profit) | Can't just merge | DP |

---

## 12. Medium-Level Example Problem

**LeetCode #56 — Merge Intervals**

> Given a list of intervals, merge all overlapping intervals.

---

## 13–14. Solution and Implementation

Already covered above in sections 6–7.

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "Insert a new interval into a non-overlapping set?" | LeetCode #57. Find position, merge with overlapping neighbors |
| "What if intervals are given as a stream?" | Maintain a sorted structure (multiset) and merge on insertion |
| "Total length covered by all intervals?" | Merge first, then sum `end - start` for each merged interval |

---

## 16–19. Variations, Related, Practice

**Variations**: Insert Interval (#57), Interval List Intersections (#986), Remove Covered Intervals (#1288)

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #252 — Meeting Rooms | Easy | Check any overlap exists |
| 2 | LeetCode #56 — Merge Intervals | Medium | Classic merge |
| 3 | LeetCode #57 — Insert Interval | Medium | Insert + merge |
| 4 | LeetCode #986 — Interval List Intersections | Medium | Two-pointer merge |
| 5 | LeetCode #352 — Data Stream as Disjoint Intervals | Hard | Online merging |

**Memory Trick:**

> 🧠 **"Overlapping Rugs"** — If you lay rugs (intervals) on a floor sorted by their left edge, any rug whose left edge lands on top of the previous rug's right side overlaps. Just extend the combined rug to the farthest right edge.

---
---

# Pattern 22: Sweep Line

```
═══════════════════════════════════════════════════════════════
  SWEEP LINE
  OA Frequency: ★★★★☆
  Companies: Google | Amazon | Microsoft | Uber | Atlassian
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"At any given point in time, **how many events are active**?" or "What's the **maximum number of concurrent events**?"

Sweep line imagines a vertical line sweeping left to right across the timeline. At each event boundary (start or end), the count of active events changes by +1 or -1. The maximum count is the answer.

**Why was this pattern invented?**

It converts a 2D problem (events over intervals) into a 1D problem (process event boundaries in order). Instead of checking every point, you only check the O(n) points where something changes.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem ask for the **maximum/minimum overlap count**?
- [ ] Does it ask for the **number of active events at a point**?
- [ ] Does it involve **resource allocation** (rooms, platforms, servers)?
- [ ] Can the problem be modeled as "events starting and ending"?

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Minimum meeting rooms" | Max concurrent meetings = sweep line |
| "Minimum platforms needed" | Max concurrent trains |
| "Maximum population year" | +1 at birth, -1 at death |
| "How many events at time t" | Sweep across event boundaries |
| "Skyline problem" | Advanced sweep line |

---

## 4. Constraint Analysis

| Constraint | Brute Force | Sweep Line | Verdict |
|---|---|---|---|
| n ≤ 10⁵ | O(n × time_range) ❌ | O(n log n) ✅ | Sweep line required |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each point in time, count how many intervals contain it." → O(n × range).

### Step 2: Key Observation
> "The count only changes at interval boundaries (start and end). Between boundaries, the count is constant."

### Step 3: Optimization
> "Create events: +1 at each start, -1 at each end. Sort events by time. Sweep through, maintaining a running count."

### Step 4: Optimal
> "O(n log n) for sorting + O(n) sweep = O(n log n) total."

---

## 6. Generic Algorithm

```
SWEEP_LINE(intervals):
    events = []
    for each interval [start, end]:
        events.append((start, +1))   // event begins
        events.append((end, -1))     // event ends
    
    sort events by time (break ties: -1 before +1 if "end before start")
    
    count = 0
    max_count = 0
    
    for each (time, delta) in events:
        count += delta
        max_count = max(max_count, count)
    
    return max_count
```

**Tie-breaking**: If an event ends at time T and another starts at time T:
- If events are **non-overlapping at boundary**: process `-1` before `+1` (same room can be reused).
- If events **overlap at boundary**: process `+1` before `-1`.

This depends on the problem's definition of "overlap."

---

## 7. Reusable C++ Template

```cpp
// Template: Sweep Line — Maximum Concurrent Events
// Time: O(n log n), Space: O(n)

#include <vector>
#include <algorithm>
using namespace std;

int maxConcurrent(vector<vector<int>>& intervals) {
    vector<pair<int, int>> events;

    for (auto& interval : intervals) {
        events.push_back({interval[0], +1});  // start
        events.push_back({interval[1], -1});  // end
    }

    // Sort by time; if tie, end (-1) before start (+1)
    sort(events.begin(), events.end());

    int count = 0, maxCount = 0;
    for (auto& [time, delta] : events) {
        count += delta;
        maxCount = max(maxCount, count);
    }

    return maxCount;
}
```

```cpp
// Alternative: Difference Array approach (when time is bounded)
// Use when time range is small (e.g., 0 to 10⁶)
int maxConcurrentDiffArray(vector<vector<int>>& intervals, int maxTime) {
    vector<int> diff(maxTime + 2, 0);

    for (auto& interval : intervals) {
        diff[interval[0]]++;
        diff[interval[1]]--;  // or diff[interval[1]+1]-- depending on inclusive/exclusive
    }

    int count = 0, maxCount = 0;
    for (int i = 0; i <= maxTime; i++) {
        count += diff[i];
        maxCount = max(maxCount, count);
    }

    return maxCount;
}
```

---

## 8. Complexity Analysis

| Approach | Time | Space | When to Use |
|---|---|---|---|
| Sort-based sweep | O(n log n) | O(n) | General case |
| Difference array | O(n + range) | O(range) | Small time range |
| Heap-based (Meeting Rooms II) | O(n log n) | O(n) | When you need more info per event |

---

## 9. Dry Run

**Problem**: Minimum meeting rooms for `[[0,30], [5,10], [15,20]]`.

```
Events:
  (0, +1)   Meeting 1 starts
  (5, +1)   Meeting 2 starts
  (10, -1)  Meeting 2 ends
  (15, +1)  Meeting 3 starts
  (20, -1)  Meeting 3 ends
  (30, -1)  Meeting 1 ends

Sorted: (0,+1), (5,+1), (10,-1), (15,+1), (20,-1), (30,-1)

Sweep:
  t=0:  count = 0+1 = 1    max = 1
  t=5:  count = 1+1 = 2    max = 2
  t=10: count = 2-1 = 1    max = 2
  t=15: count = 1+1 = 2    max = 2
  t=20: count = 2-1 = 1    max = 2
  t=30: count = 1-1 = 0    max = 2

  Timeline:
  [0=========================30]  Room 1
       [5====10]                   Room 2
                 [15===20]         Room 2 (reused)

Answer: 2 rooms needed ✓
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Wrong tie-breaking order | Think about whether boundary points overlap or not |
| Using inclusive vs exclusive end inconsistently | Clarify: does `[1,3]` occupy time 3? If exclusive, end event is at 3. If inclusive, at 4. |
| Not considering the difference array approach | For small ranges, difference array is simpler and avoids sorting |
| Integer overflow with large time values | Use `long long` if time values > 10⁹ |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Need to merge overlapping intervals | Sweep counts; merge combines | Merge Intervals |
| Need to know WHICH events overlap | Sweep only counts | Sorting + explicit tracking |
| Need weighted overlap (total weight at each point) | Simple +1/-1 doesn't capture weights | Weighted sweep line or segment tree |

---

## 12. Medium-Level Example Problem

**LeetCode #253 — Meeting Rooms II**

> Given an array of meeting time intervals, find the minimum number of conference rooms required.

---

## 13–14. Solution and Implementation

See sections 7 and 9 above for the full sweep line approach.

**Heap-based alternative:**

```cpp
#include <vector>
#include <algorithm>
#include <queue>
using namespace std;

// Meeting Rooms II — Heap approach
// Pattern: Sort + Min-Heap
// Time: O(n log n), Space: O(n)
int minMeetingRooms(vector<vector<int>>& intervals) {
    sort(intervals.begin(), intervals.end());  // sort by start time

    priority_queue<int, vector<int>, greater<int>> endTimes;  // min-heap of end times

    for (auto& interval : intervals) {
        // If earliest ending room is free, reuse it
        if (!endTimes.empty() && endTimes.top() <= interval[0]) {
            endTimes.pop();
        }
        endTimes.push(interval[1]);  // allocate room (or extend)
    }

    return endTimes.size();  // number of rooms in use
}
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "Can a meeting end and another start at the same time?" | Adjust tie-breaking: process ends before starts |
| "What if meetings have different room requirements?" | Weighted sweep line or more complex scheduling |
| "Skyline problem?" | LeetCode #218. Sweep line + max-heap for heights |

---

## 16–19. Variations, Related, Practice

**Variations**: Skyline Problem (#218), Car Pooling (#1094), Corporate Flight Bookings (#1109, see Difference Array)

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #1094 — Car Pooling | Easy | Difference array / sweep line |
| 2 | LeetCode #253 — Meeting Rooms II | Medium | Classic sweep line / heap |
| 3 | LeetCode #1854 — Maximum Population Year | Medium | Simple sweep |
| 4 | LeetCode #218 — The Skyline Problem | Hard | Advanced sweep line |
| 5 | LeetCode #850 — Rectangle Area II | Hard | 2D sweep line |

**Memory Trick:**

> 🧠 **"The Doorway Counter"** — Imagine a mall with a counter at the entrance. It goes +1 when someone enters and -1 when someone exits. At any moment, the counter shows how many people are inside. Sweep line does exactly this for intervals: +1 at start, -1 at end, and you read the counter as you sweep through time.

---
---

*Previous chapter: [06 — Stack & Queue](06_Stack_and_Queue.md)*
*Next chapter: [08 — Bit Manipulation](08_Bit_Manipulation.md)*
