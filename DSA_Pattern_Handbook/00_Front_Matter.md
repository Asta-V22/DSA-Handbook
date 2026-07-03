<!--
═══════════════════════════════════════════════════════════════════════════════
  DSA PATTERN HANDBOOK — FRONT MATTER
  Chapter 00
  
  Formatting Rules (internal reference for consistency):
  - Heading hierarchy: # (chapter) → ## (major section) → ### (subsection)
  - Pattern template: 20 sections per pattern
  - Complexity notation: O(n), O(n log n), O(n²), Θ, Ω as needed
  - Star rating: ★ (filled), ☆ (empty), scale 1–5
  - Code style: C++17, 4-space indent, STL preferred
  - Tables: GitHub-flavored markdown pipe tables
  - Diagrams: ASCII art, indented in code blocks
  - Terminology: "pattern" (not "technique" or "method")
  - Problem references: "LeetCode #ID — Name" format
═══════════════════════════════════════════════════════════════════════════════
-->

# DSA Pattern Handbook

### A Comprehensive, Placement-Oriented Guide to Data Structures and Algorithms for Software Engineering Online Assessments

---

> *"The goal is not to memorize 500 problems. The goal is to learn 50 patterns and recognize them in 5000 problems."*

---

## About This Handbook

This handbook is designed for software engineering students preparing for **placement season** — online assessments (OAs), technical interviews, and coding rounds. It is not a textbook. It is not a problem set. It is a **pattern recognition guide** that teaches you to think like an interviewer expects you to think.

Most students make the mistake of solving hundreds of problems without developing the ability to recognize **why** an approach works. When they encounter a new problem during an OA, they freeze — not because they lack knowledge, but because they lack **pattern intuition**.

This handbook fixes that.

For every pattern, you will learn:

