<!--
═══════════════════════════════════════════════════════════════════════════════
  DSA PATTERN HANDBOOK — SORTING & GREEDY
  Chapter 05
  
  Patterns in this chapter:
    Pattern 12: Sorting + Greedy
    Pattern 13: Greedy Algorithms
  
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

# Chapter 5: Sorting & Greedy

Greedy algorithms make the **locally optimal choice** at each step, hoping it leads to a globally optimal solution. Unlike DP, greedy doesn't reconsider past decisions. When it works, greedy is simple and efficient. The challenge is proving that the greedy approach is correct.

Sorting is often the first step in greedy algorithms. By imposing order on the input, sorting reveals structure that makes the greedy choice obvious.

**Key question for every greedy problem**: *"Can I prove that the locally best choice never hurts the global optimum?"* If yes, greedy works. If not, consider DP.

---
---

# Pattern 12: Sorting + Greedy

```
═══════════════════════════════════════════════════════════════
  SORTING + GREEDY
  OA Frequency: ★★★★★
  Companies: Amazon | Google | Microsoft | Adobe | Goldman Sachs | Flipkart | Walmart
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Problems where the input is **unordered** and you need to make **sequential choices** (assign, schedule, pair, distribute). Sorting transforms chaos into order, and once ordered, the greedy choice becomes natural.

**Why was this pattern invented?**

Consider: "Assign n tasks to workers to minimize total time." Without sorting, you'd need to try all n! permutations. With sorting (sort both tasks and workers by difficulty/capability), you can match greedily — O(n log n).

Sorting + Greedy reduces exponential search spaces to polynomial ones when the **optimal substructure** property holds.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem involve **assigning, pairing, or scheduling** items?
- [ ] Would the answer become obvious if the input were sorted?
- [ ] Does the problem ask for "minimum number of operations" or "maximum items selected"?
- [ ] Does sorting by one property (start time, size, cost) simplify the decision?
- [ ] Is there a clear "best first" strategy after sorting?

**If you checked 2+ of these → Sorting + Greedy.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Minimum number of platforms/rooms" | Sort by arrival, greedy counting |
| "Non-overlapping intervals" | Sort by end time, greedy select |
| "Assign tasks to workers" | Sort both, match greedily |
| "Minimum cost to connect" | Sort by cost, take cheapest first |
| "Maximum meetings in one room" | Sort by end time, greedy select |
| "Pair students/teams" | Sort, pair adjacent or match optimally |
| "Rearrange to form largest number" | Custom sort comparator |

---

## 4. Constraint Analysis

| Constraint | Brute Force | Sorting + Greedy | Verdict |
|---|---|---|---|
| n ≤ 10 | O(n!) = 3.6M ✅ | O(n log n) ✅ | Both work |
| n ≤ 20 | O(n!) ≈ 2.4 × 10¹⁸ ❌ | O(n log n) ✅ | Greedy required |
| n ≤ 10⁵ | Impossible | O(n log n) ✅ | Greedy required |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "Try every possible ordering/assignment and pick the best." → O(n!) or O(2ⁿ).

### Step 2: Key Observation
> "What if I process items in a specific order? If I sort by deadline/size/value, the best choice at each step becomes clear."

### Step 3: Sort Criterion
> Identify WHAT to sort by. Common criteria:
> - **By end time** → interval scheduling
> - **By size/difficulty** → assignment matching
> - **By ratio (value/weight)** → fractional knapsack
> - **By deadline** → job scheduling
> - **By start time** → merge intervals

### Step 4: Greedy Rule
> After sorting, the greedy rule is usually: "Take the next item if it doesn't conflict" or "Assign to the smallest/cheapest available resource."

---

## 6. Generic Algorithm

```
SORTING_PLUS_GREEDY(items):
    sort(items, by = CHOSEN_CRITERION)   // e.g., end time, cost, size
    
    result = initial_value
    state = initial_state                 // e.g., last selected end time
    
    for each item in sorted_items:
        if can_select(item, state):       // greedy decision
            select(item)
            update(state)
            result = update_result(result, item)
    
    return result
```

---

## 7. Reusable C++ Template

```cpp
// Template: Sorting + Greedy — Interval Selection
// Use when: select maximum non-overlapping intervals
// Time: O(n log n), Space: O(1)

#include <vector>
#include <algorithm>
using namespace std;

