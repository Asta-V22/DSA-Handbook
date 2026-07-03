<!--
═══════════════════════════════════════════════════════════════════════════════
  DSA PATTERN HANDBOOK — SLIDING WINDOW & POINTERS
  Chapter 03
  
  Patterns in this chapter:
    Pattern 6:  Sliding Window (Fixed)
    Pattern 7:  Sliding Window (Variable)
    Pattern 8:  Two Pointers
    Pattern 9:  Fast & Slow Pointer
  
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

# Chapter 3: Sliding Window & Pointers

This chapter covers four closely related patterns that all operate by maintaining **pointers** into a linear data structure (array or string). These patterns convert O(n²) brute force into O(n) by avoiding redundant recomputation.

**Key insight for this chapter**: All four patterns share the idea of "don't re-examine elements you've already processed." The difference is in how the pointers move.

| Pattern | Pointer Movement | Use Case |
|---|---|---|
| Fixed Sliding Window | Both move in lockstep | Window of fixed size k |
| Variable Sliding Window | Right expands, left shrinks | Window with a condition |
| Two Pointers | Both converge or diverge | Sorted array pair/triple |
| Fast & Slow | Different speeds | Cycle detection, midpoint |

---
---

# Pattern 6: Sliding Window (Fixed)

```
═══════════════════════════════════════════════════════════════
  SLIDING WINDOW (FIXED SIZE)
  OA Frequency: ★★★★☆
  Companies: Amazon | Microsoft | Google | Goldman Sachs | Adobe
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find the best/sum/property of all **contiguous subarrays of exactly size k**."

Imagine you're looking through a window that's exactly k elements wide. You slide this window one position to the right. The new window shares k-1 elements with the old window — only one element leaves and one enters. Why recompute everything from scratch?

**Why was this pattern invented?**

Brute force: For each of the n-k+1 starting positions, sum k elements → O(n·k).
Fixed sliding window: Maintain a running result, subtract the leaving element, add the entering element → O(n).

The window "slides" by **adding one element on the right and removing one element on the left**.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem ask about **subarrays/substrings of fixed size k**?
- [ ] Is the property (sum, max, count) **incrementally updateable**?
- [ ] Does the problem say "contiguous" and specify a window size?
- [ ] Can you express "new window result" as "old result + entering - leaving"?

**If you checked 2+ of these → Fixed Sliding Window.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Subarray of size k" | Direct application |
| "Contiguous k elements" | Fixed window |
| "Maximum sum of k consecutive" | Classic fixed window |
| "Average of subarrays of size k" | Divide running sum by k |
| "Number of distinct elements in each window of size k" | Window + hash map |

---

## 4. Constraint Analysis

| Constraint | Brute Force O(n·k) | Fixed Window O(n) | Verdict |
|---|---|---|---|
| n ≤ 1000, k ≤ 100 | 10⁵ ✅ | 10³ ✅ | Both work |
| n ≤ 10⁵, k ≤ 10⁴ | 10⁹ ❌ | 10⁵ ✅ | Window required |
| n ≤ 10⁶, k ≤ 10⁶ | 10¹² ❌ | 10⁶ ✅ | Window required |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each starting index i, compute sum of elements from i to i+k-1." → O(n·k).

### Step 2: Key Observation
> "When the window slides from [i, i+k-1] to [i+1, i+k], only one element leaves (nums[i]) and one enters (nums[i+k]). The rest stay the same."

```
Window at position i:   [ . . X X X X X . . . ]
                              ↑           ↑
                              i         i+k-1

Window at position i+1: [ . . . X X X X X . . ]
                                ↑           ↑
                              i+1         i+k

Change: -nums[i] +nums[i+k]
```

### Step 3: Optimization
> "Maintain a running sum. When sliding: `sum = sum - nums[i] + nums[i+k]`."

### Step 4: Optimal Solution
> Initialize window with first k elements. Slide across array. O(n) total.

---

## 6. Generic Algorithm

```
FIXED_SLIDING_WINDOW(nums, k):
    n = length(nums)
    
    // Initialize: compute result for first window [0, k-1]
    window_result = compute(nums[0..k-1])
    best = window_result
    
    // Slide: for each new position
    for i from k to n-1:
        // Add entering element (right side)
        window_result = add(window_result, nums[i])
        
        // Remove leaving element (left side)
        window_result = remove(window_result, nums[i - k])
        
        // Update answer
        best = better(best, window_result)
    
    return best
```

---

## 7. Reusable C++ Template

```cpp
// Template: Fixed Sliding Window
// Use when: find best property of all subarrays of size k
// Time: O(n), Space: O(1) for sum; O(k) for tracking elements

#include <vector>
#include <algorithm>
using namespace std;

int fixedSlidingWindow(vector<int>& nums, int k) {
    int n = nums.size();
    
    // Step 1: Initialize first window
    int windowResult = 0;  // MODIFY: initial value
    for (int i = 0; i < k; i++) {
        windowResult += nums[i];  // MODIFY: add operation
    }
    
    int best = windowResult;  // MODIFY: initial best
    
    // Step 2: Slide the window
    for (int i = k; i < n; i++) {
        windowResult += nums[i];      // add entering element
        windowResult -= nums[i - k];  // remove leaving element
        best = max(best, windowResult);  // MODIFY: update best
    }
    
    return best;
}
```

---

## 8. Complexity Analysis

| Metric | Value | Why |
|---|---|---|
| Time | O(n) | Each element enters and leaves the window exactly once |
| Space | O(1) | For sum/average. O(k) if tracking window elements |

**Why O(n) and not O(n·k)?** Each element is processed at most twice: once when it enters the window, once when it leaves. Total work = 2n = O(n).

---

## 9. Dry Run

**Problem**: Maximum sum of subarray of size k=3 in `[2, 1, 5, 1, 3, 2]`.

```
Array: [2, 1, 5, 1, 3, 2]     k = 3

