<!--
═══════════════════════════════════════════════════════════════════════════════
  DSA PATTERN HANDBOOK — DYNAMIC PROGRAMMING
  Chapter 14
  
  Patterns in this chapter:
    Pattern 42: DP Introduction & Framework
    Pattern 43: 1D DP (Linear)
    Pattern 44: 2D DP (Grid/Two-Sequence)
    Pattern 45: Knapsack Variants
    Pattern 46: Longest Increasing Subsequence
    Pattern 47: Digit DP* (Advanced)
    Pattern 48: State Compression DP* (Bitmask DP)
  
  Formatting Rules (internal reference for consistency):
  - Heading hierarchy: # (chapter) → ## (pattern) → ### (section)
  - 20 sections per pattern
  - Star rating: ★ (filled), ☆ (empty), scale 1–5
  - Code style: C++17, 4-space indent, STL preferred
  - Tables: GitHub-flavored markdown pipe tables
  - Problem references: "LeetCode #ID — Name" format
═══════════════════════════════════════════════════════════════════════════════
-->

# Chapter 14: Dynamic Programming

Dynamic Programming (DP) is the most feared pattern in interviews — and the most tested one. This chapter provides a systematic framework for tackling any DP problem.

**Core idea**: DP = Recursion + Memoization. If you can write a recursive solution with overlapping subproblems, you can convert it to DP.

---
---

# Pattern 42: DP Introduction & Framework

```
═══════════════════════════════════════════════════════════════
  DP INTRODUCTION & FRAMEWORK
  OA Frequency: ★★★★★
  Companies: ALL major companies
═══════════════════════════════════════════════════════════════
```

---

## 1. When to Use DP

**DP solves problems with two properties:**

| Property | Meaning | Test |
|---|---|---|
| **Optimal Substructure** | Optimal solution uses optimal sub-solutions | Can you define the answer in terms of smaller answers? |
| **Overlapping Subproblems** | Same subproblems appear multiple times | Does recursion recompute the same states? |

**Quick check**: If brute force involves "choose or skip" decisions with exponential branching AND subproblems repeat, it's DP.

---

## 2. The DP Framework (4 Steps)

### Step 1: Define the State
> "What information do I need to uniquely describe a subproblem?"
>
> This becomes `dp[i]`, `dp[i][j]`, `dp[i][j][k]`, etc.

### Step 2: Write the Recurrence
> "How does `dp[i]` relate to smaller states?"
>
> This is the transition formula.

### Step 3: Identify Base Cases
> "What are the trivially solvable subproblems?"
>
> These initialize the DP table.

### Step 4: Determine Iteration Order
> "In what order should I fill the table so that dependencies are already computed?"
>
> Usually left-to-right, bottom-up, or diagonally.

---

## 3. Top-Down vs Bottom-Up

| Approach | How | Pros | Cons |
|---|---|---|---|
| **Top-Down (Memoization)** | Recursion + cache | Natural to write; only computes needed states | Stack overflow risk; overhead |
| **Bottom-Up (Tabulation)** | Iterative table filling | No recursion stack; can optimize space | Must figure out fill order; may compute unnecessary states |

**Start with top-down** (easier to derive), then convert to bottom-up if needed.

---

## 4. DP vs Greedy

| Aspect | DP | Greedy |
|---|---|---|
| Makes choice | Considers ALL options | Picks the BEST local option |
| When correct | Always (if formulated correctly) | Only when greedy choice property holds |
| Time | Usually O(n²) or O(n·W) | Usually O(n) or O(n log n) |
| Use when | "Can't decide without seeing future" | "Local best = global best" |

---

## 5. Common DP State Patterns

| State | Meaning | Examples |
|---|---|---|
| `dp[i]` | Answer for first i elements | Fibonacci, House Robber, Climbing Stairs |
| `dp[i][j]` | Answer for subarray [i..j] or using first i items with capacity j | Knapsack, Matrix Chain, Palindrome |
| `dp[i][j]` | Answer for matching first i chars of string A and first j of string B | LCS, Edit Distance |
| `dp[mask]` | Answer for subset represented by bitmask | TSP, Partition into subsets |
| `dp[i][0/1]` | Answer at position i with binary state (bought/sold, taken/not) | Stock problems |

