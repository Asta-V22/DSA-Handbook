<!--
═══════════════════════════════════════════════════════════════════════════════
  DSA PATTERN HANDBOOK — FUNDAMENTALS
  Chapter 02
  
  Patterns in this chapter:
    Pattern 1: Brute Force Thinking
    Pattern 2: Prefix Sum
    Pattern 3: Difference Array
    Pattern 4: Hashing
    Pattern 5: Frequency Counting
  
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

# Chapter 2: Fundamentals

This chapter covers the foundational patterns that every other pattern in this handbook builds upon. Master these first — they appear in virtually every OA, often as sub-steps of more complex patterns.

---
---

# Pattern 1: Brute Force Thinking

```
═══════════════════════════════════════════════════════════════
  BRUTE FORCE THINKING
  OA Frequency: ★★★★★
  Companies: Every company — this is the starting point
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Every problem. Brute force isn't a "pattern" in the traditional sense — it's a **thinking methodology**. It answers the question: *"If I had unlimited time, how would I check every possible answer?"*

Why is brute force important?

1. **It's always correct** — by checking everything, you can't miss the answer.
2. **It reveals structure** — the brute force often shows you what work is being repeated, which guides optimization.
3. **It's your safety net** — if you can't find the optimal solution in an OA, a brute force that handles small inputs is better than nothing.
4. **Interviewers expect it** — "Start with brute force, then optimize" is the standard interview workflow.

> **The golden rule**: Never optimize before you can write the brute force. If you can't brute force it, you don't understand the problem.

---

## 2. Pattern Identification Checklist

- [x] Can you enumerate all possible answers? (Almost always yes)
- [x] Is the constraint small enough for exponential or polynomial brute force? (Check n ≤ 20 for exponential, n ≤ 1000 for O(n²))
- [x] Are you stuck on a problem and need a starting point?
- [x] Do you need to verify your optimized solution's correctness?

**If you checked any of these**: Start with brute force before anything else.

---

## 3. Recognition Keywords

Brute force applies to literally every problem. But it's **especially the final answer** when you see:

| Keyword / Constraint | Why Brute Force Works |
|---|---|
| n ≤ 10 | O(n!) ≈ 3.6M — feasible |
| n ≤ 15 | O(2ⁿ) ≈ 32K — feasible |
| n ≤ 20 | O(2ⁿ) ≈ 1M — feasible |
| "All permutations" | Inherently O(n!) |
| "All subsets" | Inherently O(2ⁿ) |
| "Check every pair" (n ≤ 5000) | O(n²) ≈ 25M — usually feasible |

---

## 4. Constraint Analysis

| Constraint | Brute Force Complexity | Feasible? |
|---|---|---|
| n ≤ 8 | O(n!) = 40,320 | ✅ Easily |
| n ≤ 10 | O(n!) = 3,628,800 | ✅ Yes |
| n ≤ 15 | O(2ⁿ · n) ≈ 500K | ✅ Yes |
| n ≤ 20 | O(2ⁿ) ≈ 1M | ✅ Yes |
| n ≤ 25 | O(2ⁿ) ≈ 33M | ⚠️ Tight |
| n ≤ 1000 | O(n²) = 1M | ✅ Yes |
| n ≤ 5000 | O(n²) = 25M | ⚠️ Tight |
| n ≤ 10,000 | O(n²) = 100M | ❌ Too slow |
| n ≤ 100,000 | Need O(n log n) or O(n) | ❌ Much too slow |

---

## 5. Evolution of Thought

This pattern IS the brute force, so the "evolution" here is about **how brute force thinking leads to better patterns**:

### Step 1: State the Brute Force
> "I'll try every possible combination/pair/subset and check which one satisfies the condition."

### Step 2: Identify Redundant Work
> "I'm recomputing the same subarray sum over and over." → **Prefix Sum**
> "I'm scanning the same elements repeatedly to find a value." → **Hashing**
> "I'm checking overlapping windows from scratch." → **Sliding Window**
> "I'm solving the same subproblem multiple times." → **DP**

### Step 3: Eliminate Redundancy
Choose the pattern that removes the repeated work.

### Step 4: Verify
Run both brute force and optimized solution on small inputs. They should match.

**This evolution of thought IS the core skill of DSA problem-solving.**

---

## 6. Generic Algorithm

```
BRUTE_FORCE(problem):
    best_answer = worst_possible_value
    
    for each possible_candidate in ALL_CANDIDATES:
        if is_valid(possible_candidate):
            best_answer = better(best_answer, evaluate(possible_candidate))
    
    return best_answer
```

Common forms:
- **Single loop**: Check each element — O(n)
- **Nested loops**: Check each pair — O(n²)
- **Triple loops**: Check each triple — O(n³)
- **Subset enumeration**: Check each subset — O(2ⁿ)
- **Permutation enumeration**: Check each ordering — O(n!)

---

## 7. Reusable C++ Template

```cpp
// Template: Brute Force — Check All Pairs
// Use when: n ≤ 5000 and you need to find a pair with some property
// Time: O(n²), Space: O(1)

#include <vector>
using namespace std;

int bruteForceAllPairs(vector<int>& nums, int target) {
    int n = nums.size();
    int result = /* INITIAL VALUE */;

    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {  // MODIFY: j = 0 if pairs can repeat
            // MODIFY: Check your condition here
            if (/* CONDITION(nums[i], nums[j]) */) {
                result = /* UPDATE RESULT */;
            }
        }
    }

    return result;
}
```

```cpp
// Template: Brute Force — Enumerate All Subsets (Bitmask)
// Use when: n ≤ 20
// Time: O(2ⁿ · n), Space: O(n)

#include <vector>
using namespace std;

int bruteForceSubsets(vector<int>& nums) {
    int n = nums.size();
    int result = /* INITIAL VALUE */;

    for (int mask = 0; mask < (1 << n); mask++) {
        // Build current subset
        vector<int> subset;
        for (int i = 0; i < n; i++) {
            if (mask & (1 << i)) {
                subset.push_back(nums[i]);
            }
        }

        // MODIFY: Evaluate this subset
        if (/* IS_VALID(subset) */) {
            result = /* UPDATE(result, EVALUATE(subset)) */;
        }
    }

    return result;
}
```

---

## 8. Complexity Analysis

| Form | Time | Space | Why |
|---|---|---|---|
| All pairs | O(n²) | O(1) | Two nested loops, each up to n |
| All triples | O(n³) | O(1) | Three nested loops |
| All subsets | O(2ⁿ · n) | O(n) | 2ⁿ subsets, each takes O(n) to evaluate |
| All permutations | O(n! · n) | O(n) | n! orderings, each takes O(n) to evaluate |
| All subarrays | O(n²) to enumerate, O(n³) with sum | O(1) | n² subarrays, each up to length n |

---

## 9. Dry Run

**Problem**: Find two numbers in `[2, 7, 11, 15]` that sum to `9`.

```
Brute Force: Check all pairs