Step 1: Initialize window [0,1,2]
  sum = 2 + 1 + 5 = 8
  best = 8
  
  [2, 1, 5, 1, 3, 2]
   --------
   window = 8

Step 2: Slide to i=3
  sum = 8 + nums[3] - nums[0] = 8 + 1 - 2 = 7
  best = max(8, 7) = 8
  
  [2, 1, 5, 1, 3, 2]
      --------
      window = 7

Step 3: Slide to i=4
  sum = 7 + nums[4] - nums[1] = 7 + 3 - 1 = 9
  best = max(8, 9) = 9
  
  [2, 1, 5, 1, 3, 2]
         --------
         window = 9

Step 4: Slide to i=5
  sum = 9 + nums[5] - nums[2] = 9 + 2 - 5 = 6
  best = max(9, 6) = 9
  
  [2, 1, 5, 1, 3, 2]
            --------
            window = 6

Answer: 9 (subarray [5, 1, 3])
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Starting the slide loop at i=0 instead of i=k | First window is built separately; sliding starts at index k |
| Using `nums[i - k + 1]` instead of `nums[i - k]` | The leaving element is at index `i - k` (k positions before current right end) |
| Not handling k > n | Check `if (k > n) return -1;` at the start |
| Trying to use variable sliding window logic | Fixed window has NO condition for shrinking — both pointers move in lockstep |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Window size is not fixed | Size varies based on a condition | Variable Sliding Window |
| Need the maximum element in each window (not sum) | Sum can be updated in O(1); max cannot | Monotonic Deque |
| Non-contiguous subsets | Window must be contiguous | DP or backtracking |

---

## 12. Medium-Level Example Problem

**LeetCode #643 — Maximum Average Subarray I**

> Given an integer array `nums` and an integer `k`, find the contiguous subarray of length k that has the maximum average value and return this value.

**Why this represents the pattern well**: It's the purest fixed sliding window problem — find maximum sum of k consecutive elements, then divide by k.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. "Maximum average of k elements = maximum sum of k elements / k."
2. "This is 'find the maximum sum subarray of exactly size k.'"
3. "Brute force: check all n-k+1 windows → O(n·k)."
4. "Optimize: sliding window — maintain running sum, slide by adding/removing one element → O(n)."

---

## 14. C++ Implementation

```cpp
#include <vector>
#include <algorithm>
#include <climits>
using namespace std;

// Maximum Average Subarray I
// Pattern: Fixed Sliding Window
// Time: O(n), Space: O(1)
double findMaxAverage(vector<int>& nums, int k) {
    int n = nums.size();

    // Initialize: sum of first window
    double windowSum = 0;
    for (int i = 0; i < k; i++) {
        windowSum += nums[i];
    }

    double maxSum = windowSum;

    // Slide the window
    for (int i = k; i < n; i++) {
        windowSum += nums[i];      // entering element
        windowSum -= nums[i - k];  // leaving element
        maxSum = max(maxSum, windowSum);
    }

    return maxSum / k;
}
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "Find maximum element in each window of size k?" | Use Monotonic Deque — O(n) instead of O(n·k) naive |
| "Find the number of distinct elements in each window?" | Use hash map, update on slide: add entering, decrement leaving |
| "What if k is not given — find k that maximizes average?" | Trick: average of entire array is always the answer (or binary search on k) |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Maximum in each window** | Use Monotonic Deque instead of simple sum |
| **Count windows with sum ≥ threshold** | Check condition at each slide position |
| **Minimum window sum** | Change `max` to `min` |
| **Product of window** | Use multiplication, handle zeros carefully |

---

## 17. Related Patterns

| Pattern | Relationship |
|---|---|
| **Variable Sliding Window** | When window size is determined by a condition, not fixed |
| **Prefix Sum** | Alternative for sum queries; prefix sum is more general (any range), window is O(1) space |
| **Monotonic Deque** | Handles max/min in sliding window; fixed window only handles sum-like operations |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #643 — Maximum Average Subarray I | Easy | Pure fixed sliding window |
| 2 | LeetCode #1456 — Max Number of Vowels in Substring of Given Length | Medium | Count vowels in window of size k |
| 3 | LeetCode #1343 — Number of Sub-arrays of Size K and Average ≥ Threshold | Medium | Fixed window with condition |
| 4 | LeetCode #239 — Sliding Window Maximum | Hard | Fixed window + monotonic deque |
| 5 | LeetCode #480 — Sliding Window Median | Hard | Fixed window + two heaps |

---

## 19. Memory Trick

> 🧠 **"The Train Window"**
>
> You're on a train looking through a window. As the train moves forward by one seat, one new building comes into view and one old building disappears. You don't re-examine all buildings — you just update: +1 new, -1 old.

---
---

# Pattern 7: Sliding Window (Variable)

```
═══════════════════════════════════════════════════════════════
  SLIDING WINDOW (VARIABLE SIZE)
  OA Frequency: ★★★★★
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Uber | Flipkart
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find the longest/shortest **subarray or substring** that satisfies a condition."

Unlike the fixed window where k is given, here the window **grows and shrinks dynamically** based on whether the current window satisfies a condition. The right pointer expands the window to include more elements; the left pointer contracts it when the condition is violated.

**Why was this pattern invented?**

Brute force: Check all O(n²) subarrays, validate each → O(n²) or O(n³).
Variable sliding window: Each element is added once (right pointer) and removed at most once (left pointer) → O(n).

The key invariant: **the window always represents a valid (or best candidate) subarray/substring.**

---

## 2. Pattern Identification Checklist

