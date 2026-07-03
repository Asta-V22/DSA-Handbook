<!--
═══════════════════════════════════════════════════════════════════════════════
  DSA PATTERN HANDBOOK — RECURSION & BACKTRACKING
  Chapter 10
  
  Patterns in this chapter:
    Pattern 26: Recursion
    Pattern 27: Backtracking
  
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

# Chapter 10: Recursion & Backtracking

Recursion is the foundation of many algorithms — DFS, divide & conquer, DP, and backtracking all use recursive thinking. Backtracking is recursion with a twist: you explore choices, and when a choice leads to a dead end, you **undo** it and try the next option.

---
---

# Pattern 26: Recursion

```
═══════════════════════════════════════════════════════════════
  RECURSION
  OA Frequency: ★★★★☆
  Companies: Amazon | Google | Microsoft | Meta | Adobe
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Any problem that can be defined in terms of **smaller versions of itself**. "To solve a problem of size n, first solve it for size n-1, then combine."

**Why was this pattern invented?**

Recursion mirrors mathematical induction. If you can:
1. Solve the base case (n = 0 or n = 1).
2. Express the solution for n in terms of n-1 (or n/2).
Then recursion solves it elegantly.

---

## 2. Pattern Identification Checklist

- [ ] Can the problem be **divided into smaller sub-problems** of the same type?
- [ ] Does it have a clear **base case** (trivial to solve directly)?
- [ ] Does it involve **trees** (inherently recursive structures)?
- [ ] Can it be expressed as a **mathematical recurrence**?

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points to Recursion |
|---|---|
| "Divide and conquer" | Split → solve → combine |
| "Tree traversal" | Trees are recursive structures |
| "Generate all" | Recursive enumeration |
| "Power function" / "fast exponentiation" | Recursive halving |
| "Fibonacci" / "recurrence" | Mathematical recursion |

---

## 4. The Three Components of Every Recursive Function

```
RECURSIVE_FUNCTION(input):
    // 1. BASE CASE — when to stop
    if input is trivial:
        return trivial_answer
    
    // 2. RECURSIVE CALL — solve smaller version
    smaller_result = RECURSIVE_FUNCTION(smaller_input)
    
    // 3. COMBINE — build answer from smaller result
    return combine(smaller_result, current_input)
```

**Common recursive structures:**

| Structure | Example |
|---|---|
| **Linear recursion** | `f(n) = f(n-1) + something` |
| **Binary recursion** | `f(n) = f(n/2) + f(n/2)` (merge sort) |
| **Tree recursion** | `f(node) = combine(f(left), f(right))` |
| **Multiple branches** | `f(n) = f(n-1) + f(n-2)` (Fibonacci) |

---

## 5. Reusable C++ Template

```cpp
// Template: Recursion — Tree problems

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

// Example: Max depth of binary tree
int maxDepth(TreeNode* root) {
    // Base case
    if (!root) return 0;
    
    // Recursive calls
    int leftDepth = maxDepth(root->left);
    int rightDepth = maxDepth(root->right);
    
    // Combine
    return 1 + max(leftDepth, rightDepth);
}
```

```cpp
// Template: Recursion — Divide and Conquer

// Example: Power function (fast exponentiation)
// Time: O(log n)
double myPow(double x, long long n) {
    if (n == 0) return 1.0;
    if (n < 0) return 1.0 / myPow(x, -n);
    
    double half = myPow(x, n / 2);
    if (n % 2 == 0) return half * half;
    else return half * half * x;
}
```

---

## 6. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| No base case → infinite recursion | Always define at least one base case first |
| Wrong base case → incorrect answer | Test with the smallest inputs |
| Stack overflow for large n | Convert to iteration or use tail recursion |
| Redundant recursive calls (e.g., fib) | Add memoization → becomes DP |
| Not returning from base case | Always `return` in the base case |

---

## 7. When NOT to Use

| Situation | Why Not | Use Instead |
|---|---|---|
| Large n (>10⁴) without memoization | Stack overflow | Iterative DP or BFS |
| Overlapping subproblems | Pure recursion is exponential | Memoization / DP |
| Tail recursion available | Compilers may not optimize | Convert to loop |

---

## 8. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #509 — Fibonacci Number | Easy | Basic recursion with memoization |
| 2 | LeetCode #104 — Maximum Depth of Binary Tree | Easy | Tree recursion |
| 3 | LeetCode #50 — Pow(x, n) | Medium | Divide & conquer |
| 4 | LeetCode #23 — Merge K Sorted Lists | Hard | Divide & conquer merge |
| 5 | LeetCode #241 — Different Ways to Add Parentheses | Medium | Expression tree recursion |

**Memory Trick:**

> 🧠 **"Russian Dolls"** — Each doll contains a smaller version of itself. To count all dolls, open the outer one (solve for n), count what's inside (recursive call for n-1), and add 1 (combine). The smallest doll has nothing inside — that's the base case.

---
---

# Pattern 27: Backtracking

```
═══════════════════════════════════════════════════════════════
  BACKTRACKING
  OA Frequency: ★★★★★
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Uber | Flipkart
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Generate **all valid configurations** (permutations, combinations, subsets) or find **any valid configuration** (N-Queens, Sudoku) by exploring choices and undoing bad ones."