Pair (0,1): nums[0] + nums[1] = 2 + 7 = 9  ✓ FOUND!
    → Return [0, 1]

(No need to check remaining pairs)

Total comparisons: 1 (lucky case)
Worst case: n(n-1)/2 = 6 pairs for n=4
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Starting with j = 0 when pairs should be distinct | Use `j = i + 1` for distinct pairs |
| Forgetting that brute force IS a valid solution for small n | Always check constraints first |
| Not writing brute force because "it's too slow" | Write it anyway — it guides optimization |
| Off-by-one in subset bitmask | `mask < (1 << n)`, not `mask <= (1 << n)` |
| Trying to optimize before understanding the problem | Write brute force first, THEN optimize |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| n > 5000 and O(n²) is required | TLE guaranteed | Look for O(n log n) or O(n) patterns |
| n > 20 and subset enumeration | 2²⁰ ≈ 1M is the limit | DP, Greedy, or Binary Search |
| Problem explicitly asks for "efficient" solution | Brute force won't earn full marks | Optimize after identifying the pattern |

**However**: Even when brute force won't pass, **write it mentally** to understand the structure.

---

## 12. Medium-Level Example Problem

**LeetCode #1 — Two Sum**

> Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

**Why this represents the pattern well**: It's the simplest possible "check all pairs" problem. The brute force is O(n²), and it naturally leads to the hash map optimization (O(n)) — showing how brute force thinking evolves into a better solution.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. **"What's the simplest approach?"** → Check every pair of numbers.
2. **Write the brute force**: Two nested loops, check if `nums[i] + nums[j] == target`.
3. **Identify redundancy**: For each `nums[i]`, I'm scanning the entire array to find `target - nums[i]`. That's a lookup problem.
4. **Optimize**: Use a hash map to store numbers I've already seen. For each `nums[i]`, check if `target - nums[i]` is in the map.
5. **Complexity drops**: O(n²) → O(n).

**Key insight**: The brute force revealed that the inner loop is just "searching for a complement." Hash maps do searches in O(1).

---

## 14. C++ Implementation

```cpp
#include <vector>
#include <unordered_map>
using namespace std;

// Brute Force: O(n²) time, O(1) space
vector<int> twoSumBrute(vector<int>& nums, int target) {
    int n = nums.size();
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if (nums[i] + nums[j] == target) {
                return {i, j};
            }
        }
    }
    return {};  // no solution found
}

// Optimized: O(n) time, O(n) space
vector<int> twoSumOptimal(vector<int>& nums, int target) {
    unordered_map<int, int> seen;  // value → index

    for (int i = 0; i < (int)nums.size(); i++) {
        int complement = target - nums[i];

        if (seen.count(complement)) {
            return {seen[complement], i};
        }

        seen[nums[i]] = i;  // store current number for future lookups
    }

    return {};  // no solution found
}
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "What if the array is sorted?" | Use Two Pointers instead of hash map — O(1) space |
| "What if there are multiple valid pairs?" | Return all pairs; use hash map but don't stop at first match |
| "What if you need three numbers summing to target?" | 3Sum — sort + fix one element + two pointers for remaining |
| "Can duplicate indices be used?" | Change inner loop to start from `j = 0` (or `j = i`) |

---

## 16. Variations

| Variation | Change |
|---|---|
| **Three Sum** | Fix one element, reduce to Two Sum on the rest |
| **Four Sum** | Fix two elements, reduce to Two Sum |
| **Subset Sum** | Generalization to k elements — use backtracking or DP |
| **Count pairs with property** | Instead of returning first pair, count all valid pairs |

---

## 17. Related Patterns

| Pattern | Relationship |
|---|---|
| **Hashing** | Brute force inner loop often becomes a hash lookup |
| **Two Pointers** | Brute force on sorted arrays optimizes to two pointers |
| **Backtracking** | Brute force with pruning |
| **DP** | Brute force with memoization |
| **Binary Search on Answer** | Brute force search space replaced with binary search |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #1 — Two Sum | Easy | Basic pair enumeration → hash optimization |
| 2 | LeetCode #217 — Contains Duplicate | Easy | Brute O(n²) vs Hash O(n) |
| 3 | LeetCode #15 — 3Sum | Medium | Brute O(n³) → Sort + Two Pointers O(n²) |
| 4 | LeetCode #78 — Subsets | Medium | Subset enumeration via bitmask |
| 5 | LeetCode #46 — Permutations | Medium | Permutation enumeration via backtracking |

---

## 19. Memory Trick

> 🧠 **"BFT — Brute Force Then Transform"**
>
> Always write the Brute Force first, Then Transform it into the optimal solution. The transformation IS the pattern you're looking for.

---
---

# Pattern 2: Prefix Sum

```
═══════════════════════════════════════════════════════════════
  PREFIX SUM
  OA Frequency: ★★★★★
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Flipkart
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Imagine you're a cashier keeping a running total. Every time a customer buys something, you add to the total. If someone asks, "How much was spent between the 3rd and 7th customer?", you don't re-add items 3 through 7. You subtract: `total_after_7 - total_after_2`.

That's prefix sum.

**Why was this pattern invented?**

The brute force for "sum of subarray from index l to r" is O(r - l + 1) per query. If you have Q queries, it's O(n · Q). With prefix sum, you precompute all cumulative sums once (O(n)), and then answer each query in O(1). Total: O(n + Q).

It transforms a **repeated computation** problem into a **precomputation + lookup** problem.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem involve **subarray sums** or **range sums**?
- [ ] Are there **multiple queries** asking for sums of different ranges?
- [ ] Can the problem be reduced to "find a subarray with sum = k"?
- [ ] Does the problem involve **cumulative** properties (sum, XOR, product)?
- [ ] Would precomputing partial results speed up repeated calculations?

**If you checked 2+ of these → Prefix Sum is likely the right pattern.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points to Prefix Sum |
|---|---|
| "Subarray sum" | Direct application |
| "Range sum query" | Multiple queries on subarray sums |
| "Sum between index i and j" | prefix[j+1] - prefix[i] |
| "Count subarrays with sum = k" | Prefix sum + hash map |
| "Cumulative sum / running total" | Definition of prefix sum |
| "Sum of elements to the left/right" | prefix[i] or suffix[i] |
| "Equal partition" | Total sum / 2 with prefix scan |
| "Balance point" | prefix[i] == suffix[i] |

---

## 4. Constraint Analysis