- [ ] Does the problem ask for the **longest/shortest subarray or substring** with a condition?
- [ ] Is the condition **monotonic** — adding elements can only violate it, and removing from the left can restore it (or vice versa)?
- [ ] Does the window only need to move **forward** (never backward)?
- [ ] Are all elements **non-negative** (for sum-based conditions)?

**If you checked 3+ of these → Variable Sliding Window.**

> **Critical**: Variable sliding window requires **monotonicity**. If adding an element to the window can both improve AND worsen the condition (e.g., elements can be negative), the pattern may not work directly.

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Longest substring" | Classic variable window |
| "Shortest subarray with sum ≥ k" | Shrink when condition is met |
| "At most k distinct characters" | Condition based on distinct count |
| "Maximum length with sum ≤ k" | Expand while valid |
| "Minimum window containing all characters" | Shrink to minimize while valid |
| "Without repeating characters" | Window validity = no repeats |
| "Subarray with at most k zeros" | Count-based condition |

---

## 4. Constraint Analysis

| Constraint | Brute Force O(n²) | Variable Window O(n) | Verdict |
|---|---|---|---|
| n ≤ 5000 | 2.5 × 10⁷ ⚠️ | 5000 ✅ | Window preferred |
| n ≤ 10⁵ | 10¹⁰ ❌ | 10⁵ ✅ | Window required |
| n ≤ 10⁶ | 10¹² ❌ | 10⁶ ✅ | Window required |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each starting index l, extend r as far as possible while the condition holds. Record the best window." → O(n²).

### Step 2: Key Observation
> "When I move l to l+1, the previous best r can only stay or increase — the valid window starting at l+1 is at least as far as the one starting at l. So I don't need to reset r."

```
If window [l, r] is valid and [l, r+1] is not:
  Move l → l+1. Now [l+1, r] might be valid again.
  Don't reset r to l+1 — start checking from r onward.

l only moves right. r only moves right. Total moves = 2n.
```

### Step 3: Optimization
> "Use two pointers l and r. Expand r to grow the window. When the condition breaks, shrink by moving l. Both pointers only move forward."

### Step 4: Optimal Solution
> O(n) — each element is visited by r once and by l at most once.

---

## 6. Generic Algorithm

```
VARIABLE_SLIDING_WINDOW(input):
    left = 0
    best = initial_worst_value
    window_state = initial_state    // e.g., sum, frequency map, count
    
    for right from 0 to n-1:
        // EXPAND: add input[right] to window
        update_window_state(window_state, input[right])
        
        // SHRINK: while window is INVALID, remove from left
        while window_is_invalid(window_state):
            remove_from_window(window_state, input[left])
            left += 1
        
        // UPDATE: current window [left, right] is valid
        best = better(best, right - left + 1)
    
    return best
```

**Two templates based on what you're optimizing:**

| Goal | Shrink When | Update Best |
|---|---|---|
| **Longest** valid window | Window becomes invalid | After shrinking (window is now valid) |
| **Shortest** valid window | Window IS valid (shrink to minimize) | While shrinking (before losing validity) |

---

## 7. Reusable C++ Template

```cpp
// Template A: Variable Sliding Window — LONGEST valid window
// Use when: "find longest subarray/substring satisfying condition"
// Time: O(n), Space: O(k) for hash map/set

#include <vector>
#include <unordered_map>
using namespace std;

int longestValidWindow(vector<int>& nums, int k /* condition param */) {
    int n = nums.size();
    int left = 0;
    int best = 0;
    // MODIFY: window state (sum, freq map, count, etc.)
    int windowState = 0;

    for (int right = 0; right < n; right++) {
        // EXPAND: include nums[right]
        windowState += nums[right];  // MODIFY: add operation

        // SHRINK: while window is INVALID
        while (windowState > k /* MODIFY: invalid condition */) {
            windowState -= nums[left];  // MODIFY: remove operation
            left++;
        }

        // UPDATE: window [left, right] is valid
        best = max(best, right - left + 1);
    }

    return best;
}
```

```cpp
// Template B: Variable Sliding Window — SHORTEST valid window
// Use when: "find shortest subarray/substring satisfying condition"

int shortestValidWindow(vector<int>& nums, int target) {
    int n = nums.size();
    int left = 0;
    int best = INT_MAX;
    int windowSum = 0;

    for (int right = 0; right < n; right++) {
        windowSum += nums[right];

        // SHRINK: while window IS VALID (try to make it shorter)
        while (windowSum >= target) {
            best = min(best, right - left + 1);  // record before shrinking
            windowSum -= nums[left];
            left++;
        }
    }

    return best == INT_MAX ? 0 : best;
}
```

---

## 8. Complexity Analysis

| Metric | Value | Why |
|---|---|---|
| Time | O(n) | Each element enters the window once (right++) and leaves at most once (left++) |
| Space | O(1) to O(k) | O(1) for sum; O(k) if using hash map with k distinct elements |

**Amortized analysis**: The left pointer moves at most n times total across all iterations of the outer loop. So inner while loop doesn't make it O(n²) — it's O(n) amortized.

---

## 9. Dry Run

**Problem**: Longest subarray with sum ≤ 7 in `[3, 1, 2, 7, 4, 2, 1, 1, 5]`.

