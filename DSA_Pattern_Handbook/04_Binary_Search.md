<!--
═══════════════════════════════════════════════════════════════════════════════
  DSA PATTERN HANDBOOK — BINARY SEARCH
  Chapter 04
  
  Patterns in this chapter:
    Pattern 10: Binary Search on Sorted Arrays
    Pattern 11: Binary Search on Answer
  
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

# Chapter 4: Binary Search

Binary search is arguably the most important algorithmic pattern in placements. It appears in two very different forms:

1. **Classical Binary Search** — find a target in a sorted array.
2. **Binary Search on Answer** — find the optimal value in a search space where feasibility is monotonic.

The classical form is well-known. The "binary search on answer" form is what separates good candidates from great ones in OAs.

**Core principle**: Binary search works whenever the search space can be divided into two halves where one half is guaranteed to not contain the answer. This requires a **monotonic property** — once something becomes true (or false), it stays true (or false).

---
---

# Pattern 10: Binary Search on Sorted Arrays

```
═══════════════════════════════════════════════════════════════
  BINARY SEARCH ON SORTED ARRAYS
  OA Frequency: ★★★★★
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Samsung | Flipkart
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find a target value (or its position) in a **sorted** collection."

Linear search checks every element — O(n). Binary search eliminates half the remaining elements at each step — O(log n). For n = 10⁶, that's 20 comparisons instead of 1,000,000.

**Why was this pattern invented?**

Sorting creates order. Order means: if `arr[mid] < target`, then **all elements before mid** are also less than target. We can discard half the array in one comparison. This "halving" is the essence of logarithmic time.

---

## 2. Pattern Identification Checklist

- [ ] Is the input **sorted** (or can be treated as sorted)?
- [ ] Are you looking for a **specific value** or a **boundary** (first/last occurrence)?
- [ ] Does the problem involve finding a **position** in a sorted structure?
- [ ] Can you eliminate half the search space with a single comparison?
- [ ] Does the problem mention "rotated sorted array"?

**If you checked 2+ of these → Binary Search on Sorted Arrays.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Sorted array" + "find" | Direct application |
| "First position of target" | Lower bound binary search |
| "Last position of target" | Upper bound binary search |
| "Search insert position" | Lower bound |
| "Rotated sorted array" | Modified binary search |
| "Peak element" | Binary search on bitonic array |
| "Square root" / "nth root" | Binary search on integers |
| "Search in 2D matrix" | Treat as 1D sorted array |

---

## 4. Constraint Analysis

| Constraint | Linear Search O(n) | Binary Search O(log n) | Verdict |
|---|---|---|---|
| n ≤ 100 | Trivial ✅ | Trivial ✅ | Both fine |
| n ≤ 10⁵ | 10⁵ ✅ | 17 ✅ | Both fine, BS faster |
| n ≤ 10⁹ | 10⁹ ❌ | 30 ✅ | BS required |
| n ≤ 10¹⁸ | Impossible ❌ | 60 ✅ | BS required |

**Key insight**: If the constraint is very large (10⁹ or 10¹⁸), O(log n) is often the only viable approach.

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "Scan from left to right until I find the target." → O(n).

### Step 2: Key Observation
> "The array is sorted. If `arr[mid] < target`, the target must be in the right half. I've just eliminated half the array with one comparison."

### Step 3: Optimization
> "Repeatedly halve the search space: check the middle element, go left or right based on comparison."

### Step 4: Boundary Variants
> "What if there are duplicates and I need the FIRST occurrence? When `arr[mid] == target`, don't stop — continue searching LEFT for an earlier occurrence."

---

## 6. Generic Algorithm

```
BINARY_SEARCH(arr, target):
    lo = 0
    hi = length(arr) - 1
    
    while lo <= hi:
        mid = lo + (hi - lo) / 2     // avoid overflow
        
        if arr[mid] == target:
            return mid
        else if arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    
    return -1     // not found

LOWER_BOUND(arr, target):    // first position where arr[i] >= target
    lo = 0
    hi = length(arr)          // note: hi = n (not n-1)
    
    while lo < hi:            // note: strict <
        mid = lo + (hi - lo) / 2
        
        if arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid          // don't skip mid — it could be the answer
    
    return lo                 // first position >= target