| Constraint | Brute Force | With Prefix Sum | Verdict |
|---|---|---|---|
| n ≤ 1000, Q ≤ 1000 | O(n · Q) = 10⁶ ✅ | O(n + Q) ✅ | Both work, but prefix sum is cleaner |
| n ≤ 10⁵, Q ≤ 10⁵ | O(n · Q) = 10¹⁰ ❌ | O(n + Q) = 2 × 10⁵ ✅ | Prefix sum required |
| n ≤ 10⁶, single query | O(n) ✅ | O(n) ✅ | Prefix sum not needed for single query |

**Rule**: Prefix sum shines when you have **multiple range queries** or need to **convert subarray problems to point problems**.

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "To find the sum from index l to r, I'll loop from l to r and add everything."
> Time per query: O(n). Total for Q queries: O(n · Q).

### Step 2: Key Observation
> "Wait — the sum from l to r is actually `(sum of first r+1 elements) - (sum of first l elements)`. If I precompute all prefix sums, I can answer in O(1)."

### Step 3: Optimization
> Build an array `prefix[]` where `prefix[i] = nums[0] + nums[1] + ... + nums[i-1]`.
> Then `sum(l, r) = prefix[r+1] - prefix[l]`.

### Step 4: Optimal Solution
> Precompute: O(n). Each query: O(1). Total: O(n + Q).

---

## 6. Generic Algorithm

```
BUILD_PREFIX_SUM(nums):
    n = length(nums)
    prefix = array of size (n + 1), initialized to 0
    
    for i from 0 to n-1:
        prefix[i + 1] = prefix[i] + nums[i]
    
    return prefix

RANGE_SUM(prefix, l, r):
    return prefix[r + 1] - prefix[l]
```

**Why size n+1?** So that `prefix[0] = 0` (empty prefix), which makes the formula work cleanly for `l = 0`.

---

## 7. Reusable C++ Template

```cpp
// Template: Prefix Sum — Range Sum Queries
// Use when: multiple queries for sum of subarray [l, r]
// Time: O(n) build + O(1) per query, Space: O(n)

#include <vector>
using namespace std;

class PrefixSum {
    vector<long long> prefix;
public:
    // Build prefix sum from input array
    PrefixSum(const vector<int>& nums) {
        int n = nums.size();
        prefix.resize(n + 1, 0);
        for (int i = 0; i < n; i++) {
            prefix[i + 1] = prefix[i] + nums[i];
        }
    }

    // Query sum of nums[l..r] (inclusive)
    long long rangeSum(int l, int r) {
        return prefix[r + 1] - prefix[l];
    }
};

// ─── Usage ───────────────────────────────────────────────
// vector<int> nums = {1, 2, 3, 4, 5};
// PrefixSum ps(nums);
// ps.rangeSum(1, 3);  // sum of nums[1..3] = 2+3+4 = 9
```

---

## 8. Complexity Analysis

| Operation | Time | Space | Why |
|---|---|---|---|
| Build prefix array | O(n) | O(n) | Single pass through array |
| Answer one range query | O(1) | — | Simple subtraction |
| Answer Q range queries | O(Q) | — | O(1) each |
| **Total** | **O(n + Q)** | **O(n)** | Build once, query many |

**Why O(n) space?** We store the prefix array. In some problems, you can compute it in-place if you don't need the original array.

---

## 9. Dry Run

**Input**: `nums = [3, 1, 4, 1, 5]`

```
Step 1: Build prefix sum array
  prefix[0] = 0
  prefix[1] = 0 + 3 = 3
  prefix[2] = 3 + 1 = 4
  prefix[3] = 4 + 4 = 8
  prefix[4] = 8 + 1 = 9
  prefix[5] = 9 + 5 = 14

  Index:    0   1   2   3   4
  nums:   [ 3 | 1 | 4 | 1 | 5 ]

  Index:  0   1   2   3   4   5
  prefix: 0   3   4   8   9  14

Step 2: Query sum(1, 3) → sum of nums[1..3] = 1 + 4 + 1 = 6
  prefix[3+1] - prefix[1] = prefix[4] - prefix[1] = 9 - 3 = 6  ✓

Step 3: Query sum(0, 4) → sum of entire array = 14
  prefix[4+1] - prefix[0] = prefix[5] - prefix[0] = 14 - 0 = 14  ✓

Step 4: Query sum(2, 2) → single element nums[2] = 4
  prefix[2+1] - prefix[2] = prefix[3] - prefix[2] = 8 - 4 = 4  ✓
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Off-by-one: `prefix[r] - prefix[l]` instead of `prefix[r+1] - prefix[l]` | Use size `n+1` array; `prefix[i]` = sum of first `i` elements |
| Integer overflow: summing large values in `int` | Use `long long` for the prefix array |
| Forgetting `prefix[0] = 0` | Initialize the array with 0 at index 0 |
| Modifying the original array when building prefix sum | Use a separate array unless in-place is intentional |
| Not recognizing prefix sum in disguise (e.g., "subarray sum = k") | When you see "subarray" + "sum", think prefix sum |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Array elements change between queries | Prefix sum must be rebuilt — O(n) per update | Fenwick Tree or Segment Tree |
| You need range minimum/maximum, not sum | Prefix sum only works for associative operations with inverses | Sparse Table, Segment Tree |
| Single query on the entire array | Just sum directly — no need for prefix array | Simple loop |
| Problem asks for subarray product | Division doesn't work well with 0s | Use logarithms or sliding window |

---

## 12. Medium-Level Example Problem

**LeetCode #560 — Subarray Sum Equals K**

> Given an integer array `nums` and an integer `k`, return the total number of subarrays whose sum equals `k`.

**Why this represents the pattern well**: It combines prefix sum with hashing. You can't use a simple sliding window because elements can be negative. The prefix sum converts "subarray sum = k" into "find two prefix values that differ by k" — a classic pattern transformation.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. **Brute force**: Check all O(n²) subarrays, compute each sum → O(n²) or O(n³).
2. **Observation**: Sum of subarray `[l, r]` = `prefix[r+1] - prefix[l]`. We want this to equal `k`.
3. **Rearrange**: `prefix[r+1] - k = prefix[l]`. So for each `r`, we need to count how many earlier prefix values equal `prefix[r+1] - k`.
4. **This is a lookup problem!** Use a hash map to count occurrences of each prefix sum seen so far.
5. **Algorithm**: Walk through the array, maintaining a running prefix sum. At each step, check if `prefix_sum - k` exists in the hash map.

**Key insight**: Prefix sum transforms a "subarray sum" problem into a "find a complement" problem — just like Two Sum!

---

## 14. C++ Implementation

```cpp
#include <vector>
#include <unordered_map>
using namespace std;