```
Array: [3, 1, 2, 7, 4, 2, 1, 1, 5]    target ≤ 7

right=0: add 3, sum=3 ≤ 7 ✓    window=[3]        len=1, best=1
  [3, 1, 2, 7, 4, 2, 1, 1, 5]
   ↑
   L,R

right=1: add 1, sum=4 ≤ 7 ✓    window=[3,1]      len=2, best=2
  [3, 1, 2, 7, 4, 2, 1, 1, 5]
   ↑  ↑
   L  R

right=2: add 2, sum=6 ≤ 7 ✓    window=[3,1,2]    len=3, best=3
  [3, 1, 2, 7, 4, 2, 1, 1, 5]
   ↑     ↑
   L     R

right=3: add 7, sum=13 > 7 ✗   
  shrink: remove 3, sum=10, L=1. Still > 7.
  shrink: remove 1, sum=9, L=2.  Still > 7.
  shrink: remove 2, sum=7, L=3.  7 ≤ 7 ✓
  window=[7]                     len=1, best=3 (unchanged)

right=4: add 4, sum=11 > 7 ✗
  shrink: remove 7, sum=4, L=4.  4 ≤ 7 ✓
  window=[4]                     len=1, best=3

right=5: add 2, sum=6 ≤ 7 ✓    window=[4,2]      len=2, best=3

right=6: add 1, sum=7 ≤ 7 ✓    window=[4,2,1]    len=3, best=3

right=7: add 1, sum=8 > 7 ✗
  shrink: remove 4, sum=4, L=5.  4 ≤ 7 ✓
  window=[2,1,1]                 len=3, best=3

right=8: add 5, sum=9 > 7 ✗
  shrink: remove 2, sum=7, L=6.  7 ≤ 7 ✓
  window=[1,1,5]                 len=3, best=3

Answer: 3
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Confusing longest vs shortest template | Longest: shrink while invalid, update after. Shortest: shrink while valid, update during. |
| Using sliding window when elements can be negative | Negative elements break monotonicity — window expansion can decrease sum. Use prefix sum + hash map instead. |
| Forgetting to update left pointer | Always increment left inside the while loop |
| Resetting right when left moves | Never! Both pointers only move forward. |
| Not handling empty result | Return 0 or -1 if no valid window exists |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Elements can be negative (for sum condition) | Adding negative elements can restore validity → non-monotonic | Prefix Sum + Hash Map, or Deque approach |
| Need ALL valid subarrays (not just longest/shortest) | Window can skip valid subarrays | Brute force or DP |
| Condition is not about a contiguous subarray | Window is inherently contiguous | Subset DP / Backtracking |
| "Subarray sum exactly equals k" (with negatives) | Not monotonic | Prefix Sum + Hash Map |

---

## 12. Medium-Level Example Problem

**LeetCode #3 — Longest Substring Without Repeating Characters**

> Given a string `s`, find the length of the longest substring without repeating characters.

**Why this represents the pattern well**: The condition (no repeats) is monotonic — adding a character can only introduce a repeat, and removing from the left can only remove a repeat. It naturally uses a hash set to track characters in the window.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. **Brute force**: Check all O(n²) substrings, verify each has no repeats → O(n³) or O(n²).
2. **Observe**: If substring [l, r] has no repeats and we extend to r+1:
   - If s[r+1] is new → still valid, expand.
   - If s[r+1] is a repeat → shrink from left until the duplicate is removed.
3. **Data structure**: Use a hash set to track characters in the current window.
4. **Monotonicity**: Adding a character can create a repeat (invalid). Removing from left can only remove characters (restore validity). ✓

---

## 14. C++ Implementation

```cpp
#include <string>
#include <unordered_set>
#include <algorithm>
using namespace std;

// Longest Substring Without Repeating Characters
// Pattern: Variable Sliding Window + Hash Set
// Time: O(n), Space: O(min(n, alphabet_size))
int lengthOfLongestSubstring(string s) {
    unordered_set<char> window;  // characters in current window
    int left = 0;
    int best = 0;

    for (int right = 0; right < (int)s.size(); right++) {
        // SHRINK: while s[right] causes a repeat
        while (window.count(s[right])) {
            window.erase(s[left]);  // remove leftmost character
            left++;
        }

        // EXPAND: add s[right] to window
        window.insert(s[right]);

        // UPDATE: current window is valid (no repeats)
        best = max(best, right - left + 1);
    }

    return best;
}
```

**Alternative (faster — using last index):**
```cpp
#include <string>
#include <unordered_map>
#include <algorithm>
using namespace std;

int lengthOfLongestSubstring(string s) {
    unordered_map<char, int> lastSeen;  // char → last index
    int left = 0, best = 0;

    for (int right = 0; right < (int)s.size(); right++) {
        if (lastSeen.count(s[right]) && lastSeen[s[right]] >= left) {
            left = lastSeen[s[right]] + 1;  // jump left past the duplicate
        }
        lastSeen[s[right]] = right;
        best = max(best, right - left + 1);
    }

    return best;
}
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "What if we allow at most k repeats?" | Track frequency in hash map; shrink when any char has freq > k |
| "What if we want longest with at most k distinct characters?" | Track distinct count; shrink when distinct > k |
| "What about shortest substring containing all characters of another string?" | LeetCode #76 — Minimum Window Substring. Shrink while valid. |
| "What if the string is very long (streaming)?" | Same approach works — window moves forward, O(1) per character |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **At most k distinct characters** | LeetCode #340. Track distinct count in window. |
| **Minimum window substring** | LeetCode #76. Shortest window containing all target chars. Shrink while valid. |
| **Maximum consecutive ones with k flips** | LeetCode #1004. Count zeros in window ≤ k. |
| **Longest subarray after deleting one element** | LeetCode #1493. Allow one "hole" in window. |
| **Longest repeating character replacement** | LeetCode #424. Most frequent char in window + k ≥ window size. |

---

## 17. Related Patterns

| Pattern | Relationship |
|---|---|
| **Fixed Sliding Window** | When k is given; variable window when k is determined by condition |
| **Two Pointers** | Both use two pointers, but two pointers typically work on sorted arrays; sliding window on unsorted |
| **Prefix Sum + Hashing** | Use when sliding window fails (e.g., negative elements with sum condition) |
| **Monotonic Deque** | Augments sliding window to track max/min within the window |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #209 — Minimum Size Subarray Sum | Medium | Shortest window with sum ≥ k (positive only) |
| 2 | LeetCode #3 — Longest Substring Without Repeating Characters | Medium | Classic variable window + hash set |
| 3 | LeetCode #424 — Longest Repeating Character Replacement | Medium | Window + frequency tracking |
| 4 | LeetCode #76 — Minimum Window Substring | Hard | Shortest window containing all chars |
| 5 | LeetCode #992 — Subarrays with K Different Integers | Hard | atMost(k) - atMost(k-1) technique |