UPPER_BOUND(arr, target):    // first position where arr[i] > target
    lo = 0
    hi = length(arr)
    
    while lo < hi:
        mid = lo + (hi - lo) / 2
        
        if arr[mid] <= target:
            lo = mid + 1
        else:
            hi = mid
    
    return lo
```

---

## 7. Reusable C++ Template

```cpp
// Template: Binary Search — Standard, Lower Bound, Upper Bound
// Time: O(log n), Space: O(1)

#include <vector>
using namespace std;

// Standard: find exact target
int binarySearch(vector<int>& arr, int target) {
    int lo = 0, hi = (int)arr.size() - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) lo = mid + 1;
        else hi = mid - 1;
    }
    return -1;
}

// Lower bound: first index where arr[i] >= target
int lowerBound(vector<int>& arr, int target) {
    int lo = 0, hi = (int)arr.size();
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] < target) lo = mid + 1;
        else hi = mid;
    }
    return lo;
}

// Upper bound: first index where arr[i] > target
int upperBound(vector<int>& arr, int target) {
    int lo = 0, hi = (int)arr.size();
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] <= target) lo = mid + 1;
        else hi = mid;
    }
    return lo;
}

// Note: STL equivalents exist:
// lower_bound(arr.begin(), arr.end(), target) - arr.begin()
// upper_bound(arr.begin(), arr.end(), target) - arr.begin()
```

---

## 8. Complexity Analysis

| Metric | Value | Why |
|---|---|---|
| Time | O(log n) | Search space halves each step: n → n/2 → n/4 → ... → 1 = log₂(n) steps |
| Space | O(1) iterative, O(log n) recursive | Iterative uses constant space; recursive uses stack frames |

**Why O(log n)?** After k comparisons, the remaining search space is n/2^k. We stop when n/2^k = 1, so k = log₂(n).

---

## 9. Dry Run

**Problem**: Find 7 in `[1, 3, 5, 7, 9, 11, 13]`.

```
Array: [1, 3, 5, 7, 9, 11, 13]    target = 7

Step 1: lo=0, hi=6, mid=3
  arr[3] = 7 == target → FOUND at index 3!

(Harder example: find first occurrence of 5 in [2, 3, 5, 5, 5, 8, 9])

Lower bound of 5:

Step 1: lo=0, hi=7, mid=3
  arr[3] = 5 >= 5 → hi = 3
  [2, 3, 5, 5 | 5, 8, 9]
   ↑        ↑
   lo       hi

Step 2: lo=0, hi=3, mid=1
  arr[1] = 3 < 5 → lo = 2
  [2, 3, 5, 5 | 5, 8, 9]
         ↑  ↑
         lo hi

Step 3: lo=2, hi=3, mid=2
  arr[2] = 5 >= 5 → hi = 2
  [2, 3, 5, 5 | 5, 8, 9]
         ↑
       lo=hi

lo == hi → return 2. First occurrence of 5 is at index 2. ✓
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Integer overflow: `(lo + hi) / 2` | Use `lo + (hi - lo) / 2` instead |
| Infinite loop with `lo <= hi` and `hi = mid` | Use `lo < hi` for lower/upper bound; `lo <= hi` only for exact search |
| Off-by-one: `hi = n - 1` vs `hi = n` | For exact search: `hi = n-1`. For lower/upper bound: `hi = n` |
| Confusing lower_bound and upper_bound | Lower: first `>= target`. Upper: first `> target`. Count of target = upper - lower. |
| Forgetting to handle "not found" | Check if returned index is valid and arr[index] == target |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Array is unsorted and can't be sorted | Binary search requires monotonicity | Hash map, linear scan |
| Need to find a subarray or range property | Binary search finds points, not ranges | Sliding window, prefix sum |
| Multiple queries but array changes between queries | Pre-sorted array becomes invalid | Balanced BST, Segment Tree |

---

## 12. Medium-Level Example Problem

**LeetCode #34 — Find First and Last Position of Element in Sorted Array**

> Given a sorted array of integers and a target, find the starting and ending position. Return [-1, -1] if not found.