// Count subarrays with sum exactly equal to k
// Pattern: Prefix Sum + Hash Map
// Time: O(n), Space: O(n)
int subarraySum(vector<int>& nums, int k) {
    unordered_map<int, int> prefixCount;
    prefixCount[0] = 1;  // empty prefix has sum 0 (occurs once)

    int currentSum = 0;
    int count = 0;

    for (int num : nums) {
        currentSum += num;  // running prefix sum

        // How many earlier prefixes have sum = currentSum - k?
        // If prefix[l] = currentSum - k, then sum(l, current) = k
        if (prefixCount.count(currentSum - k)) {
            count += prefixCount[currentSum - k];
        }

        // Record this prefix sum
        prefixCount[currentSum]++;
    }

    return count;
}
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "What if elements are all positive?" | Can use Variable Sliding Window instead — O(1) space |
| "Find the longest subarray with sum = k?" | Store first occurrence of each prefix sum instead of count |
| "What if we need sum divisible by k?" | Use `currentSum % k` as the key (handle negative mods) |
| "2D version: submatrix sum?" | Use 2D prefix sum: `prefix[i][j] = sum of submatrix (0,0) to (i-1,j-1)` |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **2D Prefix Sum** | `prefix[i][j]` stores sum of rectangle from (0,0) to (i-1,j-1). Query uses inclusion-exclusion. |
| **Prefix XOR** | Same idea but with XOR instead of sum. XOR is its own inverse. |
| **Prefix Product** | Use multiplication. Beware of zeros — can't divide by zero. |
| **Suffix Sum** | Build from right to left. Useful when you need "sum of elements to the right." |

**2D Prefix Sum Query Formula**:
```
sum(r1, c1, r2, c2) = prefix[r2+1][c2+1] 
                     - prefix[r1][c2+1] 
                     - prefix[r2+1][c1] 
                     + prefix[r1][c1]
```

---

## 17. Related Patterns

| Pattern | Relationship |
|---|---|
| **Difference Array** | "Inverse" of prefix sum — prefix sum of difference array = original array |
| **Sliding Window** | Both handle subarray queries; sliding window for optimization constraints, prefix sum for exact sums |
| **Hashing** | Prefix sum + hashing is a powerful combo for "subarray sum = k" |
| **Fenwick Tree** | Dynamic version of prefix sum that supports updates |
| **Segment Tree** | Generalized prefix sum for arbitrary associative operations |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #303 — Range Sum Query (Immutable) | Easy | Direct prefix sum application |
| 2 | LeetCode #724 — Find Pivot Index | Easy | Prefix sum = Suffix sum |
| 3 | LeetCode #560 — Subarray Sum Equals K | Medium | Prefix sum + hash map |
| 4 | LeetCode #304 — Range Sum Query 2D (Immutable) | Medium | 2D prefix sum |
| 5 | LeetCode #437 — Path Sum III | Hard | Prefix sum on a tree path |

---

## 19. Memory Trick

> 🧠 **"Prefix Sum = Running Total Receipt"**
>
> Think of it like a receipt at a grocery store. Each line shows the running total. To find how much you spent on items 3–7, subtract the total after item 2 from the total after item 7. You never re-scan the items.

---
---

# Pattern 3: Difference Array

```
═══════════════════════════════════════════════════════════════
  DIFFERENCE ARRAY
  OA Frequency: ★★★☆☆
  Companies: Google | Amazon | Microsoft | Uber | Codeforces favorites
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Imagine you're a teacher assigning bonus points. You say: "Students 3 through 7, you all get +5 points." Then: "Students 2 through 5, you all get +3 points." And so on, for many such updates.

The brute force would be: for each update, loop through the range and add the value. If you have Q updates on an array of size n, that's O(n · Q).

The difference array solves this in O(n + Q).

**Why was this pattern invented?**

Prefix sum answers range **queries** efficiently. Difference array handles range **updates** efficiently. They are duals of each other:

- Prefix sum of a difference array = original array
- Difference of a prefix sum array = original array

---

## 2. Pattern Identification Checklist

- [ ] Does the problem involve **adding a value to all elements in a range** `[l, r]`?
- [ ] Are there **multiple range update** operations?
- [ ] Do you only need the **final state** of the array (not intermediate states)?
- [ ] Can the problem be modeled as "increment a range by some value"?

**If you checked 2+ of these → Difference Array is likely the right pattern.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points to Difference Array |
|---|---|
| "Add k to all elements from index l to r" | Direct application |
| "Increment range" / "range update" | Core use case |
| "Number of overlapping intervals at each point" | Sweep line variant of difference array |
| "Bus/flight booking" / "how many passengers at each stop" | Classic problem |
| "Apply multiple operations, find final result" | Multiple range updates → difference array |

---

## 4. Constraint Analysis

| Constraint | Brute Force (per update) | Difference Array | Verdict |
|---|---|---|---|
| n ≤ 1000, Q ≤ 100 | O(n · Q) = 10⁵ ✅ | O(n + Q) ✅ | Both work |
| n ≤ 10⁵, Q ≤ 10⁵ | O(n · Q) = 10¹⁰ ❌ | O(n + Q) = 2 × 10⁵ ✅ | Difference array required |
| n ≤ 10⁶, Q ≤ 10⁶ | O(n · Q) = 10¹² ❌ | O(n + Q) = 2 × 10⁶ ✅ | Difference array required |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each update (l, r, val), loop from l to r and add val to each element."
> Time per update: O(n). Total: O(n · Q).

### Step 2: Key Observation
> "Adding val to range [l, r] is like saying: 'from index l onward, add val; from index r+1 onward, subtract val.' This way, when I compute the prefix sum, only positions l through r get the net +val."

### Step 3: Optimization
> Create a difference array. For update (l, r, val):
> - `diff[l] += val`
> - `diff[r + 1] -= val` (if r+1 < n)
>
> After all updates, take the prefix sum of diff[] to get the final array.

### Step 4: Optimal Solution
> Each update: O(1). Final reconstruction: O(n). Total: O(n + Q).

---

## 6. Generic Algorithm

```
INIT_DIFF(n):
    diff = array of size (n + 1), initialized to 0

RANGE_UPDATE(diff, l, r, val):
    diff[l] += val
    diff[r + 1] -= val

RECONSTRUCT(diff, n):
    result = array of size n
    running_sum = 0
    for i from 0 to n-1:
        running_sum += diff[i]
        result[i] = running_sum
    return result
```

---

## 7. Reusable C++ Template

```cpp
// Template: Difference Array — Range Updates + Final Reconstruction
// Use when: multiple "add val to range [l, r]" operations, query final array
// Time: O(1) per update + O(n) to reconstruct, Space: O(n)

#include <vector>
using namespace std;