---

## 19. Memory Trick

> 🧠 **"The Caterpillar"**
>
> A variable sliding window moves like a caterpillar: the front (right) stretches forward, and when the body gets too long (invalid), the back (left) catches up. The caterpillar never moves backward — both ends only go forward.

---
---

# Pattern 8: Two Pointers

```
═══════════════════════════════════════════════════════════════
  TWO POINTERS
  OA Frequency: ★★★★★
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Goldman Sachs | Flipkart
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find a pair (or set of elements) in a **sorted** array that satisfies a condition."

With brute force, checking all pairs takes O(n²). But if the array is sorted, we can use two pointers starting from opposite ends. If the current sum is too small, move the left pointer right (to increase it). If too large, move the right pointer left (to decrease it). Each step eliminates an entire row/column of possibilities.

**Why was this pattern invented?**

Sorting creates a **monotonic relationship** between pointer positions and values. This lets us make informed decisions about which pointer to move, guaranteeing we never miss the answer while skipping most of the O(n²) pairs.

---

## 2. Pattern Identification Checklist

- [ ] Is the input **sorted** (or can you sort it)?
- [ ] Are you looking for a **pair/triple** with a specific property (sum, difference)?
- [ ] Can you determine which direction to move based on comparing the current result to the target?
- [ ] Does the problem involve **partitioning** or **merging** from two ends?
- [ ] Is the problem about two sequences that need to be compared/merged?

**If you checked 2+ of these → Two Pointers.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Pair with sum = target" (sorted) | Classic two pointer convergence |
| "3Sum" / "triplet" | Fix one, two-pointer on rest |
| "Container with most water" | Width × height from two ends |
| "Sorted array, remove duplicates" | In-place two pointer |
| "Merge two sorted arrays" | Two pointers advancing independently |
| "Palindrome check" | Compare from both ends |
| "Partition array" | Dutch National Flag-style partitioning |

---

## 4. Constraint Analysis

| Constraint | Brute Force O(n²) | Two Pointers O(n) | Verdict |
|---|---|---|---|
| n ≤ 5000 | 2.5 × 10⁷ ⚠️ | 5000 ✅ | Two pointers preferred |
| n ≤ 10⁵ | 10¹⁰ ❌ | 10⁵ ✅ | Two pointers required (after sort: O(n log n)) |

**Note**: If the array isn't sorted and you sort it, the total time is O(n log n) for sorting + O(n) for two pointers = O(n log n). This is fine when n ≤ 10⁵.

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "Check all pairs (i, j) where i < j. For each pair, check if nums[i] + nums[j] == target." → O(n²).

### Step 2: Key Observation
> "If the array is sorted and `nums[left] + nums[right] < target`, then moving left rightward increases the sum. If `> target`, moving right leftward decreases it. We eliminate possibilities without checking them."

```
Sorted: [1, 3, 5, 7, 9]   target = 10

left=0, right=4: 1 + 9 = 10 ✓ FOUND!

If sum < target: left++  (need larger values)
If sum > target: right-- (need smaller values)
If sum = target: found!
```

### Step 3: Optimization
> "Start with left=0, right=n-1. Move inward based on the comparison. Each step eliminates one position." → O(n).

### Step 4: Optimal Solution
> O(n) after sorting (O(n log n) total). Each element is visited at most once by either pointer.

---

## 6. Generic Algorithm

```
TWO_POINTER_CONVERGE(sorted_array, target):
    left = 0
    right = length(sorted_array) - 1
    
    while left < right:
        current = compute(sorted_array[left], sorted_array[right])
        
        if current == target:
            return (left, right)   // found!
        else if current < target:
            left += 1              // need more
        else:
            right -= 1             // need less
    
    return NOT_FOUND
```

**Other two-pointer styles:**

| Style | Pointer Start | Movement | Use Case |
|---|---|---|---|
| **Converging** | Opposite ends | Move inward | Pair sum, container water |
| **Same direction** | Both at start | Fast/slow movement | Remove duplicates, partition |
| **Two arrays** | One in each | Advance independently | Merge sorted arrays |

---

## 7. Reusable C++ Template

```cpp
// Template A: Two Pointers — Converging (pair with property)
// Use when: sorted array, find pair satisfying condition
// Time: O(n), Space: O(1)

#include <vector>
using namespace std;

pair<int, int> twoPointerConverge(vector<int>& sorted, int target) {
    int left = 0, right = (int)sorted.size() - 1;

    while (left < right) {
        int sum = sorted[left] + sorted[right];  // MODIFY: your computation

        if (sum == target) {
            return {left, right};     // MODIFY: what to return
        } else if (sum < target) {
            left++;                   // need larger value
        } else {
            right--;                  // need smaller value
        }
    }

    return {-1, -1};  // not found
}
```

```cpp
// Template B: Two Pointers — Same Direction (in-place modification)
// Use when: remove duplicates, partition, compact array
// Time: O(n), Space: O(1)