**Why this represents the pattern well**: It requires TWO binary searches — lower_bound for the first position and upper_bound - 1 for the last position. This tests true understanding of binary search boundaries.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. "First position of target = lower_bound(target)."
2. "Last position of target = upper_bound(target) - 1."
3. "Verify that the found position actually contains the target (handle not-found case)."
4. "Two binary searches, each O(log n). Total: O(log n)."

---

## 14. C++ Implementation

```cpp
#include <vector>
using namespace std;

// Find First and Last Position of Element in Sorted Array
// Pattern: Binary Search (lower and upper bound)
// Time: O(log n), Space: O(1)
vector<int> searchRange(vector<int>& nums, int target) {
    int n = nums.size();

    // Lower bound: first index where nums[i] >= target
    int lo = 0, hi = n;
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] < target) lo = mid + 1;
        else hi = mid;
    }
    int first = lo;

    // Check if target exists
    if (first == n || nums[first] != target) {
        return {-1, -1};
    }

    // Upper bound: first index where nums[i] > target
    lo = first;
    hi = n;
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] <= target) lo = mid + 1;
        else hi = mid;
    }
    int last = lo - 1;

    return {first, last};
}
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "What if the array is rotated?" | LeetCode #33. Find pivot, then binary search on correct half. |
| "Count occurrences of target?" | upper_bound - lower_bound |
| "Search in a 2D sorted matrix?" | Treat as 1D array: row = mid/cols, col = mid%cols |
| "Find peak element in unsorted array?" | Compare mid with mid+1 to decide direction |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Rotated sorted array search** | LeetCode #33. Check which half is sorted, decide direction. |
| **Search in 2D matrix** | LeetCode #74. Flatten to 1D conceptually. |
| **Peak element** | LeetCode #162. Binary search on non-sorted array with local max condition. |
| **Find minimum in rotated sorted** | LeetCode #153. Binary search for the "drop" point. |
| **Sqrt(x)** | LeetCode #69. Binary search for largest k where k² ≤ x. |

---

## 17. Related Patterns

| Pattern | Relationship |
|---|---|
| **Binary Search on Answer** | Same halving logic, but on the answer space instead of the array |
| **Two Pointers** | Both work on sorted data; binary search for single value, two pointers for pairs |
| **Lower/Upper Bound (STL)** | `std::lower_bound` and `std::upper_bound` implement this pattern |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #704 — Binary Search | Easy | Standard binary search |
| 2 | LeetCode #35 — Search Insert Position | Easy | Lower bound application |
| 3 | LeetCode #34 — Find First and Last Position | Medium | Lower + upper bound |
| 4 | LeetCode #33 — Search in Rotated Sorted Array | Medium | Modified binary search |
| 5 | LeetCode #4 — Median of Two Sorted Arrays | Hard | Binary search on partition |

---

## 19. Memory Trick

> 🧠 **"The Dictionary Lookup"**
>
> When you look up a word in a physical dictionary, you don't start from page 1. You open it roughly in the middle, check if your word comes before or after, then repeat on the right half. That's binary search. You never check every page — you halve the remaining pages each time.

---
---

# Pattern 11: Binary Search on Answer

```
═══════════════════════════════════════════════════════════════
  BINARY SEARCH ON ANSWER
  OA Frequency: ★★★★★
  Companies: Google | Amazon | Microsoft | Uber | Flipkart | Atlassian | Walmart
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find the **minimum/maximum value** of something such that a condition is satisfied."

This is NOT about searching in an array. Instead, you **binary search on the answer itself**. The key insight: if an answer X is feasible, then all answers worse than X are also feasible (or all answers better than X are infeasible). This monotonicity lets you binary search on the answer space.

**Why was this pattern invented?**

Some optimization problems are hard to solve directly but easy to verify. "Can I achieve value X?" is often a simple greedy check. Binary search turns an optimization problem into a sequence of decision problems.

**The paradigm shift**: Instead of computing the answer, **guess the answer** and check if it's achievable. Then use binary search to find the best achievable guess.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem ask for the **minimum of a maximum** or **maximum of a minimum**?
- [ ] Is the answer a **numeric value** in a bounded range?
- [ ] Can you write a function `canAchieve(X)` that returns true/false?
- [ ] Is `canAchieve(X)` **monotonic** — if X works, then X+1 also works (or vice versa)?
- [ ] Does the problem mention "allocate," "distribute," "partition" with optimization?