Backtracking is **brute force with pruning**. You systematically explore all possibilities, but as soon as you detect that a partial solution cannot lead to a valid complete solution, you **backtrack** (undo the last choice) and try the next option.

**Why was this pattern invented?**

Pure brute force generates ALL possible solutions, including invalid ones. Backtracking skips entire branches of the search tree when it detects they'll be invalid, dramatically reducing the work.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem ask to **generate all** permutations / combinations / subsets?
- [ ] Does it ask to **find a valid configuration** (N-Queens, Sudoku, word search)?
- [ ] Are there **constraints** that can be checked incrementally during construction?
- [ ] Is the problem essentially "explore a decision tree with pruning"?
- [ ] Is n small (≤ 15-20)?

**If you checked 2+ of these → Backtracking.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Generate all permutations" | Classic backtracking |
| "Generate all combinations" | Choose or skip each element |
| "All subsets" | Include or exclude each element |
| "Word search in grid" | DFS with backtracking |
| "N-Queens" | Place queens with constraint checking |
| "Sudoku solver" | Fill cells with constraint propagation |
| "Partition into palindromes" | Try all partition points |
| "Letter combinations of phone number" | Expand each digit's choices |

---

## 4. Constraint Analysis

| Constraint | Brute Force | With Pruning | Verdict |
|---|---|---|---|
| n ≤ 8 | 8! = 40K ✅ | Much less ✅ | Both work |
| n ≤ 10 | 10! = 3.6M ✅ | ≈ 10⁵ ✅ | Pruning helps |
| n ≤ 15 | 15! ≈ 10¹² ❌ | With good pruning ⚠️ | Pruning essential |
| n ≤ 20 | 2²⁰ = 10⁶ for subsets ✅ | ✅ | Subsets OK, perms NO |
| n > 20 | ❌ | Usually ❌ | Need DP or other approach |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "Generate every possible configuration, check each for validity." → Exponential.

### Step 2: Key Observation
> "I can check validity **incrementally**. If placing a queen in row 3 conflicts with row 1, there's no point placing queens in rows 4-8 — the entire subtree is invalid."

### Step 3: Optimization (Pruning)
> "Before making a choice, check if it's compatible with previous choices. If not, skip it immediately."

### Step 4: Backtracking Framework
> "Choose → Explore → Unchoose. This is the core backtracking pattern."

```
                        []
                    /    |    \
                  [1]   [2]   [3]      ← Choose first element
                 / \    / \    / \
              [1,2][1,3][2,1]...        ← Choose second element
              ...   ...   ...           ← If invalid, PRUNE (don't go deeper)
```

---

## 6. Generic Algorithm

```
BACKTRACK(state, choices):
    if state is a COMPLETE SOLUTION:
        record(state)
        return
    
    for each choice in available_choices(state):
        if is_valid(state, choice):    // PRUNE: skip invalid
            make_choice(state, choice)     // CHOOSE
            BACKTRACK(state, remaining_choices)  // EXPLORE
            undo_choice(state, choice)     // UNCHOOSE (backtrack)
```

**The three types of backtracking problems:**

| Type | Goal | When to Record |
|---|---|---|
| **Find all solutions** | Generate every valid complete configuration | When state is complete |
| **Find one solution** | Return the first valid configuration | Return true when found |
| **Count solutions** | Count valid configurations | Increment counter when complete |