int twoPointerSameDir(vector<int>& nums) {
    int slow = 0;  // write position
    for (int fast = 0; fast < (int)nums.size(); fast++) {
        if (/* CONDITION: should we keep nums[fast]? */) {
            nums[slow] = nums[fast];  // write to slow position
            slow++;
        }
    }
    return slow;  // new length
}
```

---

## 8. Complexity Analysis

| Metric | Value | Why |
|---|---|---|
| Time | O(n) for the two-pointer pass | Each pointer moves at most n steps total |
| Time (with sorting) | O(n log n) | Sorting dominates |
| Space | O(1) | Only two pointer variables |

---

## 9. Dry Run

**Problem**: Find two numbers in sorted array `[1, 3, 5, 7, 9, 11]` that sum to 12.

```
Array: [1, 3, 5, 7, 9, 11]    target = 12

Step 1: L=0, R=5 → 1 + 11 = 12 = target ✓ → FOUND at (0, 5)!

(Quick find! Let's show a harder case with target = 10)

Step 1: L=0, R=5 → 1 + 11 = 12 > 10 → R--
  [1, 3, 5, 7, 9, 11]
   ↑            ↑
   L            R

Step 2: L=0, R=4 → 1 + 9 = 10 = target ✓ → FOUND at (0, 4)!

(One more: target = 8)

Step 1: L=0, R=5 → 1 + 11 = 12 > 8 → R--
Step 2: L=0, R=4 → 1 + 9 = 10 > 8 → R--
Step 3: L=0, R=3 → 1 + 7 = 8 = target ✓ → FOUND at (0, 3)!
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Forgetting to sort the array first | Two pointers only works on sorted arrays! |
| Using `left <= right` instead of `left < right` | For pairs, `left < right` (can't pair element with itself) |
| Not handling duplicates (in 3Sum) | Skip equal consecutive elements: `while (left < right && nums[left] == nums[left-1]) left++` |
| Assuming two pointers works on unsorted arrays | It doesn't for sum problems — use hashing for unsorted |
| Integer overflow when computing sum | Use `long long` if values are large |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Array is unsorted and you can't sort (need original indices) | Sorting loses index information | Hashing |
| Need to find subarray, not pair | Two pointers find pairs; subarrays use sliding window | Sliding Window |
| Non-monotonic property | Can't decide which pointer to move | Brute force, DP |
| Finding an element (not a pair) | Only one value to find | Binary Search |

---

## 12. Medium-Level Example Problem

**LeetCode #15 — 3Sum**

> Given an integer array `nums`, return all triplets `[nums[i], nums[j], nums[k]]` such that `i ≠ j ≠ k` and `nums[i] + nums[j] + nums[k] == 0`. No duplicate triplets.

**Why this represents the pattern well**: It reduces a 3-pointer problem to a fixed-element + 2-pointer problem. Sorting enables both the two-pointer technique and duplicate skipping.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. **Brute force**: Three nested loops → O(n³). Too slow for n ≤ 3000.
2. **Reduce**: Fix one element `nums[i]`, then find two elements that sum to `-nums[i]`. This is Two Sum on the remaining array.
3. **Sort the array**: Now Two Sum becomes a two-pointer problem → O(n) per fixed element.
4. **Handle duplicates**: Skip consecutive equal values to avoid duplicate triplets.
5. **Total**: O(n²) — fix each element (O(n)) × two pointers (O(n)).

---

## 14. C++ Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

// 3Sum: find all unique triplets that sum to zero
// Pattern: Sorting + Two Pointers
// Time: O(n²), Space: O(1) extra (O(n) for sorting)
vector<vector<int>> threeSum(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    vector<vector<int>> result;
    int n = nums.size();

    for (int i = 0; i < n - 2; i++) {
        // Skip duplicates for first element
        if (i > 0 && nums[i] == nums[i - 1]) continue;

        // Early termination: if smallest triple > 0, no solution
        if (nums[i] > 0) break;

        int target = -nums[i];
        int left = i + 1, right = n - 1;

        while (left < right) {
            int sum = nums[left] + nums[right];

            if (sum == target) {
                result.push_back({nums[i], nums[left], nums[right]});

                // Skip duplicates
                while (left < right && nums[left] == nums[left + 1]) left++;
                while (left < right && nums[right] == nums[right - 1]) right--;

                left++;
                right--;
            } else if (sum < target) {
                left++;
            } else {
                right--;
            }
        }
    }

    return result;
}
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "What about 4Sum?" | Fix two elements, two-pointer on remaining → O(n³) |
| "Closest 3Sum to target?" | Track minimum difference instead of exact match |
| "Count triplets instead of listing them?" | Count valid pairs per fixed element instead of listing |
| "What if duplicates are allowed in output?" | Remove the duplicate-skipping logic |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Two Sum II (sorted)** | Simple convergence from both ends |
| **3Sum Closest** | Track closest sum; don't need exact match |
| **4Sum** | Fix two, two-pointer on two → O(n³) |
| **Remove Duplicates (sorted)** | Same direction: slow=write, fast=read |
| **Container With Most Water** | Two pointers, move the shorter side inward |
| **Trapping Rain Water** | Two pointers tracking left_max and right_max |

---

## 17. Related Patterns

| Pattern | Relationship |
|---|---|
| **Sliding Window** | Both use two pointers, but sliding window handles contiguous subarrays; two pointers handle pairs in sorted arrays |
| **Binary Search** | Both work on sorted data; binary search finds one value, two pointers find pairs |
| **Hashing** | Alternative for pair problems on unsorted arrays; uses O(n) space vs O(1) for two pointers |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #167 — Two Sum II (Sorted) | Medium | Basic converging two pointers |
| 2 | LeetCode #11 — Container With Most Water | Medium | Width × height optimization |
| 3 | LeetCode #15 — 3Sum | Medium | Fix one + two pointers |
| 4 | LeetCode #42 — Trapping Rain Water | Hard | Two pointers with max tracking |
| 5 | LeetCode #16 — 3Sum Closest | Medium | Two pointers with closest tracking |

---

## 19. Memory Trick

> 🧠 **"The Handshake"**
>
> Two pointers start at opposite ends of a sorted array and walk toward each other like two people approaching for a handshake. If they're too far apart (sum too large), the right person steps forward. If too close (sum too small), the left person steps forward. They meet in the middle.

---
---

# Pattern 9: Fast & Slow Pointer

```
═══════════════════════════════════════════════════════════════
  FAST & SLOW POINTER (FLOYD'S TORTOISE AND HARE)
  OA Frequency: ★★★☆☆
  Companies: Amazon | Google | Microsoft | Apple | LinkedIn
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Two fundamental problems:
1. **Cycle detection**: Does a linked list (or sequence) contain a cycle?
2. **Finding the middle**: Where is the midpoint of a linked list?

Without the fast/slow pointer trick, cycle detection requires O(n) space (hash set of visited nodes). The fast/slow pointer does it in O(1) space.

**Why was this pattern invented?**

If two runners start at the same point on a circular track and one runs twice as fast, the fast runner will eventually "lap" the slow runner — they'll meet again. This is Floyd's insight: if there's a cycle, the fast pointer (2 steps) and slow pointer (1 step) will eventually collide.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem involve a **linked list** and mention "cycle"?
- [ ] Do you need to find the **middle node** of a linked list?
- [ ] Does a function mapping produce a sequence that might repeat?
- [ ] Does the problem say "detect loop" or "find entry point of loop"?
- [ ] Can the problem be modeled as following a chain of `next` pointers?

**If you checked 2+ of these → Fast & Slow Pointer.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Linked list cycle" | Direct application |
| "Find middle of linked list" | When fast reaches end, slow is at middle |
| "Happy number" | Sequence eventually cycles |
| "Find duplicate number" (in [1,n]) | Array as linked list → cycle detection |
| "Starting point of cycle" | Phase 2 of Floyd's algorithm |

---

## 4. Constraint Analysis

| Constraint | Hash Set Approach | Fast & Slow | Verdict |
|---|---|---|---|
| n ≤ 10⁵ nodes | O(n) time, O(n) space ✅ | O(n) time, O(1) space ✅ | Fast & slow preferred (less space) |
| Space constraint O(1) | ❌ Hash set uses O(n) | ✅ Only two pointers | Fast & slow required |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "Visit each node, store it in a hash set. If I visit a node that's already in the set, there's a cycle." → O(n) time, O(n) space.

### Step 2: Key Observation
> "In a cycle, a faster pointer will eventually catch up to a slower pointer (like runners on a circular track). This eliminates the need for a hash set."

### Step 3: Optimization
> "Use two pointers: slow moves 1 step, fast moves 2 steps. If they meet, there's a cycle. If fast reaches NULL, no cycle."

### Step 4: Finding Cycle Entry Point
> "After detection (meeting point), reset one pointer to head. Move both at speed 1. They meet at the cycle's entry. (Mathematical proof: distance from head to entry = distance from meeting point to entry going around the cycle.)"

---

## 6. Generic Algorithm

```
CYCLE_DETECTION(head):
    slow = head
    fast = head
    
    while fast != NULL and fast.next != NULL:
        slow = slow.next          // 1 step
        fast = fast.next.next     // 2 steps
        
        if slow == fast:
            return true           // cycle detected!
    
    return false                  // no cycle (fast reached end)

FIND_CYCLE_ENTRY(head):
    // Phase 1: Detect cycle
    slow = head
    fast = head
    while fast != NULL and fast.next != NULL:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            break
    
    if fast == NULL or fast.next == NULL:
        return NULL               // no cycle
    
    // Phase 2: Find entry point
    slow = head                   // reset slow to head
    while slow != fast:
        slow = slow.next          // both move at speed 1
        fast = fast.next
    
    return slow                   // meeting point = cycle entry

FIND_MIDDLE(head):
    slow = head
    fast = head
    while fast != NULL and fast.next != NULL:
        slow = slow.next          // 1 step
        fast = fast.next.next     // 2 steps
    return slow                   // slow is at middle
```

---

## 7. Reusable C++ Template

```cpp
// Template: Fast & Slow Pointer
// Use when: cycle detection, finding middle, or sequence cycle

struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};

// Detect cycle in linked list
// Time: O(n), Space: O(1)
bool hasCycle(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head;

    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) return true;
    }
    return false;
}