**If you checked 3+ of these → Binary Search on Answer.**

> **This is one of the highest-value patterns to learn.** Many students miss it because they don't recognize that the answer can be binary-searched.

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Minimum possible maximum" | Minimize the worst case → BS on answer |
| "Maximum possible minimum" | Maximize the best case → BS on answer |
| "Allocate pages / books to students" | Classic BS on answer |
| "Aggressive cows" / "maximum min distance" | BS on minimum distance |
| "Split array into k subarrays minimizing max sum" | BS on max sum |
| "Can you do it with at most k operations?" | Feasibility check → BS on answer |
| "Minimum capacity for shipping" | BS on capacity |
| "Koko eating bananas" / "minimum speed" | BS on speed |
| "Smallest divisor given a threshold" | BS on divisor |

---

## 4. Constraint Analysis

| Constraint | Direct Optimization | BS on Answer | Verdict |
|---|---|---|---|
| Answer range ≤ 10⁹ | Often unknown | log₂(10⁹) ≈ 30 checks ✅ | BS on answer works |
| Each check is O(n) | — | O(n log(range)) ✅ | Very efficient |
| n ≤ 10⁵, range ≤ 10⁹ | — | 10⁵ × 30 = 3 × 10⁶ ✅ | Perfect |

**The power of BS on answer**: Even with a billion possible answers, you only check ~30 of them.

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "Try every possible answer value from min to max. For each, check if it's feasible." → O(range × n).

### Step 2: Key Observation
> "The feasibility function is monotonic: if answer=X works, then answer=X+1 also works (for 'minimum' problems). This means I can binary search!"

```
Answer:     1   2   3   4   5   6   7   8   9   10
Feasible?:  ✗   ✗   ✗   ✗   ✓   ✓   ✓   ✓   ✓   ✓
                              ↑
                        First feasible = optimal answer

This is just lower_bound on the answer space!
```

### Step 3: Optimization
> "Binary search on the answer range. For each candidate, run a greedy O(n) check. Total: O(n log(range))."

### Step 4: Optimal Solution
> O(n · log(answer_range)). The hardest part is writing the `canAchieve(X)` function correctly.

---

## 6. Generic Algorithm

```
BINARY_SEARCH_ON_ANSWER(problem):
    lo = minimum_possible_answer
    hi = maximum_possible_answer
    
    while lo < hi:
        mid = lo + (hi - lo) / 2
        
        if canAchieve(mid):
            hi = mid              // mid works — try smaller (for minimization)
        else:
            lo = mid + 1          // mid doesn't work — need larger
    
    return lo                     // optimal answer

// For MAXIMIZATION (maximum possible minimum):
    while lo < hi:
        mid = lo + (hi - lo + 1) / 2   // ceiling division to avoid infinite loop
        
        if canAchieve(mid):
            lo = mid              // mid works — try larger
        else:
            hi = mid - 1          // mid doesn't work — need smaller
    
    return lo
```

**Two variants:**

| Goal | When `canAchieve(mid)` is True | Move |
|---|---|---|
| **Minimize** answer | `hi = mid` (try smaller) | Search left half |
| **Maximize** answer | `lo = mid` (try larger) | Search right half |

---

## 7. Reusable C++ Template

```cpp
// Template: Binary Search on Answer
// Use when: find min/max value where a condition is satisfied
// Time: O(n · log(range)), Space: O(1)

#include <vector>
using namespace std;

// The feasibility check function
// MODIFY: implement your specific greedy/check logic here
bool canAchieve(vector<int>& nums, int guess, int k) {
    // Example: can we split nums into ≤ k groups
    // where each group's sum ≤ guess?
    int groups = 1;
    int currentSum = 0;
    for (int num : nums) {
        if (currentSum + num > guess) {
            groups++;
            currentSum = num;
            if (num > guess) return false;  // single element exceeds guess
        } else {
            currentSum += num;
        }
    }
    return groups <= k;
}

// Minimization: find smallest feasible answer
int binarySearchMinimize(vector<int>& nums, int k) {
    int lo = *max_element(nums.begin(), nums.end());  // MODIFY: min possible answer
    int hi = accumulate(nums.begin(), nums.end(), 0);  // MODIFY: max possible answer

    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (canAchieve(nums, mid, k)) {
            hi = mid;          // feasible — try smaller
        } else {
            lo = mid + 1;      // not feasible — need larger
        }
    }
    return lo;
}

// Maximization: find largest feasible answer
int binarySearchMaximize(vector<int>& nums, int k) {
    int lo = 0;                   // MODIFY: min possible answer
    int hi = 1000000000;          // MODIFY: max possible answer

    while (lo < hi) {
        int mid = lo + (hi - lo + 1) / 2;  // ceiling to prevent infinite loop
        if (canAchieve(nums, mid, k)) {
            lo = mid;          // feasible — try larger
        } else {
            hi = mid - 1;      // not feasible — need smaller
        }
    }
    return lo;
}
```