---

## 6. Memory Trick

> 🧠 **"DP = Smart Brute Force"**
>
> Brute force tries all combinations. DP says: "I've already computed this combination before — let me reuse the answer." It's not a new algorithm — it's recursion that remembers.
>
> **The 4-Step Mantra**: "State → Recurrence → Base → Order."

---
---

# Pattern 43: 1D DP (Linear DP)

```
═══════════════════════════════════════════════════════════════
  1D DP (LINEAR)
  OA Frequency: ★★★★★
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Goldman Sachs | Flipkart
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Make decisions for each element in a sequence, where each decision depends on previous decisions." The DP state is a single index: `dp[i]` = answer considering the first i elements.

---

## 2. Pattern Identification Checklist

- [ ] Is the input a **1D array or sequence**?
- [ ] Does the answer at position i depend on answers at positions i-1, i-2, etc.?
- [ ] Does the problem involve **choosing or skipping** elements in order?
- [ ] Can you define `dp[i]` as "best answer using the first i elements"?

---

## 3. Recognition Keywords

| Keyword / Phrase | Example |
|---|---|
| "Climbing stairs" | dp[i] = dp[i-1] + dp[i-2] |
| "House robber" | Choose or skip each house |
| "Maximum subarray" (Kadane's) | dp[i] = max(nums[i], dp[i-1] + nums[i]) |
| "Decode ways" | dp[i] = dp[i-1] + dp[i-2] with conditions |
| "Word break" | dp[i] = any dp[j] where substring[j..i] is valid |
| "Coin change" | dp[i] = min over all coins(dp[i-coin] + 1) |

---

## 4. Evolution of Thought

### Example: House Robber

> "Rob houses along a street. Can't rob two adjacent. Maximize loot."

**Step 1: Brute Force**
> Try all subsets of houses where no two are adjacent → O(2ⁿ).

**Step 2: Define State**
> `dp[i]` = maximum loot considering houses 0..i.

**Step 3: Recurrence**
> At house i, two choices:
> - **Rob house i**: gain `nums[i]`, can't use i-1 → `dp[i-2] + nums[i]`
> - **Skip house i**: keep `dp[i-1]`
> `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`

**Step 4: Base Cases**
> `dp[0] = nums[0]`, `dp[1] = max(nums[0], nums[1])`

---

## 5. Reusable C++ Templates

```cpp
// Template A: 1D DP — Choose or Skip (House Robber)
// Time: O(n), Space: O(1) with optimization

#include <vector>
#include <algorithm>
using namespace std;

int houseRobber(vector<int>& nums) {
    if (nums.empty()) return 0;
    if (nums.size() == 1) return nums[0];

    int prev2 = nums[0];
    int prev1 = max(nums[0], nums[1]);

    for (int i = 2; i < (int)nums.size(); i++) {
        int curr = max(prev1, prev2 + nums[i]);
        prev2 = prev1;
        prev1 = curr;
    }

    return prev1;
}
```

```cpp
// Template B: 1D DP — Kadane's Algorithm (Max Subarray Sum)
// Time: O(n), Space: O(1)

int maxSubArray(vector<int>& nums) {
    int maxSum = nums[0];
    int currentSum = nums[0];

    for (int i = 1; i < (int)nums.size(); i++) {
        currentSum = max(nums[i], currentSum + nums[i]);
        maxSum = max(maxSum, currentSum);
    }

    return maxSum;
}
```

```cpp
// Template C: 1D DP — Coin Change (Unbounded)
// Time: O(n × amount), Space: O(amount)

int coinChange(vector<int>& coins, int amount) {
    vector<int> dp(amount + 1, amount + 1);  // initialize to "impossible"
    dp[0] = 0;

    for (int i = 1; i <= amount; i++) {
        for (int coin : coins) {
            if (coin <= i && dp[i - coin] != amount + 1) {
                dp[i] = min(dp[i], dp[i - coin] + 1);
            }
        }
    }

    return dp[amount] > amount ? -1 : dp[amount];
}
```

---

## 6. Space Optimization

Many 1D DP problems only use `dp[i-1]` and `dp[i-2]`. Replace the array with two variables:

```
dp[i] = f(dp[i-1], dp[i-2])