// Find cycle entry point
// Time: O(n), Space: O(1)
ListNode* detectCycleEntry(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head;

    // Phase 1: Find meeting point
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) break;
    }
    if (!fast || !fast->next) return nullptr;

    // Phase 2: Find entry
    slow = head;
    while (slow != fast) {
        slow = slow->next;
        fast = fast->next;
    }
    return slow;
}

// Find middle node
// Time: O(n), Space: O(1)
ListNode* findMiddle(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head;

    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    return slow;  // for even length: second middle
}
```

---

## 8. Complexity Analysis

| Operation | Time | Space | Why |
|---|---|---|---|
| Cycle detection | O(n) | O(1) | Fast pointer traverses at most 2n steps |
| Cycle entry | O(n) | O(1) | Phase 1 + Phase 2, each O(n) |
| Find middle | O(n/2) = O(n) | O(1) | Slow reaches middle when fast reaches end |

**Why O(n) for cycle detection?** If there's a cycle of length c, the fast pointer gains 1 step per iteration on the slow pointer. After at most c iterations of being in the cycle, fast catches slow. Total steps ≤ n + c ≤ 2n.

---

## 9. Dry Run

**Cycle Detection**: List `1 → 2 → 3 → 4 → 5 → 3` (cycle back to node 3)

```
        1 → 2 → 3 → 4 → 5
                ↑         |
                └─────────┘