int maxNonOverlapping(vector<vector<int>>& intervals) {
    // Sort by end time (greedy: finish earliest first)
    sort(intervals.begin(), intervals.end(), 
         [](const auto& a, const auto& b) {
             return a[1] < b[1];  // MODIFY: sort criterion
         });

    int count = 0;
    int lastEnd = INT_MIN;  // MODIFY: initial state

    for (auto& interval : intervals) {
        if (interval[0] >= lastEnd) {     // MODIFY: selection condition
            count++;
            lastEnd = interval[1];        // MODIFY: update state
        }
    }

    return count;
}
```

```cpp
// Template: Sorting + Greedy — Assignment/Matching
// Use when: assign items to slots/workers optimally

int assignGreedily(vector<int>& items, vector<int>& slots) {
    sort(items.begin(), items.end());
    sort(slots.begin(), slots.end());

    int matched = 0;
    int j = 0;  // pointer into slots

    for (int i = 0; i < (int)items.size() && j < (int)slots.size(); ) {
        if (slots[j] >= items[i]) {  // MODIFY: matching condition
            matched++;
            i++;
            j++;
        } else {
            j++;  // slot too small, try next
        }
    }

    return matched;
}
```

---

## 8. Complexity Analysis

| Metric | Value | Why |
|---|---|---|
| Time | O(n log n) | Sorting dominates; greedy pass is O(n) |
| Space | O(1) to O(n) | O(1) if sort in-place; O(n) if new sorted array |

---

## 9. Dry Run

**Problem**: Maximum meetings in one room. Meetings: `[(1,4), (2,3), (3,5), (4,6), (5,7)]`. Sort by end time.

```
Sorted by end time: [(2,3), (1,4), (3,5), (4,6), (5,7)]

Meeting (2,3): start=2 ≥ lastEnd=-∞ → SELECT. lastEnd=3. Count=1.
  ──[2===3]─────────────
  
Meeting (1,4): start=1 < lastEnd=3 → SKIP (overlaps).

Meeting (3,5): start=3 ≥ lastEnd=3 → SELECT. lastEnd=5. Count=2.
  ──[2===3][3=======5]──

Meeting (4,6): start=4 < lastEnd=5 → SKIP (overlaps).

Meeting (5,7): start=5 ≥ lastEnd=5 → SELECT. lastEnd=7. Count=3.
  ──[2===3][3=======5][5=======7]