---

## 7. Reusable C++ Template

```cpp
// Template A: Backtracking — Generate All Permutations
// Time: O(n!), Space: O(n)

#include <vector>
using namespace std;

class Backtracker {
    vector<vector<int>> results;
    
    void backtrack(vector<int>& nums, vector<int>& current, vector<bool>& used) {
        // Base case: complete permutation
        if (current.size() == nums.size()) {
            results.push_back(current);
            return;
        }
        
        for (int i = 0; i < (int)nums.size(); i++) {
            if (used[i]) continue;    // PRUNE: already used
            
            // CHOOSE
            used[i] = true;
            current.push_back(nums[i]);
            
            // EXPLORE
            backtrack(nums, current, used);
            
            // UNCHOOSE
            current.pop_back();
            used[i] = false;
        }
    }
    
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<int> current;
        vector<bool> used(nums.size(), false);
        backtrack(nums, current, used);
        return results;
    }
};
```

```cpp
// Template B: Backtracking — Generate All Subsets
// Time: O(2ⁿ), Space: O(n)

void subsets(vector<int>& nums, int start, vector<int>& current,
             vector<vector<int>>& results) {
    results.push_back(current);  // every partial state is a valid subset
    
    for (int i = start; i < (int)nums.size(); i++) {
        current.push_back(nums[i]);      // CHOOSE
        subsets(nums, i + 1, current, results);  // EXPLORE (i+1 to avoid reuse)
        current.pop_back();              // UNCHOOSE
    }
}
```

```cpp
// Template C: Backtracking — Constraint Satisfaction (N-Queens style)

bool solve(vector<vector<int>>& board, int row) {
    if (row == board.size()) return true;  // all placed successfully
    
    for (int col = 0; col < (int)board.size(); col++) {
        if (isValid(board, row, col)) {    // PRUNE
            board[row][col] = 1;           // CHOOSE
            if (solve(board, row + 1))     // EXPLORE
                return true;               // found solution!
            board[row][col] = 0;           // UNCHOOSE
        }
    }
    
    return false;  // no valid placement in this row
}
```

---

## 8. Complexity Analysis

| Problem Type | Time | Space | Why |
|---|---|---|---|
| All permutations | O(n · n!) | O(n) call stack | n! leaves, O(n) per leaf to copy |
| All subsets | O(n · 2ⁿ) | O(n) | 2ⁿ subsets, O(n) per copy |
| All combinations C(n,k) | O(k · C(n,k)) | O(k) | C(n,k) leaves |
| N-Queens | O(n!) worst | O(n) | Pruning reduces dramatically |

**With pruning**: The actual runtime is often much less than the worst case, but the theoretical bound stays the same.

---

## 9. Dry Run

**Problem**: Generate all permutations of `[1, 2, 3]`.