Step 1: slow=1, fast=1
Step 2: slow=2, fast=3   (slow: 1→2, fast: 1→2→3)
Step 3: slow=3, fast=5   (slow: 2→3, fast: 3→4→5)
Step 4: slow=4, fast=4   (slow: 3→4, fast: 5→3→4)
  slow == fast → CYCLE DETECTED at node 4!

Phase 2 (find entry):
  Reset slow to head (node 1). Both move speed 1.
  Step 1: slow=2, fast=5
  Step 2: slow=3, fast=3
  slow == fast → CYCLE ENTRY at node 3! ✓
```

**Find Middle**: List `1 → 2 → 3 → 4 → 5`

```
Step 1: slow=1, fast=1
Step 2: slow=2, fast=3
Step 3: slow=3, fast=5
  fast->next is NULL → STOP
  Middle = slow = 3 ✓
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Checking `fast->next->next` without checking `fast->next` first | Always check `fast && fast->next` before advancing |
| Initializing slow and fast to different starting points | Both start at head |
| Forgetting Phase 2 when finding cycle entry | Detection (meeting) ≠ entry point |
| Applying to arrays without understanding the "next" function | Map the array to a functional graph first (e.g., nums[i] → next node) |
| Using fast & slow for problems that need hash set (e.g., visited tracking) | Fast & slow only detects cycles, not arbitrary revisits |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Need to detect cycle in a generic graph (not linked list) | Fast/slow needs a single "next" — graphs have multiple neighbors | DFS coloring |
| Need to find ALL nodes in the cycle | Fast/slow finds one meeting point | DFS/BFS after finding entry |
| Need cycle detection with extra information (path, length) | Fast/slow gives limited info | Hash set stores full path |
| Doubly linked list problems | Usually solved with direct traversal | Standard pointer manipulation |

---

## 12. Medium-Level Example Problem

**LeetCode #287 — Find the Duplicate Number**

> Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]`, there is exactly one repeated number. Find it without modifying the array and using only O(1) extra space.

**Why this represents the pattern well**: The array can be interpreted as a linked list where `nums[i]` points to the next node. Since there's a duplicate, two nodes point to the same destination → creating a cycle. Finding the duplicate = finding the cycle entry point.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. **Constraints**: Can't modify array, O(1) space → can't sort, can't use hash set.
2. **Model as linked list**: Index 0 → nums[0] → nums[nums[0]] → ... Since values are in [1,n], we never reach index 0 again (no self-loops from index 0). The duplicate creates a cycle.
3. **Apply Floyd's algorithm**: Phase 1 finds meeting point, Phase 2 finds entry point. The entry point is the duplicate number.

**Key insight**: An array of integers in range [1,n] IS a linked list. Index i points to index nums[i]. A duplicate value means two indices point to the same index → cycle.

---

## 14. C++ Implementation

```cpp
#include <vector>
using namespace std;

// Find the Duplicate Number
// Pattern: Fast & Slow Pointer (Floyd's Cycle Detection)
// Time: O(n), Space: O(1)
int findDuplicate(vector<int>& nums) {
    // Treat array as linked list: index i → index nums[i]
    int slow = nums[0];
    int fast = nums[0];

    // Phase 1: Find meeting point in cycle
    do {
        slow = nums[slow];            // 1 step
        fast = nums[nums[fast]];      // 2 steps
    } while (slow != fast);

    // Phase 2: Find cycle entry (= duplicate number)
    slow = nums[0];
    while (slow != fast) {
        slow = nums[slow];
        fast = nums[fast];
    }

    return slow;  // the duplicate number
}
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "What if there are multiple duplicates?" | Floyd's only finds one. Need different approach (e.g., cycling through multiple times or different method) |
| "Can you find the duplicate in O(n log n) with binary search?" | Binary search on value: count elements ≤ mid. If count > mid, duplicate is in [1, mid] |
| "What if you can modify the array?" | Mark visited by negating: if nums[abs(nums[i])] is already negative, abs(nums[i]) is duplicate |
| "Prove why Phase 2 works?" | Mathematical proof: if distance from head to entry is a, and from entry to meeting point is b, then a ≡ 0 (mod cycle length). So both pointers from head and meeting point meet at entry. |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Happy Number** | LeetCode #202. Sequence n → sum of digit squares. Cycles if not happy. |
| **Linked List Cycle II** | LeetCode #142. Find the cycle entry node. |
| **Middle of Linked List** | LeetCode #876. Fast reaches end → slow is at middle. |
| **Palindrome Linked List** | Find middle, reverse second half, compare. |

---

## 17. Related Patterns

| Pattern | Relationship |
|---|---|
| **Two Pointers** | Both use two pointers, but fast/slow move at different speeds on the same path; two pointers move on sorted arrays from opposite ends |
| **Hashing** | Alternative for cycle detection using O(n) space |
| **DFS** | Used for cycle detection in general graphs (not just linked lists) |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #141 — Linked List Cycle | Easy | Basic cycle detection |
| 2 | LeetCode #876 — Middle of Linked List | Easy | Fast/slow for midpoint |
| 3 | LeetCode #142 — Linked List Cycle II | Medium | Cycle entry (Phase 2) |
| 4 | LeetCode #287 — Find the Duplicate Number | Medium | Array as linked list |
| 5 | LeetCode #457 — Circular Array Loop | Medium | Cycle detection on circular array |

---

## 19. Memory Trick

> 🧠 **"The Racetrack"**
>
> Imagine two runners on a circular track: one jogs (slow), one sprints (fast). If the track is circular (cycle exists), the sprinter will eventually lap the jogger and they'll meet. If the track is straight (no cycle), the sprinter just reaches the end first. After they meet, to find WHERE the loop starts, send one runner back to the start line — when they meet again, that's the loop entrance.

---
---

*Previous chapter: [02 — Fundamentals](02_Fundamentals.md)*
*Next chapter: [04 — Binary Search](04_Binary_Search.md)*