Answer: 3 meetings maximum.
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Sorting by start time instead of end time for interval scheduling | End time greedy is provably optimal for max non-overlapping |
| Not considering the sort criterion carefully | Ask: "WHY does sorting by X give a correct greedy?" |
| Assuming greedy works without proof | Always verify: "If greedy picks X first, is there a solution without X that's at least as good?" |
| Forgetting custom comparator syntax | `sort(v.begin(), v.end(), [](auto& a, auto& b) { return ...; });` |
| Modifying the sorted order during iteration | Don't modify elements you're iterating over |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| 0/1 Knapsack (can't take fractions) | Greedy by value/weight ratio doesn't work for 0/1 | DP |
| Need to consider future consequences | Greedy is myopic — only sees current step | DP |
| "Count ALL optimal solutions" | Greedy finds one; DP counts all | DP |
| Problem has a "penalty for NOT choosing" | Greedy can't handle opportunity costs easily | DP |

**The Greedy vs DP Test**: 
- Can you prove: "choosing the locally best option never hurts globally"? → Greedy.
- Does the problem have "overlapping subproblems"? → DP.

---

## 12. Medium-Level Example Problem

**LeetCode #435 — Non-overlapping Intervals**

> Given an array of intervals, return the minimum number of intervals you need to remove to make the rest non-overlapping.

**Why this represents the pattern well**: It's the classic interval scheduling problem. Sort by end time, greedily keep the maximum non-overlapping intervals. Answer = total - max_non_overlapping.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. "Minimum removals = Total - Maximum we can keep."
2. "Maximum non-overlapping intervals = classic greedy with sort by end time."
3. "Why end time? Because finishing early leaves the most room for future intervals."
4. "Proof: If we have an optimal solution that doesn't include the earliest-ending interval, we can swap the first interval for the earliest-ending one without losing any other intervals."

---

## 14. C++ Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

// Non-overlapping Intervals — Minimum Removals
// Pattern: Sorting + Greedy
// Time: O(n log n), Space: O(1)
int eraseOverlapIntervals(vector<vector<int>>& intervals) {
    if (intervals.empty()) return 0;

    // Sort by end time
    sort(intervals.begin(), intervals.end(),
         [](const auto& a, const auto& b) {
             return a[1] < b[1];
         });

    int kept = 1;  // keep the first interval
    int lastEnd = intervals[0][1];

    for (int i = 1; i < (int)intervals.size(); i++) {
        if (intervals[i][0] >= lastEnd) {
            // No overlap — keep this interval
            kept++;
            lastEnd = intervals[i][1];
        }
        // else: overlap — skip (remove) this interval
    }

    return (int)intervals.size() - kept;
}
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "What if we want to minimize the number of ROOMS instead?" | Sort by start time, use a min-heap of end times (Meeting Rooms II) |
| "What if intervals have weights and we want max total weight?" | Weighted interval scheduling — needs DP, not greedy |
| "What if intervals can be merged?" | Merge Intervals pattern (sort by start, merge overlapping) |
| "What about the minimum number of points to cover all intervals?" | Sort by end, place point at each interval's end greedily |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Activity Selection** | Classic: maximize number of non-overlapping activities |
| **Merge Intervals** | Sort by start, merge overlapping |
| **Meeting Rooms II** | Sort by start, use heap for room allocation |
| **Job Scheduling with Deadlines** | Sort by deadline, use Union Find or greedy |
| **Minimum Platforms** | Sort arrivals and departures, sweep line |

---

## 17. Related Patterns

| Pattern | Relationship |
|---|---|
| **Greedy (general)** | Sorting + Greedy is a specific form of greedy |
| **Merge Intervals** | Uses sorting but merges rather than selects |
| **Sweep Line** | Another sorted-intervals technique |
| **DP** | When greedy fails (weighted intervals, 0/1 choices) |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #455 — Assign Cookies | Easy | Sort both, match greedily |
| 2 | LeetCode #452 — Minimum Number of Arrows to Burst Balloons | Medium | Sort by end, greedy |
| 3 | LeetCode #435 — Non-overlapping Intervals | Medium | Classic interval scheduling |
| 4 | LeetCode #1029 — Two City Scheduling | Medium | Sort by cost difference |
| 5 | LeetCode #621 — Task Scheduler | Medium | Frequency-based greedy |

---

## 19. Memory Trick

> 🧠 **"The Buffet Strategy"**
>
> At an all-you-can-eat buffet with limited time, you'd sort dishes by how quickly you can eat them (end time = finish quickly). Eat the fastest-to-finish dish first, then the next one that doesn't overlap with your full mouth. That's exactly interval scheduling: sort by end time, select non-overlapping greedily.

---
---

# Pattern 13: Greedy Algorithms

```
═══════════════════════════════════════════════════════════════
  GREEDY ALGORITHMS (GENERAL)
  OA Frequency: ★★★★★
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Goldman Sachs | Uber | Flipkart
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Make a sequence of decisions to optimize something, where at each step, there's a **clearly best choice** that doesn't need to be reconsidered."

Greedy algorithms are powerful because they're usually O(n) or O(n log n) — much faster than DP's O(n²) or worse. But they only work when the **greedy choice property** holds: the locally optimal choice leads to a globally optimal solution.

**Why was this pattern invented?**

Many optimization problems have a structure where you can prove: "if the best local choice is made at every step, the result is globally optimal." When this is true, you don't need DP's expensive table-filling. Greedy gives you the answer in a single pass.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem have an **optimal substructure** — optimal solution contains optimal sub-solutions?
- [ ] Can you identify a **clear ordering** for processing elements?
- [ ] At each step, is there a **single best choice** that doesn't depend on future decisions?
- [ ] Can you prove (even informally) that no better global solution exists by making a different local choice?
- [ ] Does the problem NOT have the "take it or leave it" characteristic (that would need DP)?

**If you checked 3+ of these → Greedy.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points to Greedy |
|---|---|
| "Minimum number of coins" (with standard denominations) | Classic greedy (largest first) |
| "Maximum profit with constraints" | Sometimes greedy, sometimes DP |
| "Jump game" | Greedy forward reach |
| "Gas station circuit" | Greedy from best starting point |
| "Reorganize string" | Frequency-based greedy |
| "Minimum operations" (certain types) | Often greedy |
| "Largest number from digits" | Greedy arrangement |
| "Task scheduler with cooldown" | Frequency-based greedy |

---

## 4. Constraint Analysis

| Constraint | DP | Greedy | When to Use Greedy |
|---|---|---|---|
| n ≤ 1000 | O(n²) ✅ | O(n) ✅ | Both work, greedy is faster |
| n ≤ 10⁵ | O(n²) ❌ | O(n) ✅ | Greedy needed if applicable |
| n ≤ 10⁶ | O(n²) ❌ | O(n) ✅ | Must be greedy or O(n) |

**Rule**: If greedy is correct for the problem, it's almost always the best approach (simplest code, fastest runtime).

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "Try all possible sequences of decisions." → Exponential.

### Step 2: Key Observation
> "What if I always pick the [largest/smallest/earliest/cheapest] option first? Does it ever backfire?"

### Step 3: Greedy Hypothesis
> "I believe that picking the locally best choice at each step gives the global optimum."

### Step 4: Proof (Exchange Argument)
> "Suppose there's an optimal solution that makes a different choice at step i. I can swap my greedy choice into that solution without making it worse. Therefore, greedy is at least as good."

This is the **exchange argument** — the standard way to prove greedy correctness:
1. Assume an optimal solution O.
2. Assume greedy makes a different choice at some step.
3. Show you can modify O to include the greedy choice without worsening the result.
4. Therefore, there exists an optimal solution that agrees with greedy.

---

## 6. Generic Algorithm

```
GREEDY(problem):
    // Step 1: Sort or preprocess if needed
    preprocess(problem)
    
    // Step 2: Make greedy choices
    result = initial_value
    
    for each decision_point in problem:
        best_local_choice = find_best_choice(decision_point)
        apply(best_local_choice)
        update(result)
    
    return result
```

**Common greedy strategies:**

| Strategy | When to Use |
|---|---|
| **Largest/Smallest first** | Coin change, fractional knapsack |
| **Earliest deadline first** | Job scheduling |
| **Earliest finish first** | Interval scheduling |
| **Most frequent first** | Task scheduling with cooldown |
| **Best ratio first** | Fractional knapsack (value/weight) |
| **Minimum cost first** | Huffman coding, MST |

---

## 7. Reusable C++ Template

```cpp
// Template: Greedy — Forward Pass
// Use when: make optimal choice at each step going left to right
// Time: O(n), Space: O(1)

#include <vector>
using namespace std;

int greedyForwardPass(vector<int>& nums) {
    int result = 0;        // MODIFY: initial result
    int state = 0;         // MODIFY: tracking variable

    for (int i = 0; i < (int)nums.size(); i++) {
        // MODIFY: greedy decision
        state = max(state, nums[i]);  // or: state += nums[i], etc.

        // MODIFY: check if goal achieved or update result
        if (/* condition */) {
            result++;
            // MODIFY: reset or update state
        }
    }

    return result;
}
```

```cpp
// Template: Greedy — Priority Queue Based
// Use when: always process the "best" available item
// Time: O(n log n), Space: O(n)

#include <vector>
#include <queue>
using namespace std;

int greedyWithHeap(vector<int>& tasks) {
    // Max-heap (use greater<int> for min-heap)
    priority_queue<int> pq;

    for (int task : tasks) {
        pq.push(task);  // MODIFY: what to push
    }

    int result = 0;
    while (!pq.empty()) {
        int best = pq.top();
        pq.pop();

        // MODIFY: process best item
        result += best;

        // MODIFY: re-insert modified item if needed
        if (best - 1 > 0) {
            pq.push(best - 1);
        }
    }

    return result;
}
```

---

## 8. Complexity Analysis

| Greedy Type | Time | Space | Example |
|---|---|---|---|
| Single pass | O(n) | O(1) | Jump Game, Gas Station |
| Sort + pass | O(n log n) | O(1) | Interval scheduling |
| Heap-based | O(n log n) | O(n) | Task Scheduler, Reorganize String |

---

## 9. Dry Run

**Problem**: Jump Game (LeetCode #55). Array `[2, 3, 1, 1, 4]`. Can you reach the last index?

```
nums: [2, 3, 1, 1, 4]
Greedy: track the farthest reachable index.

i=0: nums[0]=2 → can reach index 0+2=2. maxReach = max(0, 2) = 2.
  [2, 3, 1, 1, 4]
   ↑  ─  ─>         reach=2

i=1: 1 ≤ maxReach(2) ✓. nums[1]=3 → can reach 1+3=4. maxReach = max(2, 4) = 4.
  [2, 3, 1, 1, 4]
      ↑  ─  ─  ─>   reach=4

maxReach(4) ≥ last index(4) → return true!

(Don't even need to check i=2, 3, 4)
```

**Why greedy works**: If you can reach index i, and from i you can jump to i+nums[i], then maxReach never decreases. You greedily update the farthest reachable point. If maxReach ever falls below i, you're stuck.

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Assuming greedy works without verification | Always check with counterexamples or use exchange argument |
| Using greedy when DP is needed | If "locally best" choice can lock out better future options, use DP |
| Implementing the wrong greedy criterion | E.g., sorting by start time when you should sort by end time |
| Not handling edge cases | Empty input, single element, all same values |
| Greedy on 0/1 Knapsack | Doesn't work! value/weight ratio greedy fails for integer items |

**Classic counterexample showing greedy fails:**
```
Knapsack: capacity = 4
Items: (weight=3, value=4), (weight=2, value=3), (weight=2, value=3)

Greedy by value/weight: take item 1 (v/w=1.33, weight=3), then nothing fits.
  Value = 4.

DP optimal: take items 2 and 3 (weight=2+2=4, value=3+3=6).
  Value = 6. ✗ Greedy was wrong!
```

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| 0/1 Knapsack | Can't take fractions; greedy by ratio fails | DP |
| Longest Common Subsequence | No greedy choice property | DP |
| Edit Distance | Locally optimal edits can be globally suboptimal | DP |
| Coin Change (arbitrary denominations) | Greedy works for standard denominations but fails for arbitrary | DP |
| Weighted Interval Scheduling | Weights break the simple greedy argument | DP |

---

## 12. Medium-Level Example Problem

**LeetCode #55 — Jump Game**

> Given an array of non-negative integers `nums`, you're initially at index 0. Each element represents max jump length. Return if you can reach the last index.

**Why this represents the pattern well**: Pure greedy thinking — maintain the farthest reachable index. If you can reach index i and from i you can reach further, update maxReach. Simple, elegant, and commonly asked.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. **Brute force**: Try all possible jump sequences (DFS/BFS) → exponential.
2. **Observe**: We don't care WHICH path — just WHETHER the last index is reachable.
3. **Greedy insight**: At each reachable index, update the farthest we can reach. If we ever find that we can't move forward (maxReach < current index), we're stuck.
4. **One pass**: Scan left to right, maintaining maxReach.

---

## 14. C++ Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

// Jump Game — Can you reach the last index?
// Pattern: Greedy (track farthest reach)
// Time: O(n), Space: O(1)
bool canJump(vector<int>& nums) {
    int maxReach = 0;

    for (int i = 0; i < (int)nums.size(); i++) {
        if (i > maxReach) {
            return false;  // can't reach this index
        }
        maxReach = max(maxReach, i + nums[i]);

        if (maxReach >= (int)nums.size() - 1) {
            return true;   // can reach the end
        }
    }

    return true;
}
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "What's the MINIMUM number of jumps?" | LeetCode #45. Greedy: track current reach and next reach. Increment jumps at boundaries. |
| "What if you can also jump backward?" | BFS from index 0 — greedy doesn't apply |
| "What if jump costs have different values?" | Weighted problem → DP (Dijkstra on indices) |
| "What if array has negative numbers (jump length)?" | Problem changes fundamentally — needs different approach |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Jump Game II** | LeetCode #45. Minimum jumps — BFS-like greedy. |
| **Gas Station** | LeetCode #134. Greedy: if total gas ≥ total cost, start from the index where cumulative sum is lowest. |
| **Candy** | LeetCode #135. Two-pass greedy (left-to-right, then right-to-left). |
| **Lemonade Change** | LeetCode #860. Greedy change-making with limited denominations. |
| **Fractional Knapsack** | Sort by value/weight, take as much as possible — greedy works for fractions. |

---

## 17. Related Patterns

| Pattern | Relationship |
|---|---|
| **Sorting + Greedy** | Many greedy algorithms require sorting first |
| **DP** | When greedy fails, DP is usually the fallback |
| **Binary Search on Answer** | BS + greedy check is a common combination |
| **Heap** | Priority queue enables greedy selection of best item efficiently |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #860 — Lemonade Change | Easy | Simple greedy decision-making |
| 2 | LeetCode #55 — Jump Game | Medium | Track farthest reach |
| 3 | LeetCode #134 — Gas Station | Medium | Circular greedy |
| 4 | LeetCode #45 — Jump Game II | Medium | Minimum jumps (BFS-like greedy) |
| 5 | LeetCode #135 — Candy | Hard | Two-pass greedy |

---

## 19. Memory Trick

> 🧠 **"The Greedy Shopper"**
>
> A greedy algorithm shops like someone with no patience: at each shelf, grab the best deal visible right now without checking future aisles. This works when every aisle's "best deal" doesn't affect what's available later. When it does (limited stock, dependencies), you need the "careful planner" (DP).
>
> **The Greedy vs DP Mnemonic**: "Can I decide now and never regret it?" → Greedy. "Might I regret this later?" → DP.

---
---

*Previous chapter: [04 — Binary Search](04_Binary_Search.md)*
*Next chapter: [06 — Stack & Queue](06_Stack_and_Queue.md)*