```
backtrack([], used=[F,F,F])
├── choose 1: backtrack([1], used=[T,F,F])
│   ├── choose 2: backtrack([1,2], used=[T,T,F])
│   │   └── choose 3: [1,2,3] ✓ RECORD
│   │       └── unchoose 3 → [1,2]
│   ├── unchoose 2 → [1]
│   ├── choose 3: backtrack([1,3], used=[T,F,T])
│   │   └── choose 2: [1,3,2] ✓ RECORD
│   │       └── unchoose 2 → [1,3]
│   └── unchoose 3 → [1]
├── unchoose 1 → []
├── choose 2: backtrack([2], used=[F,T,F])
│   ├── choose 1: [2,1,3] ✓
│   ├── choose 3: [2,3,1] ✓
├── choose 3: backtrack([3], used=[F,F,T])
│   ├── choose 1: [3,1,2] ✓
│   └── choose 2: [3,2,1] ✓

Result: [1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]
6 permutations = 3! ✓
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Forgetting to UNCHOOSE (undo the choice) | Every choose must have a corresponding unchoose |
| Not pruning — exploring invalid states | Add validity check before making the choice |
| Duplicates in results | Sort input + skip duplicates: `if (i > start && nums[i] == nums[i-1]) continue;` |
| Wrong starting index for combinations | Use `i = start` (not 0) to avoid generating the same combination in different orders |
| Stack overflow for large inputs | Backtracking is inherently recursive — check if n is small enough |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| n > 20 (for subset problems) | 2²⁰ ≈ 10⁶ is the limit | DP or mathematical formula |
| Need count only (not actual configurations) | Backtracking generates all; counting is wasteful | DP (counting) |
| Optimal solution (min/max) | Backtracking finds all; optimization needs DP | DP |
| No pruning opportunities | Backtracking = brute force without pruning | Different approach |

---

## 12. Medium-Level Example Problem

**LeetCode #39 — Combination Sum**

> Given an array of distinct integers `candidates` and a target, return all unique combinations that sum to target. Each number may be used unlimited times.

**Why this represents the pattern well**: Classic backtracking with a twist — you CAN reuse elements (start from `i`, not `i+1`). The "sum exceeds target" check provides natural pruning.

---

## 13. Solution Walkthrough

1. "Generate combinations by choosing whether to include each candidate."
2. "Since we can reuse, start from `i` (not `i+1`) in the recursive call."
3. "Prune when `remaining < 0` — sum exceeded target."
4. "Base case: `remaining == 0` — found a valid combination."

---

## 14. C++ Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

// Combination Sum
// Pattern: Backtracking (with element reuse)
// Time: O(N^(T/M)) where T=target, M=min candidate
class Solution {
    vector<vector<int>> results;
    
    void backtrack(vector<int>& candidates, int target, int start, 
                   vector<int>& current) {
        if (target == 0) {
            results.push_back(current);
            return;
        }
        
        for (int i = start; i < (int)candidates.size(); i++) {
            if (candidates[i] > target) break;  // PRUNE (array is sorted)
            
            current.push_back(candidates[i]);              // CHOOSE
            backtrack(candidates, target - candidates[i], 
                      i, current);                         // EXPLORE (i, not i+1)
            current.pop_back();                            // UNCHOOSE
        }
    }
    
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());  // enables pruning
        vector<int> current;
        backtrack(candidates, target, 0, current);
        return results;
    }
};
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "Each number can only be used once?" | Change to `i + 1` in recursive call; skip duplicates |
| "Return count only?" | Replace vector recording with counter — or use DP |
| "Find ONE combination (not all)?" | Return `true` on first success; short-circuit |
| "What's the time complexity?" | Hard to state precisely — depends on branching factor and pruning |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Combination Sum II** | LeetCode #40. Each element once + skip duplicates |
| **Permutations II** | LeetCode #47. Skip duplicate elements in sorted array |
| **N-Queens** | LeetCode #51. Constraint: no two queens attack each other |
| **Sudoku Solver** | LeetCode #37. Constraint propagation + backtracking |
| **Word Search** | LeetCode #79. DFS on grid with backtracking |
| **Palindrome Partitioning** | LeetCode #131. Partition string into palindromic substrings |
| **Generate Parentheses** | LeetCode #22. Track open/close count as constraint |

---

## 17. Related Patterns

| Pattern | Relationship |
|---|---|
| **Recursion** | Backtracking IS recursion with choose/unchoose |
| **DFS** | Backtracking is DFS on a decision tree |
| **DP** | When overlapping subproblems exist, add memoization to backtracking → DP |
| **Bitmask** | For n ≤ 20, bitmask enumeration can replace backtracking for subsets |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #78 — Subsets | Medium | Choose/skip each element |
| 2 | LeetCode #46 — Permutations | Medium | Classic backtracking |
| 3 | LeetCode #39 — Combination Sum | Medium | Backtracking with reuse |
| 4 | LeetCode #51 — N-Queens | Hard | Constraint backtracking |
| 5 | LeetCode #37 — Sudoku Solver | Hard | Complex constraint propagation |

---

## 19. Memory Trick

> 🧠 **"The Maze Explorer"**
>
> Backtracking is like exploring a maze. At each fork, you pick a path (CHOOSE). If it's a dead end, you walk back to the fork (UNCHOOSE) and try the next path (EXPLORE). You mark where you've been to avoid loops. If you find the exit, you're done.
>
> **The Three Steps**: "Choose → Explore → Unchoose." Tattoo this on your brain. Every backtracking problem follows this exact pattern.

---
---

*Previous chapter: [09 — Matrix & Simulation](09_Matrix_and_Simulation.md)*
*Next chapter: [11 — Graph Traversal](11_Graph_Traversal.md)*