---

## 8. Complexity Analysis

| Metric | Value | Why |
|---|---|---|
| Time | O(n · log(range)) | log(range) binary search iterations, each running O(n) check |
| Space | O(1) | Only pointer variables and running totals |

| Example Values | Calculations |
|---|---|
| n = 10⁵, range = 10⁹ | 10⁵ × 30 = 3 × 10⁶ ✅ |
| n = 10⁵, range = 10¹⁸ | 10⁵ × 60 = 6 × 10⁶ ✅ |

---

## 9. Dry Run

**Problem**: Split `[7, 2, 5, 10, 8]` into at most 2 subarrays minimizing the maximum sum.

```
Answer range: lo = max(arr) = 10, hi = sum(arr) = 32

Can we achieve max_sum ≤ 21?
  Group 1: 7+2+5 = 14 ≤ 21 ✓, add 10 → 24 > 21 → new group
  Group 2: 10+8 = 18 ≤ 21 ✓
  Groups = 2 ≤ 2 → FEASIBLE ✓

Step 1: lo=10, hi=32, mid=21 → feasible → hi=21
Step 2: lo=10, hi=21, mid=15 → check:
  Group 1: 7+2+5 = 14 ≤ 15 ✓, +10 = 24 > 15 → new group
  Group 2: 10 ≤ 15 ✓, +8 = 18 > 15 → new group
  Group 3: 8
  Groups = 3 > 2 → NOT FEASIBLE ✗ → lo=16

Step 3: lo=16, hi=21, mid=18 → check:
  Group 1: 7+2+5 = 14 ≤ 18 ✓, +10 = 24 > 18 → new group
  Group 2: 10+8 = 18 ≤ 18 ✓
  Groups = 2 ≤ 2 → FEASIBLE ✓ → hi=18

Step 4: lo=16, hi=18, mid=17 → check:
  Group 1: 7+2+5 = 14 ≤ 17 ✓, +10 = 24 > 17 → new group
  Group 2: 10 ≤ 17 ✓, +8 = 18 > 17 → new group
  Groups = 3 > 2 → NOT FEASIBLE ✗ → lo=18

lo == hi == 18 → Answer: 18

Verify: [7, 2, 5 | 10, 8] → sums [14, 18] → max = 18 ✓
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Getting the direction wrong (hi=mid vs lo=mid) | Remember: minimize → hi=mid when feasible. Maximize → lo=mid when feasible. |
| Infinite loop with `lo=mid` | Use `mid = lo + (hi - lo + 1) / 2` (ceiling) when doing `lo = mid` |
| Wrong bounds for lo and hi | lo = smallest possible answer, hi = largest possible answer. Think about what they mean. |
| Incorrect feasibility function | Test your canAchieve() independently before using it in binary search |
| Forgetting edge cases in the check function | Handle: single element > guess, empty groups, exactly k groups |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Feasibility is NOT monotonic | BS requires: feasible(X) ⇒ feasible(X+1) | DP or direct optimization |
| The answer is not a numeric value | Can't binary search on discrete choices | DP, Greedy |
| Checking feasibility is too expensive (> O(n)) | Total time becomes too high | Direct algorithm, DP |
| Answer space is not bounded | Need known lo and hi | Mathematical analysis first |

---

## 12. Medium-Level Example Problem

**LeetCode #875 — Koko Eating Bananas**

> Koko has `n` piles of bananas. She can eat `k` bananas per hour. Each hour, she chooses a pile and eats `k` bananas. If the pile has fewer than `k`, she eats the whole pile. She has `h` hours. Find the minimum integer `k` such that she finishes all bananas within `h` hours.

**Why this represents the pattern well**: It's a clean binary search on answer problem. The answer (eating speed k) has a clear range [1, max(piles)]. The feasibility function is simple: calculate hours needed at speed k. If hours ≤ h, k is feasible. Feasibility is monotonic: higher k always means fewer hours.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. **Identify the answer**: We're looking for the minimum eating speed `k`.
2. **Define the range**: k ∈ [1, max(piles)]. At max speed, each pile takes 1 hour.
3. **Monotonicity check**: If speed k works (finishes in ≤ h hours), then speed k+1 also works (even faster). ✓
4. **Write canFinish(k)**: For each pile, hours = ⌈pile/k⌉. Sum all hours. Return sum ≤ h.
5. **Binary search**: Find smallest k where canFinish(k) is true.

**Key detail**: ⌈a/b⌉ in integer math = `(a + b - 1) / b` or `(a - 1) / b + 1`.

---

## 14. C++ Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

// Koko Eating Bananas
// Pattern: Binary Search on Answer
// Time: O(n · log(max_pile)), Space: O(1)
int minEatingSpeed(vector<int>& piles, int h) {
    // Range: [1, max(piles)]
    int lo = 1;
    int hi = *max_element(piles.begin(), piles.end());

    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;

        // Check: can Koko finish all piles at speed mid in ≤ h hours?
        long long hoursNeeded = 0;
        for (int pile : piles) {
            hoursNeeded += (pile + mid - 1) / mid;  // ceil(pile / mid)
        }

        if (hoursNeeded <= h) {
            hi = mid;      // feasible — try eating slower
        } else {
            lo = mid + 1;  // not feasible — need to eat faster
        }
    }

    return lo;
}
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "What if Koko can eat from multiple piles per hour?" | Change the feasibility function; binary search structure stays the same |
| "What if some piles grow over time?" | More complex feasibility check, but BS on answer still applies |
| "Can you solve it without binary search?" | Not efficiently — direct formulas don't exist for general inputs |
| "What if k can be a real number?" | Binary search on real numbers: while (hi - lo > 1e-9) |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Allocate Books** | Minimize max pages per student. BS on max pages, greedy allocation check. |
| **Aggressive Cows** | Maximize min distance between cows. BS on distance, greedy placement check. |
| **Split Array Largest Sum** | LeetCode #410. Minimize the largest sum. BS on sum, greedy split check. |
| **Capacity to Ship Packages** | LeetCode #1011. Minimize ship capacity. BS on capacity, greedy loading check. |
| **Magnetic Force Between Balls** | LeetCode #1552. Maximize min distance. BS on distance. |
| **Minimum Days to Make Bouquets** | LeetCode #1482. BS on days. |

---

## 17. Related Patterns

| Pattern | Relationship |
|---|---|
| **Binary Search on Sorted Arrays** | Same halving principle, but on array elements rather than answer space |
| **Greedy** | The feasibility check (canAchieve) is usually a greedy algorithm |
| **Two Pointers** | Some problems can be solved by either BS on answer or two pointers |
| **DP** | When feasibility isn't monotonic, DP may be needed instead of BS |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #69 — Sqrt(x) | Easy | BS on answer (simplest form) |
| 2 | LeetCode #875 — Koko Eating Bananas | Medium | Classic BS on answer |
| 3 | LeetCode #1011 — Capacity to Ship Packages | Medium | BS on capacity |
| 4 | LeetCode #410 — Split Array Largest Sum | Hard | BS on max subarray sum |
| 5 | LeetCode #774 — Minimize Max Distance to Gas Station | Hard | BS on real-valued answer |

---

## 19. Memory Trick

> 🧠 **"The Goldilocks Method"**
>
> Binary search on answer is like Goldilocks testing porridge. "Is 50 too much? Yes → try smaller. Is 25 too little? No, it works → but can I go smaller? Is 12 too little? Yes → the answer is between 12 and 25." You're not searching an array — you're searching for the "just right" answer.

---
---

*Previous chapter: [03 — Sliding Window & Pointers](03_Sliding_Window_and_Pointers.md)*
*Next chapter: [05 — Sorting & Greedy](05_Sorting_and_Greedy.md)*