→ prev2 = dp[i-2]
  prev1 = dp[i-1]
  curr  = f(prev1, prev2)
  prev2 = prev1
  prev1 = curr
```

This reduces O(n) space to **O(1)**.

---

## 7. Dry Run

**House Robber**: `nums = [2, 7, 9, 3, 1]`

```
dp[0] = 2
dp[1] = max(2, 7) = 7
dp[2] = max(dp[1], dp[0]+9) = max(7, 11) = 11      (rob 2, 9)
dp[3] = max(dp[2], dp[1]+3) = max(11, 10) = 11      (skip 3)
dp[4] = max(dp[3], dp[2]+1) = max(11, 12) = 12      (rob 2, 9, 1)

Answer: 12 (rob houses 0, 2, 4: 2+9+1=12) ✓
```

---

## 8. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Wrong base case | Test with n=0, n=1, n=2 manually |
| Off-by-one in indexing | Be clear: is dp[i] about element i or first i elements? |
| Not considering the "skip" option | Most 1D DP involves "take it or leave it" |
| Forgetting space optimization | If only prev values needed, use O(1) space |

---

## 9. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #70 — Climbing Stairs | Easy | dp[i] = dp[i-1] + dp[i-2] |
| 2 | LeetCode #198 — House Robber | Medium | Choose or skip |
| 3 | LeetCode #53 — Maximum Subarray | Medium | Kadane's algorithm |
| 4 | LeetCode #322 — Coin Change | Medium | Unbounded DP |
| 5 | LeetCode #139 — Word Break | Medium | String DP with dictionary |

---
---

# Pattern 44: 2D DP (Grid / Two-Sequence)

```
═══════════════════════════════════════════════════════════════
  2D DP (GRID / TWO-SEQUENCE)
  OA Frequency: ★★★★★
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Goldman Sachs
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Two major forms:

1. **Grid DP**: "Find the minimum/maximum cost path in a 2D grid."
   - State: `dp[i][j]` = answer at cell (i, j).
   - Recurrence: `dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])`.

2. **Two-Sequence DP**: "Compare two strings/sequences."
   - State: `dp[i][j]` = answer for first i chars of string A and first j chars of string B.
   - Classic problems: LCS, Edit Distance.

---

## 2. Pattern Identification Checklist

- [ ] Is the input a **2D grid** with path-finding?
- [ ] Are there **two strings/sequences** to compare?
- [ ] Does the problem involve **matching, aligning, or transforming** one sequence into another?
- [ ] Can you define `dp[i][j]` meaningfully?

---

## 3. Recognition Keywords

| Keyword / Phrase | Sub-Type |
|---|---|
| "Minimum path sum in grid" | Grid DP |
| "Unique paths" | Grid DP (counting) |
| "Longest common subsequence" | Two-sequence DP |
| "Edit distance" | Two-sequence DP |
| "Interleaving strings" | Two-sequence DP |
| "Regular expression matching" | Two-sequence DP |

---

## 4. Reusable C++ Templates

```cpp
// Template A: Grid DP — Minimum Path Sum
// Time: O(m × n), Space: O(n) with optimization

#include <vector>
#include <algorithm>
using namespace std;

int minPathSum(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    vector<vector<int>> dp(m, vector<int>(n, 0));
    
    dp[0][0] = grid[0][0];
    
    // First row
    for (int j = 1; j < n; j++)
        dp[0][j] = dp[0][j-1] + grid[0][j];
    
    // First column
    for (int i = 1; i < m; i++)
        dp[i][0] = dp[i-1][0] + grid[i][0];
    
    // Fill rest
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1]);
        }
    }
    
    return dp[m-1][n-1];
}
```

```cpp
// Template B: Two-Sequence DP — Longest Common Subsequence
// Time: O(m × n), Space: O(m × n) [O(n) with optimization]

int longestCommonSubsequence(string text1, string text2) {
    int m = text1.size(), n = text2.size();
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (text1[i-1] == text2[j-1]) {
                dp[i][j] = dp[i-1][j-1] + 1;     // match → extend
            } else {
                dp[i][j] = max(dp[i-1][j], dp[i][j-1]);  // skip one
            }
        }
    }
    
    return dp[m][n];
}
```