class DifferenceArray {
    vector<long long> diff;
    int n;
public:
    DifferenceArray(int size) : n(size), diff(size + 1, 0) {}

    // Add val to all elements in range [l, r] (0-indexed, inclusive)
    void rangeUpdate(int l, int r, long long val) {
        diff[l] += val;
        if (r + 1 <= n) diff[r + 1] -= val;
    }

    // Reconstruct the final array after all updates
    vector<long long> reconstruct() {
        vector<long long> result(n);
        long long running = 0;
        for (int i = 0; i < n; i++) {
            running += diff[i];
            result[i] = running;
        }
        return result;
    }
};

// ─── Usage ───────────────────────────────────────────────
// DifferenceArray da(5);
// da.rangeUpdate(1, 3, 10);  // add 10 to indices 1,2,3
// da.rangeUpdate(2, 4, 5);   // add 5 to indices 2,3,4
// auto result = da.reconstruct();  // [0, 10, 15, 15, 5]
```

---

## 8. Complexity Analysis

| Operation | Time | Space | Why |
|---|---|---|---|
| One range update | O(1) | — | Two array writes |
| Q range updates | O(Q) | — | O(1) each |
| Reconstruction | O(n) | O(n) | Single prefix sum pass |
| **Total** | **O(n + Q)** | **O(n)** | |

---

## 9. Dry Run

**Operations** on array of size 5 (initially all zeros):
1. Add 2 to range [1, 3]
2. Add 3 to range [2, 4]
3. Add -1 to range [0, 2]

```
Initial diff: [0, 0, 0, 0, 0, 0]  (size n+1 = 6)

Operation 1: add 2 to [1, 3]
  diff[1] += 2, diff[4] -= 2
  diff: [0, 2, 0, 0, -2, 0]

Operation 2: add 3 to [2, 4]
  diff[2] += 3, diff[5] -= 3
  diff: [0, 2, 3, 0, -2, -3]

Operation 3: add -1 to [0, 2]
  diff[0] += -1, diff[3] -= -1
  diff: [-1, 2, 3, 1, -2, -3]