- **What** the pattern solves
- **When** to recognize it (keywords, constraints, problem structure)
- **Why** it works (intuition before implementation)
- **How** to derive the solution naturally (brute force → observation → optimization)
- **Where** it commonly appears (company tags, frequency ratings)
- **What NOT to do** (common mistakes, when the pattern doesn't apply)

---

## Who Should Read This

This handbook assumes you:

| Prerequisite | Level Required |
|---|---|
| C++ syntax | Comfortable (variables, loops, functions, classes) |
| STL basics | Can use `vector`, `map`, `set`, `sort`, `pair` |
| Arrays & Strings | Understand indexing, traversal, basic operations |
| Recursion | Understand base case, recursive calls, call stack |
| Time Complexity | Know what O(n), O(n²), O(log n) mean |
| Basic Math | Modular arithmetic, prime numbers, GCD |

If you meet these prerequisites, this handbook will take you from "I can solve easy problems" to "I can recognize and solve medium-hard problems under time pressure."

---

## How to Use This Handbook

### For First-Time Readers

1. **Start with Chapter 01** — the Pattern Recognition Flowchart. This gives you a bird's-eye view of all patterns and when to use each.
2. **Read patterns in order** — they are arranged from fundamental to advanced. Earlier patterns build intuition for later ones.
3. **For each pattern**, read the Intuition and Evolution of Thought sections first. Do NOT jump to the code.
4. **Try the example problem yourself** before reading the solution walkthrough.
5. **Use the reusable template** during practice — modify it for each problem.

### For Revision Before an OA

1. **30-Minute Revision Guide** (Chapter 18) — one-line summary of every pattern.
2. **Pattern Atlas** (Chapter 19) — look up problem keywords → find the pattern instantly.
3. **Complexity Cheat Sheet** (Chapter 18) — verify your complexity analysis.
4. **STL Cheat Sheet** (Chapter 18) — quick syntax reference.

### For Targeted Practice

1. Look at the **Practice Problems** section of any pattern.
2. Problems are ordered: 2 Easy → 2 Medium → 1 Hard.
3. All problems include LeetCode IDs for quick lookup.

---

## Legend and Conventions

### OA Frequency Rating

Each pattern is rated on how frequently it appears in software engineering OAs and interviews:

| Rating | Meaning |
|---|---|
| ★☆☆☆☆ | Rare — appears in specialized rounds only |
| ★★☆☆☆ | Uncommon — shows up occasionally |
| ★★★☆☆ | Moderate — expect it in every placement season |
| ★★★★☆ | Common — appears in most companies' OAs |
| ★★★★★ | Very Common — almost guaranteed to appear |

### Company Tags

Company names appear at the top of each pattern, indicating where the pattern is commonly tested. Sources: LeetCode discussion forums, interview experience posts, and publicly available data.

Example:
```
Companies: Amazon | Google | Microsoft | Adobe | Flipkart
```

### Complexity Notation

| Symbol | Meaning |
|---|---|
| O(·) | Upper bound (worst case) |
| Θ(·) | Tight bound (average case) |
| n | Input size |
| k | Window size / parameter |
| m | Second dimension (rows, edges, etc.) |
| V, E | Vertices, Edges (graph problems) |

### Code Conventions

All code in this handbook follows these rules:

- **Language**: C++17
- **Indentation**: 4 spaces
- **Naming**: `camelCase` for variables, `PascalCase` for classes
- **STL preferred**: Use `vector`, `unordered_map`, `priority_queue`, etc.
- **No competitive programming shortcuts**: No `#define`, no `bits/stdc++.h`, no macro tricks
- **Well-commented**: Every non-obvious line is explained
- **Interview-quality**: Code you could write on a whiteboard

Example style:

```cpp
#include <vector>
#include <algorithm>
using namespace std;

// Returns the maximum sum of any subarray of size k
// Pattern: Fixed Sliding Window
// Time: O(n), Space: O(1)
int maxSumSubarray(vector<int>& nums, int k) {
    int n = nums.size();
    int windowSum = 0;
    int maxSum = INT_MIN;

    for (int i = 0; i < n; i++) {
        windowSum += nums[i];          // expand window

        if (i >= k) {
            windowSum -= nums[i - k];  // shrink window
        }

        if (i >= k - 1) {
            maxSum = max(maxSum, windowSum);  // record answer
        }
    }

    return maxSum;
}
```

### Diagram Conventions

ASCII diagrams are used throughout for:
- Array states during dry runs
- Tree and graph structures
- Pointer movements
- Stack/queue states

Example:
```
Array:  [2, 1, 5, 1, 3, 2]
         ↑        ↑
         L        R
Window: [1, 5, 1]  Sum = 7
```

### Problem Reference Format

```
LeetCode #53 — Maximum Subarray
Difficulty: Medium
Link: https://leetcode.com/problems/maximum-subarray/
```

When problems are from other platforms:
```
Codeforces #1234A — Problem Name
GFG: Problem Name (https://practice.geeksforgeeks.org/problems/...)
```

---

## Constraint-Based Algorithm Selection

One of the most important skills in an OA is looking at the **constraints** to determine the expected time complexity. Here is the master reference:

| Input Size (n) | Maximum Acceptable Complexity | Typical Patterns |
|---|---|---|
| n ≤ 10 | O(n!) or O(2ⁿ) | Brute force, permutations, backtracking |
| n ≤ 20 | O(2ⁿ) or O(n · 2ⁿ) | Bitmask DP, subset enumeration |
| n ≤ 100 | O(n³) | Floyd-Warshall, 3 nested loops |
| n ≤ 500 | O(n³) | DP on intervals, matrix chain |
| n ≤ 5,000 | O(n²) | 2D DP, brute force with optimization |
| n ≤ 10⁵ | O(n log n) | Sorting, binary search, divide & conquer |
| n ≤ 10⁶ | O(n) or O(n log n) | Linear scan, sliding window, two pointers |
| n ≤ 10⁷ | O(n) | Sieve, prefix sum, single pass |
| n ≤ 10⁸+ | O(log n) or O(1) | Math, binary search on answer |

> **Rule of thumb**: Modern computers perform ~10⁸ simple operations per second. If your algorithm does more operations than the time limit allows, you need a faster approach.

### How to Use This Table During an OA

1. **Read the constraints first** — before even understanding the problem fully.
2. **Determine the expected complexity** from the table above.
3. **This immediately eliminates patterns** — if n = 10⁵, you know O(n²) won't pass.
4. **Match remaining patterns** to the problem description.

---

## Global Recognition Keywords Table

This table maps common phrases in problem statements to the most likely DSA pattern. Use this as your first filter when reading a problem.

| Keyword / Phrase in Problem | Most Likely Pattern(s) |
|---|---|
| "Subarray sum equals k" | Prefix Sum + Hashing |
| "Maximum / minimum subarray" | Sliding Window / Kadane's (DP) |
| "Longest substring without repeating" | Variable Sliding Window + Hashing |
| "Contiguous subarray of size k" | Fixed Sliding Window |
| "Sorted array" + "find target" | Binary Search |
| "Minimum / maximum possible value" + "check if feasible" | Binary Search on Answer |
| "Pair with given sum" | Two Pointers / Hashing |
| "Cycle detection" in linked list | Fast & Slow Pointer |
| "Next greater / smaller element" | Monotonic Stack |
| "Maximum in sliding window" | Monotonic Queue / Deque |
| "Kth largest / smallest" | Heap / Quick Select |
| "Merge overlapping intervals" | Merge Intervals (Sort + Sweep) |
| "Number of events at time t" | Sweep Line / Difference Array |
| "Range update, point query" | Difference Array |
| "Range query, point update" | Fenwick Tree / Segment Tree |
| "Connected components" | DFS / BFS / Union Find |
| "Number of islands" | DFS / BFS on Grid |
| "Shortest path" (unweighted) | BFS |
| "Shortest path" (weighted, non-negative) | Dijkstra |
| "Shortest path" (negative weights) | Bellman-Ford |
| "All pairs shortest path" | Floyd-Warshall |
| "Minimum spanning tree" | Kruskal / Prim |
| "Course schedule" / "prerequisites" | Topological Sort |
| "Word search" / "generate permutations" | Backtracking |
| "Minimum cost" / "maximum profit" | Dynamic Programming |
| "Longest increasing subsequence" | LIS Pattern (DP + Binary Search) |
| "0/1 choices" + "capacity constraint" | Knapsack DP |
| "Count ways to reach" | 1D / 2D DP |
| "Edit distance" / "LCS" | 2D DP |
| "Prefix match" / "autocomplete" | Trie |
| "Lowest common ancestor" | LCA (Binary Lifting / DFS) |
| "Pattern matching in string" | KMP / Z Algorithm / Rabin-Karp |
| "Subset sum" / "partition" | DP / Backtracking |
| "Power of 2" / "count set bits" | Bit Manipulation |
| "Rotate matrix" / "spiral order" | Matrix Traversal / Simulation |
| "Check if bipartite" | BFS / DFS + Coloring |
| "Detect negative cycle" | Bellman-Ford |
| "Multi-source shortest path" | Multi-Source BFS |
| "Tasks with dependencies" | Topological Sort |
| "Minimum operations to make equal" | Binary Search on Answer / Greedy |
| "Rearrange to maximize / minimize" | Sorting + Greedy |
| "Non-overlapping intervals" | Greedy (Interval Scheduling) |
| "Expression evaluation" | Stack |
| "Valid parentheses" | Stack |
| "Median from data stream" | Two Heaps |
| "Stock span" / "daily temperatures" | Monotonic Stack |
| "Trap rain water" | Monotonic Stack / Two Pointers |
| "Number at each digit position" | Digit DP |
| "Assign items to groups" (small n) | State Compression DP |

> **Tip**: Many problems combine 2–3 patterns. If a single keyword doesn't pinpoint the pattern, look for constraint + keyword combinations.

---

## Chapter Overview

| Chapter | Title | Patterns Covered | Count |
|---|---|---|---|
| 00 | Front Matter | — | — |
| 01 | Pattern Recognition Flowchart | Decision tree | — |
| 02 | Fundamentals | Brute Force, Prefix Sum, Difference Array, Hashing, Frequency Counting | 5 |
| 03 | Sliding Window & Pointers | Fixed Window, Variable Window, Two Pointers, Fast & Slow | 4 |
| 04 | Binary Search | On Sorted Arrays, On Answer | 2 |
| 05 | Sorting & Greedy | Sorting+Greedy, Greedy Algorithms | 2 |
| 06 | Stack & Queue | Stack, Queue, Deque, Monotonic Stack, Monotonic Queue | 5 |
| 07 | Heap & Intervals | Heap/PQ, Interval Problems, Merge Intervals, Sweep Line | 4 |
| 08 | Bit Manipulation | Bit Manipulation | 1 |
| 09 | Matrix & Simulation | Matrix Traversal, Simulation | 2 |
| 10 | Recursion & Backtracking | Recursion, Backtracking | 2 |
| 11 | Graph Traversal | DFS, BFS, Multi-Source BFS, Topological Sort | 4 |
| 12 | Advanced Graphs | Union Find, Dijkstra, Floyd-Warshall, Bellman-Ford, MST | 5 |
| 13 | Trees | Binary Tree DFS, Binary Tree BFS, BST, LCA, Trie | 5 |
| 14 | Dynamic Programming | Intro, 1D, 2D, Knapsack, LIS, Digit DP, State Compression DP | 7 |
| 15 | String Algorithms | KMP, Z Algorithm, Rabin-Karp | 3 |
| 16 | Advanced Data Structures | Segment Tree, Fenwick Tree, Binary Lifting | 3 |
| 17 | Miscellaneous | Line Sweep, Coordinate Compression, Mathematical Observation | 3 |
| 18 | Cheat Sheets | Complexity, STL, Interview Tips, OA Mistakes, 30-Min Revision | — |
| 19 | Pattern Atlas | Quick-reference decision table | — |

**Total patterns covered: 57**

---

## Acknowledgments

This handbook draws on problems from LeetCode, Codeforces, GeeksforGeeks, and HackerRank. Company tags are sourced from publicly available interview experiences. Pattern categorizations follow widely accepted competitive programming and interview preparation conventions.

---

*Next chapter: [01 — Pattern Recognition Flowchart](01_Pattern_Recognition.md)*