```cpp
// Template C: Two-Sequence DP — Edit Distance
// Time: O(m × n), Space: O(m × n)

int editDistance(string word1, string word2) {
    int m = word1.size(), n = word2.size();
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    
    // Base cases
    for (int i = 0; i <= m; i++) dp[i][0] = i;  // delete all
    for (int j = 0; j <= n; j++) dp[0][j] = j;  // insert all
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (word1[i-1] == word2[j-1]) {
                dp[i][j] = dp[i-1][j-1];          // no operation needed
            } else {
                dp[i][j] = 1 + min({dp[i-1][j],    // delete
                                    dp[i][j-1],    // insert
                                    dp[i-1][j-1]}); // replace
            }
        }
    }
    
    return dp[m][n];
}
```

---

## 5. Dry Run — LCS

**LCS("abcde", "ace")**

```
       ""  a  c  e
  ""  [ 0  0  0  0 ]
   a  [ 0  1  1  1 ]    a==a → dp[1][1] = dp[0][0]+1 = 1
   b  [ 0  1  1  1 ]    b≠a,c,e → max of neighbors
   c  [ 0  1  2  2 ]    c==c → dp[3][2] = dp[2][1]+1 = 2
   d  [ 0  1  2  2 ]    d≠a,c,e → max of neighbors
   e  [ 0  1  2  3 ]    e==e → dp[5][3] = dp[4][2]+1 = 3

Answer: LCS = 3 ("ace") ✓
```

---

## 6. Space Optimization for 2D DP

When `dp[i][j]` only depends on row `i` and row `i-1`, keep only two rows:

```cpp
// O(n) space for grid DP / LCS
vector<int> prev(n+1, 0), curr(n+1, 0);
for (int i = 1; i <= m; i++) {
    for (int j = 1; j <= n; j++) {
        // use prev[j], prev[j-1], curr[j-1] instead of dp[i-1][j], dp[i-1][j-1], dp[i][j-1]
    }
    swap(prev, curr);
}
```

---

## 7. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #62 — Unique Paths | Medium | Grid counting DP |
| 2 | LeetCode #64 — Minimum Path Sum | Medium | Grid optimization DP |
| 3 | LeetCode #1143 — Longest Common Subsequence | Medium | Classic two-sequence |
| 4 | LeetCode #72 — Edit Distance | Medium | Three-operation recurrence |
| 5 | LeetCode #10 — Regular Expression Matching | Hard | Complex two-sequence |

---
---

# Pattern 45: Knapsack Variants

```
═══════════════════════════════════════════════════════════════
  KNAPSACK VARIANTS
  OA Frequency: ★★★★★
  Companies: Amazon | Google | Microsoft | Goldman Sachs | Flipkart
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Given items with costs and values, and a capacity constraint, maximize value (or count ways, or check feasibility)." The knapsack is the quintessential DP problem because the greedy approach fails — you must consider all combinations.

---

## 2. The Three Knapsack Types

| Type | Item Reuse | State | Example |
|---|---|---|---|
| **0/1 Knapsack** | Each item once | `dp[i][w]` | Subset Sum, Partition Equal |
| **Unbounded Knapsack** | Unlimited reuse | `dp[w]` (1D) | Coin Change, Rod Cutting |
| **Bounded Knapsack** | Limited copies per item | `dp[i][w]` with count | Specific item limits |

---

## 3. Recognition Keywords

| Keyword / Phrase | Knapsack Type |
|---|---|
| "Subset sum" / "can you form sum X?" | 0/1 Knapsack (feasibility) |
| "Partition into two equal subsets" | 0/1 Knapsack (target = totalSum/2) |
| "Coin change — minimum coins" | Unbounded Knapsack (optimization) |
| "Coin change — number of ways" | Unbounded Knapsack (counting) |
| "Target sum with +/-" | 0/1 Knapsack (transformed) |
| "Rod cutting" | Unbounded Knapsack |

---

## 4. Reusable C++ Templates

```cpp
// Template A: 0/1 Knapsack
// Time: O(n × W), Space: O(W) with 1D optimization