Reconstruction (prefix sum of diff):
  i=0: running = -1      result[0] = -1
  i=1: running = -1+2=1  result[1] = 1
  i=2: running = 1+3=4   result[2] = 4
  i=3: running = 4+1=5   result[3] = 5  (wait, let's verify)
  i=4: running = 5-2=3   result[4] = 3

Verification:
  Index 0: -1          (only op 3 applies: -1) ✓
  Index 1: +2 -1 = 1   (op 1 and op 3) ✓
  Index 2: +2 +3 -1 = 4 (all three ops) ✓
  Index 3: +2 +3 +1 = ?
  
  Wait — let me re-check operation 3: add -1 to [0, 2]
  Index 3: +2 (op1) + 3 (op2) = 5. Op 3 doesn't apply (range is [0,2]).
  
  diff[3] -= (-1) → diff[3] += 1. So running at index 3 = 4 + 1 = 5. ✓
  Index 4: +3 (op2 only) = 3. Running = 5 - 2 = 3. ✓
  
  Final result: [-1, 1, 4, 5, 3]  ✓
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Forgetting `diff[r+1] -= val` | Always do both operations — they're a pair |
| Array out of bounds when `r = n-1` | Make diff size `n+1` (extra element at end) |
| Using difference array when you need intermediate states | Difference array only gives the final result after ALL updates |
| Confusing with prefix sum | Prefix sum = range queries; Difference array = range updates |
| Integer overflow | Use `long long` when values can accumulate |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Need array state after EACH update (not just final) | Difference array only works for batch updates | Segment Tree with lazy propagation |
| Point updates + range queries | Wrong direction — this handles range updates + point queries | Fenwick Tree / Segment Tree |
| Need to query during updates | Must reconstruct first | Segment Tree |

---

## 12. Medium-Level Example Problem

**LeetCode #1109 — Corporate Flight Bookings**

> There are `n` flights labeled from 1 to n. You are given bookings where `bookings[i] = [first_i, last_i, seats_i]` represents a booking for every flight from `first_i` to `last_i` (inclusive) with `seats_i` seats reserved. Return an array of length n where answer[i] is the total seats reserved for flight i.

**Why this represents the pattern well**: It's literally "apply multiple range updates and return the final array" — a textbook difference array problem.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. **Read the problem**: Multiple range updates (add `seats` to range `[first, last]`), return final array.
2. **Brute force**: For each booking, loop from `first` to `last` and add seats. O(n · Q).
3. **Recognize the pattern**: "Multiple range additions, query final state" → Difference Array.
4. **Apply the pattern**: For each booking, `diff[first-1] += seats` and `diff[last] -= seats` (convert to 0-indexed).
5. **Reconstruct**: Prefix sum of diff gives the answer.

---

## 14. C++ Implementation

```cpp
#include <vector>
using namespace std;

// Corporate Flight Bookings
// Pattern: Difference Array
// Time: O(n + Q), Space: O(n)
vector<int> corpFlightBookings(vector<vector<int>>& bookings, int n) {
    vector<int> diff(n + 1, 0);  // difference array (size n+1 for safety)

    for (auto& booking : bookings) {
        int first = booking[0];  // 1-indexed
        int last = booking[1];   // 1-indexed
        int seats = booking[2];

        diff[first - 1] += seats;  // convert to 0-indexed
        if (last < n) {
            diff[last] -= seats;   // r+1 in 0-indexed = last (since last is 1-indexed)
        }
    }

    // Reconstruct: prefix sum
    vector<int> result(n);
    int running = 0;
    for (int i = 0; i < n; i++) {
        running += diff[i];
        result[i] = running;
    }

    return result;
}
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "What if bookings can be cancelled (subtract)?" | Same approach — use negative values |
| "What if you need to answer 'max seats on any flight'?" | After reconstruction, scan for max — still O(n + Q) |
| "What if flights are not 1 to n but arbitrary times?" | Use coordinate compression first, then difference array |
| "What if updates happen online (interleaved with queries)?" | Switch to Segment Tree with lazy propagation |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **2D Difference Array** | For adding val to a submatrix (r1,c1)→(r2,c2). Four updates per operation. |
| **Sweep Line** | Conceptually similar: +1 at event start, -1 at event end, prefix sum gives count at each point. |
| **Polynomial Difference Array** | For range updates of the form "add l, l+1, l+2, ..." (arithmetic progression). Use second-order differences. |

---

## 17. Related Patterns

| Pattern | Relationship |
|---|---|
| **Prefix Sum** | Dual: prefix sum handles range queries, difference array handles range updates |
| **Sweep Line** | Sweep line = difference array on event boundaries |
| **Segment Tree (Lazy)** | Generalization: handles range updates AND range queries dynamically |
| **Fenwick Tree** | Can simulate difference array with dynamic updates |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #1094 — Car Pooling | Easy | Range add/subtract for passengers |
| 2 | LeetCode #1109 — Corporate Flight Bookings | Medium | Direct difference array application |
| 3 | LeetCode #370 — Range Addition | Medium | Pure difference array (premium) |
| 4 | LeetCode #253 — Meeting Rooms II | Medium | Sweep line variant |
| 5 | Codeforces #295A — Greg and Array | Hard | Nested difference arrays |

---

## 19. Memory Trick

> 🧠 **"Bookends"**
>
> A difference array update is like placing bookends on a shelf. Put a +val bookend at the start of the range and a -val bookend at the end. When you sweep across the shelf (prefix sum), only the items between the bookends get the value.

---
---

# Pattern 4: Hashing

```
═══════════════════════════════════════════════════════════════
  HASHING
  OA Frequency: ★★★★★
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Goldman Sachs | Flipkart
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

The fundamental problem: **"Have I seen this value before?"** and **"How quickly can I look it up?"**

Without hashing, searching for an element in an unsorted array takes O(n). With a hash map/set, it takes O(1) on average.

**Why was this pattern invented?**

Many brute force solutions have an inner loop that's just "searching for something." If that search is O(n), the total is O(n²). Hashing reduces the search to O(1), making the total O(n).

Hashing converts **search problems into lookup problems**.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem require **checking if a value exists** in a collection?
- [ ] Do you need to **count occurrences** of values?
- [ ] Is the inner loop of your brute force essentially a **search/lookup**?
- [ ] Do you need to **map** values to indices, frequencies, or other values?
- [ ] Does the problem involve **finding duplicates**, complements, or pairs?
- [ ] Would a "dictionary" / "lookup table" make the problem trivial?

**If you checked 2+ of these → Hashing is likely the right pattern.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points to Hashing |
|---|---|
| "Contains duplicate" | Hash set for O(1) existence check |
| "Two sum" / "pair with sum k" | Hash map to find complement |
| "First non-repeating" | Hash map for frequency |
| "Anagram" / "permutation of string" | Hash map for character frequency |
| "Group by property" | Hash map with property as key |
| "Subarray sum equals k" | Prefix sum + hash map |
| "Longest substring without repeating" | Hash set/map for window tracking |
| "Isomorphic" / "mapping between elements" | Hash map for bijection |

---

## 4. Constraint Analysis

| Constraint | Without Hashing | With Hashing | Verdict |
|---|---|---|---|
| n ≤ 1000 | O(n²) = 10⁶ ✅ | O(n) ✅ | Both work |
| n ≤ 10⁵ | O(n²) = 10¹⁰ ❌ | O(n) ✅ | Hashing required |
| n ≤ 10⁶ | O(n²) = 10¹² ❌ | O(n) ✅ | Hashing required |

**Warning**: `unordered_map` has O(n) worst case due to hash collisions. In competitive programming, adversarial inputs can cause this. In interviews, average case O(1) is fine.

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each element, scan the rest of the array to find what I need." → O(n²).

### Step 2: Key Observation
> "The inner scan is just 'Does value X exist in my collection?' That's a lookup, not a computation."

### Step 3: Optimization
> "If I store elements in a hash set/map, lookups become O(1)."

### Step 4: Optimal Solution
> "Single pass: for each element, check the hash table, then add the element to the hash table." → O(n).

---

## 6. Generic Algorithm

```
HASH_LOOKUP_PATTERN(input):
    table = empty hash map or hash set
    
    for each element in input:
        key = compute_key(element)       // what to look up
        
        if key in table:
            process_match(element, table[key])  // found what we need
        
        table[element_key] = element_value  // store for future lookups
    
    return result
```

---

## 7. Reusable C++ Template

```cpp
// Template: Hash Map — Find Complement / Match
// Use when: for each element, check if a related value was seen before
// Time: O(n), Space: O(n)

#include <vector>
#include <unordered_map>
#include <unordered_set>
using namespace std;

// Pattern A: Hash Set — Existence check
bool hasDuplicate(vector<int>& nums) {
    unordered_set<int> seen;
    for (int num : nums) {
        if (seen.count(num)) return true;  // MODIFY: check condition
        seen.insert(num);                  // MODIFY: what to store
    }
    return false;
}

// Pattern B: Hash Map — Value lookup
vector<int> findComplement(vector<int>& nums, int target) {
    unordered_map<int, int> seen;  // value → index
    for (int i = 0; i < (int)nums.size(); i++) {
        int complement = target - nums[i];  // MODIFY: what to look for
        if (seen.count(complement)) {
            return {seen[complement], i};   // MODIFY: what to return
        }
        seen[nums[i]] = i;                 // MODIFY: what to store
    }
    return {};
}
```

---

## 8. Complexity Analysis

| Operation | Average | Worst Case | Why |
|---|---|---|---|
| Insert into hash table | O(1) | O(n) | Hash collision chains |
| Lookup in hash table | O(1) | O(n) | Hash collision chains |
| Delete from hash table | O(1) | O(n) | Hash collision chains |
| **Typical algorithm** | **O(n)** | **O(n²)** | n operations × O(1) avg each |
| Space | O(n) | O(n) | Storing up to n elements |

**For interviews**: Always state "O(1) average, O(n) worst case" for hash operations. Interviewers appreciate the distinction.

---

## 9. Dry Run

**Problem**: Find if array `[3, 1, 4, 1, 5]` contains a duplicate.

```
Hash Set: {}

i=0: num=3, not in set → add 3. Set: {3}
i=1: num=1, not in set → add 1. Set: {3, 1}
i=2: num=4, not in set → add 4. Set: {3, 1, 4}
i=3: num=1, IN SET ✓ → return true (duplicate found!)

Total operations: 4 lookups + 3 inserts = O(n)
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Using `map` instead of `unordered_map` | `map` is O(log n) per operation (red-black tree). Use `unordered_map` for O(1) average. |
| Not handling hash collisions in competitive programming | Use custom hash or `map` if TLE with `unordered_map` |
| Storing wrong key/value in the map | Clearly define: what's the key (what you look up) vs. value (what you store) |
| Modifying collection while iterating | Use a separate loop or copy |
| Forgetting to check `.count()` before accessing | Access with `.at()` or check first to avoid inserting default values |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Need ordered iteration | Hash tables don't maintain order | `map` (ordered) or sorting |
| Need range queries (elements between X and Y) | Hash tables don't support range queries | Balanced BST (`set`), Segment Tree |
| Very tight memory constraints | Hash tables have overhead (~2x space) | Sorting + binary search |
| Need to find min/max efficiently | Hash tables don't support this | Heap / sorted structure |

---

## 12. Medium-Level Example Problem

**LeetCode #49 — Group Anagrams**

> Given an array of strings `strs`, group the anagrams together. An anagram is a word formed by rearranging the letters.

**Why this represents the pattern well**: It requires designing a clever hash key — sorting each string to create a canonical form, then grouping by that key. It tests both the hashing pattern and key design skills.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. **What defines an anagram?** Two words with the same characters in the same frequencies.
2. **How to check if two words are anagrams?** Sort them — if they become the same string, they're anagrams.
3. **How to group efficiently?** Use a hash map where the key is the sorted string, and the value is a list of original strings.
4. **Algorithm**: For each string, sort it to get the key, add the original string to `map[key]`.

**Key insight**: The hash key doesn't have to be the original data. **Design the key to capture the property you're grouping by.**

---

## 14. C++ Implementation

```cpp
#include <vector>
#include <string>
#include <unordered_map>
#include <algorithm>
using namespace std;

// Group Anagrams
// Pattern: Hashing (with designed key)
// Time: O(n · k log k) where k = max string length, Space: O(n · k)
vector<vector<string>> groupAnagrams(vector<string>& strs) {
    unordered_map<string, vector<string>> groups;

    for (const string& s : strs) {
        string key = s;
        sort(key.begin(), key.end());  // canonical form: sorted characters
        groups[key].push_back(s);      // group by canonical form
    }

    // Collect all groups
    vector<vector<string>> result;
    for (auto& [key, group] : groups) {
        result.push_back(move(group));
    }

    return result;
}
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "Can you do it without sorting each string?" | Use character frequency as key: "a2b1c1" format → O(n·k) instead of O(n·k log k) |
| "What if strings are very long?" | Frequency counting is better than sorting for long strings |
| "What if we only need to check if two specific strings are anagrams?" | Just compare sorted versions or character counts — no hash map needed |
| "How would you handle Unicode characters?" | Use `unordered_map<char, int>` for frequency instead of fixed array |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Two Sum variants** | Hash map stores complement → index |
| **Frequency Map** | Count occurrences instead of just existence (see Pattern 5) |
| **Rolling Hash** | Compute hash value mathematically for substrings — O(1) update |
| **Hash Map of Hash Maps** | For problems requiring nested grouping |

---

## 17. Related Patterns

| Pattern | Relationship |
|---|---|
| **Frequency Counting** | Special case of hashing where value = count |
| **Prefix Sum + Hashing** | Combined pattern for subarray sum problems |
| **Sliding Window + Hashing** | Hash map/set tracks elements in the current window |
| **Two Pointers** | Both reduce O(n²) to O(n), but two pointers requires sorted input |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #217 — Contains Duplicate | Easy | Basic hash set |
| 2 | LeetCode #242 — Valid Anagram | Easy | Character frequency comparison |
| 3 | LeetCode #49 — Group Anagrams | Medium | Key design in hash map |
| 4 | LeetCode #128 — Longest Consecutive Sequence | Medium | Hash set for O(n) sequence finding |
| 5 | LeetCode #41 — First Missing Positive | Hard | In-place hashing (array as hash map) |

---

## 19. Memory Trick

> 🧠 **"Hash = Speed Dial"**
>
> A hash map is like your phone's speed dial. Instead of scrolling through all contacts (O(n) search), you press one button (O(1) lookup). The key is the speed dial number, the value is the contact. Design your speed dial (key) wisely.

---
---

# Pattern 5: Frequency Counting

```
═══════════════════════════════════════════════════════════════
  FREQUENCY COUNTING
  OA Frequency: ★★★★★
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Flipkart | Goldman Sachs
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"How many times does each element appear?"

This sounds trivial, but an enormous number of interview problems boil down to frequency counting. Anagrams? Frequency counting. "Most frequent element"? Frequency counting. "Minimum deletions to make equal"? Frequency counting.

**Why was this pattern invented?**

When you need to make decisions based on **how often** elements occur (not just whether they exist), you need frequency counting. It's a specialized form of hashing where the value is always a count.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem mention "frequency," "count," or "occurrences"?
- [ ] Do you need to find the **most/least common** element?
- [ ] Does the problem involve **anagrams** or **character rearrangement**?
- [ ] Does "make all elements equal" or "minimum operations" appear?
- [ ] Would knowing "how many of each" solve the problem immediately?

**If you checked 2+ of these → Frequency Counting is likely the right pattern.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points to Frequency Counting |
|---|---|
| "Most frequent element" | Direct frequency query |
| "Top K frequent" | Frequency + sorting/heap |
| "Anagram" | Compare character frequencies |
| "Majority element" | Element with count > n/2 |
| "Minimum deletions to make unique" | Analyze the frequency distribution |
| "Rearrange string so no two adjacent are same" | Frequency-based greedy |
| "Check if permutation" | Same frequency profile |
| "Frequency of frequency" | Meta-counting (count how many elements appear k times) |

---

## 4. Constraint Analysis

| Constraint | Approach | Why |
|---|---|---|
| Values in range [0, 1000] | Use array `int freq[1001]` | Faster than hash map |
| Values can be any integer | Use `unordered_map<int, int>` | Handles arbitrary keys |
| Characters (lowercase a-z) | Use array `int freq[26]` | Most common in interviews |
| Values up to 10⁶ | Use array if memory allows, else hash map | Array is faster |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each unique element, count how many times it appears by scanning the whole array." → O(n²).

### Step 2: Key Observation
> "I can count all frequencies in a single pass using a hash map."

### Step 3: Optimization
> Walk through the array once. For each element, increment its count in the map.

### Step 4: Optimal Solution
> O(n) time, O(k) space where k = number of distinct elements.

---

## 6. Generic Algorithm

```
FREQUENCY_COUNT(input):
    freq = empty hash map
    
    for each element in input:
        freq[element] += 1
    
    return freq
```

Common post-processing:
- **Find max frequency**: Scan the map for the highest value
- **Find element with frequency k**: Filter the map
- **Sort by frequency**: Convert map to vector of pairs, sort by second
- **Frequency of frequencies**: Build a second map from the first

---

## 7. Reusable C++ Template

```cpp
// Template: Frequency Counting
// Use when: need to count occurrences of elements
// Time: O(n), Space: O(k) where k = distinct elements

#include <vector>
#include <unordered_map>
#include <string>
using namespace std;

// Pattern A: Count integers
unordered_map<int, int> countFrequencies(vector<int>& nums) {
    unordered_map<int, int> freq;
    for (int num : nums) {
        freq[num]++;
    }
    return freq;
}

// Pattern B: Count characters (when only lowercase letters)
// Faster than hash map for small alphabets
void countChars(const string& s, int freq[26]) {
    fill(freq, freq + 26, 0);  // initialize to 0
    for (char c : s) {
        freq[c - 'a']++;       // MODIFY: c - 'A' for uppercase
    }
}

// Pattern C: Find most frequent element
int mostFrequent(vector<int>& nums) {
    unordered_map<int, int> freq;
    for (int num : nums) freq[num]++;

    int maxFreq = 0, result = 0;
    for (auto& [val, count] : freq) {
        if (count > maxFreq) {
            maxFreq = count;
            result = val;
        }
    }
    return result;
}
```

---

## 8. Complexity Analysis

| Operation | Time | Space | Why |
|---|---|---|---|
| Build frequency map | O(n) | O(k) | One pass, k distinct elements |
| Find max frequency | O(k) | O(1) | Scan the map |
| Sort by frequency | O(k log k) | O(k) | Sort the entries |
| **Typical total** | **O(n)** | **O(k)** | Building the map dominates |

---

## 9. Dry Run

**Problem**: Find the most frequent element in `[1, 3, 2, 1, 4, 1, 3]`.

```
Walk through array, building frequency map:

i=0: num=1 → freq={1:1}
i=1: num=3 → freq={1:1, 3:1}
i=2: num=2 → freq={1:1, 3:1, 2:1}
i=3: num=1 → freq={1:2, 3:1, 2:1}
i=4: num=4 → freq={1:2, 3:1, 2:1, 4:1}
i=5: num=1 → freq={1:3, 3:1, 2:1, 4:1}
i=6: num=3 → freq={1:3, 3:2, 2:1, 4:1}

Scan for max: 1 appears 3 times → answer = 1
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Using `freq[c]` without initializing (in arrays) | Always `fill()` or `memset()` to 0 first |
| Off-by-one with character indexing (`c - 'a'`) | Verify: 'a'-'a'=0, 'z'-'a'=25 → array size 26 |
| Not considering ties (two elements with same max frequency) | Define the tiebreaker: smallest value? first occurrence? |
| Using `map` instead of `unordered_map` | `map` is O(log n) per operation; use `unordered_map` for O(1) avg |
| Forgetting that frequency counting is the sub-step, not the full solution | Usually you count frequencies THEN do something with them |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Need to maintain order of first occurrence | `unordered_map` doesn't preserve insertion order | Use `map` or a vector of pairs |
| Need to find median / kth element | Frequency alone doesn't help with order statistics | Sort or heap |
| Counting over a range (not the entire array) | Need per-range counting | Segment Tree or Merge Sort Tree |
| Data is streaming with deletions | Simple frequency map doesn't handle efficient deletions | Multiset or augmented structure |

---

## 12. Medium-Level Example Problem

**LeetCode #347 — Top K Frequent Elements**

> Given an integer array `nums` and an integer `k`, return the `k` most frequent elements.

**Why this represents the pattern well**: It combines frequency counting with a follow-up question: "Now that you have the frequencies, how do you efficiently find the top k?" This naturally leads to discussing heaps, bucket sort, or quickselect.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. **Step 1**: Count frequencies → `unordered_map<int, int>`.
2. **Step 2**: How to find top k by frequency?
   - **Option A**: Sort all entries by frequency → O(n log n)
   - **Option B**: Use a min-heap of size k → O(n log k)
   - **Option C**: Bucket sort (bucket by frequency) → O(n)
3. **Best approach for interviews**: Bucket sort is O(n) and impressive.

**Bucket sort insight**: Create buckets where `bucket[i]` = list of elements with frequency `i`. Max frequency is `n`. Walk buckets from high to low, collecting elements until you have k.

---

## 14. C++ Implementation

```cpp
#include <vector>
#include <unordered_map>
using namespace std;

// Top K Frequent Elements
// Pattern: Frequency Counting + Bucket Sort
// Time: O(n), Space: O(n)
vector<int> topKFrequent(vector<int>& nums, int k) {
    // Step 1: Count frequencies
    unordered_map<int, int> freq;
    for (int num : nums) {
        freq[num]++;
    }

    // Step 2: Bucket sort by frequency
    // bucket[i] = list of elements that appear exactly i times
    int n = nums.size();
    vector<vector<int>> buckets(n + 1);
    for (auto& [val, count] : freq) {
        buckets[count].push_back(val);
    }

    // Step 3: Collect top k from highest frequency
    vector<int> result;
    for (int i = n; i >= 1 && (int)result.size() < k; i--) {
        for (int val : buckets[i]) {
            result.push_back(val);
            if ((int)result.size() == k) break;
        }
    }

    return result;
}
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "What if k = 1?" | Just find the element with max frequency — no need for bucket sort |
| "What if elements are streaming?" | Use a min-heap of size k; when a frequency changes, update the heap |
| "Return top k in sorted order of frequency?" | After bucket sort, results are naturally in decreasing frequency |
| "What if there are ties at the kth position?" | Clarify with interviewer — return any k, or all tied elements? |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Frequency of frequencies** | Build a second frequency map from the first. E.g., "How many elements appear exactly 3 times?" |
| **Sort by frequency** | Sort elements by their frequency (descending), breaking ties by value |
| **Frequency-based filtering** | "Remove all elements that appear less than k times" |
| **Sliding window frequency** | Maintain a frequency map for the current window (add entering element, remove exiting) |

---

## 17. Related Patterns

| Pattern | Relationship |
|---|---|
| **Hashing** | Frequency counting IS hashing with counts as values |
| **Heap** | Used after frequency counting for "top K" problems |
| **Sliding Window** | Maintains a dynamic frequency map as the window moves |
| **Bucket Sort** | Natural follow-up when you need to sort by frequency |
| **Boyer-Moore Voting** | Special case: finding the majority element (freq > n/2) in O(1) space |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #169 — Majority Element | Easy | Boyer-Moore or frequency counting |
| 2 | LeetCode #387 — First Unique Character in a String | Easy | Frequency array + scan |
| 3 | LeetCode #347 — Top K Frequent Elements | Medium | Frequency + bucket sort/heap |
| 4 | LeetCode #451 — Sort Characters by Frequency | Medium | Frequency + sorting |
| 5 | LeetCode #895 — Maximum Frequency Stack | Hard | Frequency tracking with stack |

---

## 19. Memory Trick

> 🧠 **"The Census Taker"**
>
> Frequency counting is like taking a census: walk through the population once, tally each type in a table. Once the census is done, you can answer any question about the population distribution instantly. The walk is O(n), every subsequent query is O(1).

---
---

*Previous chapter: [01 — Pattern Recognition Flowchart](01_Pattern_Recognition.md)*
*Next chapter: [03 — Sliding Window & Pointers](03_Sliding_Window_and_Pointers.md)*