// Can we form a subset with sum = target?
bool subsetSum(vector<int>& nums, int target) {
    vector<bool> dp(target + 1, false);
    dp[0] = true;

    for (int num : nums) {
        // REVERSE ORDER: ensures each item used at most once
        for (int w = target; w >= num; w--) {
            dp[w] = dp[w] || dp[w - num];
        }
    }

    return dp[target];
}

// Max value with weight capacity
int knapsack01(vector<int>& weights, vector<int>& values, int W) {
    vector<int> dp(W + 1, 0);

    for (int i = 0; i < (int)weights.size(); i++) {
        for (int w = W; w >= weights[i]; w--) {  // REVERSE
            dp[w] = max(dp[w], dp[w - weights[i]] + values[i]);
        }
    }

    return dp[W];
}
```

```cpp
// Template B: Unbounded Knapsack
// Time: O(n × W), Space: O(W)

// Minimum coins to make amount
int coinChange(vector<int>& coins, int amount) {
    vector<int> dp(amount + 1, amount + 1);
    dp[0] = 0;

    for (int i = 1; i <= amount; i++) {
        for (int coin : coins) {
            if (coin <= i && dp[i - coin] != amount + 1) {
                dp[i] = min(dp[i], dp[i - coin] + 1);
            }
        }
    }

    return dp[amount] > amount ? -1 : dp[amount];
}

// Number of ways to make amount
int coinChangeWays(vector<int>& coins, int amount) {
    vector<int> dp(amount + 1, 0);
    dp[0] = 1;

    for (int coin : coins) {          // iterate coins in outer loop to avoid duplicates
        for (int w = coin; w <= amount; w++) {  // FORWARD ORDER: unlimited reuse
            dp[w] += dp[w - coin];
        }
    }

    return dp[amount];
}
```

---

## 5. Critical Difference: Loop Order

| 0/1 Knapsack | Unbounded Knapsack |
|---|---|
| Inner loop: **reverse** (target → 0) | Inner loop: **forward** (0 → target) |
| Ensures each item used once | Allows reusing items |
| Outer: items, Inner: capacity ↓ | Can vary, but forward allows reuse |

**Why reverse for 0/1?** Processing in reverse ensures `dp[w - weight[i]]` hasn't been updated with item `i` yet in this iteration. Forward would let item `i` be used multiple times.

---

## 6. Dry Run — 0/1 Knapsack

**Subset Sum**: Can `[3, 1, 5, 2]` form sum 6?

```
dp = [T, F, F, F, F, F, F]   (dp[0] = true)

num=3: w=6→3 (reverse)
  dp[6] = dp[6]||dp[3] = F||F = F
  dp[5] = dp[5]||dp[2] = F||F = F
  dp[4] = dp[4]||dp[1] = F||F = F
  dp[3] = dp[3]||dp[0] = F||T = T
dp = [T, F, F, T, F, F, F]

num=1: w=6→1
  dp[4] = dp[4]||dp[3] = F||T = T
  dp[1] = dp[1]||dp[0] = F||T = T
dp = [T, T, F, T, T, F, F]

num=5: w=6→5
  dp[6] = dp[6]||dp[1] = F||T = T  ← Found!
dp = [T, T, F, T, T, F, T]

dp[6] = true → Yes, subset sum 6 exists (3+1+2 or 1+5) ✓
```

---

## 7. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #416 — Partition Equal Subset Sum | Medium | 0/1 Knapsack: target = sum/2 |
| 2 | LeetCode #322 — Coin Change | Medium | Unbounded: min coins |
| 3 | LeetCode #518 — Coin Change II | Medium | Unbounded: count ways |
| 4 | LeetCode #494 — Target Sum | Medium | 0/1 Knapsack transform |
| 5 | LeetCode #474 — Ones and Zeroes | Medium | 2D 0/1 Knapsack |

**Memory Trick:**

> 🧠 **"The Suitcase Packer"** — 0/1 Knapsack: You're packing a suitcase. Each item can be taken once. Reverse iteration = "I haven't decided about this item yet." Unbounded: It's a store — buy as many copies as you want. Forward iteration = "I can buy another one."

---
---

# Pattern 46: Longest Increasing Subsequence (LIS)

```
═══════════════════════════════════════════════════════════════
  LONGEST INCREASING SUBSEQUENCE
  OA Frequency: ★★★★☆
  Companies: Google | Amazon | Microsoft | Meta | Goldman Sachs
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find the longest subsequence where elements are strictly increasing." This is a classic DP problem with an elegant O(n log n) optimization using binary search.

---

## 2. Two Approaches

| Approach | Time | Space | How |
|---|---|---|---|
| **DP** | O(n²) | O(n) | `dp[i] = max(dp[j] + 1)` for all `j < i` where `nums[j] < nums[i]` |
| **Patience Sorting** | O(n log n) | O(n) | Maintain sorted "tails" array, binary search for insertion point |

---

## 3. Reusable C++ Templates

```cpp
// Template A: LIS — O(n²) DP
// dp[i] = length of LIS ending at index i

int lisDP(vector<int>& nums) {
    int n = nums.size();
    vector<int> dp(n, 1);  // each element is a LIS of length 1
    int maxLen = 1;

    for (int i = 1; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[j] < nums[i]) {
                dp[i] = max(dp[i], dp[j] + 1);
            }
        }
        maxLen = max(maxLen, dp[i]);
    }

    return maxLen;
}
```

```cpp
// Template B: LIS — O(n log n) with Binary Search
// tails[i] = smallest ending element of all increasing subsequences of length i+1

#include <algorithm>

int lisOptimal(vector<int>& nums) {
    vector<int> tails;  // sorted array of "tails"

    for (int num : nums) {
        auto it = lower_bound(tails.begin(), tails.end(), num);
        if (it == tails.end()) {
            tails.push_back(num);    // extend longest subsequence
        } else {
            *it = num;               // replace with smaller tail
        }
    }

    return tails.size();
}
```

---

## 4. Why O(n log n) Works

```
nums = [10, 9, 2, 5, 3, 7, 101, 18]

Process each number:
  10 → tails: [10]
   9 → 9 < 10, replace → tails: [9]
   2 → 2 < 9, replace → tails: [2]
   5 → 5 > 2, append → tails: [2, 5]
   3 → 3 > 2, 3 < 5, replace → tails: [2, 3]
   7 → 7 > 3, append → tails: [2, 3, 7]
 101 → 101 > 7, append → tails: [2, 3, 7, 101]
  18 → 18 > 7, 18 < 101, replace → tails: [2, 3, 7, 18]

Length = 4. (LIS: 2, 3, 7, 18 or 2, 5, 7, 101, etc.) ✓

Note: tails is NOT the actual LIS — it's the smallest possible "ending" for each length.
```

---

## 5. Variations

| Variation | Key Change |
|---|---|
| **Longest Non-Decreasing Subsequence** | Use `upper_bound` instead of `lower_bound` |
| **Number of LIS** | Need additional array tracking count |
| **Print the actual LIS** | Track predecessors during DP |
| **Russian Doll Envelopes** | 2D LIS: sort by width, LIS on height |
| **Longest Chain of Pairs** | Sort by second element, LIS on first |

---

## 6. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #300 — Longest Increasing Subsequence | Medium | Classic LIS |
| 2 | LeetCode #673 — Number of Longest Increasing Subsequence | Medium | Count LIS |
| 3 | LeetCode #354 — Russian Doll Envelopes | Hard | 2D LIS |
| 4 | LeetCode #1048 — Longest String Chain | Medium | LIS with word matching |
| 5 | LeetCode #368 — Largest Divisible Subset | Medium | LIS variant |

**Memory Trick:**

> 🧠 **"Patience Solitaire"** — The O(n log n) LIS is exactly the patience sorting card game. Place each card on the leftmost pile where it fits (binary search). The number of piles = LIS length.

---
---

# Pattern 47: Digit DP* (Advanced)

```
═══════════════════════════════════════════════════════════════
  DIGIT DP (ADVANCED — OPTIONAL)
  OA Frequency: ★★☆☆☆
  Companies: Google | Microsoft | Atlassian
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Count numbers in range [L, R] satisfying a digit-based property." Examples: count numbers with digit sum ≤ K, count numbers without repeated digits, count numbers divisible by X.

**Why Digit DP?**

Brute force checks every number in [1, R] — up to 10¹⁸. Digit DP processes one digit at a time (18 digits max), considering: "What can I place at the current position while staying within bounds?"

---

## 2. Pattern Identification Checklist

- [ ] Does the problem involve counting numbers in a **range [L, R]**?
- [ ] Is the constraint on individual **digits** (sum, uniqueness, pattern)?
- [ ] Is R very large (up to 10¹⁸)?

---

## 3. Framework

```
count(L, R) = count(0, R) - count(0, L-1)

DIGIT_DP(pos, tight, state):
    if pos == numDigits:
        return is_valid(state) ? 1 : 0
    
    if memo[pos][tight][state] exists:
        return memo[pos][tight][state]
    
    limit = tight ? digit[pos] : 9
    result = 0
    
    for d = 0 to limit:
        new_tight = tight AND (d == limit)
        new_state = update(state, d)
        result += DIGIT_DP(pos+1, new_tight, new_state)
    
    memo[pos][tight][state] = result
    return result
```

**Key insight**: The `tight` flag tracks whether previous digits exactly match the upper bound. If tight, the current digit can be at most `digit[pos]`. Otherwise, it can be 0-9.

---

## 4. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #233 — Number of Digit One | Hard | Count 1s in range |
| 2 | LeetCode #357 — Count Numbers with Unique Digits | Medium | Digit DP or math |
| 3 | LeetCode #902 — Numbers At Most N Given Digit Set | Hard | Digit DP |

---
---

# Pattern 48: State Compression DP (Bitmask DP)*

```
═══════════════════════════════════════════════════════════════
  STATE COMPRESSION DP (BITMASK DP) — ADVANCED
  OA Frequency: ★★☆☆☆
  Companies: Google | Amazon | Microsoft
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Optimize over **subsets** when n is small (≤ 20)." The state is a bitmask where each bit represents whether an element is included. This compresses the "which elements have been used" information into a single integer.

---

## 2. Pattern Identification Checklist

- [ ] Is n ≤ 20?
- [ ] Does the problem involve **visiting all items** in an optimal order (like TSP)?
- [ ] Is the state "which items have been used" important?

---

## 3. Reusable C++ Template

```cpp
// Template: Bitmask DP — Traveling Salesman variant
// Time: O(2ⁿ × n²), Space: O(2ⁿ × n)

#include <vector>
#include <climits>
using namespace std;

int bitmaskDP(vector<vector<int>>& cost) {
    int n = cost.size();
    int fullMask = (1 << n) - 1;
    // dp[mask][last] = min cost to visit all cities in mask, ending at last
    vector<vector<int>> dp(1 << n, vector<int>(n, INT_MAX));
    
    // Base: start at city 0
    dp[1][0] = 0;
    
    for (int mask = 1; mask <= fullMask; mask++) {
        for (int last = 0; last < n; last++) {
            if (dp[mask][last] == INT_MAX) continue;
            if (!(mask & (1 << last))) continue;
            
            for (int next = 0; next < n; next++) {
                if (mask & (1 << next)) continue;  // already visited
                int newMask = mask | (1 << next);
                dp[newMask][next] = min(dp[newMask][next],
                                        dp[mask][last] + cost[last][next]);
            }
        }
    }
    
    // Find min cost visiting all cities
    int result = INT_MAX;
    for (int last = 0; last < n; last++) {
        result = min(result, dp[fullMask][last]);
    }
    return result;
}
```

---

## 4. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #698 — Partition to K Equal Sum Subsets | Medium | Bitmask DP |
| 2 | LeetCode #1125 — Smallest Sufficient Team | Hard | Set cover with bitmask |
| 3 | LeetCode #943 — Find Shortest Superstring | Hard | TSP variant |

**Memory Trick:**

> 🧠 **"The Binary Checklist"** — Each bit is a checkbox. Bit 0 = item 0 visited? Bit 1 = item 1 visited? The entire checklist fits in one integer. With n ≤ 20, there are only 2²⁰ ≈ 10⁶ possible checklists — small enough to DP over.

---
---

*Previous chapter: [13 — Trees](13_Trees.md)*
*Next chapter: [15 — String Algorithms](15_String_Algorithms.md)*
