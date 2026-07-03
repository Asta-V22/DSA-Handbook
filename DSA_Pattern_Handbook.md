<!--
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DSA PATTERN HANDBOOK â€” FRONT MATTER
  Chapter 00
  
  Formatting Rules (internal reference for consistency):
  - Heading hierarchy: # (chapter) â†’ ## (major section) â†’ ### (subsection)
  - Pattern template: 20 sections per pattern
  - Complexity notation: O(n), O(n log n), O(nÂ²), Î˜, Î© as needed
  - Star rating: â˜… (filled), â˜† (empty), scale 1â€“5
  - Code style: C++17, 4-space indent, STL preferred
  - Tables: GitHub-flavored markdown pipe tables
  - Diagrams: ASCII art, indented in code blocks
  - Terminology: "pattern" (not "technique" or "method")
  - Problem references: "LeetCode #ID â€” Name" format
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-->

# DSA Pattern Handbook

### A Comprehensive, Placement-Oriented Guide to Data Structures and Algorithms for Software Engineering Online Assessments

---

> *"The goal is not to memorize 500 problems. The goal is to learn 50 patterns and recognize them in 5000 problems."*

---

## About This Handbook

This handbook is designed for software engineering students preparing for **placement season** â€” online assessments (OAs), technical interviews, and coding rounds. It is not a textbook. It is not a problem set. It is a **pattern recognition guide** that teaches you to think like an interviewer expects you to think.

Most students make the mistake of solving hundreds of problems without developing the ability to recognize **why** an approach works. When they encounter a new problem during an OA, they freeze â€” not because they lack knowledge, but because they lack **pattern intuition**.

This handbook fixes that.

For every pattern, you will learn:

- **What** the pattern solves
- **When** to recognize it (keywords, constraints, problem structure)
- **Why** it works (intuition before implementation)
- **How** to derive the solution naturally (brute force â†’ observation â†’ optimization)
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
| Time Complexity | Know what O(n), O(nÂ²), O(log n) mean |
| Basic Math | Modular arithmetic, prime numbers, GCD |

If you meet these prerequisites, this handbook will take you from "I can solve easy problems" to "I can recognize and solve medium-hard problems under time pressure."

---

## How to Use This Handbook

### For First-Time Readers

1. **Start with Chapter 01** â€” the Pattern Recognition Flowchart. This gives you a bird's-eye view of all patterns and when to use each.
2. **Read patterns in order** â€” they are arranged from fundamental to advanced. Earlier patterns build intuition for later ones.
3. **For each pattern**, read the Intuition and Evolution of Thought sections first. Do NOT jump to the code.
4. **Try the example problem yourself** before reading the solution walkthrough.
5. **Use the reusable template** during practice â€” modify it for each problem.

### For Revision Before an OA

1. **30-Minute Revision Guide** (Chapter 18) â€” one-line summary of every pattern.
2. **Pattern Atlas** (Chapter 19) â€” look up problem keywords â†’ find the pattern instantly.
3. **Complexity Cheat Sheet** (Chapter 18) â€” verify your complexity analysis.
4. **STL Cheat Sheet** (Chapter 18) â€” quick syntax reference.

### For Targeted Practice

1. Look at the **Practice Problems** section of any pattern.
2. Problems are ordered: 2 Easy â†’ 2 Medium â†’ 1 Hard.
3. All problems include LeetCode IDs for quick lookup.

---

## Legend and Conventions

### OA Frequency Rating

Each pattern is rated on how frequently it appears in software engineering OAs and interviews:

| Rating | Meaning |
|---|---|
| â˜…â˜†â˜†â˜†â˜† | Rare â€” appears in specialized rounds only |
| â˜…â˜…â˜†â˜†â˜† | Uncommon â€” shows up occasionally |
| â˜…â˜…â˜…â˜†â˜† | Moderate â€” expect it in every placement season |
| â˜…â˜…â˜…â˜…â˜† | Common â€” appears in most companies' OAs |
| â˜…â˜…â˜…â˜…â˜… | Very Common â€” almost guaranteed to appear |

### Company Tags

Company names appear at the top of each pattern, indicating where the pattern is commonly tested. Sources: LeetCode discussion forums, interview experience posts, and publicly available data.

Example:
```
Companies: Amazon | Google | Microsoft | Adobe | Flipkart
```

### Complexity Notation

| Symbol | Meaning |
|---|---|
| O(Â·) | Upper bound (worst case) |
| Î˜(Â·) | Tight bound (average case) |
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
         â†‘        â†‘
         L        R
Window: [1, 5, 1]  Sum = 7
```

### Problem Reference Format

```
LeetCode #53 â€” Maximum Subarray
Difficulty: Medium
Link: https://leetcode.com/problems/maximum-subarray/
```

When problems are from other platforms:
```
Codeforces #1234A â€” Problem Name
GFG: Problem Name (https://practice.geeksforgeeks.org/problems/...)
```

---

## Constraint-Based Algorithm Selection

One of the most important skills in an OA is looking at the **constraints** to determine the expected time complexity. Here is the master reference:

| Input Size (n) | Maximum Acceptable Complexity | Typical Patterns |
|---|---|---|
| n â‰¤ 10 | O(n!) or O(2â¿) | Brute force, permutations, backtracking |
| n â‰¤ 20 | O(2â¿) or O(n Â· 2â¿) | Bitmask DP, subset enumeration |
| n â‰¤ 100 | O(nÂ³) | Floyd-Warshall, 3 nested loops |
| n â‰¤ 500 | O(nÂ³) | DP on intervals, matrix chain |
| n â‰¤ 5,000 | O(nÂ²) | 2D DP, brute force with optimization |
| n â‰¤ 10âµ | O(n log n) | Sorting, binary search, divide & conquer |
| n â‰¤ 10â¶ | O(n) or O(n log n) | Linear scan, sliding window, two pointers |
| n â‰¤ 10â· | O(n) | Sieve, prefix sum, single pass |
| n â‰¤ 10â¸+ | O(log n) or O(1) | Math, binary search on answer |

> **Rule of thumb**: Modern computers perform ~10â¸ simple operations per second. If your algorithm does more operations than the time limit allows, you need a faster approach.

### How to Use This Table During an OA

1. **Read the constraints first** â€” before even understanding the problem fully.
2. **Determine the expected complexity** from the table above.
3. **This immediately eliminates patterns** â€” if n = 10âµ, you know O(nÂ²) won't pass.
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

> **Tip**: Many problems combine 2â€“3 patterns. If a single keyword doesn't pinpoint the pattern, look for constraint + keyword combinations.

---

## Chapter Overview

| Chapter | Title | Patterns Covered | Count |
|---|---|---|---|
| 00 | Front Matter | â€” | â€” |
| 01 | Pattern Recognition Flowchart | Decision tree | â€” |
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
| 18 | Cheat Sheets | Complexity, STL, Interview Tips, OA Mistakes, 30-Min Revision | â€” |
| 19 | Pattern Atlas | Quick-reference decision table | â€” |

**Total patterns covered: 57**

---

## Acknowledgments

This handbook draws on problems from LeetCode, Codeforces, GeeksforGeeks, and HackerRank. Company tags are sourced from publicly available interview experiences. Pattern categorizations follow widely accepted competitive programming and interview preparation conventions.

---

*Next chapter: [01 â€” Pattern Recognition Flowchart](01_Pattern_Recognition.md)*
<!--
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DSA PATTERN HANDBOOK â€” PATTERN RECOGNITION FLOWCHART
  Chapter 01
  
  Formatting Rules (internal reference for consistency):
  - Heading hierarchy: # (chapter) â†’ ## (major section) â†’ ### (subsection)
  - Pattern template: 20 sections per pattern
  - Complexity notation: O(n), O(n log n), O(nÂ²), Î˜, Î© as needed
  - Star rating: â˜… (filled), â˜† (empty), scale 1â€“5
  - Code style: C++17, 4-space indent, STL preferred
  - Tables: GitHub-flavored markdown pipe tables
  - Diagrams: ASCII art, indented in code blocks
  - Terminology: "pattern" (not "technique" or "method")
  - Problem references: "LeetCode #ID â€” Name" format
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-->

# Chapter 1: Pattern Recognition Flowchart

---

> *"An expert is someone who has made every possible mistake in a very narrow field. A fast expert is someone who recognizes the pattern before making the mistake."*

---

## Why a Flowchart?

During an OA, you have limited time. The most dangerous trap is **trying the wrong approach first** â€” spending 15 minutes on a brute force solution only to realize you need a sliding window, or attempting DP when a greedy approach works.

This chapter provides a systematic **decision framework** that you can follow in the first 2â€“3 minutes after reading a problem. It won't always give you the exact pattern, but it will **narrow your search to 2â€“3 candidates** instantly.

---

## The 5-Step Recognition Process

Before looking at the flowchart, internalize this 5-step process:

### Step 1: Read the Constraints

| Constraint | What It Tells You |
|---|---|
| n â‰¤ 15â€“20 | Bitmask / brute force / backtracking is expected |
| n â‰¤ 1000â€“5000 | O(nÂ²) is acceptable â€” consider DP, nested loops |
| n â‰¤ 10âµ | O(n log n) â€” sorting, binary search, divide & conquer |
| n â‰¤ 10â¶â€“10â· | O(n) â€” single pass, sliding window, prefix sum |
| n â‰¤ 10â¹ | O(log n) or O(1) â€” math formula, binary search on answer |

### Step 2: Identify the Data Structure

| Input Type | Likely Patterns |
|---|---|
| **Array (unsorted)** | Hashing, Prefix Sum, Sliding Window, Two Pointers (after sort) |
| **Array (sorted)** | Binary Search, Two Pointers |
| **String** | Sliding Window, Hashing, Trie, KMP/Z, DP |
| **Linked List** | Fast & Slow, Two Pointers |
| **Tree** | DFS, BFS, Binary Lifting, LCA |
| **Graph (unweighted)** | BFS, DFS, Topological Sort, Union Find |
| **Graph (weighted)** | Dijkstra, Bellman-Ford, Floyd-Warshall, MST |
| **Matrix / Grid** | BFS, DFS, DP |
| **Intervals** | Sorting + Greedy, Sweep Line, Merge Intervals |

### Step 3: Identify the Question Type

| What They Ask | Likely Patterns |
|---|---|
| "Find / count" | Hashing, Prefix Sum, DP |
| "Maximum / minimum" | Sliding Window, DP, Binary Search on Answer, Greedy |
| "Longest / shortest" | Sliding Window, DP, BFS |
| "All possibilities" | Backtracking, Recursion |
| "Yes / No feasibility" | Binary Search on Answer, Greedy, DP |
| "Kth element" | Heap, Quick Select, Binary Search |
| "Topological order" | Topological Sort (BFS Kahn's / DFS) |
| "Connected" | DFS, BFS, Union Find |
| "Optimal partition" | DP, Greedy |

### Step 4: Look for Signal Keywords

Refer to the Global Recognition Keywords Table in Chapter 00. Specific phrases in the problem statement almost always map to specific patterns.

### Step 5: Verify with Complexity

After selecting a candidate pattern, verify:
- Does the algorithm's time complexity fit within the constraint?
- Does the space complexity fit in memory (usually 256 MB)?
- Can you handle the edge cases?

If yes, proceed. If no, consider the next candidate pattern.

---

## Master Decision Flowchart

```
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚                     READ THE PROBLEM                                 â”‚
â”‚              What is the input? What is asked?                       â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                           â”‚
                           â–¼
              â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
              â”‚  Is the input a GRAPH  â”‚
              â”‚  or TREE?              â”‚
              â””â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”˜
                YES â”‚          â”‚ NO
                    â–¼          â–¼
        â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”   â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
        â”‚ Go to GRAPH / â”‚   â”‚ Is the input an ARRAY    â”‚
        â”‚ TREE section  â”‚   â”‚ or STRING?               â”‚
        â”‚ (below)       â”‚   â””â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”˜
        â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜     YES â”‚              â”‚ NO
                                  â–¼              â–¼
                    â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”  â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
                    â”‚ Go to ARRAY /    â”‚  â”‚ Is it about MATH,    â”‚
                    â”‚ STRING section   â”‚  â”‚ BITS, or SIMULATION? â”‚
                    â”‚ (below)          â”‚  â””â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”˜
                    â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜   YES â”‚            â”‚ NO
                                               â–¼            â–¼
                                 â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”  â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
                                 â”‚ Math/Bit/Sim  â”‚  â”‚ INTERVALS?   â”‚
                                 â”‚ section       â”‚  â”‚ â†’ Merge/Sweepâ”‚
                                 â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜  â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```

---

## Branch 1: Array / String Problems

```
                        ARRAY / STRING INPUT
                              â”‚
              â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
              â”‚               â”‚                   â”‚
              â–¼               â–¼                   â–¼
     "Subarray / Substring"  "Find target"    "All subsets /
      with some property      or "Sorted"      permutations"
              â”‚               â”‚                   â”‚
              â–¼               â–¼                   â–¼
  â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”  â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”     â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
  â”‚ Is the window     â”‚  â”‚ Binary   â”‚     â”‚ Backtracking â”‚
  â”‚ size fixed?       â”‚  â”‚ Search   â”‚     â”‚ / Recursion  â”‚
  â””â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”˜  â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜     â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
  YESâ”‚          â”‚NO
     â–¼          â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”  â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚ Fixed    â”‚  â”‚ Does the window shrink/grow based         â”‚
â”‚ Sliding  â”‚  â”‚ on a condition?                           â”‚
â”‚ Window   â”‚  â””â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜     YES â”‚                      â”‚ NO
                     â–¼                      â–¼
            â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”    â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
            â”‚ Variable     â”‚    â”‚ Can you precompute        â”‚
            â”‚ Sliding      â”‚    â”‚ cumulative information?   â”‚
            â”‚ Window       â”‚    â””â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”˜
            â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜     YES â”‚               â”‚ NO
                                     â–¼               â–¼
                            â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”  â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
                            â”‚ Prefix Sum / â”‚  â”‚ Is the array â”‚
                            â”‚ Hashing      â”‚  â”‚ sorted?      â”‚
                            â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜  â””â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”˜
                                              YESâ”‚       â”‚NO
                                                 â–¼       â–¼
                                         â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â” â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
                                         â”‚ Two      â”‚ â”‚ Consider â”‚
                                         â”‚ Pointers â”‚ â”‚ Sorting  â”‚
                                         â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜ â”‚ first,   â”‚
                                                      â”‚ then Two â”‚
                                                      â”‚ Pointers â”‚
                                                      â”‚ or Greedyâ”‚
                                                      â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```

### Array / String Decision Summary

| Situation | Pattern |
|---|---|
| Fixed-size subarray/substring property | Fixed Sliding Window |
| Variable-size subarray with a condition | Variable Sliding Window |
| Sum of subarray = k, prefix-based queries | Prefix Sum + Hash Map |
| Range updates efficiently | Difference Array |
| Frequency / count occurrences | Frequency Counting / Hash Map |
| Sorted array, find target or pair | Binary Search / Two Pointers |
| "Longest increasing subsequence" | LIS Pattern (DP + BS) |
| "Minimum cost to do X" | Dynamic Programming |
| "Next greater element" | Monotonic Stack |
| "Maximum in all windows of size k" | Monotonic Queue |
| "Kth largest/smallest" | Heap |
| "Rearrange optimally" | Sorting + Greedy |

---

## Branch 2: Graph Problems

```
                          GRAPH INPUT
                              â”‚
              â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
              â”‚               â”‚                   â”‚
              â–¼               â–¼                   â–¼
        Unweighted?      Weighted?          "Connected
              â”‚               â”‚              components?"
              â”‚               â”‚                   â”‚
              â–¼               â–¼                   â–¼
    â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”  â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”    â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
    â”‚ Shortest     â”‚  â”‚ Shortest     â”‚    â”‚ Union Find   â”‚
    â”‚ path?        â”‚  â”‚ path?        â”‚    â”‚ or DFS/BFS   â”‚
    â”‚ â†’ BFS        â”‚  â”‚              â”‚    â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
    â”‚              â”‚  â”‚ Non-negative â”‚
    â”‚ Traversal?   â”‚  â”‚ â†’ Dijkstra   â”‚
    â”‚ â†’ DFS/BFS    â”‚  â”‚              â”‚
    â”‚              â”‚  â”‚ Negative     â”‚
    â”‚ Dependencies â”‚  â”‚ â†’ Bellman-F  â”‚
    â”‚ â†’ Topo Sort  â”‚  â”‚              â”‚
    â”‚              â”‚  â”‚ All pairs    â”‚
    â”‚ Cycle?       â”‚  â”‚ â†’ Floyd-W    â”‚
    â”‚ â†’ DFS color  â”‚  â”‚              â”‚
    â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜  â”‚ Min tree     â”‚
                      â”‚ â†’ MST        â”‚
                      â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```

### Graph Decision Summary

| Situation | Pattern |
|---|---|
| Traverse all nodes/edges | DFS or BFS |
| Shortest path (unweighted) | BFS |
| Shortest path (weighted, non-negative) | Dijkstra |
| Shortest path (negative weights possible) | Bellman-Ford |
| Detect negative cycle | Bellman-Ford |
| All pairs shortest path (small V) | Floyd-Warshall |
| Connected components | DFS / BFS / Union Find |
| Cycle detection (directed) | DFS with coloring / Topological Sort |
| Cycle detection (undirected) | DFS / Union Find |
| Dependency ordering | Topological Sort |
| Minimum cost to connect all nodes | Minimum Spanning Tree |
| Multi-source spread (fire, rotten oranges) | Multi-Source BFS |
| Bipartite check | BFS / DFS + 2-coloring |

---

## Branch 3: Tree Problems

```
                           TREE INPUT
                              â”‚
              â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
              â”‚               â”‚                   â”‚
              â–¼               â–¼                   â–¼
        "Path / depth /   "Level-order /     "Ancestor /
         subtree sum"      width / zigzag"    distance"
              â”‚               â”‚                   â”‚
              â–¼               â–¼                   â–¼
    â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”  â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”    â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
    â”‚ Binary Tree  â”‚  â”‚ Binary Tree  â”‚    â”‚ LCA          â”‚
    â”‚ DFS          â”‚  â”‚ BFS          â”‚    â”‚ (Binary      â”‚
    â”‚ (recursion)  â”‚  â”‚ (queue)      â”‚    â”‚  Lifting)    â”‚
    â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜  â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜    â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
              â”‚
              â–¼
    â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
    â”‚ Is it a BST? â”‚
    â”‚ â†’ BST-specificâ”‚
    â”‚   properties  â”‚
    â”‚   (inorder =  â”‚
    â”‚    sorted)    â”‚
    â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```

### Tree Decision Summary

| Situation | Pattern |
|---|---|
| Path sum, depth, diameter, subtree queries | Binary Tree DFS |
| Level-order traversal, width, zigzag | Binary Tree BFS |
| "Is valid BST?", "Kth smallest in BST" | BST Properties (inorder) |
| Lowest Common Ancestor | LCA (recursion or Binary Lifting) |
| Prefix search, autocomplete | Trie |

---

## Branch 4: Optimization / Decision Problems

```
                    "Find optimal / count ways"
                              â”‚
              â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
              â”‚               â”‚                   â”‚
              â–¼               â–¼                   â–¼
     "Does a greedy     "Overlapping         "Check if X
      choice lead to     subproblems?"         is feasible
      global optimum?"        â”‚                for value V?"
              â”‚               â–¼                      â”‚
              â–¼       â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”               â–¼
    â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”  â”‚ Dynamic      â”‚    â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
    â”‚ Greedy       â”‚  â”‚ Programming  â”‚    â”‚ Binary Search    â”‚
    â”‚ Algorithm    â”‚  â”‚              â”‚    â”‚ on Answer        â”‚
    â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜  â”‚ 1D? 2D?     â”‚    â”‚                  â”‚
                      â”‚ Knapsack?   â”‚    â”‚ + Greedy/Check   â”‚
                      â”‚ LIS?        â”‚    â”‚   function       â”‚
                      â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜    â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```

### Optimization Decision Summary

| Situation | Pattern |
|---|---|
| "Can we always take the locally optimal choice?" | Greedy |
| "Count ways" / "minimum cost" / subproblems overlap | DP |
| "Is X achievable?" + monotonic feasibility | Binary Search on Answer |
| "Partition into k groups with min max" | Binary Search on Answer |
| 0/1 choices with weight/capacity | Knapsack DP |
| "Longest increasing subsequence" | LIS Pattern |

---

## Branch 5: Intervals, Geometry, and Special Problems

```
                    INTERVALS / RANGES
                          â”‚
          â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
          â”‚               â”‚              â”‚
          â–¼               â–¼              â–¼
   "Merge / combine"  "Count events   "Schedule non-
    overlapping        at time t"       overlapping"
          â”‚               â”‚              â”‚
          â–¼               â–¼              â–¼
  â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â” â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â” â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
  â”‚ Merge        â”‚ â”‚ Sweep Line / â”‚ â”‚ Greedy       â”‚
  â”‚ Intervals    â”‚ â”‚ Difference   â”‚ â”‚ (sort by end)â”‚
  â”‚ (sort+merge) â”‚ â”‚ Array        â”‚ â”‚              â”‚
  â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜ â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜ â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```

---

## The "I'm Stuck" Recovery Flowchart

When you've been staring at a problem for 5+ minutes with no idea:

```
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚                        I'M STUCK                            â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                          â”‚
                          â–¼
              â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
              â”‚ 1. Re-read constraintsâ”‚  â† Most common fix
              â”‚    What complexity    â”‚
              â”‚    is expected?       â”‚
              â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                          â”‚
                          â–¼
              â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
              â”‚ 2. Write brute force  â”‚  â† Even if you know
              â”‚    Can you see a      â”‚     it won't pass,
              â”‚    repeated pattern?  â”‚     it reveals structure
              â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                          â”‚
                          â–¼
              â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
              â”‚ 3. Try small examples â”‚  â† n=3, n=4, n=5
              â”‚    Do you see         â”‚     Look for patterns
              â”‚    repeated work?     â”‚     in the output
              â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                          â”‚
                          â–¼
              â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
              â”‚ 4. Think about what   â”‚
              â”‚    information you    â”‚  â† "What if I knew
              â”‚    WISH you had       â”‚     the answer for
              â”‚                       â”‚     n-1 elements?"
              â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                          â”‚
                          â–¼
              â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
              â”‚ 5. Consider the       â”‚
              â”‚    OPPOSITE question  â”‚  â† "Instead of finding
              â”‚                       â”‚     max, what if I
              â”‚                       â”‚     binary search it?"
              â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                          â”‚
                          â–¼
              â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
              â”‚ 6. Sort the input     â”‚  â† Sorting often
              â”‚    Does anything      â”‚     reveals structure
              â”‚    become clearer?    â”‚     you couldn't see
              â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```

---

## Quick Pattern Selector by Constraint + Question Type

This is the most practical table in the handbook. Cross-reference the constraint size with the question type:

| Question Type â†“ \ Constraint â†’ | n â‰¤ 20 | n â‰¤ 5000 | n â‰¤ 10âµ | n â‰¤ 10â¶ |
|---|---|---|---|---|
| **Subarray sum/property** | Brute Force | Prefix Sum | Prefix Sum + Hash | Prefix Sum |
| **Longest substring** | Brute Force | O(nÂ²) check | Variable Sliding Window | Variable Sliding Window |
| **Find target in sorted** | Linear | Binary Search | Binary Search | Binary Search |
| **Shortest path** | BFS/DFS | Dijkstra/BFS | Dijkstra | Dijkstra + optimized |
| **All subsets** | Bitmask | â€” | â€” | â€” |
| **Permutations** | Backtracking | â€” | â€” | â€” |
| **Count ways** | Recursion | DP | DP + optimization | DP + math |
| **Min-max optimization** | Brute Force | DP | BS on Answer | BS on Answer |
| **Connected components** | DFS | DFS/BFS | Union Find | Union Find |
| **Range queries** | Brute Force | Prefix Sum | Segment/Fenwick | Segment/Fenwick |

---

## Common Multi-Pattern Combinations

Real OA problems often combine patterns. Here are the most common combos:

| Combination | Example Problem | Why Both? |
|---|---|---|
| **Sorting + Two Pointers** | 3Sum (LeetCode #15) | Sort to enable directional pointer movement |
| **Binary Search + Greedy** | Aggressive Cows / Allocate Books | BS determines the answer; greedy validates |
| **BFS + Hashing** | Word Ladder (LeetCode #127) | BFS for shortest path; hash set for dictionary |
| **DFS + Backtracking** | N-Queens (LeetCode #51) | DFS traversal with constraint backtracking |
| **Prefix Sum + Hashing** | Subarray Sum = K (LeetCode #560) | Prefix sum converts to "find pair" â†’ hash map |
| **Sorting + Greedy + Heap** | Meeting Rooms II (LeetCode #253) | Sort by start; heap tracks earliest end time |
| **DP + Binary Search** | LIS O(n log n) (LeetCode #300) | DP for structure; binary search for position |
| **Graph + DP** | Shortest path in DAG | Topological sort + DP relaxation |
| **Sliding Window + Hash Map** | Longest Substring Without Repeat (#3) | Window for range; hash map for tracking |
| **Union Find + Sorting** | Kruskal's MST | Sort edges; union find for cycle detection |

---

## Your First 3 Minutes on Any OA Problem

```
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚         MINUTE 0:00 â€“ 1:00              â”‚
â”‚                                         â”‚
â”‚  â€¢ Read the problem ONCE completely     â”‚
â”‚  â€¢ Identify: input type, output type    â”‚
â”‚  â€¢ Note the constraints (n = ?)         â”‚
â”‚  â€¢ Circle keywords (max, min, shortest, â”‚
â”‚    count, subarray, path, etc.)         â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                      â”‚
                      â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚         MINUTE 1:00 â€“ 2:00              â”‚
â”‚                                         â”‚
â”‚  â€¢ Determine expected complexity        â”‚
â”‚    from constraint table                â”‚
â”‚  â€¢ Match keywords to patterns           â”‚
â”‚    using Recognition Keywords Table     â”‚
â”‚  â€¢ Narrow down to 2-3 candidate        â”‚
â”‚    patterns                             â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                      â”‚
                      â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚         MINUTE 2:00 â€“ 3:00              â”‚
â”‚                                         â”‚
â”‚  â€¢ Think through brute force mentally   â”‚
â”‚  â€¢ Identify the bottleneck              â”‚
â”‚  â€¢ Confirm which pattern eliminates     â”‚
â”‚    the bottleneck                       â”‚
â”‚  â€¢ Start coding (template first)        â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```

---

## What's Next?

Now that you have the framework for recognizing patterns, the rest of this handbook teaches you each pattern in depth. Starting from Chapter 02 (Fundamentals), every pattern follows the same comprehensive 20-section template so you can build both **understanding** and **muscle memory**.

The patterns are ordered from fundamental to advanced. Each builds on concepts from earlier chapters, but every chapter is also self-contained â€” you can jump to any pattern you need.

---

*Previous chapter: [00 â€” Front Matter](00_Front_Matter.md)*
*Next chapter: [02 â€” Fundamentals](02_Fundamentals.md)*
<!--
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DSA PATTERN HANDBOOK â€” FUNDAMENTALS
  Chapter 02
  
  Patterns in this chapter:
    Pattern 1: Brute Force Thinking
    Pattern 2: Prefix Sum
    Pattern 3: Difference Array
    Pattern 4: Hashing
    Pattern 5: Frequency Counting
  
  Formatting Rules (internal reference for consistency):
  - Heading hierarchy: # (chapter) â†’ ## (pattern) â†’ ### (section)
  - 20 sections per pattern
  - Complexity notation: O(n), O(n log n), O(nÂ²)
  - Star rating: â˜… (filled), â˜† (empty), scale 1â€“5
  - Code style: C++17, 4-space indent, STL preferred
  - Tables: GitHub-flavored markdown pipe tables
  - Diagrams: ASCII art in code blocks
  - Problem references: "LeetCode #ID â€” Name" format
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-->

# Chapter 2: Fundamentals

This chapter covers the foundational patterns that every other pattern in this handbook builds upon. Master these first â€” they appear in virtually every OA, often as sub-steps of more complex patterns.

---
---

# Pattern 1: Brute Force Thinking

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  BRUTE FORCE THINKING
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: Every company â€” this is the starting point
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Every problem. Brute force isn't a "pattern" in the traditional sense â€” it's a **thinking methodology**. It answers the question: *"If I had unlimited time, how would I check every possible answer?"*

Why is brute force important?

1. **It's always correct** â€” by checking everything, you can't miss the answer.
2. **It reveals structure** â€” the brute force often shows you what work is being repeated, which guides optimization.
3. **It's your safety net** â€” if you can't find the optimal solution in an OA, a brute force that handles small inputs is better than nothing.
4. **Interviewers expect it** â€” "Start with brute force, then optimize" is the standard interview workflow.

> **The golden rule**: Never optimize before you can write the brute force. If you can't brute force it, you don't understand the problem.

---

## 2. Pattern Identification Checklist

- [x] Can you enumerate all possible answers? (Almost always yes)
- [x] Is the constraint small enough for exponential or polynomial brute force? (Check n â‰¤ 20 for exponential, n â‰¤ 1000 for O(nÂ²))
- [x] Are you stuck on a problem and need a starting point?
- [x] Do you need to verify your optimized solution's correctness?

**If you checked any of these**: Start with brute force before anything else.

---

## 3. Recognition Keywords

Brute force applies to literally every problem. But it's **especially the final answer** when you see:

| Keyword / Constraint | Why Brute Force Works |
|---|---|
| n â‰¤ 10 | O(n!) â‰ˆ 3.6M â€” feasible |
| n â‰¤ 15 | O(2â¿) â‰ˆ 32K â€” feasible |
| n â‰¤ 20 | O(2â¿) â‰ˆ 1M â€” feasible |
| "All permutations" | Inherently O(n!) |
| "All subsets" | Inherently O(2â¿) |
| "Check every pair" (n â‰¤ 5000) | O(nÂ²) â‰ˆ 25M â€” usually feasible |

---

## 4. Constraint Analysis

| Constraint | Brute Force Complexity | Feasible? |
|---|---|---|
| n â‰¤ 8 | O(n!) = 40,320 | âœ… Easily |
| n â‰¤ 10 | O(n!) = 3,628,800 | âœ… Yes |
| n â‰¤ 15 | O(2â¿ Â· n) â‰ˆ 500K | âœ… Yes |
| n â‰¤ 20 | O(2â¿) â‰ˆ 1M | âœ… Yes |
| n â‰¤ 25 | O(2â¿) â‰ˆ 33M | âš ï¸ Tight |
| n â‰¤ 1000 | O(nÂ²) = 1M | âœ… Yes |
| n â‰¤ 5000 | O(nÂ²) = 25M | âš ï¸ Tight |
| n â‰¤ 10,000 | O(nÂ²) = 100M | âŒ Too slow |
| n â‰¤ 100,000 | Need O(n log n) or O(n) | âŒ Much too slow |

---

## 5. Evolution of Thought

This pattern IS the brute force, so the "evolution" here is about **how brute force thinking leads to better patterns**:

### Step 1: State the Brute Force
> "I'll try every possible combination/pair/subset and check which one satisfies the condition."

### Step 2: Identify Redundant Work
> "I'm recomputing the same subarray sum over and over." â†’ **Prefix Sum**
> "I'm scanning the same elements repeatedly to find a value." â†’ **Hashing**
> "I'm checking overlapping windows from scratch." â†’ **Sliding Window**
> "I'm solving the same subproblem multiple times." â†’ **DP**

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
- **Single loop**: Check each element â€” O(n)
- **Nested loops**: Check each pair â€” O(nÂ²)
- **Triple loops**: Check each triple â€” O(nÂ³)
- **Subset enumeration**: Check each subset â€” O(2â¿)
- **Permutation enumeration**: Check each ordering â€” O(n!)

---

## 7. Reusable C++ Template

```cpp
// Template: Brute Force â€” Check All Pairs
// Use when: n â‰¤ 5000 and you need to find a pair with some property
// Time: O(nÂ²), Space: O(1)

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
// Template: Brute Force â€” Enumerate All Subsets (Bitmask)
// Use when: n â‰¤ 20
// Time: O(2â¿ Â· n), Space: O(n)

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
| All pairs | O(nÂ²) | O(1) | Two nested loops, each up to n |
| All triples | O(nÂ³) | O(1) | Three nested loops |
| All subsets | O(2â¿ Â· n) | O(n) | 2â¿ subsets, each takes O(n) to evaluate |
| All permutations | O(n! Â· n) | O(n) | n! orderings, each takes O(n) to evaluate |
| All subarrays | O(nÂ²) to enumerate, O(nÂ³) with sum | O(1) | nÂ² subarrays, each up to length n |

---

## 9. Dry Run

**Problem**: Find two numbers in `[2, 7, 11, 15]` that sum to `9`.

```
Brute Force: Check all pairs

Pair (0,1): nums[0] + nums[1] = 2 + 7 = 9  âœ“ FOUND!
    â†’ Return [0, 1]

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
| Not writing brute force because "it's too slow" | Write it anyway â€” it guides optimization |
| Off-by-one in subset bitmask | `mask < (1 << n)`, not `mask <= (1 << n)` |
| Trying to optimize before understanding the problem | Write brute force first, THEN optimize |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| n > 5000 and O(nÂ²) is required | TLE guaranteed | Look for O(n log n) or O(n) patterns |
| n > 20 and subset enumeration | 2Â²â° â‰ˆ 1M is the limit | DP, Greedy, or Binary Search |
| Problem explicitly asks for "efficient" solution | Brute force won't earn full marks | Optimize after identifying the pattern |

**However**: Even when brute force won't pass, **write it mentally** to understand the structure.

---

## 12. Medium-Level Example Problem

**LeetCode #1 â€” Two Sum**

> Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

**Why this represents the pattern well**: It's the simplest possible "check all pairs" problem. The brute force is O(nÂ²), and it naturally leads to the hash map optimization (O(n)) â€” showing how brute force thinking evolves into a better solution.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. **"What's the simplest approach?"** â†’ Check every pair of numbers.
2. **Write the brute force**: Two nested loops, check if `nums[i] + nums[j] == target`.
3. **Identify redundancy**: For each `nums[i]`, I'm scanning the entire array to find `target - nums[i]`. That's a lookup problem.
4. **Optimize**: Use a hash map to store numbers I've already seen. For each `nums[i]`, check if `target - nums[i]` is in the map.
5. **Complexity drops**: O(nÂ²) â†’ O(n).

**Key insight**: The brute force revealed that the inner loop is just "searching for a complement." Hash maps do searches in O(1).

---

## 14. C++ Implementation

```cpp
#include <vector>
#include <unordered_map>
using namespace std;

// Brute Force: O(nÂ²) time, O(1) space
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
    unordered_map<int, int> seen;  // value â†’ index

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
| "What if the array is sorted?" | Use Two Pointers instead of hash map â€” O(1) space |
| "What if there are multiple valid pairs?" | Return all pairs; use hash map but don't stop at first match |
| "What if you need three numbers summing to target?" | 3Sum â€” sort + fix one element + two pointers for remaining |
| "Can duplicate indices be used?" | Change inner loop to start from `j = 0` (or `j = i`) |

---

## 16. Variations

| Variation | Change |
|---|---|
| **Three Sum** | Fix one element, reduce to Two Sum on the rest |
| **Four Sum** | Fix two elements, reduce to Two Sum |
| **Subset Sum** | Generalization to k elements â€” use backtracking or DP |
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
| 1 | LeetCode #1 â€” Two Sum | Easy | Basic pair enumeration â†’ hash optimization |
| 2 | LeetCode #217 â€” Contains Duplicate | Easy | Brute O(nÂ²) vs Hash O(n) |
| 3 | LeetCode #15 â€” 3Sum | Medium | Brute O(nÂ³) â†’ Sort + Two Pointers O(nÂ²) |
| 4 | LeetCode #78 â€” Subsets | Medium | Subset enumeration via bitmask |
| 5 | LeetCode #46 â€” Permutations | Medium | Permutation enumeration via backtracking |

---

## 19. Memory Trick

> ðŸ§  **"BFT â€” Brute Force Then Transform"**
>
> Always write the Brute Force first, Then Transform it into the optimal solution. The transformation IS the pattern you're looking for.

---
---

# Pattern 2: Prefix Sum

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  PREFIX SUM
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Flipkart
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Imagine you're a cashier keeping a running total. Every time a customer buys something, you add to the total. If someone asks, "How much was spent between the 3rd and 7th customer?", you don't re-add items 3 through 7. You subtract: `total_after_7 - total_after_2`.

That's prefix sum.

**Why was this pattern invented?**

The brute force for "sum of subarray from index l to r" is O(r - l + 1) per query. If you have Q queries, it's O(n Â· Q). With prefix sum, you precompute all cumulative sums once (O(n)), and then answer each query in O(1). Total: O(n + Q).

It transforms a **repeated computation** problem into a **precomputation + lookup** problem.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem involve **subarray sums** or **range sums**?
- [ ] Are there **multiple queries** asking for sums of different ranges?
- [ ] Can the problem be reduced to "find a subarray with sum = k"?
- [ ] Does the problem involve **cumulative** properties (sum, XOR, product)?
- [ ] Would precomputing partial results speed up repeated calculations?

**If you checked 2+ of these â†’ Prefix Sum is likely the right pattern.**

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
| n â‰¤ 1000, Q â‰¤ 1000 | O(n Â· Q) = 10â¶ âœ… | O(n + Q) âœ… | Both work, but prefix sum is cleaner |
| n â‰¤ 10âµ, Q â‰¤ 10âµ | O(n Â· Q) = 10Â¹â° âŒ | O(n + Q) = 2 Ã— 10âµ âœ… | Prefix sum required |
| n â‰¤ 10â¶, single query | O(n) âœ… | O(n) âœ… | Prefix sum not needed for single query |

**Rule**: Prefix sum shines when you have **multiple range queries** or need to **convert subarray problems to point problems**.

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "To find the sum from index l to r, I'll loop from l to r and add everything."
> Time per query: O(n). Total for Q queries: O(n Â· Q).

### Step 2: Key Observation
> "Wait â€” the sum from l to r is actually `(sum of first r+1 elements) - (sum of first l elements)`. If I precompute all prefix sums, I can answer in O(1)."

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
// Template: Prefix Sum â€” Range Sum Queries
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

// â”€â”€â”€ Usage â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
// vector<int> nums = {1, 2, 3, 4, 5};
// PrefixSum ps(nums);
// ps.rangeSum(1, 3);  // sum of nums[1..3] = 2+3+4 = 9
```

---

## 8. Complexity Analysis

| Operation | Time | Space | Why |
|---|---|---|---|
| Build prefix array | O(n) | O(n) | Single pass through array |
| Answer one range query | O(1) | â€” | Simple subtraction |
| Answer Q range queries | O(Q) | â€” | O(1) each |
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

Step 2: Query sum(1, 3) â†’ sum of nums[1..3] = 1 + 4 + 1 = 6
  prefix[3+1] - prefix[1] = prefix[4] - prefix[1] = 9 - 3 = 6  âœ“

Step 3: Query sum(0, 4) â†’ sum of entire array = 14
  prefix[4+1] - prefix[0] = prefix[5] - prefix[0] = 14 - 0 = 14  âœ“

Step 4: Query sum(2, 2) â†’ single element nums[2] = 4
  prefix[2+1] - prefix[2] = prefix[3] - prefix[2] = 8 - 4 = 4  âœ“
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
| Array elements change between queries | Prefix sum must be rebuilt â€” O(n) per update | Fenwick Tree or Segment Tree |
| You need range minimum/maximum, not sum | Prefix sum only works for associative operations with inverses | Sparse Table, Segment Tree |
| Single query on the entire array | Just sum directly â€” no need for prefix array | Simple loop |
| Problem asks for subarray product | Division doesn't work well with 0s | Use logarithms or sliding window |

---

## 12. Medium-Level Example Problem

**LeetCode #560 â€” Subarray Sum Equals K**

> Given an integer array `nums` and an integer `k`, return the total number of subarrays whose sum equals `k`.

**Why this represents the pattern well**: It combines prefix sum with hashing. You can't use a simple sliding window because elements can be negative. The prefix sum converts "subarray sum = k" into "find two prefix values that differ by k" â€” a classic pattern transformation.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. **Brute force**: Check all O(nÂ²) subarrays, compute each sum â†’ O(nÂ²) or O(nÂ³).
2. **Observation**: Sum of subarray `[l, r]` = `prefix[r+1] - prefix[l]`. We want this to equal `k`.
3. **Rearrange**: `prefix[r+1] - k = prefix[l]`. So for each `r`, we need to count how many earlier prefix values equal `prefix[r+1] - k`.
4. **This is a lookup problem!** Use a hash map to count occurrences of each prefix sum seen so far.
5. **Algorithm**: Walk through the array, maintaining a running prefix sum. At each step, check if `prefix_sum - k` exists in the hash map.

**Key insight**: Prefix sum transforms a "subarray sum" problem into a "find a complement" problem â€” just like Two Sum!

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
| "What if elements are all positive?" | Can use Variable Sliding Window instead â€” O(1) space |
| "Find the longest subarray with sum = k?" | Store first occurrence of each prefix sum instead of count |
| "What if we need sum divisible by k?" | Use `currentSum % k` as the key (handle negative mods) |
| "2D version: submatrix sum?" | Use 2D prefix sum: `prefix[i][j] = sum of submatrix (0,0) to (i-1,j-1)` |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **2D Prefix Sum** | `prefix[i][j]` stores sum of rectangle from (0,0) to (i-1,j-1). Query uses inclusion-exclusion. |
| **Prefix XOR** | Same idea but with XOR instead of sum. XOR is its own inverse. |
| **Prefix Product** | Use multiplication. Beware of zeros â€” can't divide by zero. |
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
| **Difference Array** | "Inverse" of prefix sum â€” prefix sum of difference array = original array |
| **Sliding Window** | Both handle subarray queries; sliding window for optimization constraints, prefix sum for exact sums |
| **Hashing** | Prefix sum + hashing is a powerful combo for "subarray sum = k" |
| **Fenwick Tree** | Dynamic version of prefix sum that supports updates |
| **Segment Tree** | Generalized prefix sum for arbitrary associative operations |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #303 â€” Range Sum Query (Immutable) | Easy | Direct prefix sum application |
| 2 | LeetCode #724 â€” Find Pivot Index | Easy | Prefix sum = Suffix sum |
| 3 | LeetCode #560 â€” Subarray Sum Equals K | Medium | Prefix sum + hash map |
| 4 | LeetCode #304 â€” Range Sum Query 2D (Immutable) | Medium | 2D prefix sum |
| 5 | LeetCode #437 â€” Path Sum III | Hard | Prefix sum on a tree path |

---

## 19. Memory Trick

> ðŸ§  **"Prefix Sum = Running Total Receipt"**
>
> Think of it like a receipt at a grocery store. Each line shows the running total. To find how much you spent on items 3â€“7, subtract the total after item 2 from the total after item 7. You never re-scan the items.

---
---

# Pattern 3: Difference Array

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DIFFERENCE ARRAY
  OA Frequency: â˜…â˜…â˜…â˜†â˜†
  Companies: Google | Amazon | Microsoft | Uber | Codeforces favorites
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Imagine you're a teacher assigning bonus points. You say: "Students 3 through 7, you all get +5 points." Then: "Students 2 through 5, you all get +3 points." And so on, for many such updates.

The brute force would be: for each update, loop through the range and add the value. If you have Q updates on an array of size n, that's O(n Â· Q).

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

**If you checked 2+ of these â†’ Difference Array is likely the right pattern.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points to Difference Array |
|---|---|
| "Add k to all elements from index l to r" | Direct application |
| "Increment range" / "range update" | Core use case |
| "Number of overlapping intervals at each point" | Sweep line variant of difference array |
| "Bus/flight booking" / "how many passengers at each stop" | Classic problem |
| "Apply multiple operations, find final result" | Multiple range updates â†’ difference array |

---

## 4. Constraint Analysis

| Constraint | Brute Force (per update) | Difference Array | Verdict |
|---|---|---|---|
| n â‰¤ 1000, Q â‰¤ 100 | O(n Â· Q) = 10âµ âœ… | O(n + Q) âœ… | Both work |
| n â‰¤ 10âµ, Q â‰¤ 10âµ | O(n Â· Q) = 10Â¹â° âŒ | O(n + Q) = 2 Ã— 10âµ âœ… | Difference array required |
| n â‰¤ 10â¶, Q â‰¤ 10â¶ | O(n Â· Q) = 10Â¹Â² âŒ | O(n + Q) = 2 Ã— 10â¶ âœ… | Difference array required |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each update (l, r, val), loop from l to r and add val to each element."
> Time per update: O(n). Total: O(n Â· Q).

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
// Template: Difference Array â€” Range Updates + Final Reconstruction
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

// â”€â”€â”€ Usage â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
// DifferenceArray da(5);
// da.rangeUpdate(1, 3, 10);  // add 10 to indices 1,2,3
// da.rangeUpdate(2, 4, 5);   // add 5 to indices 2,3,4
// auto result = da.reconstruct();  // [0, 10, 15, 15, 5]
```

---

## 8. Complexity Analysis

| Operation | Time | Space | Why |
|---|---|---|---|
| One range update | O(1) | â€” | Two array writes |
| Q range updates | O(Q) | â€” | O(1) each |
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
  Index 0: -1          (only op 3 applies: -1) âœ“
  Index 1: +2 -1 = 1   (op 1 and op 3) âœ“
  Index 2: +2 +3 -1 = 4 (all three ops) âœ“
  Index 3: +2 +3 +1 = ?
  
  Wait â€” let me re-check operation 3: add -1 to [0, 2]
  Index 3: +2 (op1) + 3 (op2) = 5. Op 3 doesn't apply (range is [0,2]).
  
  diff[3] -= (-1) â†’ diff[3] += 1. So running at index 3 = 4 + 1 = 5. âœ“
  Index 4: +3 (op2 only) = 3. Running = 5 - 2 = 3. âœ“
  
  Final result: [-1, 1, 4, 5, 3]  âœ“
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Forgetting `diff[r+1] -= val` | Always do both operations â€” they're a pair |
| Array out of bounds when `r = n-1` | Make diff size `n+1` (extra element at end) |
| Using difference array when you need intermediate states | Difference array only gives the final result after ALL updates |
| Confusing with prefix sum | Prefix sum = range queries; Difference array = range updates |
| Integer overflow | Use `long long` when values can accumulate |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Need array state after EACH update (not just final) | Difference array only works for batch updates | Segment Tree with lazy propagation |
| Point updates + range queries | Wrong direction â€” this handles range updates + point queries | Fenwick Tree / Segment Tree |
| Need to query during updates | Must reconstruct first | Segment Tree |

---

## 12. Medium-Level Example Problem

**LeetCode #1109 â€” Corporate Flight Bookings**

> There are `n` flights labeled from 1 to n. You are given bookings where `bookings[i] = [first_i, last_i, seats_i]` represents a booking for every flight from `first_i` to `last_i` (inclusive) with `seats_i` seats reserved. Return an array of length n where answer[i] is the total seats reserved for flight i.

**Why this represents the pattern well**: It's literally "apply multiple range updates and return the final array" â€” a textbook difference array problem.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. **Read the problem**: Multiple range updates (add `seats` to range `[first, last]`), return final array.
2. **Brute force**: For each booking, loop from `first` to `last` and add seats. O(n Â· Q).
3. **Recognize the pattern**: "Multiple range additions, query final state" â†’ Difference Array.
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
| "What if bookings can be cancelled (subtract)?" | Same approach â€” use negative values |
| "What if you need to answer 'max seats on any flight'?" | After reconstruction, scan for max â€” still O(n + Q) |
| "What if flights are not 1 to n but arbitrary times?" | Use coordinate compression first, then difference array |
| "What if updates happen online (interleaved with queries)?" | Switch to Segment Tree with lazy propagation |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **2D Difference Array** | For adding val to a submatrix (r1,c1)â†’(r2,c2). Four updates per operation. |
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
| 1 | LeetCode #1094 â€” Car Pooling | Easy | Range add/subtract for passengers |
| 2 | LeetCode #1109 â€” Corporate Flight Bookings | Medium | Direct difference array application |
| 3 | LeetCode #370 â€” Range Addition | Medium | Pure difference array (premium) |
| 4 | LeetCode #253 â€” Meeting Rooms II | Medium | Sweep line variant |
| 5 | Codeforces #295A â€” Greg and Array | Hard | Nested difference arrays |

---

## 19. Memory Trick

> ðŸ§  **"Bookends"**
>
> A difference array update is like placing bookends on a shelf. Put a +val bookend at the start of the range and a -val bookend at the end. When you sweep across the shelf (prefix sum), only the items between the bookends get the value.

---
---

# Pattern 4: Hashing

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  HASHING
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Goldman Sachs | Flipkart
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

The fundamental problem: **"Have I seen this value before?"** and **"How quickly can I look it up?"**

Without hashing, searching for an element in an unsorted array takes O(n). With a hash map/set, it takes O(1) on average.

**Why was this pattern invented?**

Many brute force solutions have an inner loop that's just "searching for something." If that search is O(n), the total is O(nÂ²). Hashing reduces the search to O(1), making the total O(n).

Hashing converts **search problems into lookup problems**.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem require **checking if a value exists** in a collection?
- [ ] Do you need to **count occurrences** of values?
- [ ] Is the inner loop of your brute force essentially a **search/lookup**?
- [ ] Do you need to **map** values to indices, frequencies, or other values?
- [ ] Does the problem involve **finding duplicates**, complements, or pairs?
- [ ] Would a "dictionary" / "lookup table" make the problem trivial?

**If you checked 2+ of these â†’ Hashing is likely the right pattern.**

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
| n â‰¤ 1000 | O(nÂ²) = 10â¶ âœ… | O(n) âœ… | Both work |
| n â‰¤ 10âµ | O(nÂ²) = 10Â¹â° âŒ | O(n) âœ… | Hashing required |
| n â‰¤ 10â¶ | O(nÂ²) = 10Â¹Â² âŒ | O(n) âœ… | Hashing required |

**Warning**: `unordered_map` has O(n) worst case due to hash collisions. In competitive programming, adversarial inputs can cause this. In interviews, average case O(1) is fine.

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each element, scan the rest of the array to find what I need." â†’ O(nÂ²).

### Step 2: Key Observation
> "The inner scan is just 'Does value X exist in my collection?' That's a lookup, not a computation."

### Step 3: Optimization
> "If I store elements in a hash set/map, lookups become O(1)."

### Step 4: Optimal Solution
> "Single pass: for each element, check the hash table, then add the element to the hash table." â†’ O(n).

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
// Template: Hash Map â€” Find Complement / Match
// Use when: for each element, check if a related value was seen before
// Time: O(n), Space: O(n)

#include <vector>
#include <unordered_map>
#include <unordered_set>
using namespace std;

// Pattern A: Hash Set â€” Existence check
bool hasDuplicate(vector<int>& nums) {
    unordered_set<int> seen;
    for (int num : nums) {
        if (seen.count(num)) return true;  // MODIFY: check condition
        seen.insert(num);                  // MODIFY: what to store
    }
    return false;
}

// Pattern B: Hash Map â€” Value lookup
vector<int> findComplement(vector<int>& nums, int target) {
    unordered_map<int, int> seen;  // value â†’ index
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
| **Typical algorithm** | **O(n)** | **O(nÂ²)** | n operations Ã— O(1) avg each |
| Space | O(n) | O(n) | Storing up to n elements |

**For interviews**: Always state "O(1) average, O(n) worst case" for hash operations. Interviewers appreciate the distinction.

---

## 9. Dry Run

**Problem**: Find if array `[3, 1, 4, 1, 5]` contains a duplicate.

```
Hash Set: {}

i=0: num=3, not in set â†’ add 3. Set: {3}
i=1: num=1, not in set â†’ add 1. Set: {3, 1}
i=2: num=4, not in set â†’ add 4. Set: {3, 1, 4}
i=3: num=1, IN SET âœ“ â†’ return true (duplicate found!)

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

**LeetCode #49 â€” Group Anagrams**

> Given an array of strings `strs`, group the anagrams together. An anagram is a word formed by rearranging the letters.

**Why this represents the pattern well**: It requires designing a clever hash key â€” sorting each string to create a canonical form, then grouping by that key. It tests both the hashing pattern and key design skills.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. **What defines an anagram?** Two words with the same characters in the same frequencies.
2. **How to check if two words are anagrams?** Sort them â€” if they become the same string, they're anagrams.
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
// Time: O(n Â· k log k) where k = max string length, Space: O(n Â· k)
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
| "Can you do it without sorting each string?" | Use character frequency as key: "a2b1c1" format â†’ O(nÂ·k) instead of O(nÂ·k log k) |
| "What if strings are very long?" | Frequency counting is better than sorting for long strings |
| "What if we only need to check if two specific strings are anagrams?" | Just compare sorted versions or character counts â€” no hash map needed |
| "How would you handle Unicode characters?" | Use `unordered_map<char, int>` for frequency instead of fixed array |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Two Sum variants** | Hash map stores complement â†’ index |
| **Frequency Map** | Count occurrences instead of just existence (see Pattern 5) |
| **Rolling Hash** | Compute hash value mathematically for substrings â€” O(1) update |
| **Hash Map of Hash Maps** | For problems requiring nested grouping |

---

## 17. Related Patterns

| Pattern | Relationship |
|---|---|
| **Frequency Counting** | Special case of hashing where value = count |
| **Prefix Sum + Hashing** | Combined pattern for subarray sum problems |
| **Sliding Window + Hashing** | Hash map/set tracks elements in the current window |
| **Two Pointers** | Both reduce O(nÂ²) to O(n), but two pointers requires sorted input |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #217 â€” Contains Duplicate | Easy | Basic hash set |
| 2 | LeetCode #242 â€” Valid Anagram | Easy | Character frequency comparison |
| 3 | LeetCode #49 â€” Group Anagrams | Medium | Key design in hash map |
| 4 | LeetCode #128 â€” Longest Consecutive Sequence | Medium | Hash set for O(n) sequence finding |
| 5 | LeetCode #41 â€” First Missing Positive | Hard | In-place hashing (array as hash map) |

---

## 19. Memory Trick

> ðŸ§  **"Hash = Speed Dial"**
>
> A hash map is like your phone's speed dial. Instead of scrolling through all contacts (O(n) search), you press one button (O(1) lookup). The key is the speed dial number, the value is the contact. Design your speed dial (key) wisely.

---
---

# Pattern 5: Frequency Counting

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  FREQUENCY COUNTING
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Flipkart | Goldman Sachs
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
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

**If you checked 2+ of these â†’ Frequency Counting is likely the right pattern.**

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
| Values up to 10â¶ | Use array if memory allows, else hash map | Array is faster |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each unique element, count how many times it appears by scanning the whole array." â†’ O(nÂ²).

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

i=0: num=1 â†’ freq={1:1}
i=1: num=3 â†’ freq={1:1, 3:1}
i=2: num=2 â†’ freq={1:1, 3:1, 2:1}
i=3: num=1 â†’ freq={1:2, 3:1, 2:1}
i=4: num=4 â†’ freq={1:2, 3:1, 2:1, 4:1}
i=5: num=1 â†’ freq={1:3, 3:1, 2:1, 4:1}
i=6: num=3 â†’ freq={1:3, 3:2, 2:1, 4:1}

Scan for max: 1 appears 3 times â†’ answer = 1
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Using `freq[c]` without initializing (in arrays) | Always `fill()` or `memset()` to 0 first |
| Off-by-one with character indexing (`c - 'a'`) | Verify: 'a'-'a'=0, 'z'-'a'=25 â†’ array size 26 |
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

**LeetCode #347 â€” Top K Frequent Elements**

> Given an integer array `nums` and an integer `k`, return the `k` most frequent elements.

**Why this represents the pattern well**: It combines frequency counting with a follow-up question: "Now that you have the frequencies, how do you efficiently find the top k?" This naturally leads to discussing heaps, bucket sort, or quickselect.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. **Step 1**: Count frequencies â†’ `unordered_map<int, int>`.
2. **Step 2**: How to find top k by frequency?
   - **Option A**: Sort all entries by frequency â†’ O(n log n)
   - **Option B**: Use a min-heap of size k â†’ O(n log k)
   - **Option C**: Bucket sort (bucket by frequency) â†’ O(n)
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
| "What if k = 1?" | Just find the element with max frequency â€” no need for bucket sort |
| "What if elements are streaming?" | Use a min-heap of size k; when a frequency changes, update the heap |
| "Return top k in sorted order of frequency?" | After bucket sort, results are naturally in decreasing frequency |
| "What if there are ties at the kth position?" | Clarify with interviewer â€” return any k, or all tied elements? |

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
| 1 | LeetCode #169 â€” Majority Element | Easy | Boyer-Moore or frequency counting |
| 2 | LeetCode #387 â€” First Unique Character in a String | Easy | Frequency array + scan |
| 3 | LeetCode #347 â€” Top K Frequent Elements | Medium | Frequency + bucket sort/heap |
| 4 | LeetCode #451 â€” Sort Characters by Frequency | Medium | Frequency + sorting |
| 5 | LeetCode #895 â€” Maximum Frequency Stack | Hard | Frequency tracking with stack |

---

## 19. Memory Trick

> ðŸ§  **"The Census Taker"**
>
> Frequency counting is like taking a census: walk through the population once, tally each type in a table. Once the census is done, you can answer any question about the population distribution instantly. The walk is O(n), every subsequent query is O(1).

---
---

*Previous chapter: [01 â€” Pattern Recognition Flowchart](01_Pattern_Recognition.md)*
*Next chapter: [03 â€” Sliding Window & Pointers](03_Sliding_Window_and_Pointers.md)*
<!--
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DSA PATTERN HANDBOOK â€” SLIDING WINDOW & POINTERS
  Chapter 03
  
  Patterns in this chapter:
    Pattern 6:  Sliding Window (Fixed)
    Pattern 7:  Sliding Window (Variable)
    Pattern 8:  Two Pointers
    Pattern 9:  Fast & Slow Pointer
  
  Formatting Rules (internal reference for consistency):
  - Heading hierarchy: # (chapter) â†’ ## (pattern) â†’ ### (section)
  - 20 sections per pattern
  - Complexity notation: O(n), O(n log n), O(nÂ²)
  - Star rating: â˜… (filled), â˜† (empty), scale 1â€“5
  - Code style: C++17, 4-space indent, STL preferred
  - Tables: GitHub-flavored markdown pipe tables
  - Diagrams: ASCII art in code blocks
  - Problem references: "LeetCode #ID â€” Name" format
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-->

# Chapter 3: Sliding Window & Pointers

This chapter covers four closely related patterns that all operate by maintaining **pointers** into a linear data structure (array or string). These patterns convert O(nÂ²) brute force into O(n) by avoiding redundant recomputation.

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
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  SLIDING WINDOW (FIXED SIZE)
  OA Frequency: â˜…â˜…â˜…â˜…â˜†
  Companies: Amazon | Microsoft | Google | Goldman Sachs | Adobe
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find the best/sum/property of all **contiguous subarrays of exactly size k**."

Imagine you're looking through a window that's exactly k elements wide. You slide this window one position to the right. The new window shares k-1 elements with the old window â€” only one element leaves and one enters. Why recompute everything from scratch?

**Why was this pattern invented?**

Brute force: For each of the n-k+1 starting positions, sum k elements â†’ O(nÂ·k).
Fixed sliding window: Maintain a running result, subtract the leaving element, add the entering element â†’ O(n).

The window "slides" by **adding one element on the right and removing one element on the left**.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem ask about **subarrays/substrings of fixed size k**?
- [ ] Is the property (sum, max, count) **incrementally updateable**?
- [ ] Does the problem say "contiguous" and specify a window size?
- [ ] Can you express "new window result" as "old result + entering - leaving"?

**If you checked 2+ of these â†’ Fixed Sliding Window.**

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

| Constraint | Brute Force O(nÂ·k) | Fixed Window O(n) | Verdict |
|---|---|---|---|
| n â‰¤ 1000, k â‰¤ 100 | 10âµ âœ… | 10Â³ âœ… | Both work |
| n â‰¤ 10âµ, k â‰¤ 10â´ | 10â¹ âŒ | 10âµ âœ… | Window required |
| n â‰¤ 10â¶, k â‰¤ 10â¶ | 10Â¹Â² âŒ | 10â¶ âœ… | Window required |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each starting index i, compute sum of elements from i to i+k-1." â†’ O(nÂ·k).

### Step 2: Key Observation
> "When the window slides from [i, i+k-1] to [i+1, i+k], only one element leaves (nums[i]) and one enters (nums[i+k]). The rest stay the same."

```
Window at position i:   [ . . X X X X X . . . ]
                              â†‘           â†‘
                              i         i+k-1

Window at position i+1: [ . . . X X X X X . . ]
                                â†‘           â†‘
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

**Why O(n) and not O(nÂ·k)?** Each element is processed at most twice: once when it enters the window, once when it leaves. Total work = 2n = O(n).

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
| Trying to use variable sliding window logic | Fixed window has NO condition for shrinking â€” both pointers move in lockstep |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Window size is not fixed | Size varies based on a condition | Variable Sliding Window |
| Need the maximum element in each window (not sum) | Sum can be updated in O(1); max cannot | Monotonic Deque |
| Non-contiguous subsets | Window must be contiguous | DP or backtracking |

---

## 12. Medium-Level Example Problem

**LeetCode #643 â€” Maximum Average Subarray I**

> Given an integer array `nums` and an integer `k`, find the contiguous subarray of length k that has the maximum average value and return this value.

**Why this represents the pattern well**: It's the purest fixed sliding window problem â€” find maximum sum of k consecutive elements, then divide by k.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. "Maximum average of k elements = maximum sum of k elements / k."
2. "This is 'find the maximum sum subarray of exactly size k.'"
3. "Brute force: check all n-k+1 windows â†’ O(nÂ·k)."
4. "Optimize: sliding window â€” maintain running sum, slide by adding/removing one element â†’ O(n)."

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
| "Find maximum element in each window of size k?" | Use Monotonic Deque â€” O(n) instead of O(nÂ·k) naive |
| "Find the number of distinct elements in each window?" | Use hash map, update on slide: add entering, decrement leaving |
| "What if k is not given â€” find k that maximizes average?" | Trick: average of entire array is always the answer (or binary search on k) |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Maximum in each window** | Use Monotonic Deque instead of simple sum |
| **Count windows with sum â‰¥ threshold** | Check condition at each slide position |
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
| 1 | LeetCode #643 â€” Maximum Average Subarray I | Easy | Pure fixed sliding window |
| 2 | LeetCode #1456 â€” Max Number of Vowels in Substring of Given Length | Medium | Count vowels in window of size k |
| 3 | LeetCode #1343 â€” Number of Sub-arrays of Size K and Average â‰¥ Threshold | Medium | Fixed window with condition |
| 4 | LeetCode #239 â€” Sliding Window Maximum | Hard | Fixed window + monotonic deque |
| 5 | LeetCode #480 â€” Sliding Window Median | Hard | Fixed window + two heaps |

---

## 19. Memory Trick

> ðŸ§  **"The Train Window"**
>
> You're on a train looking through a window. As the train moves forward by one seat, one new building comes into view and one old building disappears. You don't re-examine all buildings â€” you just update: +1 new, -1 old.

---
---

# Pattern 7: Sliding Window (Variable)

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  SLIDING WINDOW (VARIABLE SIZE)
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Uber | Flipkart
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find the longest/shortest **subarray or substring** that satisfies a condition."

Unlike the fixed window where k is given, here the window **grows and shrinks dynamically** based on whether the current window satisfies a condition. The right pointer expands the window to include more elements; the left pointer contracts it when the condition is violated.

**Why was this pattern invented?**

Brute force: Check all O(nÂ²) subarrays, validate each â†’ O(nÂ²) or O(nÂ³).
Variable sliding window: Each element is added once (right pointer) and removed at most once (left pointer) â†’ O(n).

The key invariant: **the window always represents a valid (or best candidate) subarray/substring.**

---

## 2. Pattern Identification Checklist

- [ ] Does the problem ask for the **longest/shortest subarray or substring** with a condition?
- [ ] Is the condition **monotonic** â€” adding elements can only violate it, and removing from the left can restore it (or vice versa)?
- [ ] Does the window only need to move **forward** (never backward)?
- [ ] Are all elements **non-negative** (for sum-based conditions)?

**If you checked 3+ of these â†’ Variable Sliding Window.**

> **Critical**: Variable sliding window requires **monotonicity**. If adding an element to the window can both improve AND worsen the condition (e.g., elements can be negative), the pattern may not work directly.

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Longest substring" | Classic variable window |
| "Shortest subarray with sum â‰¥ k" | Shrink when condition is met |
| "At most k distinct characters" | Condition based on distinct count |
| "Maximum length with sum â‰¤ k" | Expand while valid |
| "Minimum window containing all characters" | Shrink to minimize while valid |
| "Without repeating characters" | Window validity = no repeats |
| "Subarray with at most k zeros" | Count-based condition |

---

## 4. Constraint Analysis

| Constraint | Brute Force O(nÂ²) | Variable Window O(n) | Verdict |
|---|---|---|---|
| n â‰¤ 5000 | 2.5 Ã— 10â· âš ï¸ | 5000 âœ… | Window preferred |
| n â‰¤ 10âµ | 10Â¹â° âŒ | 10âµ âœ… | Window required |
| n â‰¤ 10â¶ | 10Â¹Â² âŒ | 10â¶ âœ… | Window required |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each starting index l, extend r as far as possible while the condition holds. Record the best window." â†’ O(nÂ²).

### Step 2: Key Observation
> "When I move l to l+1, the previous best r can only stay or increase â€” the valid window starting at l+1 is at least as far as the one starting at l. So I don't need to reset r."

```
If window [l, r] is valid and [l, r+1] is not:
  Move l â†’ l+1. Now [l+1, r] might be valid again.
  Don't reset r to l+1 â€” start checking from r onward.

l only moves right. r only moves right. Total moves = 2n.
```

### Step 3: Optimization
> "Use two pointers l and r. Expand r to grow the window. When the condition breaks, shrink by moving l. Both pointers only move forward."

### Step 4: Optimal Solution
> O(n) â€” each element is visited by r once and by l at most once.

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
// Template A: Variable Sliding Window â€” LONGEST valid window
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
// Template B: Variable Sliding Window â€” SHORTEST valid window
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

**Amortized analysis**: The left pointer moves at most n times total across all iterations of the outer loop. So inner while loop doesn't make it O(nÂ²) â€” it's O(n) amortized.

---

## 9. Dry Run

**Problem**: Longest subarray with sum â‰¤ 7 in `[3, 1, 2, 7, 4, 2, 1, 1, 5]`.

```
Array: [3, 1, 2, 7, 4, 2, 1, 1, 5]    target â‰¤ 7

right=0: add 3, sum=3 â‰¤ 7 âœ“    window=[3]        len=1, best=1
  [3, 1, 2, 7, 4, 2, 1, 1, 5]
   â†‘
   L,R

right=1: add 1, sum=4 â‰¤ 7 âœ“    window=[3,1]      len=2, best=2
  [3, 1, 2, 7, 4, 2, 1, 1, 5]
   â†‘  â†‘
   L  R

right=2: add 2, sum=6 â‰¤ 7 âœ“    window=[3,1,2]    len=3, best=3
  [3, 1, 2, 7, 4, 2, 1, 1, 5]
   â†‘     â†‘
   L     R

right=3: add 7, sum=13 > 7 âœ—   
  shrink: remove 3, sum=10, L=1. Still > 7.
  shrink: remove 1, sum=9, L=2.  Still > 7.
  shrink: remove 2, sum=7, L=3.  7 â‰¤ 7 âœ“
  window=[7]                     len=1, best=3 (unchanged)

right=4: add 4, sum=11 > 7 âœ—
  shrink: remove 7, sum=4, L=4.  4 â‰¤ 7 âœ“
  window=[4]                     len=1, best=3

right=5: add 2, sum=6 â‰¤ 7 âœ“    window=[4,2]      len=2, best=3

right=6: add 1, sum=7 â‰¤ 7 âœ“    window=[4,2,1]    len=3, best=3

right=7: add 1, sum=8 > 7 âœ—
  shrink: remove 4, sum=4, L=5.  4 â‰¤ 7 âœ“
  window=[2,1,1]                 len=3, best=3

right=8: add 5, sum=9 > 7 âœ—
  shrink: remove 2, sum=7, L=6.  7 â‰¤ 7 âœ“
  window=[1,1,5]                 len=3, best=3

Answer: 3
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Confusing longest vs shortest template | Longest: shrink while invalid, update after. Shortest: shrink while valid, update during. |
| Using sliding window when elements can be negative | Negative elements break monotonicity â€” window expansion can decrease sum. Use prefix sum + hash map instead. |
| Forgetting to update left pointer | Always increment left inside the while loop |
| Resetting right when left moves | Never! Both pointers only move forward. |
| Not handling empty result | Return 0 or -1 if no valid window exists |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Elements can be negative (for sum condition) | Adding negative elements can restore validity â†’ non-monotonic | Prefix Sum + Hash Map, or Deque approach |
| Need ALL valid subarrays (not just longest/shortest) | Window can skip valid subarrays | Brute force or DP |
| Condition is not about a contiguous subarray | Window is inherently contiguous | Subset DP / Backtracking |
| "Subarray sum exactly equals k" (with negatives) | Not monotonic | Prefix Sum + Hash Map |

---

## 12. Medium-Level Example Problem

**LeetCode #3 â€” Longest Substring Without Repeating Characters**

> Given a string `s`, find the length of the longest substring without repeating characters.

**Why this represents the pattern well**: The condition (no repeats) is monotonic â€” adding a character can only introduce a repeat, and removing from the left can only remove a repeat. It naturally uses a hash set to track characters in the window.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. **Brute force**: Check all O(nÂ²) substrings, verify each has no repeats â†’ O(nÂ³) or O(nÂ²).
2. **Observe**: If substring [l, r] has no repeats and we extend to r+1:
   - If s[r+1] is new â†’ still valid, expand.
   - If s[r+1] is a repeat â†’ shrink from left until the duplicate is removed.
3. **Data structure**: Use a hash set to track characters in the current window.
4. **Monotonicity**: Adding a character can create a repeat (invalid). Removing from left can only remove characters (restore validity). âœ“

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

**Alternative (faster â€” using last index):**
```cpp
#include <string>
#include <unordered_map>
#include <algorithm>
using namespace std;

int lengthOfLongestSubstring(string s) {
    unordered_map<char, int> lastSeen;  // char â†’ last index
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
| "What about shortest substring containing all characters of another string?" | LeetCode #76 â€” Minimum Window Substring. Shrink while valid. |
| "What if the string is very long (streaming)?" | Same approach works â€” window moves forward, O(1) per character |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **At most k distinct characters** | LeetCode #340. Track distinct count in window. |
| **Minimum window substring** | LeetCode #76. Shortest window containing all target chars. Shrink while valid. |
| **Maximum consecutive ones with k flips** | LeetCode #1004. Count zeros in window â‰¤ k. |
| **Longest subarray after deleting one element** | LeetCode #1493. Allow one "hole" in window. |
| **Longest repeating character replacement** | LeetCode #424. Most frequent char in window + k â‰¥ window size. |

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
| 1 | LeetCode #209 â€” Minimum Size Subarray Sum | Medium | Shortest window with sum â‰¥ k (positive only) |
| 2 | LeetCode #3 â€” Longest Substring Without Repeating Characters | Medium | Classic variable window + hash set |
| 3 | LeetCode #424 â€” Longest Repeating Character Replacement | Medium | Window + frequency tracking |
| 4 | LeetCode #76 â€” Minimum Window Substring | Hard | Shortest window containing all chars |
| 5 | LeetCode #992 â€” Subarrays with K Different Integers | Hard | atMost(k) - atMost(k-1) technique |

---

## 19. Memory Trick

> ðŸ§  **"The Caterpillar"**
>
> A variable sliding window moves like a caterpillar: the front (right) stretches forward, and when the body gets too long (invalid), the back (left) catches up. The caterpillar never moves backward â€” both ends only go forward.

---
---

# Pattern 8: Two Pointers

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  TWO POINTERS
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Goldman Sachs | Flipkart
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find a pair (or set of elements) in a **sorted** array that satisfies a condition."

With brute force, checking all pairs takes O(nÂ²). But if the array is sorted, we can use two pointers starting from opposite ends. If the current sum is too small, move the left pointer right (to increase it). If too large, move the right pointer left (to decrease it). Each step eliminates an entire row/column of possibilities.

**Why was this pattern invented?**

Sorting creates a **monotonic relationship** between pointer positions and values. This lets us make informed decisions about which pointer to move, guaranteeing we never miss the answer while skipping most of the O(nÂ²) pairs.

---

## 2. Pattern Identification Checklist

- [ ] Is the input **sorted** (or can you sort it)?
- [ ] Are you looking for a **pair/triple** with a specific property (sum, difference)?
- [ ] Can you determine which direction to move based on comparing the current result to the target?
- [ ] Does the problem involve **partitioning** or **merging** from two ends?
- [ ] Is the problem about two sequences that need to be compared/merged?

**If you checked 2+ of these â†’ Two Pointers.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Pair with sum = target" (sorted) | Classic two pointer convergence |
| "3Sum" / "triplet" | Fix one, two-pointer on rest |
| "Container with most water" | Width Ã— height from two ends |
| "Sorted array, remove duplicates" | In-place two pointer |
| "Merge two sorted arrays" | Two pointers advancing independently |
| "Palindrome check" | Compare from both ends |
| "Partition array" | Dutch National Flag-style partitioning |

---

## 4. Constraint Analysis

| Constraint | Brute Force O(nÂ²) | Two Pointers O(n) | Verdict |
|---|---|---|---|
| n â‰¤ 5000 | 2.5 Ã— 10â· âš ï¸ | 5000 âœ… | Two pointers preferred |
| n â‰¤ 10âµ | 10Â¹â° âŒ | 10âµ âœ… | Two pointers required (after sort: O(n log n)) |

**Note**: If the array isn't sorted and you sort it, the total time is O(n log n) for sorting + O(n) for two pointers = O(n log n). This is fine when n â‰¤ 10âµ.

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "Check all pairs (i, j) where i < j. For each pair, check if nums[i] + nums[j] == target." â†’ O(nÂ²).

### Step 2: Key Observation
> "If the array is sorted and `nums[left] + nums[right] < target`, then moving left rightward increases the sum. If `> target`, moving right leftward decreases it. We eliminate possibilities without checking them."

```
Sorted: [1, 3, 5, 7, 9]   target = 10

left=0, right=4: 1 + 9 = 10 âœ“ FOUND!

If sum < target: left++  (need larger values)
If sum > target: right-- (need smaller values)
If sum = target: found!
```

### Step 3: Optimization
> "Start with left=0, right=n-1. Move inward based on the comparison. Each step eliminates one position." â†’ O(n).

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
// Template A: Two Pointers â€” Converging (pair with property)
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
// Template B: Two Pointers â€” Same Direction (in-place modification)
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

Step 1: L=0, R=5 â†’ 1 + 11 = 12 = target âœ“ â†’ FOUND at (0, 5)!

(Quick find! Let's show a harder case with target = 10)

Step 1: L=0, R=5 â†’ 1 + 11 = 12 > 10 â†’ R--
  [1, 3, 5, 7, 9, 11]
   â†‘            â†‘
   L            R

Step 2: L=0, R=4 â†’ 1 + 9 = 10 = target âœ“ â†’ FOUND at (0, 4)!

(One more: target = 8)

Step 1: L=0, R=5 â†’ 1 + 11 = 12 > 8 â†’ R--
Step 2: L=0, R=4 â†’ 1 + 9 = 10 > 8 â†’ R--
Step 3: L=0, R=3 â†’ 1 + 7 = 8 = target âœ“ â†’ FOUND at (0, 3)!
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Forgetting to sort the array first | Two pointers only works on sorted arrays! |
| Using `left <= right` instead of `left < right` | For pairs, `left < right` (can't pair element with itself) |
| Not handling duplicates (in 3Sum) | Skip equal consecutive elements: `while (left < right && nums[left] == nums[left-1]) left++` |
| Assuming two pointers works on unsorted arrays | It doesn't for sum problems â€” use hashing for unsorted |
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

**LeetCode #15 â€” 3Sum**

> Given an integer array `nums`, return all triplets `[nums[i], nums[j], nums[k]]` such that `i â‰  j â‰  k` and `nums[i] + nums[j] + nums[k] == 0`. No duplicate triplets.

**Why this represents the pattern well**: It reduces a 3-pointer problem to a fixed-element + 2-pointer problem. Sorting enables both the two-pointer technique and duplicate skipping.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. **Brute force**: Three nested loops â†’ O(nÂ³). Too slow for n â‰¤ 3000.
2. **Reduce**: Fix one element `nums[i]`, then find two elements that sum to `-nums[i]`. This is Two Sum on the remaining array.
3. **Sort the array**: Now Two Sum becomes a two-pointer problem â†’ O(n) per fixed element.
4. **Handle duplicates**: Skip consecutive equal values to avoid duplicate triplets.
5. **Total**: O(nÂ²) â€” fix each element (O(n)) Ã— two pointers (O(n)).

---

## 14. C++ Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

// 3Sum: find all unique triplets that sum to zero
// Pattern: Sorting + Two Pointers
// Time: O(nÂ²), Space: O(1) extra (O(n) for sorting)
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
| "What about 4Sum?" | Fix two elements, two-pointer on remaining â†’ O(nÂ³) |
| "Closest 3Sum to target?" | Track minimum difference instead of exact match |
| "Count triplets instead of listing them?" | Count valid pairs per fixed element instead of listing |
| "What if duplicates are allowed in output?" | Remove the duplicate-skipping logic |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Two Sum II (sorted)** | Simple convergence from both ends |
| **3Sum Closest** | Track closest sum; don't need exact match |
| **4Sum** | Fix two, two-pointer on two â†’ O(nÂ³) |
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
| 1 | LeetCode #167 â€” Two Sum II (Sorted) | Medium | Basic converging two pointers |
| 2 | LeetCode #11 â€” Container With Most Water | Medium | Width Ã— height optimization |
| 3 | LeetCode #15 â€” 3Sum | Medium | Fix one + two pointers |
| 4 | LeetCode #42 â€” Trapping Rain Water | Hard | Two pointers with max tracking |
| 5 | LeetCode #16 â€” 3Sum Closest | Medium | Two pointers with closest tracking |

---

## 19. Memory Trick

> ðŸ§  **"The Handshake"**
>
> Two pointers start at opposite ends of a sorted array and walk toward each other like two people approaching for a handshake. If they're too far apart (sum too large), the right person steps forward. If too close (sum too small), the left person steps forward. They meet in the middle.

---
---

# Pattern 9: Fast & Slow Pointer

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  FAST & SLOW POINTER (FLOYD'S TORTOISE AND HARE)
  OA Frequency: â˜…â˜…â˜…â˜†â˜†
  Companies: Amazon | Google | Microsoft | Apple | LinkedIn
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Two fundamental problems:
1. **Cycle detection**: Does a linked list (or sequence) contain a cycle?
2. **Finding the middle**: Where is the midpoint of a linked list?

Without the fast/slow pointer trick, cycle detection requires O(n) space (hash set of visited nodes). The fast/slow pointer does it in O(1) space.

**Why was this pattern invented?**

If two runners start at the same point on a circular track and one runs twice as fast, the fast runner will eventually "lap" the slow runner â€” they'll meet again. This is Floyd's insight: if there's a cycle, the fast pointer (2 steps) and slow pointer (1 step) will eventually collide.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem involve a **linked list** and mention "cycle"?
- [ ] Do you need to find the **middle node** of a linked list?
- [ ] Does a function mapping produce a sequence that might repeat?
- [ ] Does the problem say "detect loop" or "find entry point of loop"?
- [ ] Can the problem be modeled as following a chain of `next` pointers?

**If you checked 2+ of these â†’ Fast & Slow Pointer.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Linked list cycle" | Direct application |
| "Find middle of linked list" | When fast reaches end, slow is at middle |
| "Happy number" | Sequence eventually cycles |
| "Find duplicate number" (in [1,n]) | Array as linked list â†’ cycle detection |
| "Starting point of cycle" | Phase 2 of Floyd's algorithm |

---

## 4. Constraint Analysis

| Constraint | Hash Set Approach | Fast & Slow | Verdict |
|---|---|---|---|
| n â‰¤ 10âµ nodes | O(n) time, O(n) space âœ… | O(n) time, O(1) space âœ… | Fast & slow preferred (less space) |
| Space constraint O(1) | âŒ Hash set uses O(n) | âœ… Only two pointers | Fast & slow required |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "Visit each node, store it in a hash set. If I visit a node that's already in the set, there's a cycle." â†’ O(n) time, O(n) space.

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

**Why O(n) for cycle detection?** If there's a cycle of length c, the fast pointer gains 1 step per iteration on the slow pointer. After at most c iterations of being in the cycle, fast catches slow. Total steps â‰¤ n + c â‰¤ 2n.

---

## 9. Dry Run

**Cycle Detection**: List `1 â†’ 2 â†’ 3 â†’ 4 â†’ 5 â†’ 3` (cycle back to node 3)

```
        1 â†’ 2 â†’ 3 â†’ 4 â†’ 5
                â†‘         |
                â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜

Step 1: slow=1, fast=1
Step 2: slow=2, fast=3   (slow: 1â†’2, fast: 1â†’2â†’3)
Step 3: slow=3, fast=5   (slow: 2â†’3, fast: 3â†’4â†’5)
Step 4: slow=4, fast=4   (slow: 3â†’4, fast: 5â†’3â†’4)
  slow == fast â†’ CYCLE DETECTED at node 4!

Phase 2 (find entry):
  Reset slow to head (node 1). Both move speed 1.
  Step 1: slow=2, fast=5
  Step 2: slow=3, fast=3
  slow == fast â†’ CYCLE ENTRY at node 3! âœ“
```

**Find Middle**: List `1 â†’ 2 â†’ 3 â†’ 4 â†’ 5`

```
Step 1: slow=1, fast=1
Step 2: slow=2, fast=3
Step 3: slow=3, fast=5
  fast->next is NULL â†’ STOP
  Middle = slow = 3 âœ“
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Checking `fast->next->next` without checking `fast->next` first | Always check `fast && fast->next` before advancing |
| Initializing slow and fast to different starting points | Both start at head |
| Forgetting Phase 2 when finding cycle entry | Detection (meeting) â‰  entry point |
| Applying to arrays without understanding the "next" function | Map the array to a functional graph first (e.g., nums[i] â†’ next node) |
| Using fast & slow for problems that need hash set (e.g., visited tracking) | Fast & slow only detects cycles, not arbitrary revisits |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Need to detect cycle in a generic graph (not linked list) | Fast/slow needs a single "next" â€” graphs have multiple neighbors | DFS coloring |
| Need to find ALL nodes in the cycle | Fast/slow finds one meeting point | DFS/BFS after finding entry |
| Need cycle detection with extra information (path, length) | Fast/slow gives limited info | Hash set stores full path |
| Doubly linked list problems | Usually solved with direct traversal | Standard pointer manipulation |

---

## 12. Medium-Level Example Problem

**LeetCode #287 â€” Find the Duplicate Number**

> Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]`, there is exactly one repeated number. Find it without modifying the array and using only O(1) extra space.

**Why this represents the pattern well**: The array can be interpreted as a linked list where `nums[i]` points to the next node. Since there's a duplicate, two nodes point to the same destination â†’ creating a cycle. Finding the duplicate = finding the cycle entry point.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. **Constraints**: Can't modify array, O(1) space â†’ can't sort, can't use hash set.
2. **Model as linked list**: Index 0 â†’ nums[0] â†’ nums[nums[0]] â†’ ... Since values are in [1,n], we never reach index 0 again (no self-loops from index 0). The duplicate creates a cycle.
3. **Apply Floyd's algorithm**: Phase 1 finds meeting point, Phase 2 finds entry point. The entry point is the duplicate number.

**Key insight**: An array of integers in range [1,n] IS a linked list. Index i points to index nums[i]. A duplicate value means two indices point to the same index â†’ cycle.

---

## 14. C++ Implementation

```cpp
#include <vector>
using namespace std;

// Find the Duplicate Number
// Pattern: Fast & Slow Pointer (Floyd's Cycle Detection)
// Time: O(n), Space: O(1)
int findDuplicate(vector<int>& nums) {
    // Treat array as linked list: index i â†’ index nums[i]
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
| "Can you find the duplicate in O(n log n) with binary search?" | Binary search on value: count elements â‰¤ mid. If count > mid, duplicate is in [1, mid] |
| "What if you can modify the array?" | Mark visited by negating: if nums[abs(nums[i])] is already negative, abs(nums[i]) is duplicate |
| "Prove why Phase 2 works?" | Mathematical proof: if distance from head to entry is a, and from entry to meeting point is b, then a â‰¡ 0 (mod cycle length). So both pointers from head and meeting point meet at entry. |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Happy Number** | LeetCode #202. Sequence n â†’ sum of digit squares. Cycles if not happy. |
| **Linked List Cycle II** | LeetCode #142. Find the cycle entry node. |
| **Middle of Linked List** | LeetCode #876. Fast reaches end â†’ slow is at middle. |
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
| 1 | LeetCode #141 â€” Linked List Cycle | Easy | Basic cycle detection |
| 2 | LeetCode #876 â€” Middle of Linked List | Easy | Fast/slow for midpoint |
| 3 | LeetCode #142 â€” Linked List Cycle II | Medium | Cycle entry (Phase 2) |
| 4 | LeetCode #287 â€” Find the Duplicate Number | Medium | Array as linked list |
| 5 | LeetCode #457 â€” Circular Array Loop | Medium | Cycle detection on circular array |

---

## 19. Memory Trick

> ðŸ§  **"The Racetrack"**
>
> Imagine two runners on a circular track: one jogs (slow), one sprints (fast). If the track is circular (cycle exists), the sprinter will eventually lap the jogger and they'll meet. If the track is straight (no cycle), the sprinter just reaches the end first. After they meet, to find WHERE the loop starts, send one runner back to the start line â€” when they meet again, that's the loop entrance.

---
---

*Previous chapter: [02 â€” Fundamentals](02_Fundamentals.md)*
*Next chapter: [04 â€” Binary Search](04_Binary_Search.md)*
<!--
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DSA PATTERN HANDBOOK â€” BINARY SEARCH
  Chapter 04
  
  Patterns in this chapter:
    Pattern 10: Binary Search on Sorted Arrays
    Pattern 11: Binary Search on Answer
  
  Formatting Rules (internal reference for consistency):
  - Heading hierarchy: # (chapter) â†’ ## (pattern) â†’ ### (section)
  - 20 sections per pattern
  - Complexity notation: O(n), O(n log n), O(nÂ²)
  - Star rating: â˜… (filled), â˜† (empty), scale 1â€“5
  - Code style: C++17, 4-space indent, STL preferred
  - Tables: GitHub-flavored markdown pipe tables
  - Diagrams: ASCII art in code blocks
  - Problem references: "LeetCode #ID â€” Name" format
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-->

# Chapter 4: Binary Search

Binary search is arguably the most important algorithmic pattern in placements. It appears in two very different forms:

1. **Classical Binary Search** â€” find a target in a sorted array.
2. **Binary Search on Answer** â€” find the optimal value in a search space where feasibility is monotonic.

The classical form is well-known. The "binary search on answer" form is what separates good candidates from great ones in OAs.

**Core principle**: Binary search works whenever the search space can be divided into two halves where one half is guaranteed to not contain the answer. This requires a **monotonic property** â€” once something becomes true (or false), it stays true (or false).

---
---

# Pattern 10: Binary Search on Sorted Arrays

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  BINARY SEARCH ON SORTED ARRAYS
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Samsung | Flipkart
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find a target value (or its position) in a **sorted** collection."

Linear search checks every element â€” O(n). Binary search eliminates half the remaining elements at each step â€” O(log n). For n = 10â¶, that's 20 comparisons instead of 1,000,000.

**Why was this pattern invented?**

Sorting creates order. Order means: if `arr[mid] < target`, then **all elements before mid** are also less than target. We can discard half the array in one comparison. This "halving" is the essence of logarithmic time.

---

## 2. Pattern Identification Checklist

- [ ] Is the input **sorted** (or can be treated as sorted)?
- [ ] Are you looking for a **specific value** or a **boundary** (first/last occurrence)?
- [ ] Does the problem involve finding a **position** in a sorted structure?
- [ ] Can you eliminate half the search space with a single comparison?
- [ ] Does the problem mention "rotated sorted array"?

**If you checked 2+ of these â†’ Binary Search on Sorted Arrays.**

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
| n â‰¤ 100 | Trivial âœ… | Trivial âœ… | Both fine |
| n â‰¤ 10âµ | 10âµ âœ… | 17 âœ… | Both fine, BS faster |
| n â‰¤ 10â¹ | 10â¹ âŒ | 30 âœ… | BS required |
| n â‰¤ 10Â¹â¸ | Impossible âŒ | 60 âœ… | BS required |

**Key insight**: If the constraint is very large (10â¹ or 10Â¹â¸), O(log n) is often the only viable approach.

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "Scan from left to right until I find the target." â†’ O(n).

### Step 2: Key Observation
> "The array is sorted. If `arr[mid] < target`, the target must be in the right half. I've just eliminated half the array with one comparison."

### Step 3: Optimization
> "Repeatedly halve the search space: check the middle element, go left or right based on comparison."

### Step 4: Boundary Variants
> "What if there are duplicates and I need the FIRST occurrence? When `arr[mid] == target`, don't stop â€” continue searching LEFT for an earlier occurrence."

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
            hi = mid          // don't skip mid â€” it could be the answer
    
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
// Template: Binary Search â€” Standard, Lower Bound, Upper Bound
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
| Time | O(log n) | Search space halves each step: n â†’ n/2 â†’ n/4 â†’ ... â†’ 1 = logâ‚‚(n) steps |
| Space | O(1) iterative, O(log n) recursive | Iterative uses constant space; recursive uses stack frames |

**Why O(log n)?** After k comparisons, the remaining search space is n/2^k. We stop when n/2^k = 1, so k = logâ‚‚(n).

---

## 9. Dry Run

**Problem**: Find 7 in `[1, 3, 5, 7, 9, 11, 13]`.

```
Array: [1, 3, 5, 7, 9, 11, 13]    target = 7

Step 1: lo=0, hi=6, mid=3
  arr[3] = 7 == target â†’ FOUND at index 3!

(Harder example: find first occurrence of 5 in [2, 3, 5, 5, 5, 8, 9])

Lower bound of 5:

Step 1: lo=0, hi=7, mid=3
  arr[3] = 5 >= 5 â†’ hi = 3
  [2, 3, 5, 5 | 5, 8, 9]
   â†‘        â†‘
   lo       hi

Step 2: lo=0, hi=3, mid=1
  arr[1] = 3 < 5 â†’ lo = 2
  [2, 3, 5, 5 | 5, 8, 9]
         â†‘  â†‘
         lo hi

Step 3: lo=2, hi=3, mid=2
  arr[2] = 5 >= 5 â†’ hi = 2
  [2, 3, 5, 5 | 5, 8, 9]
         â†‘
       lo=hi

lo == hi â†’ return 2. First occurrence of 5 is at index 2. âœ“
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

**LeetCode #34 â€” Find First and Last Position of Element in Sorted Array**

> Given a sorted array of integers and a target, find the starting and ending position. Return [-1, -1] if not found.

**Why this represents the pattern well**: It requires TWO binary searches â€” lower_bound for the first position and upper_bound - 1 for the last position. This tests true understanding of binary search boundaries.

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
| **Sqrt(x)** | LeetCode #69. Binary search for largest k where kÂ² â‰¤ x. |

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
| 1 | LeetCode #704 â€” Binary Search | Easy | Standard binary search |
| 2 | LeetCode #35 â€” Search Insert Position | Easy | Lower bound application |
| 3 | LeetCode #34 â€” Find First and Last Position | Medium | Lower + upper bound |
| 4 | LeetCode #33 â€” Search in Rotated Sorted Array | Medium | Modified binary search |
| 5 | LeetCode #4 â€” Median of Two Sorted Arrays | Hard | Binary search on partition |

---

## 19. Memory Trick

> ðŸ§  **"The Dictionary Lookup"**
>
> When you look up a word in a physical dictionary, you don't start from page 1. You open it roughly in the middle, check if your word comes before or after, then repeat on the right half. That's binary search. You never check every page â€” you halve the remaining pages each time.

---
---

# Pattern 11: Binary Search on Answer

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  BINARY SEARCH ON ANSWER
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: Google | Amazon | Microsoft | Uber | Flipkart | Atlassian | Walmart
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
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
- [ ] Is `canAchieve(X)` **monotonic** â€” if X works, then X+1 also works (or vice versa)?
- [ ] Does the problem mention "allocate," "distribute," "partition" with optimization?

**If you checked 3+ of these â†’ Binary Search on Answer.**

> **This is one of the highest-value patterns to learn.** Many students miss it because they don't recognize that the answer can be binary-searched.

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Minimum possible maximum" | Minimize the worst case â†’ BS on answer |
| "Maximum possible minimum" | Maximize the best case â†’ BS on answer |
| "Allocate pages / books to students" | Classic BS on answer |
| "Aggressive cows" / "maximum min distance" | BS on minimum distance |
| "Split array into k subarrays minimizing max sum" | BS on max sum |
| "Can you do it with at most k operations?" | Feasibility check â†’ BS on answer |
| "Minimum capacity for shipping" | BS on capacity |
| "Koko eating bananas" / "minimum speed" | BS on speed |
| "Smallest divisor given a threshold" | BS on divisor |

---

## 4. Constraint Analysis

| Constraint | Direct Optimization | BS on Answer | Verdict |
|---|---|---|---|
| Answer range â‰¤ 10â¹ | Often unknown | logâ‚‚(10â¹) â‰ˆ 30 checks âœ… | BS on answer works |
| Each check is O(n) | â€” | O(n log(range)) âœ… | Very efficient |
| n â‰¤ 10âµ, range â‰¤ 10â¹ | â€” | 10âµ Ã— 30 = 3 Ã— 10â¶ âœ… | Perfect |

**The power of BS on answer**: Even with a billion possible answers, you only check ~30 of them.

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "Try every possible answer value from min to max. For each, check if it's feasible." â†’ O(range Ã— n).

### Step 2: Key Observation
> "The feasibility function is monotonic: if answer=X works, then answer=X+1 also works (for 'minimum' problems). This means I can binary search!"

```
Answer:     1   2   3   4   5   6   7   8   9   10
Feasible?:  âœ—   âœ—   âœ—   âœ—   âœ“   âœ“   âœ“   âœ“   âœ“   âœ“
                              â†‘
                        First feasible = optimal answer

This is just lower_bound on the answer space!
```

### Step 3: Optimization
> "Binary search on the answer range. For each candidate, run a greedy O(n) check. Total: O(n log(range))."

### Step 4: Optimal Solution
> O(n Â· log(answer_range)). The hardest part is writing the `canAchieve(X)` function correctly.

---

## 6. Generic Algorithm

```
BINARY_SEARCH_ON_ANSWER(problem):
    lo = minimum_possible_answer
    hi = maximum_possible_answer
    
    while lo < hi:
        mid = lo + (hi - lo) / 2
        
        if canAchieve(mid):
            hi = mid              // mid works â€” try smaller (for minimization)
        else:
            lo = mid + 1          // mid doesn't work â€” need larger
    
    return lo                     // optimal answer

// For MAXIMIZATION (maximum possible minimum):
    while lo < hi:
        mid = lo + (hi - lo + 1) / 2   // ceiling division to avoid infinite loop
        
        if canAchieve(mid):
            lo = mid              // mid works â€” try larger
        else:
            hi = mid - 1          // mid doesn't work â€” need smaller
    
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
// Time: O(n Â· log(range)), Space: O(1)

#include <vector>
using namespace std;

// The feasibility check function
// MODIFY: implement your specific greedy/check logic here
bool canAchieve(vector<int>& nums, int guess, int k) {
    // Example: can we split nums into â‰¤ k groups
    // where each group's sum â‰¤ guess?
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
            hi = mid;          // feasible â€” try smaller
        } else {
            lo = mid + 1;      // not feasible â€” need larger
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
            lo = mid;          // feasible â€” try larger
        } else {
            hi = mid - 1;      // not feasible â€” need smaller
        }
    }
    return lo;
}
```

---

## 8. Complexity Analysis

| Metric | Value | Why |
|---|---|---|
| Time | O(n Â· log(range)) | log(range) binary search iterations, each running O(n) check |
| Space | O(1) | Only pointer variables and running totals |

| Example Values | Calculations |
|---|---|
| n = 10âµ, range = 10â¹ | 10âµ Ã— 30 = 3 Ã— 10â¶ âœ… |
| n = 10âµ, range = 10Â¹â¸ | 10âµ Ã— 60 = 6 Ã— 10â¶ âœ… |

---

## 9. Dry Run

**Problem**: Split `[7, 2, 5, 10, 8]` into at most 2 subarrays minimizing the maximum sum.

```
Answer range: lo = max(arr) = 10, hi = sum(arr) = 32

Can we achieve max_sum â‰¤ 21?
  Group 1: 7+2+5 = 14 â‰¤ 21 âœ“, add 10 â†’ 24 > 21 â†’ new group
  Group 2: 10+8 = 18 â‰¤ 21 âœ“
  Groups = 2 â‰¤ 2 â†’ FEASIBLE âœ“

Step 1: lo=10, hi=32, mid=21 â†’ feasible â†’ hi=21
Step 2: lo=10, hi=21, mid=15 â†’ check:
  Group 1: 7+2+5 = 14 â‰¤ 15 âœ“, +10 = 24 > 15 â†’ new group
  Group 2: 10 â‰¤ 15 âœ“, +8 = 18 > 15 â†’ new group
  Group 3: 8
  Groups = 3 > 2 â†’ NOT FEASIBLE âœ— â†’ lo=16

Step 3: lo=16, hi=21, mid=18 â†’ check:
  Group 1: 7+2+5 = 14 â‰¤ 18 âœ“, +10 = 24 > 18 â†’ new group
  Group 2: 10+8 = 18 â‰¤ 18 âœ“
  Groups = 2 â‰¤ 2 â†’ FEASIBLE âœ“ â†’ hi=18

Step 4: lo=16, hi=18, mid=17 â†’ check:
  Group 1: 7+2+5 = 14 â‰¤ 17 âœ“, +10 = 24 > 17 â†’ new group
  Group 2: 10 â‰¤ 17 âœ“, +8 = 18 > 17 â†’ new group
  Groups = 3 > 2 â†’ NOT FEASIBLE âœ— â†’ lo=18

lo == hi == 18 â†’ Answer: 18

Verify: [7, 2, 5 | 10, 8] â†’ sums [14, 18] â†’ max = 18 âœ“
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Getting the direction wrong (hi=mid vs lo=mid) | Remember: minimize â†’ hi=mid when feasible. Maximize â†’ lo=mid when feasible. |
| Infinite loop with `lo=mid` | Use `mid = lo + (hi - lo + 1) / 2` (ceiling) when doing `lo = mid` |
| Wrong bounds for lo and hi | lo = smallest possible answer, hi = largest possible answer. Think about what they mean. |
| Incorrect feasibility function | Test your canAchieve() independently before using it in binary search |
| Forgetting edge cases in the check function | Handle: single element > guess, empty groups, exactly k groups |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Feasibility is NOT monotonic | BS requires: feasible(X) â‡’ feasible(X+1) | DP or direct optimization |
| The answer is not a numeric value | Can't binary search on discrete choices | DP, Greedy |
| Checking feasibility is too expensive (> O(n)) | Total time becomes too high | Direct algorithm, DP |
| Answer space is not bounded | Need known lo and hi | Mathematical analysis first |

---

## 12. Medium-Level Example Problem

**LeetCode #875 â€” Koko Eating Bananas**

> Koko has `n` piles of bananas. She can eat `k` bananas per hour. Each hour, she chooses a pile and eats `k` bananas. If the pile has fewer than `k`, she eats the whole pile. She has `h` hours. Find the minimum integer `k` such that she finishes all bananas within `h` hours.

**Why this represents the pattern well**: It's a clean binary search on answer problem. The answer (eating speed k) has a clear range [1, max(piles)]. The feasibility function is simple: calculate hours needed at speed k. If hours â‰¤ h, k is feasible. Feasibility is monotonic: higher k always means fewer hours.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. **Identify the answer**: We're looking for the minimum eating speed `k`.
2. **Define the range**: k âˆˆ [1, max(piles)]. At max speed, each pile takes 1 hour.
3. **Monotonicity check**: If speed k works (finishes in â‰¤ h hours), then speed k+1 also works (even faster). âœ“
4. **Write canFinish(k)**: For each pile, hours = âŒˆpile/kâŒ‰. Sum all hours. Return sum â‰¤ h.
5. **Binary search**: Find smallest k where canFinish(k) is true.

**Key detail**: âŒˆa/bâŒ‰ in integer math = `(a + b - 1) / b` or `(a - 1) / b + 1`.

---

## 14. C++ Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

// Koko Eating Bananas
// Pattern: Binary Search on Answer
// Time: O(n Â· log(max_pile)), Space: O(1)
int minEatingSpeed(vector<int>& piles, int h) {
    // Range: [1, max(piles)]
    int lo = 1;
    int hi = *max_element(piles.begin(), piles.end());

    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;

        // Check: can Koko finish all piles at speed mid in â‰¤ h hours?
        long long hoursNeeded = 0;
        for (int pile : piles) {
            hoursNeeded += (pile + mid - 1) / mid;  // ceil(pile / mid)
        }

        if (hoursNeeded <= h) {
            hi = mid;      // feasible â€” try eating slower
        } else {
            lo = mid + 1;  // not feasible â€” need to eat faster
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
| "Can you solve it without binary search?" | Not efficiently â€” direct formulas don't exist for general inputs |
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
| 1 | LeetCode #69 â€” Sqrt(x) | Easy | BS on answer (simplest form) |
| 2 | LeetCode #875 â€” Koko Eating Bananas | Medium | Classic BS on answer |
| 3 | LeetCode #1011 â€” Capacity to Ship Packages | Medium | BS on capacity |
| 4 | LeetCode #410 â€” Split Array Largest Sum | Hard | BS on max subarray sum |
| 5 | LeetCode #774 â€” Minimize Max Distance to Gas Station | Hard | BS on real-valued answer |

---

## 19. Memory Trick

> ðŸ§  **"The Goldilocks Method"**
>
> Binary search on answer is like Goldilocks testing porridge. "Is 50 too much? Yes â†’ try smaller. Is 25 too little? No, it works â†’ but can I go smaller? Is 12 too little? Yes â†’ the answer is between 12 and 25." You're not searching an array â€” you're searching for the "just right" answer.

---
---

*Previous chapter: [03 â€” Sliding Window & Pointers](03_Sliding_Window_and_Pointers.md)*
*Next chapter: [05 â€” Sorting & Greedy](05_Sorting_and_Greedy.md)*
<!--
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DSA PATTERN HANDBOOK â€” SORTING & GREEDY
  Chapter 05
  
  Patterns in this chapter:
    Pattern 12: Sorting + Greedy
    Pattern 13: Greedy Algorithms
  
  Formatting Rules (internal reference for consistency):
  - Heading hierarchy: # (chapter) â†’ ## (pattern) â†’ ### (section)
  - 20 sections per pattern
  - Complexity notation: O(n), O(n log n), O(nÂ²)
  - Star rating: â˜… (filled), â˜† (empty), scale 1â€“5
  - Code style: C++17, 4-space indent, STL preferred
  - Tables: GitHub-flavored markdown pipe tables
  - Diagrams: ASCII art in code blocks
  - Problem references: "LeetCode #ID â€” Name" format
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-->

# Chapter 5: Sorting & Greedy

Greedy algorithms make the **locally optimal choice** at each step, hoping it leads to a globally optimal solution. Unlike DP, greedy doesn't reconsider past decisions. When it works, greedy is simple and efficient. The challenge is proving that the greedy approach is correct.

Sorting is often the first step in greedy algorithms. By imposing order on the input, sorting reveals structure that makes the greedy choice obvious.

**Key question for every greedy problem**: *"Can I prove that the locally best choice never hurts the global optimum?"* If yes, greedy works. If not, consider DP.

---
---

# Pattern 12: Sorting + Greedy

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  SORTING + GREEDY
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: Amazon | Google | Microsoft | Adobe | Goldman Sachs | Flipkart | Walmart
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Problems where the input is **unordered** and you need to make **sequential choices** (assign, schedule, pair, distribute). Sorting transforms chaos into order, and once ordered, the greedy choice becomes natural.

**Why was this pattern invented?**

Consider: "Assign n tasks to workers to minimize total time." Without sorting, you'd need to try all n! permutations. With sorting (sort both tasks and workers by difficulty/capability), you can match greedily â€” O(n log n).

Sorting + Greedy reduces exponential search spaces to polynomial ones when the **optimal substructure** property holds.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem involve **assigning, pairing, or scheduling** items?
- [ ] Would the answer become obvious if the input were sorted?
- [ ] Does the problem ask for "minimum number of operations" or "maximum items selected"?
- [ ] Does sorting by one property (start time, size, cost) simplify the decision?
- [ ] Is there a clear "best first" strategy after sorting?

**If you checked 2+ of these â†’ Sorting + Greedy.**

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
| n â‰¤ 10 | O(n!) = 3.6M âœ… | O(n log n) âœ… | Both work |
| n â‰¤ 20 | O(n!) â‰ˆ 2.4 Ã— 10Â¹â¸ âŒ | O(n log n) âœ… | Greedy required |
| n â‰¤ 10âµ | Impossible | O(n log n) âœ… | Greedy required |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "Try every possible ordering/assignment and pick the best." â†’ O(n!) or O(2â¿).

### Step 2: Key Observation
> "What if I process items in a specific order? If I sort by deadline/size/value, the best choice at each step becomes clear."

### Step 3: Sort Criterion
> Identify WHAT to sort by. Common criteria:
> - **By end time** â†’ interval scheduling
> - **By size/difficulty** â†’ assignment matching
> - **By ratio (value/weight)** â†’ fractional knapsack
> - **By deadline** â†’ job scheduling
> - **By start time** â†’ merge intervals

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
// Template: Sorting + Greedy â€” Interval Selection
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
// Template: Sorting + Greedy â€” Assignment/Matching
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

Meeting (2,3): start=2 â‰¥ lastEnd=-âˆž â†’ SELECT. lastEnd=3. Count=1.
  â”€â”€[2===3]â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  
Meeting (1,4): start=1 < lastEnd=3 â†’ SKIP (overlaps).

Meeting (3,5): start=3 â‰¥ lastEnd=3 â†’ SELECT. lastEnd=5. Count=2.
  â”€â”€[2===3][3=======5]â”€â”€

Meeting (4,6): start=4 < lastEnd=5 â†’ SKIP (overlaps).

Meeting (5,7): start=5 â‰¥ lastEnd=5 â†’ SELECT. lastEnd=7. Count=3.
  â”€â”€[2===3][3=======5][5=======7]

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
| Need to consider future consequences | Greedy is myopic â€” only sees current step | DP |
| "Count ALL optimal solutions" | Greedy finds one; DP counts all | DP |
| Problem has a "penalty for NOT choosing" | Greedy can't handle opportunity costs easily | DP |

**The Greedy vs DP Test**: 
- Can you prove: "choosing the locally best option never hurts globally"? â†’ Greedy.
- Does the problem have "overlapping subproblems"? â†’ DP.

---

## 12. Medium-Level Example Problem

**LeetCode #435 â€” Non-overlapping Intervals**

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

// Non-overlapping Intervals â€” Minimum Removals
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
            // No overlap â€” keep this interval
            kept++;
            lastEnd = intervals[i][1];
        }
        // else: overlap â€” skip (remove) this interval
    }

    return (int)intervals.size() - kept;
}
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "What if we want to minimize the number of ROOMS instead?" | Sort by start time, use a min-heap of end times (Meeting Rooms II) |
| "What if intervals have weights and we want max total weight?" | Weighted interval scheduling â€” needs DP, not greedy |
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
| 1 | LeetCode #455 â€” Assign Cookies | Easy | Sort both, match greedily |
| 2 | LeetCode #452 â€” Minimum Number of Arrows to Burst Balloons | Medium | Sort by end, greedy |
| 3 | LeetCode #435 â€” Non-overlapping Intervals | Medium | Classic interval scheduling |
| 4 | LeetCode #1029 â€” Two City Scheduling | Medium | Sort by cost difference |
| 5 | LeetCode #621 â€” Task Scheduler | Medium | Frequency-based greedy |

---

## 19. Memory Trick

> ðŸ§  **"The Buffet Strategy"**
>
> At an all-you-can-eat buffet with limited time, you'd sort dishes by how quickly you can eat them (end time = finish quickly). Eat the fastest-to-finish dish first, then the next one that doesn't overlap with your full mouth. That's exactly interval scheduling: sort by end time, select non-overlapping greedily.

---
---

# Pattern 13: Greedy Algorithms

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  GREEDY ALGORITHMS (GENERAL)
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Goldman Sachs | Uber | Flipkart
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Make a sequence of decisions to optimize something, where at each step, there's a **clearly best choice** that doesn't need to be reconsidered."

Greedy algorithms are powerful because they're usually O(n) or O(n log n) â€” much faster than DP's O(nÂ²) or worse. But they only work when the **greedy choice property** holds: the locally optimal choice leads to a globally optimal solution.

**Why was this pattern invented?**

Many optimization problems have a structure where you can prove: "if the best local choice is made at every step, the result is globally optimal." When this is true, you don't need DP's expensive table-filling. Greedy gives you the answer in a single pass.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem have an **optimal substructure** â€” optimal solution contains optimal sub-solutions?
- [ ] Can you identify a **clear ordering** for processing elements?
- [ ] At each step, is there a **single best choice** that doesn't depend on future decisions?
- [ ] Can you prove (even informally) that no better global solution exists by making a different local choice?
- [ ] Does the problem NOT have the "take it or leave it" characteristic (that would need DP)?

**If you checked 3+ of these â†’ Greedy.**

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
| n â‰¤ 1000 | O(nÂ²) âœ… | O(n) âœ… | Both work, greedy is faster |
| n â‰¤ 10âµ | O(nÂ²) âŒ | O(n) âœ… | Greedy needed if applicable |
| n â‰¤ 10â¶ | O(nÂ²) âŒ | O(n) âœ… | Must be greedy or O(n) |

**Rule**: If greedy is correct for the problem, it's almost always the best approach (simplest code, fastest runtime).

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "Try all possible sequences of decisions." â†’ Exponential.

### Step 2: Key Observation
> "What if I always pick the [largest/smallest/earliest/cheapest] option first? Does it ever backfire?"

### Step 3: Greedy Hypothesis
> "I believe that picking the locally best choice at each step gives the global optimum."

### Step 4: Proof (Exchange Argument)
> "Suppose there's an optimal solution that makes a different choice at step i. I can swap my greedy choice into that solution without making it worse. Therefore, greedy is at least as good."

This is the **exchange argument** â€” the standard way to prove greedy correctness:
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
// Template: Greedy â€” Forward Pass
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
// Template: Greedy â€” Priority Queue Based
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

i=0: nums[0]=2 â†’ can reach index 0+2=2. maxReach = max(0, 2) = 2.
  [2, 3, 1, 1, 4]
   â†‘  â”€  â”€>         reach=2

i=1: 1 â‰¤ maxReach(2) âœ“. nums[1]=3 â†’ can reach 1+3=4. maxReach = max(2, 4) = 4.
  [2, 3, 1, 1, 4]
      â†‘  â”€  â”€  â”€>   reach=4

maxReach(4) â‰¥ last index(4) â†’ return true!

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
  Value = 6. âœ— Greedy was wrong!
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

**LeetCode #55 â€” Jump Game**

> Given an array of non-negative integers `nums`, you're initially at index 0. Each element represents max jump length. Return if you can reach the last index.

**Why this represents the pattern well**: Pure greedy thinking â€” maintain the farthest reachable index. If you can reach index i and from i you can reach further, update maxReach. Simple, elegant, and commonly asked.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. **Brute force**: Try all possible jump sequences (DFS/BFS) â†’ exponential.
2. **Observe**: We don't care WHICH path â€” just WHETHER the last index is reachable.
3. **Greedy insight**: At each reachable index, update the farthest we can reach. If we ever find that we can't move forward (maxReach < current index), we're stuck.
4. **One pass**: Scan left to right, maintaining maxReach.

---

## 14. C++ Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

// Jump Game â€” Can you reach the last index?
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
| "What if you can also jump backward?" | BFS from index 0 â€” greedy doesn't apply |
| "What if jump costs have different values?" | Weighted problem â†’ DP (Dijkstra on indices) |
| "What if array has negative numbers (jump length)?" | Problem changes fundamentally â€” needs different approach |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Jump Game II** | LeetCode #45. Minimum jumps â€” BFS-like greedy. |
| **Gas Station** | LeetCode #134. Greedy: if total gas â‰¥ total cost, start from the index where cumulative sum is lowest. |
| **Candy** | LeetCode #135. Two-pass greedy (left-to-right, then right-to-left). |
| **Lemonade Change** | LeetCode #860. Greedy change-making with limited denominations. |
| **Fractional Knapsack** | Sort by value/weight, take as much as possible â€” greedy works for fractions. |

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
| 1 | LeetCode #860 â€” Lemonade Change | Easy | Simple greedy decision-making |
| 2 | LeetCode #55 â€” Jump Game | Medium | Track farthest reach |
| 3 | LeetCode #134 â€” Gas Station | Medium | Circular greedy |
| 4 | LeetCode #45 â€” Jump Game II | Medium | Minimum jumps (BFS-like greedy) |
| 5 | LeetCode #135 â€” Candy | Hard | Two-pass greedy |

---

## 19. Memory Trick

> ðŸ§  **"The Greedy Shopper"**
>
> A greedy algorithm shops like someone with no patience: at each shelf, grab the best deal visible right now without checking future aisles. This works when every aisle's "best deal" doesn't affect what's available later. When it does (limited stock, dependencies), you need the "careful planner" (DP).
>
> **The Greedy vs DP Mnemonic**: "Can I decide now and never regret it?" â†’ Greedy. "Might I regret this later?" â†’ DP.

---
---

*Previous chapter: [04 â€” Binary Search](04_Binary_Search.md)*
*Next chapter: [06 â€” Stack & Queue](06_Stack_and_Queue.md)*
<!--
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DSA PATTERN HANDBOOK â€” STACK & QUEUE
  Chapter 06
  
  Patterns in this chapter:
    Pattern 14: Stack
    Pattern 15: Queue
    Pattern 16: Deque
    Pattern 17: Monotonic Stack
    Pattern 18: Monotonic Queue
  
  Formatting Rules (internal reference for consistency):
  - Heading hierarchy: # (chapter) â†’ ## (pattern) â†’ ### (section)
  - 20 sections per pattern
  - Complexity notation: O(n), O(n log n), O(nÂ²)
  - Star rating: â˜… (filled), â˜† (empty), scale 1â€“5
  - Code style: C++17, 4-space indent, STL preferred
  - Tables: GitHub-flavored markdown pipe tables
  - Diagrams: ASCII art in code blocks
  - Problem references: "LeetCode #ID â€” Name" format
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-->

# Chapter 6: Stack & Queue

Stacks and queues are fundamental data structures that support a specific access pattern. Their power in interviews comes from their constrained nature â€” by restricting how elements are accessed, they enable elegant O(n) solutions to problems that seem to require O(nÂ²).

**Key insight**: A stack processes elements in **last-in, first-out** (LIFO) order â€” perfect for matching, nesting, and "nearest previous" problems. A queue processes in **first-in, first-out** (FIFO) order â€” perfect for level-order processing and BFS.

The **monotonic** variants (monotonic stack and monotonic queue) maintain elements in sorted order, enabling powerful range queries in O(n).

---
---

# Pattern 14: Stack

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  STACK
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Goldman Sachs
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Problems involving **matching, nesting, and most-recent-element** queries. A stack is the perfect tool when:
- You need to match opening brackets with closing brackets.
- You need to evaluate nested expressions.
- You need to undo the most recent operation.
- You need to track "the last thing I haven't resolved yet."

**Why was this pattern invented?**

Stacks mirror the way nesting and recursion work. Every function call creates a stack frame. Every opening bracket waits for its matching close. Every "undo" reverts the last action. The LIFO property naturally models these relationships.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem involve **matching pairs** (brackets, tags, etc.)?
- [ ] Is there a **nesting structure** (expressions, HTML, etc.)?
- [ ] Do you need to process elements and **go back** to the most recent unprocessed one?
- [ ] Does the problem mention "undo," "backtrack," or "most recent"?
- [ ] Is there a simulation that involves "push" and "pop" behavior?

**If you checked 2+ of these â†’ Stack.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points to Stack |
|---|---|
| "Valid parentheses" | Classic stack matching |
| "Evaluate expression" | Operator precedence with stack |
| "Decode string" / "nested encoding" | Stack for nesting levels |
| "Simplify path" | Stack for directory navigation |
| "Remove adjacent duplicates" | Stack as working buffer |
| "Asteroid collision" | Stack to resolve collisions |
| "Min stack" / "design" | Stack with auxiliary tracking |

---

## 4. Constraint Analysis

| Constraint | Stack Approach | Why Stack |
|---|---|---|
| n â‰¤ 10â¶ | O(n) âœ… | Each element pushed/popped at most once |
| Nested depth â‰¤ 10â´ | O(depth) space âœ… | Stack depth = nesting depth |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each closing bracket, scan backward to find its matching opening bracket." â†’ O(nÂ²).

### Step 2: Key Observation
> "The most recent unmatched opening bracket is always the one that should match the current closing bracket. That's LIFO â€” a stack!"

### Step 3: Optimization
> "Push opening brackets onto a stack. When a closing bracket appears, pop the top. If they match, great. If the stack is empty or mismatched, it's invalid."

### Step 4: Optimal Solution
> O(n) time, O(n) space. Single pass with a stack.

---

## 6. Generic Algorithm

```
BRACKET_MATCHING(expression):
    stack = empty stack
    
    for each char in expression:
        if char is OPENING:
            stack.push(char)
        else if char is CLOSING:
            if stack is empty:
                return false
            if stack.top() matches char:
                stack.pop()
            else:
                return false
    
    return stack is empty
```

---

## 7. Reusable C++ Template

```cpp
// Template: Stack â€” Bracket / Pair Matching
// Use when: matching opening/closing pairs
// Time: O(n), Space: O(n)

#include <string>
#include <stack>
#include <unordered_map>
using namespace std;

bool isValid(string s) {
    stack<char> stk;
    unordered_map<char, char> match = {
        {')', '('}, {']', '['}, {'}', '{'}
    };

    for (char c : s) {
        if (match.count(c)) {
            // Closing bracket
            if (stk.empty() || stk.top() != match[c]) return false;
            stk.pop();
        } else {
            // Opening bracket
            stk.push(c);
        }
    }

    return stk.empty();
}
```

```cpp
// Template: Stack â€” Expression Evaluation
// Use when: evaluate mathematical expressions with operators

#include <string>
#include <stack>
using namespace std;

int evaluate(string s) {
    stack<int> nums;
    stack<char> ops;
    // MODIFY: implement operator precedence and evaluation logic
    // Process tokens left to right, using stacks for numbers and operators
    return nums.top();
}
```

---

## 8. Complexity Analysis

| Metric | Value | Why |
|---|---|---|
| Time | O(n) | Each element is pushed once and popped at most once |
| Space | O(n) | Stack can hold up to n elements in worst case |

---

## 9. Dry Run

**Problem**: Valid Parentheses â€” `"({[]})"`.

```
String: ( { [ ] } )
Stack: []

char '(' â†’ push â†’ Stack: ['(']
char '{' â†’ push â†’ Stack: ['(', '{']
char '[' â†’ push â†’ Stack: ['(', '{', '[']
char ']' â†’ matches top '[' â†’ pop â†’ Stack: ['(', '{']
char '}' â†’ matches top '{' â†’ pop â†’ Stack: ['(']
char ')' â†’ matches top '(' â†’ pop â†’ Stack: []

Stack empty â†’ return true âœ“
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Not checking `stack.empty()` before `stack.top()` | Always check emptiness before accessing top |
| Forgetting to check if stack is empty at the end | Stack must be empty for valid matching |
| Using wrong matching pairs | Use a map for clean matching logic |
| Confusing stack with queue for nested problems | Nesting = LIFO = Stack, not Queue |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| FIFO processing (level-order, BFS) | Need first-in-first-out | Queue |
| Need to access middle elements | Stack only exposes top | Deque or array |
| Sorting or ordering elements | Stack doesn't sort | Priority queue, sorting |
| Sliding window min/max | Plain stack can't handle window sliding | Monotonic Deque |

---

## 12. Medium-Level Example Problem

**LeetCode #394 â€” Decode String**

> Given an encoded string like `"3[a2[c]]"`, return the decoded string: `"accaccacc"`.

**Why this represents the pattern well**: The nested brackets create a natural stack problem. Each `[` pushes the current state, and each `]` pops and combines.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. "Nested structure â†’ Stack."
2. "When I see a `[`, I save the current string and multiplier, then start fresh."
3. "When I see a `]`, I pop the saved state, repeat the current string, and append."
4. "I need two stacks or a stack of pairs: (previous_string, multiplier)."

---

## 14. C++ Implementation

```cpp
#include <string>
#include <stack>
using namespace std;

// Decode String: "3[a2[c]]" â†’ "accaccacc"
// Pattern: Stack (nested structure)
// Time: O(output_length), Space: O(n)
string decodeString(string s) {
    stack<pair<string, int>> stk;  // (previous string, repeat count)
    string current;
    int num = 0;

    for (char c : s) {
        if (isdigit(c)) {
            num = num * 10 + (c - '0');  // build multi-digit number
        } else if (c == '[') {
            stk.push({current, num});    // save current state
            current = "";                 // reset for inner content
            num = 0;
        } else if (c == ']') {
            auto [prev, repeat] = stk.top();
            stk.pop();
            string repeated;
            for (int i = 0; i < repeat; i++) {
                repeated += current;
            }
            current = prev + repeated;   // combine with previous
        } else {
            current += c;               // regular character
        }
    }

    return current;
}
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "What if the encoding can have errors?" | Add validation: unmatched brackets, missing numbers |
| "What if the encoded string is very long with deep nesting?" | Watch for stack depth; iterative is better than recursive |
| "Implement a calculator with +, -, *, / and parentheses?" | Two stacks: numbers and operators, with precedence rules |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Expression evaluation** | Stack of operators + numbers with precedence |
| **Simplify path** | LeetCode #71. Stack of directory components, handle ".." |
| **Remove adjacent duplicates** | Use stack as working buffer |
| **Asteroid collision** | Stack to resolve left/right moving asteroids |
| **Min Stack** | Auxiliary stack tracking minimums |

---

## 17. Related Patterns

| Pattern | Relationship |
|---|---|
| **Monotonic Stack** | Specialized stack that maintains order |
| **Queue** | FIFO vs LIFO â€” different processing order |
| **Recursion** | Recursion uses the call stack implicitly; iterative stack solutions mirror recursion |
| **DFS** | DFS uses a stack (implicitly or explicitly) |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #20 â€” Valid Parentheses | Easy | Classic bracket matching |
| 2 | LeetCode #155 â€” Min Stack | Medium | Stack with auxiliary tracking |
| 3 | LeetCode #394 â€” Decode String | Medium | Nested structure processing |
| 4 | LeetCode #227 â€” Basic Calculator II | Medium | Expression evaluation |
| 5 | LeetCode #84 â€” Largest Rectangle in Histogram | Hard | Stack-based area calculation |

---

## 19. Memory Trick

> ðŸ§  **"Stack of Plates"**
>
> A stack is like a stack of plates: you always add to the top and remove from the top. When you encounter something that "opens" (a bracket, a function call, a new nesting level), put a plate on top. When you encounter something that "closes," take the top plate off and check if it matches.

---
---

# Pattern 15: Queue

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  QUEUE
  OA Frequency: â˜…â˜…â˜…â˜…â˜†
  Companies: Amazon | Google | Microsoft | Meta
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Problems involving **first-in, first-out (FIFO) processing**. When elements must be processed in the order they arrive â€” like a line at a grocery store â€” a queue is the natural choice.

**Why was this pattern invented?**

Queues model real-world fairness: "whoever arrived first gets served first." In algorithms, queues are the backbone of **BFS** (breadth-first search) â€” processing nodes level by level.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem require processing elements **in arrival order**?
- [ ] Is there a **level-order** or **breadth-first** traversal?
- [ ] Does the problem involve a **simulation** with a line/queue?
- [ ] Do elements wait for their turn to be processed?

**If you checked 2+ of these â†’ Queue.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points to Queue |
|---|---|
| "BFS" / "level-order" | BFS uses a queue |
| "Process in order of arrival" | FIFO = queue |
| "Simulation with a line" | Queue models waiting lines |
| "Task scheduling (FIFO)" | Round-robin, FIFO scheduling |
| "Sliding window" (some variants) | Deque = double-ended queue |

---

## 4. Constraint Analysis

Queues are O(1) per operation (push, pop, front). They're rarely the bottleneck â€” the algorithm using the queue determines complexity.

---

## 5. Evolution of Thought

Queue is a data structure, not an algorithmic pattern per se. The evolution is:

1. "I need to process elements in the order they arrive."
2. "I need to explore a graph/tree level by level."
3. "I need FIFO behavior." â†’ Use a queue.

---

## 6. Generic Algorithm

```
BFS_WITH_QUEUE(start):
    queue = empty queue
    visited = empty set
    queue.push(start)
    visited.add(start)
    
    while queue is not empty:
        current = queue.front()
        queue.pop()
        
        process(current)
        
        for each neighbor of current:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.push(neighbor)
```

---

## 7. Reusable C++ Template

```cpp
// Template: Queue â€” BFS / Level-order Processing
// Time: O(V + E) for graphs, O(n) for arrays
// Space: O(n)

#include <queue>
#include <vector>
using namespace std;

void bfsTemplate(int start, vector<vector<int>>& adj) {
    int n = adj.size();
    vector<bool> visited(n, false);
    queue<int> q;

    q.push(start);
    visited[start] = true;

    while (!q.empty()) {
        int node = q.front();
        q.pop();

        // MODIFY: process node here

        for (int neighbor : adj[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
        }
    }
}
```

---

## 8. Complexity Analysis

| Operation | Time | Why |
|---|---|---|
| push | O(1) amortized | Appending to back |
| pop / front | O(1) | Accessing front element |
| BFS total | O(V + E) | Each node and edge processed once |

---

## 9. Dry Run

**BFS on graph**: Start from node 0.

```
Adjacency: 0â†’[1,2], 1â†’[3], 2â†’[3,4], 3â†’[], 4â†’[]

Queue: [0]  Visited: {0}
  Pop 0 â†’ process â†’ push 1, 2
Queue: [1, 2]  Visited: {0, 1, 2}
  Pop 1 â†’ process â†’ push 3
Queue: [2, 3]  Visited: {0, 1, 2, 3}
  Pop 2 â†’ process â†’ 3 already visited, push 4
Queue: [3, 4]  Visited: {0, 1, 2, 3, 4}
  Pop 3 â†’ process â†’ no new neighbors
  Pop 4 â†’ process â†’ no new neighbors
Queue: empty â†’ done

BFS order: 0, 1, 2, 3, 4
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Not marking visited before pushing | Mark visited WHEN pushing, not when popping, to avoid duplicates |
| Using a stack instead of queue for BFS | Stack gives DFS, not BFS |
| Forgetting to check queue emptiness | Always check `!q.empty()` before accessing front |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| DFS-like exploration | Need LIFO | Stack or recursion |
| Priority-based processing | Need best-first, not FIFO | Priority Queue / Heap |
| Need to access both ends | Standard queue only has front access | Deque |

---

## 12. Medium-Level Example Problem

**LeetCode #933 â€” Number of Recent Calls**

> Implement a class that counts the number of recent requests within a time frame.

---

## 13. Solution Walkthrough

Use a queue to store timestamps. When a new request comes in, remove all timestamps older than `t - 3000` from the front. The queue size is the answer.

---

## 14. C++ Implementation

```cpp
#include <queue>
using namespace std;

class RecentCounter {
    queue<int> q;
public:
    int ping(int t) {
        q.push(t);
        while (q.front() < t - 3000) {
            q.pop();  // remove timestamps outside the window
        }
        return q.size();
    }
};
```

---

## 15â€“19. (Abbreviated for Queue â€” core usage is covered in BFS chapters)

Queue is primarily a building block for BFS (Chapter 11), Multi-Source BFS (Chapter 11), and Tree BFS (Chapter 13). See those chapters for full treatment.

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #933 â€” Number of Recent Calls | Easy | Queue as sliding window |
| 2 | LeetCode #622 â€” Design Circular Queue | Medium | Queue implementation |
| 3 | LeetCode #649 â€” Dota2 Senate | Medium | Queue simulation |
| 4 | LeetCode #950 â€” Reveal Cards in Increasing Order | Medium | Queue simulation |
| 5 | LeetCode #239 â€” Sliding Window Maximum | Hard | Queue/Deque application |

**Memory Trick:**

> ðŸ§  **"The Grocery Line"** â€” Queue = grocery store line. First person in line gets served first. No cutting. No going backward. Fair and orderly.

---
---

# Pattern 16: Deque

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DEQUE (DOUBLE-ENDED QUEUE)
  OA Frequency: â˜…â˜…â˜…â˜†â˜†
  Companies: Google | Amazon | Microsoft | Meta
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

When you need **efficient insertion and removal from BOTH ends** â€” something neither a stack (back only) nor a queue (front remove, back insert) can do alone. Deque = Stack + Queue in one structure.

**Why was this pattern invented?**

Problems like "Sliding Window Maximum" require adding new elements at one end and removing old elements from the other, while also removing elements from the same end when they become irrelevant. A deque supports all these operations in O(1).

---

## 2. Pattern Identification Checklist

- [ ] Do you need to add/remove elements from **both ends** efficiently?
- [ ] Is the problem a **sliding window** variant that needs more than just sum tracking?
- [ ] Do you need a structure that acts as both a stack and a queue?

**If you checked any of these â†’ Consider Deque.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points to Deque |
|---|---|
| "Sliding window maximum/minimum" | Monotonic deque is the standard solution |
| "Access from both ends" | Deque supports front and back operations |
| "BFS with 0-1 weights" | 0-1 BFS uses deque (push 0-weight to front, 1-weight to back) |

---

## 4. Constraint Analysis

| Operation | `deque` | `vector` | `queue` | `stack` |
|---|---|---|---|---|
| Push front | O(1) âœ… | O(n) âŒ | âŒ | âŒ |
| Push back | O(1) âœ… | O(1) âœ… | O(1) âœ… | O(1) âœ… |
| Pop front | O(1) âœ… | O(n) âŒ | O(1) âœ… | âŒ |
| Pop back | O(1) âœ… | O(1) âœ… | âŒ | O(1) âœ… |
| Access front | O(1) âœ… | O(1) âœ… | O(1) âœ… | âŒ |
| Access back | O(1) âœ… | O(1) âœ… | âŒ | O(1) âœ… |
| Random access | O(1) âœ… | O(1) âœ… | âŒ | âŒ |

---

## 5â€“7. Generic Algorithm and Template

Deque's main interview application is the **Monotonic Deque** (Pattern 18). As a standalone data structure:

```cpp
#include <deque>
using namespace std;

deque<int> dq;
dq.push_back(1);    // add to back
dq.push_front(0);   // add to front
dq.pop_back();       // remove from back
dq.pop_front();      // remove from front
dq.front();          // access front: 0
dq.back();           // access back
dq[i];               // random access
dq.size();           // current size
dq.empty();          // check emptiness
```

---

## 8â€“19. (Deque's full power is demonstrated in Pattern 18: Monotonic Queue)

Deque as a standalone pattern is primarily a data structure enabling the Monotonic Queue pattern and 0-1 BFS. See Pattern 18 below for the full treatment.

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #641 â€” Design Circular Deque | Medium | Deque implementation |
| 2 | LeetCode #239 â€” Sliding Window Maximum | Hard | Monotonic deque |
| 3 | LeetCode #862 â€” Shortest Subarray with Sum â‰¥ K | Hard | Monotonic deque with prefix sums |

**Memory Trick:**

> ðŸ§  **"The Double Door"** â€” A deque is a hallway with doors on both ends. People can enter and exit from either door. A stack has only one door (back). A queue has an entrance (back) and an exit (front) but you can't go the other way. The deque has no restrictions.

---
---

# Pattern 17: Monotonic Stack

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  MONOTONIC STACK
  OA Frequency: â˜…â˜…â˜…â˜…â˜†
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Uber
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"For each element, find the **nearest greater** (or nearest smaller) element to its left or right."

Brute force: For each element, scan left/right â†’ O(nÂ²). Monotonic stack: Process all elements in O(n) by maintaining a stack of elements in **increasing or decreasing order**.

**Why was this pattern invented?**

When you look at element `arr[i]`, the "nearest greater element to the left" is somewhere in `arr[0..i-1]`. But you don't need to check all of them. If `arr[j] â‰¤ arr[k]` and `j < k`, then `arr[j]` will never be the answer for any element to the right of `k` â€” `arr[k]` "shadows" `arr[j]`. The monotonic stack maintains only the unshadowed elements, processing each element exactly once.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem ask for the **next/previous greater/smaller** element?
- [ ] Does it involve **spans** (how far back can you go while maintaining a property)?
- [ ] Does the problem mention **stock span**, **daily temperatures**, or **histogram**?
- [ ] Can the problem be modeled as "for each element, find the nearest element satisfying a condition"?
- [ ] Is there an O(nÂ²) brute force where the inner loop searches for a boundary?

**If you checked 2+ of these â†’ Monotonic Stack.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Next greater element" | Direct application |
| "Previous smaller element" | Direct application |
| "Daily temperatures" / "days until warmer" | Next greater element in temperature |
| "Stock span" | How many consecutive days price was â‰¤ today |
| "Largest rectangle in histogram" | Stack-based boundary finding |
| "Trapping rain water" (stack approach) | Find bounding walls |
| "Remove k digits to make smallest number" | Greedy removal with monotonic stack |

---

## 4. Constraint Analysis

| Constraint | Brute Force O(nÂ²) | Monotonic Stack O(n) | Verdict |
|---|---|---|---|
| n â‰¤ 5000 | 2.5 Ã— 10â· âš ï¸ | 5000 âœ… | Stack preferred |
| n â‰¤ 10âµ | 10Â¹â° âŒ | 10âµ âœ… | Stack required |
| n â‰¤ 10â¶ | 10Â¹Â² âŒ | 10â¶ âœ… | Stack required |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each element i, scan right to find the first element greater than arr[i]." â†’ O(nÂ²).

### Step 2: Key Observation
> "When I find that arr[j] > arr[i], then arr[j] is the answer for arr[i]. But it might also be the answer for elements between i and j that are smaller than arr[j]."

```
Array: [4, 2, 1, 5, 3]

For arr[0]=4: scan right â†’ arr[3]=5 is next greater.
For arr[1]=2: scan right â†’ arr[3]=5 is next greater.
For arr[2]=1: scan right â†’ arr[3]=5 is next greater.

When we reach 5, it "answers" all three pending elements at once!
```

### Step 3: Optimization
> "Maintain a stack of elements waiting for their 'next greater.' When a new element is larger than the stack's top, pop and record the answer. Repeat until the stack's top is larger or empty."

### Step 4: Optimal Solution
> O(n) â€” each element is pushed once and popped at most once. Total operations = 2n.

---

## 6. Generic Algorithm

```
NEXT_GREATER_ELEMENT(arr):
    n = length(arr)
    result = array of size n, initialized to -1
    stack = empty stack    // stores indices
    
    for i from 0 to n-1:
        // Pop elements that are SMALLER than arr[i]
        // arr[i] is their "next greater element"
        while stack is not empty AND arr[stack.top()] < arr[i]:
            idx = stack.top()
            stack.pop()
            result[idx] = arr[i]
        
        stack.push(i)
    
    return result
    // Elements remaining in stack have no next greater element (result stays -1)
```

**Four variants:**

| Variant | Stack Order | Pop Condition | Scan Direction |
|---|---|---|---|
| Next Greater | Decreasing | `arr[top] < arr[i]` | Left â†’ Right |
| Next Smaller | Increasing | `arr[top] > arr[i]` | Left â†’ Right |
| Previous Greater | Decreasing | `arr[top] â‰¤ arr[i]` | Left â†’ Right (store before push) |
| Previous Smaller | Increasing | `arr[top] â‰¥ arr[i]` | Left â†’ Right (store before push) |

---

## 7. Reusable C++ Template

```cpp
// Template: Monotonic Stack â€” Next Greater Element
// Use when: find next greater/smaller for each element
// Time: O(n), Space: O(n)

#include <vector>
#include <stack>
using namespace std;

// Next Greater Element (right side)
vector<int> nextGreater(vector<int>& arr) {
    int n = arr.size();
    vector<int> result(n, -1);
    stack<int> stk;  // stores indices, maintains decreasing order of values

    for (int i = 0; i < n; i++) {
        // Pop elements that found their next greater
        while (!stk.empty() && arr[stk.top()] < arr[i]) {  // MODIFY: < for greater, > for smaller
            result[stk.top()] = arr[i];  // MODIFY: store value or index
            stk.pop();
        }
        stk.push(i);
    }

    return result;  // -1 means no next greater element exists
}

// Previous Smaller Element (left side)
vector<int> prevSmaller(vector<int>& arr) {
    int n = arr.size();
    vector<int> result(n, -1);
    stack<int> stk;  // stores indices, maintains increasing order of values

    for (int i = 0; i < n; i++) {
        // Pop elements that are >= current (they won't be anyone's prev smaller)
        while (!stk.empty() && arr[stk.top()] >= arr[i]) {
            stk.pop();
        }
        result[i] = stk.empty() ? -1 : arr[stk.top()];  // answer is current top
        stk.push(i);
    }

    return result;
}
```

---

## 8. Complexity Analysis

| Metric | Value | Why |
|---|---|---|
| Time | O(n) | Each element is pushed once and popped at most once: 2n total operations |
| Space | O(n) | Stack can hold up to n elements |

**Amortized argument**: Total pushes = n. Total pops â‰¤ n (can't pop more than pushed). Total = 2n = O(n).

---

## 9. Dry Run

**Problem**: Next Greater Element for `[4, 2, 1, 5, 3]`.

```
Array: [4, 2, 1, 5, 3]
Stack: [] (stores indices)

i=0: arr[0]=4
  Stack empty â†’ push 0
  Stack: [0(4)]

i=1: arr[1]=2
  2 < 4 â†’ don't pop â†’ push 1
  Stack: [0(4), 1(2)]

i=2: arr[2]=1
  1 < 2 â†’ don't pop â†’ push 2
  Stack: [0(4), 1(2), 2(1)]

i=3: arr[3]=5
  5 > 1 â†’ pop 2, result[2] = 5
  5 > 2 â†’ pop 1, result[1] = 5
  5 > 4 â†’ pop 0, result[0] = 5
  Stack empty â†’ push 3
  Stack: [3(5)]

i=4: arr[4]=3
  3 < 5 â†’ don't pop â†’ push 4
  Stack: [3(5), 4(3)]

Remaining in stack: indices 3, 4 â†’ no next greater â†’ result stays -1

Result: [5, 5, 5, -1, -1]

Verify:
  arr[0]=4 â†’ next greater = 5 âœ“
  arr[1]=2 â†’ next greater = 5 âœ“
  arr[2]=1 â†’ next greater = 5 âœ“
  arr[3]=5 â†’ no next greater âœ“
  arr[4]=3 â†’ no next greater âœ“
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Storing values instead of indices | Usually store indices â€” you can always get values from indices |
| Wrong comparison direction (< vs >) | "Next Greater" pops when `arr[top] < arr[i]`. "Next Smaller" pops when `arr[top] > arr[i]` |
| Forgetting to initialize result to -1 | Elements with no answer should return -1 |
| Using the wrong variant (next vs previous) | Next: answer is the new element. Previous: answer is the current top before pushing. |
| Not handling equal elements correctly | Decide: does "greater" mean `>` or `>=`? This affects `<` vs `<=` in the pop condition. |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Need nearest greater in a sliding window | Plain monotonic stack doesn't handle window expiry | Monotonic Deque |
| Need kth greatest (not just nearest) | Stack only tracks nearest | Sorting or priority queue |
| "Global" comparisons (not nearest neighbor) | Monotonic stack finds local relationships | Sorting, segment tree |

---

## 12. Medium-Level Example Problem

**LeetCode #739 â€” Daily Temperatures**

> Given daily temperatures, for each day find how many days you must wait until a warmer temperature. If no warmer day exists, return 0.

**Why this represents the pattern well**: It's "Next Greater Element" but returning the distance instead of the value. Perfectly demonstrates the monotonic stack's pop-and-record mechanism.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. "For each day, find the next warmer day â†’ Next Greater Element."
2. "Instead of recording the value, record the distance: `i - popped_index`."
3. "Stack stores indices of days waiting for a warmer day."
4. "When a warmer day arrives, pop all colder days and calculate wait times."

---

## 14. C++ Implementation

```cpp
#include <vector>
#include <stack>
using namespace std;

// Daily Temperatures â€” days until warmer temperature
// Pattern: Monotonic Stack (Next Greater)
// Time: O(n), Space: O(n)
vector<int> dailyTemperatures(vector<int>& temperatures) {
    int n = temperatures.size();
    vector<int> result(n, 0);  // 0 means no warmer day
    stack<int> stk;            // indices of days waiting for warmer

    for (int i = 0; i < n; i++) {
        // Pop days that found their warmer day
        while (!stk.empty() && temperatures[stk.top()] < temperatures[i]) {
            int prevDay = stk.top();
            stk.pop();
            result[prevDay] = i - prevDay;  // distance
        }
        stk.push(i);
    }

    return result;
}
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "What about PREVIOUS warmer day?" | Scan right to left, or use the "previous greater" variant |
| "What if we need next greater in a circular array?" | Process array twice (or use modular index): LeetCode #503 |
| "Largest rectangle in histogram?" | Use monotonic stack to find left and right boundaries: LeetCode #84 |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Next Greater in Circular Array** | LeetCode #503. Process `2n` elements using `i % n` |
| **Largest Rectangle in Histogram** | LeetCode #84. Find left/right smaller boundaries for each bar |
| **Stock Span** | LeetCode #901. Count consecutive days with lower price = previous greater distance |
| **Remove K Digits** | LeetCode #402. Greedy + monotonic stack to build smallest number |
| **Sum of Subarray Minimums** | LeetCode #907. Use both next-smaller and previous-smaller |

---

## 17. Related Patterns

| Pattern | Relationship |
|---|---|
| **Stack** | Monotonic stack IS a stack with an ordering invariant |
| **Monotonic Queue/Deque** | Handles sliding window version of the same problems |
| **Two Pointers** | "Trapping Rain Water" can use either two pointers or monotonic stack |
| **Segment Tree** | For range min/max queries (more powerful but more complex) |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #496 â€” Next Greater Element I | Easy | Basic next greater with hash map |
| 2 | LeetCode #739 â€” Daily Temperatures | Medium | Next greater with distance |
| 3 | LeetCode #503 â€” Next Greater Element II | Medium | Circular array variant |
| 4 | LeetCode #84 â€” Largest Rectangle in Histogram | Hard | Left/right boundaries |
| 5 | LeetCode #907 â€” Sum of Subarray Minimums | Hard | Both previous and next smaller |

---

## 19. Memory Trick

> ðŸ§  **"The Domino Effect"**
>
> Picture a row of dominoes of different heights. When a tall domino (new element) arrives, it knocks down all shorter dominoes in front of it (pop from stack). The tall domino is the "next greater" for all the short ones it knocked over. Dominoes that were too tall to be knocked down stay standing (remain in stack).

---
---

# Pattern 18: Monotonic Queue

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  MONOTONIC QUEUE (MONOTONIC DEQUE)
  OA Frequency: â˜…â˜…â˜…â˜†â˜†
  Companies: Google | Amazon | Microsoft | Uber
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find the **maximum (or minimum) element in every sliding window** of size k." More generally: maintain access to the extreme value in a dynamic range.

Brute force: For each window, scan k elements â†’ O(nÂ·k).
Monotonic deque: Maintain elements in decreasing order. The front of the deque is always the maximum. â†’ O(n).

**Why was this pattern invented?**

It's the sliding-window version of the monotonic stack. The stack can find "next greater" but can't handle elements leaving the window from the left. The **deque** supports removal from both ends: elements leave from the front (when they exit the window) and from the back (when they're dominated by a new element).

---

## 2. Pattern Identification Checklist

- [ ] Does the problem ask for **max or min in a sliding window**?
- [ ] Is there a **fixed-size window** moving across the array and you need an extreme value?
- [ ] Do you need to maintain a "best candidate" that can expire?
- [ ] Does the problem involve a dynamic range query where elements enter and exit from different ends?

**If you checked 2+ of these â†’ Monotonic Queue (Deque).**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Maximum in each window of size k" | Direct application |
| "Sliding window maximum/minimum" | Classic monotonic deque problem |
| "Shortest subarray with sum â‰¥ k" (with negatives) | Monotonic deque + prefix sums |
| "Max in subarray of bounded length" | Deque tracks candidates |

---

## 4. Constraint Analysis

| Constraint | Brute O(nÂ·k) | Heap O(n log n) | Mono Deque O(n) | Verdict |
|---|---|---|---|---|
| n â‰¤ 10â´, k â‰¤ 100 | 10â¶ âœ… | 10â´ Ã— 14 âœ… | 10â´ âœ… | All work |
| n â‰¤ 10âµ, k â‰¤ 10âµ | 10Â¹â° âŒ | 10âµ Ã— 17 âœ… | 10âµ âœ… | Heap or deque |
| n â‰¤ 10â¶ | Impossible | 10â¶ Ã— 20 âš ï¸ | 10â¶ âœ… | Deque preferred |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each window, scan all k elements to find the max." â†’ O(nÂ·k).

### Step 2: Optimization Attempt (Heap)
> "Use a max-heap. Add entering elements, remove leaving elements." â†’ But heap removal is O(n) unless using lazy deletion â†’ O(n log n) with lazy.

### Step 3: Key Observation
> "In the window [l, r], if `arr[i] â‰¤ arr[j]` and `i < j`, then `arr[i]` can never be the max while `arr[j]` is in the window. We can discard `arr[i]`."

### Step 4: Monotonic Deque
> "Maintain a deque of indices in decreasing order of their values. When a new element enters:
> 1. Remove from the back all elements â‰¤ new (they're dominated).
> 2. Add new to back.
> 3. Remove from front if the front index is outside the window.
> 4. The front is always the maximum." â†’ O(n).

---

## 6. Generic Algorithm

```
SLIDING_WINDOW_MAX(arr, k):
    n = length(arr)
    deque = empty deque (stores indices)
    result = []
    
    for i from 0 to n-1:
        // Remove indices outside the window
        while deque is not empty AND deque.front() <= i - k:
            deque.pop_front()
        
        // Remove elements from back that are â‰¤ arr[i] (dominated)
        while deque is not empty AND arr[deque.back()] <= arr[i]:
            deque.pop_back()
        
        deque.push_back(i)
        
        // Record answer once window is full
        if i >= k - 1:
            result.append(arr[deque.front()])  // front is max
    
    return result
```

---

## 7. Reusable C++ Template

```cpp
// Template: Monotonic Deque â€” Sliding Window Maximum
// Use when: find max (or min) in every window of size k
// Time: O(n), Space: O(k)

#include <vector>
#include <deque>
using namespace std;

vector<int> slidingWindowMax(vector<int>& nums, int k) {
    deque<int> dq;  // stores indices in decreasing order of values
    vector<int> result;

    for (int i = 0; i < (int)nums.size(); i++) {
        // Remove indices outside the window
        while (!dq.empty() && dq.front() <= i - k) {
            dq.pop_front();
        }

        // Remove from back: elements smaller than current (they're useless)
        while (!dq.empty() && nums[dq.back()] <= nums[i]) {  // MODIFY: <= for max, >= for min
            dq.pop_back();
        }

        dq.push_back(i);

        // Record answer once window is complete
        if (i >= k - 1) {
            result.push_back(nums[dq.front()]);  // front is the answer
        }
    }

    return result;
}
```

---

## 8. Complexity Analysis

| Metric | Value | Why |
|---|---|---|
| Time | O(n) | Each element is pushed and popped from deque at most once (2n total) |
| Space | O(k) | Deque holds at most k elements |

---

## 9. Dry Run

**Problem**: Sliding Window Maximum, `nums = [1, 3, -1, -3, 5, 3, 6, 7]`, k = 3.

```
Deque: [] (stores indices)

i=0: nums[0]=1
  Back empty â†’ push 0. Deque: [0(1)]
  Window not full yet.

i=1: nums[1]=3
  nums[back=0]=1 â‰¤ 3 â†’ pop_back. Deque: []
  Push 1. Deque: [1(3)]
  Window not full yet.

i=2: nums[2]=-1
  nums[back=1]=3 > -1 â†’ don't pop.
  Push 2. Deque: [1(3), 2(-1)]
  Window [0,1,2] full â†’ result: nums[front=1] = 3

i=3: nums[3]=-3
  nums[back=2]=-1 > -3 â†’ don't pop.
  Push 3. Deque: [1(3), 2(-1), 3(-3)]
  Check front: 1 > 3-3=0 â†’ front stays.
  result: nums[front=1] = 3

i=4: nums[4]=5
  nums[back=3]=-3 â‰¤ 5 â†’ pop. Deque: [1(3), 2(-1)]
  nums[back=2]=-1 â‰¤ 5 â†’ pop. Deque: [1(3)]
  nums[back=1]=3 â‰¤ 5 â†’ pop. Deque: []
  Push 4. Deque: [4(5)]
  Check front: 4 > 4-3=1 â†’ front stays.
  result: nums[front=4] = 5

i=5: nums[5]=3
  nums[back=4]=5 > 3 â†’ don't pop.
  Push 5. Deque: [4(5), 5(3)]
  result: nums[front=4] = 5

i=6: nums[6]=6
  nums[back=5]=3 â‰¤ 6 â†’ pop. Deque: [4(5)]
  nums[back=4]=5 â‰¤ 6 â†’ pop. Deque: []
  Push 6. Deque: [6(6)]
  result: nums[front=6] = 6

i=7: nums[7]=7
  nums[back=6]=6 â‰¤ 7 â†’ pop. Deque: []
  Push 7. Deque: [7(7)]
  result: nums[front=7] = 7

Result: [3, 3, 5, 5, 6, 7] âœ“
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Using a regular queue instead of deque | Need `pop_back` to remove dominated elements |
| Forgetting to remove expired indices from front | Check `dq.front() <= i - k` before recording answer |
| Wrong ordering (increasing vs decreasing) | For max: maintain decreasing. For min: maintain increasing. |
| Storing values instead of indices | Must store indices to check window expiry |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Need sum/average of window (not max/min) | Sum is incrementally updateable without deque | Fixed sliding window |
| Need arbitrary range queries (not sliding) | Deque works for sliding window only | Segment Tree, Sparse Table |
| Window size isn't fixed | Different approach needed | Variable sliding window |
| Need top-k elements (not just max) | Deque only maintains the one extreme | Heap or sorted structure |

---

## 12. Medium-Level Example Problem

**LeetCode #239 â€” Sliding Window Maximum**

> Given an array `nums` and a window size `k`, return the maximum element in each sliding window.

---

## 13. Solution Walkthrough

Already covered in the dry run above. The key insight: maintain a deque of indices where values are in decreasing order. Front = maximum. New elements eliminate all smaller elements from the back. Expired elements are removed from the front.

---

## 14. C++ Implementation

```cpp
#include <vector>
#include <deque>
using namespace std;

// Sliding Window Maximum
// Pattern: Monotonic Deque
// Time: O(n), Space: O(k)
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    deque<int> dq;
    vector<int> result;

    for (int i = 0; i < (int)nums.size(); i++) {
        // Remove expired elements from front
        if (!dq.empty() && dq.front() <= i - k) {
            dq.pop_front();
        }

        // Remove dominated elements from back
        while (!dq.empty() && nums[dq.back()] <= nums[i]) {
            dq.pop_back();
        }

        dq.push_back(i);

        // Window is complete
        if (i >= k - 1) {
            result.push_back(nums[dq.front()]);
        }
    }

    return result;
}
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "What about sliding window minimum?" | Change `<=` to `>=` in the pop_back condition |
| "What if k varies per window?" | More complex â€” consider segment tree |
| "What about both max AND min in each window?" | Two deques: one for max (decreasing), one for min (increasing) |
| "Shortest subarray with sum â‰¥ k (with negatives)?" | LeetCode #862. Monotonic deque on prefix sums |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Sliding Window Minimum** | Maintain increasing deque instead of decreasing |
| **Jump Game VI** | LeetCode #1696. DP + monotonic deque for sliding max |
| **Shortest Subarray with Sum â‰¥ K** | LeetCode #862. Deque on prefix sums |
| **Longest Subarray with abs diff â‰¤ limit** | LeetCode #1438. Two deques (max + min) |

---

## 17. Related Patterns

| Pattern | Relationship |
|---|---|
| **Monotonic Stack** | Stack version: finds nearest greater/smaller. Deque version: adds window expiry. |
| **Fixed Sliding Window** | Sliding window tracks sum; deque tracks max/min |
| **Heap** | Can also solve sliding window max but O(n log n) vs deque's O(n) |
| **Segment Tree** | Handles arbitrary range max/min queries, not just sliding |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #239 â€” Sliding Window Maximum | Hard | Classic monotonic deque |
| 2 | LeetCode #1438 â€” Longest Continuous Subarray with Abs Diff â‰¤ Limit | Medium | Two deques |
| 3 | LeetCode #1696 â€” Jump Game VI | Medium | DP + monotonic deque |
| 4 | LeetCode #862 â€” Shortest Subarray with Sum â‰¥ K | Hard | Deque on prefix sums |
| 5 | LeetCode #1499 â€” Max Value of Equation | Hard | Deque optimization |

---

## 19. Memory Trick

> ðŸ§  **"The VIP Lounge"**
>
> The monotonic deque is like a VIP lounge with strict rules:
> 1. **New VIPs** (big numbers) kick out all lesser VIPs already inside (pop from back).
> 2. **Old VIPs** whose passes expire (index outside window) are removed from the front.
> 3. **The most important VIP** is always at the front â€” that's your maximum.
>
> Nobody mediocre survives in the VIP lounge.

---
---

*Previous chapter: [05 â€” Sorting & Greedy](05_Sorting_and_Greedy.md)*
*Next chapter: [07 â€” Heap & Intervals](07_Heap_and_Intervals.md)*
<!--
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DSA PATTERN HANDBOOK â€” HEAP & INTERVALS
  Chapter 07
  
  Patterns in this chapter:
    Pattern 19: Heap / Priority Queue
    Pattern 20: Interval Problems
    Pattern 21: Merge Intervals
    Pattern 22: Sweep Line
  
  Formatting Rules (internal reference for consistency):
  - Heading hierarchy: # (chapter) â†’ ## (pattern) â†’ ### (section)
  - 20 sections per pattern
  - Complexity notation: O(n), O(n log n), O(nÂ²)
  - Star rating: â˜… (filled), â˜† (empty), scale 1â€“5
  - Code style: C++17, 4-space indent, STL preferred
  - Tables: GitHub-flavored markdown pipe tables
  - Diagrams: ASCII art in code blocks
  - Problem references: "LeetCode #ID â€” Name" format
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-->

# Chapter 7: Heap & Intervals

This chapter covers four patterns centered around **dynamic ordering** and **range-based problems**. Heaps efficiently track the best/worst element in a changing collection. Interval patterns handle overlapping, merging, and counting events over ranges.

---
---

# Pattern 19: Heap / Priority Queue

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  HEAP / PRIORITY QUEUE
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Uber | Flipkart
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Efficiently find the minimum or maximum element from a **changing collection**." You're repeatedly adding elements, removing elements, and querying for the best one.

A sorted array can find min/max in O(1), but insertion is O(n). An unsorted array has O(1) insertion but O(n) search. A **heap** gives O(log n) for both operations â€” the sweet spot.

**Why was this pattern invented?**

Many algorithms need to repeatedly pick the "best" item from a pool â€” Dijkstra's algorithm picks the closest unvisited node, task scheduling picks the highest-priority task, median maintenance picks from two halves. Heaps enable all of these efficiently.

---

## 2. Pattern Identification Checklist

- [ ] Do you need to repeatedly find the **min or max** from a dynamic collection?
- [ ] Does the problem mention "kth largest/smallest"?
- [ ] Does the problem involve **merging k sorted lists**?
- [ ] Does the problem mention **median of a stream**?
- [ ] Are you assigning resources to the **most/least** demanding tasks?
- [ ] Does the problem require a "greedy" choice of the current best?

**If you checked 2+ of these â†’ Heap / Priority Queue.**

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
> "Sort the collection every time I need the max." â†’ O(n log n) per query.

### Step 2: Key Observation
> "I don't need the entire sorted order â€” just the extreme (min or max). A heap maintains just enough structure to find the extreme efficiently."

### Step 3: Optimization
> "Use a priority queue. Insert in O(log n), extract max/min in O(log n)."

### Step 4: Advanced Uses
> "For kth largest: maintain a min-heap of size k â€” the top is the kth largest."
> "For median: maintain a max-heap (left half) and min-heap (right half) â€” the tops are the two middle elements."

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
// Template: Priority Queue â€” Common Patterns
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
Min-heap (size â‰¤ 2):

num=3: push â†’ heap: [3]           size=1 â‰¤ 2
num=1: push â†’ heap: [1, 3]        size=2 â‰¤ 2
num=4: push â†’ heap: [1, 3, 4]     size=3 > 2 â†’ pop min (1)
       heap: [3, 4]
num=1: 1 < top(3) â†’ skip (not in top k)
num=5: push â†’ heap: [3, 4, 5]     size=3 > 2 â†’ pop min (3)
       heap: [4, 5]
num=9: push â†’ heap: [4, 5, 9]     size=3 > 2 â†’ pop min (4)
       heap: [5, 9]

Answer: heap.top() = 5 (2nd largest) âœ“
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

**LeetCode #215 â€” Kth Largest Element in an Array**

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
| "Can you do it in O(n) average?" | Use QuickSelect (partition-based) â€” LeetCode standard |
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
| **Sorting** | Sort gives full order; heap gives only extreme â€” but cheaper for dynamic data |
| **Monotonic Deque** | Deque is O(1) for sliding window max; heap is O(log n) â€” deque is better for fixed windows |
| **Binary Search** | For kth element in static data, binary search may work |
| **Greedy** | Heap enables greedy algorithms that always pick "the best available" |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #703 â€” Kth Largest Element in Stream | Easy | Min-heap maintenance |
| 2 | LeetCode #215 â€” Kth Largest Element in Array | Medium | Core heap technique |
| 3 | LeetCode #23 â€” Merge K Sorted Lists | Hard | K-way merge |
| 4 | LeetCode #295 â€” Find Median from Data Stream | Hard | Two-heap technique |
| 5 | LeetCode #767 â€” Reorganize String | Medium | Frequency-based greedy with heap |

---

## 19. Memory Trick

> ðŸ§  **"The Talent Show"**
>
> A max-heap is like a talent show where the best performer is always on stage (at the top). New contestants enter (push), and they bubble up if they're better. When the star finishes (pop), the next best takes their place. You always know who the best is â€” just look at the stage.
>
> For kth largest with a min-heap: "Keep only the top k performers in the waiting room. The weakest of the top k (min of the heap) is at the door. If someone better shows up, the weakest gets kicked out."

---
---

# Pattern 20: Interval Problems

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  INTERVAL PROBLEMS (GENERAL)
  OA Frequency: â˜…â˜…â˜…â˜…â˜†
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Uber
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Problems involving ranges/intervals: `[start, end]`. Common questions: Do intervals overlap? How many overlap at a point? What's the minimum number of resources to handle all intervals? Can we fit all events?

**Why was this pattern invented?**

Real-world scheduling â€” meetings, flights, events â€” involves time intervals. The general pattern is: **sort the intervals** by some criterion, then process them in order to answer the question.

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

## 5â€“19. (Specific sub-patterns are covered in Patterns 21 and 22 below)

Interval problems are a category â€” the specific techniques are Merge Intervals and Sweep Line. For interval scheduling (non-overlapping maximization), see Chapter 5: Sorting + Greedy.

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #252 â€” Meeting Rooms | Easy | Sort + check overlap |
| 2 | LeetCode #56 â€” Merge Intervals | Medium | Classic merge |
| 3 | LeetCode #253 â€” Meeting Rooms II | Medium | Min rooms (heap or sweep) |
| 4 | LeetCode #435 â€” Non-overlapping Intervals | Medium | Max non-overlapping |
| 5 | LeetCode #759 â€” Employee Free Time | Hard | Merge + complement |

**Memory Trick:**

> ðŸ§  **"Sort First, Think Later"** â€” 90% of interval problems start with sorting. The only question is: sort by start or end? Start for merging, end for scheduling.

---
---

# Pattern 21: Merge Intervals

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  MERGE INTERVALS
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Flipkart | Goldman Sachs
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Given a collection of intervals, **combine overlapping intervals** into the smallest set of non-overlapping intervals that cover the same range.

Example: `[1,3], [2,6], [8,10]` â†’ `[1,6], [8,10]`.

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
| n â‰¤ 10âµ | Sort + linear merge | O(n log n) âœ… |
| Already sorted | Linear merge only | O(n) âœ… |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each pair of intervals, check if they overlap and merge." â†’ O(nÂ²).

### Step 2: Key Observation
> "If intervals are sorted by start time, overlapping intervals are adjacent. I only need to compare each interval with the last merged one."

### Step 3: Optimal
> "Sort by start. Walk through: if current overlaps with last merged interval, extend the end. Otherwise, start a new merged interval." â†’ O(n log n).

---

## 6. Generic Algorithm

```
MERGE_INTERVALS(intervals):
    sort intervals by start time
    
    merged = [intervals[0]]
    
    for i from 1 to n-1:
        if intervals[i].start <= merged.last().end:
            // Overlap â†’ extend
            merged.last().end = max(merged.last().end, intervals[i].end)
        else:
            // No overlap â†’ new interval
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
            // No overlap â†’ add new interval
            merged.push_back(interval);
        } else {
            // Overlap â†’ extend end
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

[2,6]: start=2 â‰¤ merged.back().end=3 â†’ overlap
  merged.back().end = max(3, 6) = 6
  merged = [[1,6]]

[8,10]: start=8 > merged.back().end=6 â†’ no overlap
  merged = [[1,6], [8,10]]

[15,18]: start=15 > merged.back().end=10 â†’ no overlap
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

**LeetCode #56 â€” Merge Intervals**

> Given a list of intervals, merge all overlapping intervals.

---

## 13â€“14. Solution and Implementation

Already covered above in sections 6â€“7.

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "Insert a new interval into a non-overlapping set?" | LeetCode #57. Find position, merge with overlapping neighbors |
| "What if intervals are given as a stream?" | Maintain a sorted structure (multiset) and merge on insertion |
| "Total length covered by all intervals?" | Merge first, then sum `end - start` for each merged interval |

---

## 16â€“19. Variations, Related, Practice

**Variations**: Insert Interval (#57), Interval List Intersections (#986), Remove Covered Intervals (#1288)

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #252 â€” Meeting Rooms | Easy | Check any overlap exists |
| 2 | LeetCode #56 â€” Merge Intervals | Medium | Classic merge |
| 3 | LeetCode #57 â€” Insert Interval | Medium | Insert + merge |
| 4 | LeetCode #986 â€” Interval List Intersections | Medium | Two-pointer merge |
| 5 | LeetCode #352 â€” Data Stream as Disjoint Intervals | Hard | Online merging |

**Memory Trick:**

> ðŸ§  **"Overlapping Rugs"** â€” If you lay rugs (intervals) on a floor sorted by their left edge, any rug whose left edge lands on top of the previous rug's right side overlaps. Just extend the combined rug to the farthest right edge.

---
---

# Pattern 22: Sweep Line

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  SWEEP LINE
  OA Frequency: â˜…â˜…â˜…â˜…â˜†
  Companies: Google | Amazon | Microsoft | Uber | Atlassian
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
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
| n â‰¤ 10âµ | O(n Ã— time_range) âŒ | O(n log n) âœ… | Sweep line required |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each point in time, count how many intervals contain it." â†’ O(n Ã— range).

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
// Template: Sweep Line â€” Maximum Concurrent Events
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
// Use when time range is small (e.g., 0 to 10â¶)
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

Answer: 2 rooms needed âœ“
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Wrong tie-breaking order | Think about whether boundary points overlap or not |
| Using inclusive vs exclusive end inconsistently | Clarify: does `[1,3]` occupy time 3? If exclusive, end event is at 3. If inclusive, at 4. |
| Not considering the difference array approach | For small ranges, difference array is simpler and avoids sorting |
| Integer overflow with large time values | Use `long long` if time values > 10â¹ |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Need to merge overlapping intervals | Sweep counts; merge combines | Merge Intervals |
| Need to know WHICH events overlap | Sweep only counts | Sorting + explicit tracking |
| Need weighted overlap (total weight at each point) | Simple +1/-1 doesn't capture weights | Weighted sweep line or segment tree |

---

## 12. Medium-Level Example Problem

**LeetCode #253 â€” Meeting Rooms II**

> Given an array of meeting time intervals, find the minimum number of conference rooms required.

---

## 13â€“14. Solution and Implementation

See sections 7 and 9 above for the full sweep line approach.

**Heap-based alternative:**

```cpp
#include <vector>
#include <algorithm>
#include <queue>
using namespace std;

// Meeting Rooms II â€” Heap approach
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

## 16â€“19. Variations, Related, Practice

**Variations**: Skyline Problem (#218), Car Pooling (#1094), Corporate Flight Bookings (#1109, see Difference Array)

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #1094 â€” Car Pooling | Easy | Difference array / sweep line |
| 2 | LeetCode #253 â€” Meeting Rooms II | Medium | Classic sweep line / heap |
| 3 | LeetCode #1854 â€” Maximum Population Year | Medium | Simple sweep |
| 4 | LeetCode #218 â€” The Skyline Problem | Hard | Advanced sweep line |
| 5 | LeetCode #850 â€” Rectangle Area II | Hard | 2D sweep line |

**Memory Trick:**

> ðŸ§  **"The Doorway Counter"** â€” Imagine a mall with a counter at the entrance. It goes +1 when someone enters and -1 when someone exits. At any moment, the counter shows how many people are inside. Sweep line does exactly this for intervals: +1 at start, -1 at end, and you read the counter as you sweep through time.

---
---

*Previous chapter: [06 â€” Stack & Queue](06_Stack_and_Queue.md)*
*Next chapter: [08 â€” Bit Manipulation](08_Bit_Manipulation.md)*
<!--
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DSA PATTERN HANDBOOK â€” BIT MANIPULATION
  Chapter 08
  
  Patterns in this chapter:
    Pattern 23: Bit Manipulation
  
  Formatting Rules (internal reference for consistency):
  - Heading hierarchy: # (chapter) â†’ ## (pattern) â†’ ### (section)
  - 20 sections per pattern
  - Complexity notation: O(n), O(n log n), O(nÂ²)
  - Star rating: â˜… (filled), â˜† (empty), scale 1â€“5
  - Code style: C++17, 4-space indent, STL preferred
  - Tables: GitHub-flavored markdown pipe tables
  - Diagrams: ASCII art in code blocks
  - Problem references: "LeetCode #ID â€” Name" format
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-->

# Chapter 8: Bit Manipulation

Bit manipulation uses low-level binary operations to solve problems with extreme efficiency â€” O(1) per operation, no extra space. While it may seem intimidating, the number of bit tricks needed for interviews is small and learnable.

---
---

# Pattern 23: Bit Manipulation

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  BIT MANIPULATION
  OA Frequency: â˜…â˜…â˜…â˜†â˜†
  Companies: Amazon | Google | Microsoft | Apple | Adobe | Samsung
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Problems involving **binary representations, subsets, power of 2, XOR properties, or bitwise flags**. Bits let you encode, manipulate, and query information at the hardware level â€” making operations constant time and zero space.

**Why was this pattern invented?**

Every integer is stored as bits. Operations like AND, OR, XOR, and shifts map directly to CPU instructions â€” they're the fastest operations a computer can perform. Certain mathematical properties (XOR cancellation, power-of-two check, subset enumeration) have elegant bitwise solutions.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem mention **binary representation**, bits, or powers of 2?
- [ ] Does it ask to find a **single/unique element** among duplicates?
- [ ] Does it involve **subset enumeration** (n â‰¤ 20)?
- [ ] Can the problem benefit from **XOR properties** (a âŠ• a = 0, a âŠ• 0 = a)?
- [ ] Does it ask for counting **set bits** or toggling specific bits?

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Single number" / "element appearing once" | XOR all elements |
| "Power of 2" | `n & (n-1) == 0` |
| "Count set bits" / "Hamming weight" | Brian Kernighan's algorithm |
| "Hamming distance" | XOR + count bits |
| "Subset enumeration" (n â‰¤ 20) | Bitmask iteration |
| "Missing number" | XOR of indices and values |
| "Reverse bits" | Bit-by-bit reversal |
| "Binary representation" | Direct bit operations |

---

## 4. Constraint Analysis

| Constraint | Why Bits Work |
|---|---|
| n â‰¤ 20 | Bitmask 2Â²â° â‰ˆ 10â¶ â€” feasible for subset enumeration |
| Need O(1) space for uniqueness | XOR achieves this |
| Integer operations | Bitwise ops are O(1) |

---

## 5. Evolution of Thought

**Example: Find the single non-duplicate element**

### Step 1: Brute Force
> "For each element, count its occurrences." â†’ O(nÂ²) or O(n) with hash map.

### Step 2: Key Observation
> "XOR has magical properties: `a âŠ• a = 0` and `a âŠ• 0 = a`. XOR-ing all elements cancels out duplicates."

### Step 3: Optimal
> "XOR all elements: duplicates cancel, leaving the unique one." â†’ O(n) time, O(1) space.

---

## 6. Essential Bit Operations Reference

```
Operation           C++ Syntax          Effect
â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
AND                 a & b               Both bits must be 1
OR                  a | b               Either bit is 1
XOR                 a ^ b               Exactly one bit is 1
NOT                 ~a                  Flip all bits
Left shift          a << k              Multiply by 2^k
Right shift         a >> k              Divide by 2^k (integer)
Check bit i         (a >> i) & 1        Is bit i set?
Set bit i           a | (1 << i)        Turn bit i ON
Clear bit i         a & ~(1 << i)       Turn bit i OFF
Toggle bit i        a ^ (1 << i)        Flip bit i
Clear lowest set    a & (a - 1)         Remove rightmost 1-bit
Isolate lowest set  a & (-a)            Get rightmost 1-bit
Is power of 2?      a > 0 && !(a&(a-1)) True if only one bit set
Count set bits      __builtin_popcount  GCC built-in
```

---

## 7. Reusable C++ Template

```cpp
// Template: Common Bit Manipulation Operations

#include <vector>
using namespace std;

// XOR all elements â€” find single unique
int singleNumber(vector<int>& nums) {
    int result = 0;
    for (int num : nums) {
        result ^= num;
    }
    return result;
}

// Count set bits (Kernighan's method)
int countBits(int n) {
    int count = 0;
    while (n) {
        n &= (n - 1);  // clear lowest set bit
        count++;
    }
    return count;
}

// Check if power of 2
bool isPowerOfTwo(int n) {
    return n > 0 && (n & (n - 1)) == 0;
}

// Enumerate all subsets of set represented by bitmask
void enumerateSubsets(int mask) {
    for (int sub = mask; sub > 0; sub = (sub - 1) & mask) {
        // process subset 'sub'
    }
    // don't forget the empty subset (sub = 0)
}

// Get the i-th bit (0-indexed from right)
int getBit(int n, int i) {
    return (n >> i) & 1;
}
```

---

## 8. Complexity Analysis

| Operation | Time | Space |
|---|---|---|
| Single bitwise operation | O(1) | O(1) |
| XOR all n elements | O(n) | O(1) |
| Count bits of number | O(log n) or O(set bits) | O(1) |
| Enumerate all subsets (bitmask) | O(2â¿) | O(1) per subset |

---

## 9. Dry Run

**Problem**: Find single number in `[4, 1, 2, 1, 2]`.

```
XOR all elements:
  result = 0
  result ^= 4 = 4         (binary: 100)
  result ^= 1 = 5         (binary: 101)
  result ^= 2 = 7         (binary: 111)
  result ^= 1 = 6         (binary: 110)  â† 1 cancelled
  result ^= 2 = 4         (binary: 100)  â† 2 cancelled

Answer: 4 âœ“

Why? 1 âŠ• 1 = 0, 2 âŠ• 2 = 0, leaving only 4.
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Forgetting operator precedence: `a & b == c` | Use parentheses: `(a & b) == c`. Bitwise ops have LOW precedence! |
| Using `<<` with values > 30 for `int` | `1 << 31` overflows `int`; use `1LL << 31` for `long long` |
| Confusing `&` (bitwise) with `&&` (logical) | `&` operates on bits, `&&` on booleans |
| Not handling negative numbers | Right shift of negatives is implementation-defined; use unsigned |
| Off-by-one in bit indexing | Bits are 0-indexed from the right (bit 0 = least significant) |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| n > 20 for subset enumeration | 2Â²â° â‰ˆ 10â¶ is the practical limit | DP, greedy |
| Need to store more than 64 items in a bitmask | `long long` has 64 bits | Use `bitset<N>` or hash set |
| Problem has no binary/XOR structure | Bit tricks only help when the problem is "binary" | Other patterns |

---

## 12. Medium-Level Example Problem

**LeetCode #260 â€” Single Number III**

> Given an array where exactly two elements appear once and all others appear twice, find those two elements.

**Why this represents the pattern well**: It extends the XOR trick. XOR-ing everything gives `a âŠ• b`. To separate `a` and `b`, find a set bit in `a âŠ• b` (where they differ), then partition elements by that bit.

---

## 13. Solution Walkthrough

1. "XOR all elements â†’ get `xorAll = a âŠ• b`."
2. "Find any set bit in `xorAll` â€” this bit is 1 in `a` and 0 in `b` (or vice versa)."
3. "Partition all elements into two groups by this bit."
4. "XOR each group separately â†’ duplicates cancel, leaving `a` and `b`."

---

## 14. C++ Implementation

```cpp
#include <vector>
using namespace std;

// Single Number III â€” two unique numbers
// Pattern: Bit Manipulation (XOR + partition)
// Time: O(n), Space: O(1)
vector<int> singleNumberIII(vector<int>& nums) {
    // Step 1: XOR all â†’ get a ^ b
    int xorAll = 0;
    for (int num : nums) xorAll ^= num;

    // Step 2: Find rightmost set bit (where a and b differ)
    int diffBit = xorAll & (-xorAll);  // isolate lowest set bit

    // Step 3: Partition and XOR each group
    int a = 0, b = 0;
    for (int num : nums) {
        if (num & diffBit) {
            a ^= num;    // group with this bit set
        } else {
            b ^= num;    // group without this bit
        }
    }

    return {a, b};
}
```

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "What if THREE elements appear once?" | Much harder â€” use modular counting of bits (count each bit position mod 3) |
| "What if elements appear k times and one appears once?" | General: count bits mod k |
| "Missing number from 0 to n?" | XOR all indices (0 to n) with all elements |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Single Number** | LeetCode #136. Simple XOR all |
| **Single Number II** | LeetCode #137. Count bits mod 3 |
| **Missing Number** | LeetCode #268. XOR 0..n with array |
| **Counting Bits** | LeetCode #338. `dp[i] = dp[i & (i-1)] + 1` |
| **Subsets via Bitmask** | LeetCode #78. Iterate 0 to 2^n-1 |
| **Reverse Bits** | LeetCode #190. Bit-by-bit construction |

---

## 17. Related Patterns

| Pattern | Relationship |
|---|---|
| **Hashing** | Alternative for "find unique" â€” uses O(n) space vs O(1) for XOR |
| **State Compression DP** | Uses bitmasks to represent DP states |
| **Backtracking** | Bitmasks can replace backtracking for subset enumeration when n â‰¤ 20 |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #136 â€” Single Number | Easy | XOR all |
| 2 | LeetCode #191 â€” Number of 1 Bits | Easy | Kernighan's algorithm |
| 3 | LeetCode #260 â€” Single Number III | Medium | XOR + partition |
| 4 | LeetCode #338 â€” Counting Bits | Medium | DP with bits |
| 5 | LeetCode #137 â€” Single Number II | Medium | Bit counting mod 3 |

---

## 19. Memory Trick

> ðŸ§  **"XOR = The Toggle Switch"**
>
> XOR is like a toggle switch: flip it once (a âŠ• b) to turn ON, flip it again (result âŠ• b) to turn OFF â€” back to `a`. Duplicates toggle twice, cancelling out. Only the unique element remains.
>
> **"Power of 2 = Lonely 1"** â€” A power of 2 has exactly one bit set. `n & (n-1)` removes it, leaving 0. If anything remains, it's not a power of 2.

---
---

*Previous chapter: [07 â€” Heap & Intervals](07_Heap_and_Intervals.md)*
*Next chapter: [09 â€” Matrix & Simulation](09_Matrix_and_Simulation.md)*
<!--
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DSA PATTERN HANDBOOK â€” MATRIX & SIMULATION
  Chapter 09
  
  Patterns in this chapter:
    Pattern 24: Matrix Traversal
    Pattern 25: Simulation
  
  Formatting Rules (internal reference for consistency):
  - Heading hierarchy: # (chapter) â†’ ## (pattern) â†’ ### (section)
  - 20 sections per pattern
  - Complexity notation: O(n), O(n log n), O(nÂ²)
  - Star rating: â˜… (filled), â˜† (empty), scale 1â€“5
  - Code style: C++17, 4-space indent, STL preferred
  - Tables: GitHub-flavored markdown pipe tables
  - Diagrams: ASCII art in code blocks
  - Problem references: "LeetCode #ID â€” Name" format
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-->

# Chapter 9: Matrix & Simulation

These patterns handle 2D arrays (matrices) and problems that require you to follow a set of rules step by step. While conceptually simpler than graph algorithms, they test your ability to handle indices, boundaries, and edge cases precisely.

---
---

# Pattern 24: Matrix Traversal

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  MATRIX TRAVERSAL
  OA Frequency: â˜…â˜…â˜…â˜…â˜†
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Samsung
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Navigate a 2D grid in a specific order (spiral, diagonal, zigzag) or find elements with specific properties (paths, regions, boundaries)."

Matrices are 2D arrays where movement is constrained to 4 or 8 directions. Matrix traversal problems test your ability to manage row/column indices, handle boundaries, and implement systematic scanning patterns.

---

## 2. Pattern Identification Checklist

- [ ] Is the input a **2D array / matrix / grid**?
- [ ] Does the problem ask for **spiral, diagonal, or zigzag** traversal?
- [ ] Does it ask to **rotate** or **transpose** the matrix?
- [ ] Does it involve moving through a grid following rules?
- [ ] Does "boundary" or "border" management matter?

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Spiral order" | Spiral matrix traversal |
| "Rotate matrix 90Â°" | Transpose + reverse |
| "Diagonal traversal" | Move along diagonals |
| "Set matrix zeroes" | Flag-based modification |
| "Reshape matrix" | Index remapping |
| "Boundary" / "border" | Layer-by-layer processing |

---

## 4. Key Matrix Operations Reference

```cpp
// Direction arrays for 4-directional movement
int dx[] = {-1, 1, 0, 0};  // up, down, left, right
int dy[] = {0, 0, -1, 1};

// Direction arrays for 8-directional movement
int dx[] = {-1, -1, -1, 0, 0, 1, 1, 1};
int dy[] = {-1, 0, 1, -1, 1, -1, 0, 1};

// Bounds checking
bool inBounds(int r, int c, int rows, int cols) {
    return r >= 0 && r < rows && c >= 0 && c < cols;
}

// Rotate 90Â° clockwise: transpose, then reverse each row
void rotate(vector<vector<int>>& matrix) {
    int n = matrix.size();
    // Transpose
    for (int i = 0; i < n; i++)
        for (int j = i + 1; j < n; j++)
            swap(matrix[i][j], matrix[j][i]);
    // Reverse rows
    for (auto& row : matrix)
        reverse(row.begin(), row.end());
}
```

---

## 5. Reusable C++ Template

```cpp
// Template: Spiral Matrix Traversal
// Time: O(m Ã— n), Space: O(1) extra

#include <vector>
using namespace std;

vector<int> spiralOrder(vector<vector<int>>& matrix) {
    vector<int> result;
    if (matrix.empty()) return result;

    int top = 0, bottom = matrix.size() - 1;
    int left = 0, right = matrix[0].size() - 1;

    while (top <= bottom && left <= right) {
        // Traverse right
        for (int c = left; c <= right; c++)
            result.push_back(matrix[top][c]);
        top++;

        // Traverse down
        for (int r = top; r <= bottom; r++)
            result.push_back(matrix[r][right]);
        right--;

        // Traverse left
        if (top <= bottom) {
            for (int c = right; c >= left; c--)
                result.push_back(matrix[bottom][c]);
            bottom--;
        }

        // Traverse up
        if (left <= right) {
            for (int r = bottom; r >= top; r--)
                result.push_back(matrix[r][left]);
            left++;
        }
    }

    return result;
}
```

---

## 6â€“10. Complexity, Dry Run, Mistakes

**Complexity**: O(m Ã— n) time â€” visit each cell once. O(1) extra space.

**Common Mistakes:**

| Mistake | How to Avoid |
|---|---|
| Off-by-one in boundary indices | Use inclusive bounds: `top`, `bottom`, `left`, `right` |
| Not checking `top <= bottom` and `left <= right` in inner loops | Single-row or single-column matrices cause duplicate traversal |
| Wrong direction array for diagonal movement | Draw it out: row+1, col+1 = down-right |
| Forgetting to mark cells as visited in grid DFS/BFS | Use a visited array or modify the grid in-place |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Finding shortest path in grid | Just traversal isn't enough | BFS on grid |
| Connected components in grid | Need graph traversal | DFS/BFS/Union Find |
| Dynamic queries on submatrices | Need range queries | 2D Prefix Sum, 2D Segment Tree |

---

## 12â€“19. Example, Practice

**Example**: LeetCode #54 â€” Spiral Matrix. See template above.

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #867 â€” Transpose Matrix | Easy | Basic matrix operation |
| 2 | LeetCode #48 â€” Rotate Image | Medium | In-place rotation |
| 3 | LeetCode #54 â€” Spiral Matrix | Medium | Classic spiral traversal |
| 4 | LeetCode #73 â€” Set Matrix Zeroes | Medium | In-place flag management |
| 5 | LeetCode #59 â€” Spiral Matrix II | Medium | Generate spiral-filled matrix |

**Memory Trick:**

> ðŸ§  **"Peeling the Onion"** â€” Spiral traversal peels the matrix layer by layer, like an onion. Process the outer border (top row â†’ right column â†’ bottom row â†’ left column), then move inward.

---
---

# Pattern 25: Simulation

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  SIMULATION
  OA Frequency: â˜…â˜…â˜…â˜†â˜†
  Companies: Amazon | Google | Microsoft | Apple
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Implement the exact process described in the problem, step by step." No clever algorithm â€” just translate the rules into code accurately.

Simulation problems test **implementation precision**: can you handle edge cases, boundary conditions, and complex state transitions without bugs?

---

## 2. Pattern Identification Checklist

- [ ] Does the problem describe a **process** or **game** with clear rules?
- [ ] Can you directly **translate the rules into code**?
- [ ] Is there no known algorithmic shortcut â€” you must "just do it"?
- [ ] Does the problem involve a **state machine** or step-by-step transformation?

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Game of Life" / "cellular automaton" | Simulate each step |
| "Robot on a grid" / "movement instructions" | Follow instructions |
| "Process according to rules" | Direct simulation |
| "Conway's" / "step by step" | Iterative simulation |
| "What is the state after k steps?" | Run k iterations |

---

## 4. Reusable C++ Template

```cpp
// Template: Simulation â€” Step-by-step processing
// Time: O(steps Ã— work_per_step), Space: O(state_size)

#include <vector>
using namespace std;

void simulate(vector<vector<int>>& grid, int steps) {
    int rows = grid.size(), cols = grid[0].size();

    for (int step = 0; step < steps; step++) {
        // Create next state (don't modify current while reading it)
        vector<vector<int>> next = grid;  // MODIFY: or separate computation

        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                // MODIFY: apply rules to compute next[r][c]
                // based on grid[r][c] and its neighbors
            }
        }

        grid = next;  // update to next state
    }
}
```

---

## 5â€“10. Key Principles

**Key principles for simulation problems:**

1. **Separate read and write**: Don't modify the state you're reading from. Use a copy or encode both states.
2. **Boundary checking**: Always validate indices before accessing neighbors.
3. **State encoding**: Game of Life trick â€” encode current and next state in same cell using bit manipulation.
4. **Early termination**: If state repeats (cycle detection), you can skip ahead.

**Common Mistakes:**

| Mistake | How to Avoid |
|---|---|
| Modifying state while iterating | Use a copy or double-buffering |
| Missing boundary checks | Always check `r >= 0 && r < rows && c >= 0 && c < cols` |
| Off-by-one in step counting | Clarify: does step 1 mean after 1 update or the initial state? |
| Not detecting cycles (for large step counts) | If state space is finite, cycles exist â€” detect with hash set |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Step count is very large (10â¹) | Direct simulation is too slow | Math / cycle detection / matrix exponentiation |
| There's a known formula | Simulation is needlessly slow | Direct computation |
| The process has a greedy or DP structure | More efficient approaches exist | Greedy / DP |

---

## 12â€“19. Example, Practice

**Example**: LeetCode #289 â€” Game of Life.

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #657 â€” Robot Return to Origin | Easy | Simple movement simulation |
| 2 | LeetCode #289 â€” Game of Life | Medium | Classic grid simulation with state encoding |
| 3 | LeetCode #874 â€” Walking Robot Simulation | Medium | Obstacle-aware movement |
| 4 | LeetCode #68 â€” Text Justification | Hard | String formatting simulation |
| 5 | LeetCode #65 â€” Valid Number | Hard | State machine parsing |

**Memory Trick:**

> ðŸ§  **"Be the Computer"** â€” In simulation problems, YOU are the CPU. Read the instructions, execute them exactly, track the state, and don't optimize what doesn't need optimizing. The challenge is accuracy, not cleverness.

---
---

*Previous chapter: [08 â€” Bit Manipulation](08_Bit_Manipulation.md)*
*Next chapter: [10 â€” Recursion & Backtracking](10_Recursion_and_Backtracking.md)*
<!--
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DSA PATTERN HANDBOOK â€” RECURSION & BACKTRACKING
  Chapter 10
  
  Patterns in this chapter:
    Pattern 26: Recursion
    Pattern 27: Backtracking
  
  Formatting Rules (internal reference for consistency):
  - Heading hierarchy: # (chapter) â†’ ## (pattern) â†’ ### (section)
  - 20 sections per pattern
  - Complexity notation: O(n), O(n log n), O(nÂ²)
  - Star rating: â˜… (filled), â˜† (empty), scale 1â€“5
  - Code style: C++17, 4-space indent, STL preferred
  - Tables: GitHub-flavored markdown pipe tables
  - Diagrams: ASCII art in code blocks
  - Problem references: "LeetCode #ID â€” Name" format
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-->

# Chapter 10: Recursion & Backtracking

Recursion is the foundation of many algorithms â€” DFS, divide & conquer, DP, and backtracking all use recursive thinking. Backtracking is recursion with a twist: you explore choices, and when a choice leads to a dead end, you **undo** it and try the next option.

---
---

# Pattern 26: Recursion

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  RECURSION
  OA Frequency: â˜…â˜…â˜…â˜…â˜†
  Companies: Amazon | Google | Microsoft | Meta | Adobe
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
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
| "Divide and conquer" | Split â†’ solve â†’ combine |
| "Tree traversal" | Trees are recursive structures |
| "Generate all" | Recursive enumeration |
| "Power function" / "fast exponentiation" | Recursive halving |
| "Fibonacci" / "recurrence" | Mathematical recursion |

---

## 4. The Three Components of Every Recursive Function

```
RECURSIVE_FUNCTION(input):
    // 1. BASE CASE â€” when to stop
    if input is trivial:
        return trivial_answer
    
    // 2. RECURSIVE CALL â€” solve smaller version
    smaller_result = RECURSIVE_FUNCTION(smaller_input)
    
    // 3. COMBINE â€” build answer from smaller result
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
// Template: Recursion â€” Tree problems

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
// Template: Recursion â€” Divide and Conquer

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
| No base case â†’ infinite recursion | Always define at least one base case first |
| Wrong base case â†’ incorrect answer | Test with the smallest inputs |
| Stack overflow for large n | Convert to iteration or use tail recursion |
| Redundant recursive calls (e.g., fib) | Add memoization â†’ becomes DP |
| Not returning from base case | Always `return` in the base case |

---

## 7. When NOT to Use

| Situation | Why Not | Use Instead |
|---|---|---|
| Large n (>10â´) without memoization | Stack overflow | Iterative DP or BFS |
| Overlapping subproblems | Pure recursion is exponential | Memoization / DP |
| Tail recursion available | Compilers may not optimize | Convert to loop |

---

## 8. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #509 â€” Fibonacci Number | Easy | Basic recursion with memoization |
| 2 | LeetCode #104 â€” Maximum Depth of Binary Tree | Easy | Tree recursion |
| 3 | LeetCode #50 â€” Pow(x, n) | Medium | Divide & conquer |
| 4 | LeetCode #23 â€” Merge K Sorted Lists | Hard | Divide & conquer merge |
| 5 | LeetCode #241 â€” Different Ways to Add Parentheses | Medium | Expression tree recursion |

**Memory Trick:**

> ðŸ§  **"Russian Dolls"** â€” Each doll contains a smaller version of itself. To count all dolls, open the outer one (solve for n), count what's inside (recursive call for n-1), and add 1 (combine). The smallest doll has nothing inside â€” that's the base case.

---
---

# Pattern 27: Backtracking

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  BACKTRACKING
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Uber | Flipkart
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
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
- [ ] Is n small (â‰¤ 15-20)?

**If you checked 2+ of these â†’ Backtracking.**

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
| n â‰¤ 8 | 8! = 40K âœ… | Much less âœ… | Both work |
| n â‰¤ 10 | 10! = 3.6M âœ… | â‰ˆ 10âµ âœ… | Pruning helps |
| n â‰¤ 15 | 15! â‰ˆ 10Â¹Â² âŒ | With good pruning âš ï¸ | Pruning essential |
| n â‰¤ 20 | 2Â²â° = 10â¶ for subsets âœ… | âœ… | Subsets OK, perms NO |
| n > 20 | âŒ | Usually âŒ | Need DP or other approach |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "Generate every possible configuration, check each for validity." â†’ Exponential.

### Step 2: Key Observation
> "I can check validity **incrementally**. If placing a queen in row 3 conflicts with row 1, there's no point placing queens in rows 4-8 â€” the entire subtree is invalid."

### Step 3: Optimization (Pruning)
> "Before making a choice, check if it's compatible with previous choices. If not, skip it immediately."

### Step 4: Backtracking Framework
> "Choose â†’ Explore â†’ Unchoose. This is the core backtracking pattern."

```
                        []
                    /    |    \
                  [1]   [2]   [3]      â† Choose first element
                 / \    / \    / \
              [1,2][1,3][2,1]...        â† Choose second element
              ...   ...   ...           â† If invalid, PRUNE (don't go deeper)
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
// Template A: Backtracking â€” Generate All Permutations
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
// Template B: Backtracking â€” Generate All Subsets
// Time: O(2â¿), Space: O(n)

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
// Template C: Backtracking â€” Constraint Satisfaction (N-Queens style)

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
| All permutations | O(n Â· n!) | O(n) call stack | n! leaves, O(n) per leaf to copy |
| All subsets | O(n Â· 2â¿) | O(n) | 2â¿ subsets, O(n) per copy |
| All combinations C(n,k) | O(k Â· C(n,k)) | O(k) | C(n,k) leaves |
| N-Queens | O(n!) worst | O(n) | Pruning reduces dramatically |

**With pruning**: The actual runtime is often much less than the worst case, but the theoretical bound stays the same.

---

## 9. Dry Run

**Problem**: Generate all permutations of `[1, 2, 3]`.

```
backtrack([], used=[F,F,F])
â”œâ”€â”€ choose 1: backtrack([1], used=[T,F,F])
â”‚   â”œâ”€â”€ choose 2: backtrack([1,2], used=[T,T,F])
â”‚   â”‚   â””â”€â”€ choose 3: [1,2,3] âœ“ RECORD
â”‚   â”‚       â””â”€â”€ unchoose 3 â†’ [1,2]
â”‚   â”œâ”€â”€ unchoose 2 â†’ [1]
â”‚   â”œâ”€â”€ choose 3: backtrack([1,3], used=[T,F,T])
â”‚   â”‚   â””â”€â”€ choose 2: [1,3,2] âœ“ RECORD
â”‚   â”‚       â””â”€â”€ unchoose 2 â†’ [1,3]
â”‚   â””â”€â”€ unchoose 3 â†’ [1]
â”œâ”€â”€ unchoose 1 â†’ []
â”œâ”€â”€ choose 2: backtrack([2], used=[F,T,F])
â”‚   â”œâ”€â”€ choose 1: [2,1,3] âœ“
â”‚   â”œâ”€â”€ choose 3: [2,3,1] âœ“
â”œâ”€â”€ choose 3: backtrack([3], used=[F,F,T])
â”‚   â”œâ”€â”€ choose 1: [3,1,2] âœ“
â”‚   â””â”€â”€ choose 2: [3,2,1] âœ“

Result: [1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]
6 permutations = 3! âœ“
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Forgetting to UNCHOOSE (undo the choice) | Every choose must have a corresponding unchoose |
| Not pruning â€” exploring invalid states | Add validity check before making the choice |
| Duplicates in results | Sort input + skip duplicates: `if (i > start && nums[i] == nums[i-1]) continue;` |
| Wrong starting index for combinations | Use `i = start` (not 0) to avoid generating the same combination in different orders |
| Stack overflow for large inputs | Backtracking is inherently recursive â€” check if n is small enough |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| n > 20 (for subset problems) | 2Â²â° â‰ˆ 10â¶ is the limit | DP or mathematical formula |
| Need count only (not actual configurations) | Backtracking generates all; counting is wasteful | DP (counting) |
| Optimal solution (min/max) | Backtracking finds all; optimization needs DP | DP |
| No pruning opportunities | Backtracking = brute force without pruning | Different approach |

---

## 12. Medium-Level Example Problem

**LeetCode #39 â€” Combination Sum**

> Given an array of distinct integers `candidates` and a target, return all unique combinations that sum to target. Each number may be used unlimited times.

**Why this represents the pattern well**: Classic backtracking with a twist â€” you CAN reuse elements (start from `i`, not `i+1`). The "sum exceeds target" check provides natural pruning.

---

## 13. Solution Walkthrough

1. "Generate combinations by choosing whether to include each candidate."
2. "Since we can reuse, start from `i` (not `i+1`) in the recursive call."
3. "Prune when `remaining < 0` â€” sum exceeded target."
4. "Base case: `remaining == 0` â€” found a valid combination."

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
| "Return count only?" | Replace vector recording with counter â€” or use DP |
| "Find ONE combination (not all)?" | Return `true` on first success; short-circuit |
| "What's the time complexity?" | Hard to state precisely â€” depends on branching factor and pruning |

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
| **DP** | When overlapping subproblems exist, add memoization to backtracking â†’ DP |
| **Bitmask** | For n â‰¤ 20, bitmask enumeration can replace backtracking for subsets |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #78 â€” Subsets | Medium | Choose/skip each element |
| 2 | LeetCode #46 â€” Permutations | Medium | Classic backtracking |
| 3 | LeetCode #39 â€” Combination Sum | Medium | Backtracking with reuse |
| 4 | LeetCode #51 â€” N-Queens | Hard | Constraint backtracking |
| 5 | LeetCode #37 â€” Sudoku Solver | Hard | Complex constraint propagation |

---

## 19. Memory Trick

> ðŸ§  **"The Maze Explorer"**
>
> Backtracking is like exploring a maze. At each fork, you pick a path (CHOOSE). If it's a dead end, you walk back to the fork (UNCHOOSE) and try the next path (EXPLORE). You mark where you've been to avoid loops. If you find the exit, you're done.
>
> **The Three Steps**: "Choose â†’ Explore â†’ Unchoose." Tattoo this on your brain. Every backtracking problem follows this exact pattern.

---
---

*Previous chapter: [09 â€” Matrix & Simulation](09_Matrix_and_Simulation.md)*
*Next chapter: [11 â€” Graph Traversal](11_Graph_Traversal.md)*
<!--
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DSA PATTERN HANDBOOK â€” GRAPH TRAVERSAL
  Chapter 11
  
  Patterns in this chapter:
    Pattern 28: DFS (Depth-First Search)
    Pattern 29: BFS (Breadth-First Search)
    Pattern 30: Multi-Source BFS
    Pattern 31: Topological Sort
  
  Formatting Rules (internal reference for consistency):
  - Heading hierarchy: # (chapter) â†’ ## (pattern) â†’ ### (section)
  - 20 sections per pattern
  - Complexity notation: O(n), O(n log n), O(nÂ²)
  - Star rating: â˜… (filled), â˜† (empty), scale 1â€“5
  - Code style: C++17, 4-space indent, STL preferred
  - Tables: GitHub-flavored markdown pipe tables
  - Diagrams: ASCII art in code blocks
  - Problem references: "LeetCode #ID â€” Name" format
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-->

# Chapter 11: Graph Traversal

Graph traversal is the backbone of countless interview problems. This chapter covers the four most essential traversal patterns: DFS, BFS, Multi-Source BFS, and Topological Sort.

**Key concepts** used throughout:
- **Graph representation**: Adjacency list (`vector<vector<int>>`) is the standard for interviews
- **Visited tracking**: `vector<bool>` or `unordered_set<int>`
- **Grid as graph**: Each cell is a node; 4 neighbors (up/down/left/right) are edges

---
---

# Pattern 28: DFS (Depth-First Search)

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DFS (DEPTH-FIRST SEARCH)
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Uber | Flipkart
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Explore a graph (or grid) by going as **deep as possible** along each branch before backtracking." DFS finds connected components, detects cycles, and computes paths/subtree properties.

**Why was this pattern invented?**

DFS uses the call stack (or explicit stack) to remember which nodes to explore next. It naturally processes "depth before breadth," making it ideal for:
- Exploring all paths
- Detecting cycles
- Computing subtree properties
- Finding connected components

---

## 2. Pattern Identification Checklist

- [ ] Does the problem involve **connected components** in a graph or grid?
- [ ] Does it ask about **paths** or **reachability**?
- [ ] Does it involve **cycle detection**?
- [ ] Is it about counting **regions** (like "number of islands")?
- [ ] Does the structure have **recursive sub-problems** (trees, graphs)?
- [ ] Can the problem be solved by "mark and explore"?

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points to DFS |
|---|---|
| "Number of islands" | Connected components on grid |
| "Connected components" | Standard DFS application |
| "Detect cycle" (directed graph) | DFS with 3-color marking |
| "All paths from source to target" | DFS enumeration |
| "Flood fill" | Grid DFS from a starting cell |
| "Surrounded regions" | DFS from border |
| "Clone graph" | DFS with hash map for visited |

---

## 4. Constraint Analysis

| Constraint | Time | Space | Notes |
|---|---|---|---|
| V + E â‰¤ 10âµ | O(V + E) âœ… | O(V) âœ… | Standard DFS |
| Grid m Ã— n â‰¤ 10â¶ | O(m Ã— n) âœ… | O(m Ã— n) âœ… | Grid DFS |
| V â‰¤ 10â´ (recursive) | O(V + E) | O(V) stack depth | Watch for stack overflow |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each node, check if it can reach every other node." â†’ O(VÂ²) per query.

### Step 2: Key Observation
> "I can explore all reachable nodes from a starting node in one DFS. Every visited node is in the same component."

### Step 3: Optimal
> "Run DFS from each unvisited node. Each DFS call finds one connected component. Total: O(V + E)."

---

## 6. Generic Algorithm

```
DFS(node, visited, graph):
    visited[node] = true
    process(node)
    
    for each neighbor of node in graph:
        if not visited[neighbor]:
            DFS(neighbor, visited, graph)

// For connected components:
count = 0
for each node:
    if not visited[node]:
        DFS(node, visited, graph)
        count++
return count
```

**DFS on a grid:**
```
DFS_GRID(grid, r, c, visited):
    if out_of_bounds(r, c) or visited[r][c] or grid[r][c] != TARGET:
        return
    
    visited[r][c] = true
    DFS_GRID(grid, r-1, c, visited)  // up
    DFS_GRID(grid, r+1, c, visited)  // down
    DFS_GRID(grid, r, c-1, visited)  // left
    DFS_GRID(grid, r, c+1, visited)  // right
```

---

## 7. Reusable C++ Template

```cpp
// Template A: DFS on Graph (Adjacency List)
// Time: O(V + E), Space: O(V)

#include <vector>
using namespace std;

void dfs(int node, vector<vector<int>>& adj, vector<bool>& visited) {
    visited[node] = true;
    // MODIFY: process node here
    
    for (int neighbor : adj[node]) {
        if (!visited[neighbor]) {
            dfs(neighbor, adj, visited);
        }
    }
}

// Count connected components
int countComponents(int n, vector<vector<int>>& adj) {
    vector<bool> visited(n, false);
    int count = 0;
    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            dfs(i, adj, visited);
            count++;
        }
    }
    return count;
}
```

```cpp
// Template B: DFS on Grid (Number of Islands)
// Time: O(m Ã— n), Space: O(m Ã— n) for recursion stack

void dfsGrid(vector<vector<char>>& grid, int r, int c) {
    int rows = grid.size(), cols = grid[0].size();
    if (r < 0 || r >= rows || c < 0 || c >= cols || grid[r][c] != '1')
        return;
    
    grid[r][c] = '0';  // mark visited by modifying grid
    dfsGrid(grid, r - 1, c);
    dfsGrid(grid, r + 1, c);
    dfsGrid(grid, r, c - 1);
    dfsGrid(grid, r, c + 1);
}

int numIslands(vector<vector<char>>& grid) {
    int count = 0;
    for (int r = 0; r < (int)grid.size(); r++) {
        for (int c = 0; c < (int)grid[0].size(); c++) {
            if (grid[r][c] == '1') {
                dfsGrid(grid, r, c);
                count++;
            }
        }
    }
    return count;
}
```

```cpp
// Template C: DFS Cycle Detection (Directed Graph â€” 3-color)
// Colors: 0 = unvisited, 1 = in-progress, 2 = completed

bool hasCycleDFS(int node, vector<vector<int>>& adj, vector<int>& color) {
    color[node] = 1;  // mark in-progress
    
    for (int neighbor : adj[node]) {
        if (color[neighbor] == 1) return true;   // back edge â†’ cycle!
        if (color[neighbor] == 0) {
            if (hasCycleDFS(neighbor, adj, color)) return true;
        }
    }
    
    color[node] = 2;  // mark completed
    return false;
}
```

---

## 8. Complexity Analysis

| Metric | Value | Why |
|---|---|---|
| Time | O(V + E) | Each vertex visited once, each edge examined once |
| Space | O(V) | Visited array + recursion stack (up to V deep) |
| Grid | O(m Ã— n) | Each cell visited once |

---

## 9. Dry Run

**Problem**: Number of Islands in grid.

```
Grid:
  1 1 0 0 0
  1 1 0 0 0
  0 0 1 0 0
  0 0 0 1 1

DFS from (0,0): visits (0,0), (0,1), (1,0), (1,1). Mark all as '0'. Island #1.
DFS from (2,2): visits (2,2) only. Mark as '0'. Island #2.
DFS from (3,3): visits (3,3), (3,4). Mark as '0'. Island #3.

Answer: 3 islands âœ“
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Stack overflow on large grids | Use iterative DFS with explicit stack, or increase stack size |
| Forgetting to mark visited BEFORE recursion | Mark as visited when you first encounter, not after |
| Modifying grid when you shouldn't | Use a separate visited array if the grid shouldn't change |
| Wrong bounds checking | Check ALL four conditions: r â‰¥ 0, r < rows, c â‰¥ 0, c < cols |
| Using DFS for shortest path | DFS finds A path, not the SHORTEST â€” use BFS instead |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Shortest path (unweighted) | DFS doesn't guarantee shortest path | BFS |
| Level-order processing | DFS goes deep, not level-by-level | BFS |
| Very deep graphs with limited stack | Stack overflow risk | Iterative BFS |

---

## 12. Medium-Level Example Problem

**LeetCode #200 â€” Number of Islands**

> Given a 2D grid of '1's (land) and '0's (water), count the number of islands.

---

## 13â€“14. (See Template B above for full implementation)

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "What about the number of distinct islands (by shape)?" | LeetCode #694. Normalize DFS path and use hash set |
| "What if we can't modify the grid?" | Use a separate `visited` 2D array |
| "Max area of island?" | Track DFS return value = number of cells visited |

---

## 16â€“19. Practice, Memory Trick

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #200 â€” Number of Islands | Medium | Classic grid DFS |
| 2 | LeetCode #695 â€” Max Area of Island | Medium | DFS with counting |
| 3 | LeetCode #130 â€” Surrounded Regions | Medium | DFS from border |
| 4 | LeetCode #207 â€” Course Schedule | Medium | Cycle detection DFS |
| 5 | LeetCode #332 â€” Reconstruct Itinerary | Hard | Eulerian path DFS |

**Memory Trick:**

> ðŸ§  **"The Deep Diver"** â€” DFS dives as deep as possible into the graph before coming back up. Like a cave explorer who always takes the first unexplored tunnel, goes as deep as it goes, then backtracks to try the next tunnel.

---
---

# Pattern 29: BFS (Breadth-First Search)

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  BFS (BREADTH-FIRST SEARCH)
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Uber | Flipkart
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find the **shortest path** in an unweighted graph or grid." BFS explores nodes in order of their distance from the source â€” all nodes at distance 1 before distance 2, and so on.

**Why was this pattern invented?**

BFS guarantees that the first time you visit a node, you've arrived via the **shortest path** (in terms of edges). This makes it the go-to algorithm for unweighted shortest path problems.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem ask for the **shortest path** in an unweighted graph/grid?
- [ ] Does it require **level-order** traversal?
- [ ] Does it ask for the **minimum number of steps/moves**?
- [ ] Does the problem involve **spreading** (fire, rotten oranges)?
- [ ] Is it about finding the **nearest** something?

**If you checked 2+ of these â†’ BFS.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points to BFS |
|---|---|
| "Shortest path" (unweighted) | BFS guarantees shortest |
| "Minimum moves" / "minimum steps" | BFS level = number of steps |
| "Level-order traversal" | BFS processes level by level |
| "Nearest exit / target" | BFS from starting point |
| "Rotten oranges" / "spreading" | Multi-source BFS (Pattern 30) |
| "Word ladder" | BFS on word graph |

---

## 4. Constraint Analysis

| Constraint | Time | Space |
|---|---|---|
| V + E â‰¤ 10âµ | O(V + E) âœ… | O(V) âœ… |
| Grid m Ã— n â‰¤ 10â¶ | O(m Ã— n) âœ… | O(m Ã— n) âœ… |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "Try all possible paths from source to target, track the shortest." â†’ Exponential.

### Step 2: Key Observation
> "If I explore all neighbors at distance 1 first, then distance 2, etc., the first time I reach the target is the shortest path."

### Step 3: Optimal
> "Use a queue. Process all nodes at distance d before distance d+1. When target is reached, return d."

---

## 6. Generic Algorithm

```
BFS(graph, source, target):
    queue = [(source, 0)]    // (node, distance)
    visited = {source}
    
    while queue not empty:
        (node, dist) = queue.dequeue()
        
        if node == target:
            return dist
        
        for each neighbor of node:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.enqueue((neighbor, dist + 1))
    
    return -1    // target unreachable
```

---

## 7. Reusable C++ Template

```cpp
// Template: BFS â€” Shortest Path in Unweighted Graph/Grid
// Time: O(V + E) or O(m Ã— n), Space: O(V) or O(m Ã— n)

#include <vector>
#include <queue>
using namespace std;

// BFS on graph
int bfsShortestPath(int source, int target, vector<vector<int>>& adj) {
    int n = adj.size();
    vector<bool> visited(n, false);
    queue<pair<int, int>> q;  // (node, distance)
    
    q.push({source, 0});
    visited[source] = true;
    
    while (!q.empty()) {
        auto [node, dist] = q.front();
        q.pop();
        
        if (node == target) return dist;
        
        for (int neighbor : adj[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push({neighbor, dist + 1});
            }
        }
    }
    
    return -1;  // unreachable
}

// BFS on grid
int bfsGrid(vector<vector<int>>& grid, pair<int,int> start, pair<int,int> end) {
    int rows = grid.size(), cols = grid[0].size();
    int dx[] = {-1, 1, 0, 0};
    int dy[] = {0, 0, -1, 1};
    
    vector<vector<bool>> visited(rows, vector<bool>(cols, false));
    queue<tuple<int, int, int>> q;  // (row, col, distance)
    
    q.push({start.first, start.second, 0});
    visited[start.first][start.second] = true;
    
    while (!q.empty()) {
        auto [r, c, dist] = q.front();
        q.pop();
        
        if (r == end.first && c == end.second) return dist;
        
        for (int d = 0; d < 4; d++) {
            int nr = r + dx[d], nc = c + dy[d];
            if (nr >= 0 && nr < rows && nc >= 0 && nc < cols 
                && !visited[nr][nc] && grid[nr][nc] != 0 /* MODIFY: passable condition */) {
                visited[nr][nc] = true;
                q.push({nr, nc, dist + 1});
            }
        }
    }
    
    return -1;
}
```

---

## 8. Complexity

| Metric | Value | Why |
|---|---|---|
| Time | O(V + E) | Each vertex/edge processed once |
| Space | O(V) | Queue + visited array |

---

## 9. Dry Run

**Problem**: Shortest path from 0 to 4 in graph.

```
Graph: 0-1, 0-2, 1-3, 2-3, 3-4

Queue: [(0, 0)]  Visited: {0}

Pop (0,0). Neighbors: 1, 2.
  Push (1,1), (2,1). Visited: {0,1,2}
  Queue: [(1,1), (2,1)]

Pop (1,1). Neighbors: 0(visited), 3.
  Push (3,2). Visited: {0,1,2,3}
  Queue: [(2,1), (3,2)]

Pop (2,1). Neighbors: 0(visited), 3(visited).
  Queue: [(3,2)]

Pop (3,2). Neighbors: 1(visited), 2(visited), 4.
  Push (4,3). Visited: {0,1,2,3,4}

Pop (4,3). Node 4 == target. Return 3.

Shortest path length: 3 âœ“ (0â†’1â†’3â†’4 or 0â†’2â†’3â†’4)
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Marking visited when popping, not when pushing | Mark when PUSHING to avoid duplicates in queue |
| Using DFS for shortest path | DFS finds a path, not the shortest â€” use BFS |
| Forgetting that BFS works on UNWEIGHTED graphs only | For weighted: use Dijkstra |
| Not handling the "no path" case | Return -1 if queue empties without reaching target |

---

## 11â€“19. Example, Practice

**Example**: LeetCode #1091 â€” Shortest Path in Binary Matrix (BFS on grid).

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #733 â€” Flood Fill | Easy | BFS/DFS on grid |
| 2 | LeetCode #994 â€” Rotting Oranges | Medium | Multi-source BFS |
| 3 | LeetCode #127 â€” Word Ladder | Hard | BFS on word graph |
| 4 | LeetCode #1091 â€” Shortest Path in Binary Matrix | Medium | Grid BFS |
| 5 | LeetCode #542 â€” 01 Matrix | Medium | Multi-source BFS |

**Memory Trick:**

> ðŸ§  **"The Ripple Effect"** â€” BFS spreads like ripples in water. Drop a stone (source). The first ring hits all nodes at distance 1. The second ring hits distance 2. The first ring to reach the shore (target) is the shortest path.

---
---

# Pattern 30: Multi-Source BFS

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  MULTI-SOURCE BFS
  OA Frequency: â˜…â˜…â˜…â˜…â˜†
  Companies: Amazon | Google | Microsoft | Meta | Uber
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find the **shortest distance from any one of multiple sources** to each cell." Instead of running BFS from each source separately (O(k Ã— V)), start BFS from ALL sources simultaneously â€” O(V + E) total.

**Why was this pattern invented?**

Problems like "Rotting Oranges" have multiple starting points. Running BFS from each one independently would be wasteful. Multi-source BFS initializes the queue with ALL sources at distance 0, then spreads outward. This naturally computes the correct shortest distance from the nearest source.

---

## 2. Pattern Identification Checklist

- [ ] Are there **multiple starting points** (sources)?
- [ ] Does the problem ask for "minimum distance to the **nearest** source"?
- [ ] Does it involve **simultaneous spreading** from multiple points?
- [ ] "Rotten oranges," "gates and rooms," "distance to nearest 0"?

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Rotten oranges" / "spread infection" | Multiple rot sources spread simultaneously |
| "Distance to nearest 0/gate/source" | Multi-source BFS from all 0s/gates |
| "Minimum time for all cells to be affected" | BFS level = time step |
| "Fill from borders" | Start BFS from all border cells |

---

## 4. Reusable C++ Template

```cpp
// Template: Multi-Source BFS
// Use when: shortest distance from nearest source to each cell
// Time: O(m Ã— n), Space: O(m Ã— n)

#include <vector>
#include <queue>
using namespace std;

vector<vector<int>> multiSourceBFS(vector<vector<int>>& grid) {
    int rows = grid.size(), cols = grid[0].size();
    int dx[] = {-1, 1, 0, 0};
    int dy[] = {0, 0, -1, 1};
    
    vector<vector<int>> dist(rows, vector<int>(cols, -1));
    queue<pair<int, int>> q;
    
    // Initialize: add ALL sources to queue at distance 0
    for (int r = 0; r < rows; r++) {
        for (int c = 0; c < cols; c++) {
            if (grid[r][c] == 0 /* MODIFY: source condition */) {
                q.push({r, c});
                dist[r][c] = 0;
            }
        }
    }
    
    // BFS from all sources simultaneously
    while (!q.empty()) {
        auto [r, c] = q.front();
        q.pop();
        
        for (int d = 0; d < 4; d++) {
            int nr = r + dx[d], nc = c + dy[d];
            if (nr >= 0 && nr < rows && nc >= 0 && nc < cols && dist[nr][nc] == -1) {
                dist[nr][nc] = dist[r][c] + 1;
                q.push({nr, nc});
            }
        }
    }
    
    return dist;
}
```

---

## 5. Medium-Level Example Problem

**LeetCode #994 â€” Rotting Oranges**

> In a grid, 2 = rotten orange, 1 = fresh orange, 0 = empty. Every minute, fresh oranges adjacent to rotten ones become rotten. Return the minimum minutes until no fresh oranges remain, or -1 if impossible.

**Solution**: Multi-source BFS from all rotten oranges. BFS level = minute. After BFS, check if any fresh orange remains.

```cpp
#include <vector>
#include <queue>
using namespace std;

int orangesRotting(vector<vector<int>>& grid) {
    int rows = grid.size(), cols = grid[0].size();
    int dx[] = {-1, 1, 0, 0}, dy[] = {0, 0, -1, 1};
    queue<pair<int, int>> q;
    int fresh = 0;

    for (int r = 0; r < rows; r++)
        for (int c = 0; c < cols; c++) {
            if (grid[r][c] == 2) q.push({r, c});
            else if (grid[r][c] == 1) fresh++;
        }

    int minutes = 0;
    while (!q.empty() && fresh > 0) {
        int size = q.size();
        for (int i = 0; i < size; i++) {
            auto [r, c] = q.front(); q.pop();
            for (int d = 0; d < 4; d++) {
                int nr = r + dx[d], nc = c + dy[d];
                if (nr >= 0 && nr < rows && nc >= 0 && nc < cols && grid[nr][nc] == 1) {
                    grid[nr][nc] = 2;
                    fresh--;
                    q.push({nr, nc});
                }
            }
        }
        minutes++;
    }

    return fresh == 0 ? minutes : -1;
}
```

---

## 6â€“19. Practice, Memory Trick

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #542 â€” 01 Matrix | Medium | Distance to nearest 0 |
| 2 | LeetCode #994 â€” Rotting Oranges | Medium | Multi-source spread |
| 3 | LeetCode #286 â€” Walls and Gates | Medium | Distance from gates |
| 4 | LeetCode #1162 â€” As Far from Land as Possible | Medium | Max distance from any land |
| 5 | LeetCode #934 â€” Shortest Bridge | Medium | BFS between two islands |

**Memory Trick:**

> ðŸ§  **"Multiple Fires"** â€” Multi-source BFS is like multiple fires starting simultaneously. Each fire spreads at the same speed. The time each cell catches fire is its distance to the nearest fire source. You don't run one fire simulation per source â€” you let them all burn together.

---
---

# Pattern 31: Topological Sort

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  TOPOLOGICAL SORT
  OA Frequency: â˜…â˜…â˜…â˜…â˜†
  Companies: Amazon | Google | Microsoft | Meta | Uber | Atlassian
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Given **dependencies** (A must come before B), find a valid **ordering** that respects all dependencies." This is topological sorting of a Directed Acyclic Graph (DAG).

**Why was this pattern invented?**

Real-world problems are full of dependencies: course prerequisites, build systems, task scheduling. Topological sort computes a valid execution order in O(V + E).

---

## 2. Pattern Identification Checklist

- [ ] Is the problem about **ordering with dependencies/prerequisites**?
- [ ] Is the graph **directed** and (should be) **acyclic**?
- [ ] Does it ask "Can all tasks be completed?" (= is the graph a DAG?)
- [ ] Does it ask for a **valid ordering** of tasks?

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Course schedule" / "prerequisites" | Classic topological sort |
| "Build order" / "compilation order" | Dependency ordering |
| "Can all tasks be finished?" | Is the graph a DAG? |
| "Ordering with constraints" | Topological sort |
| "Alien dictionary" | Build graph from comparisons, topological sort |

---

## 4. Two Approaches

| Approach | Method | Cycle Detection |
|---|---|---|
| **Kahn's (BFS)** | Process nodes with in-degree 0 first | If not all nodes processed, cycle exists |
| **DFS-based** | Reverse DFS finish order | Back edge detection with 3-color |

Kahn's algorithm (BFS) is generally preferred in interviews â€” it's iterative and handles cycle detection naturally.

---

## 5. Generic Algorithm (Kahn's BFS)

```
TOPOLOGICAL_SORT_KAHN(graph):
    // Step 1: Compute in-degrees
    in_degree = array of size V, all zeros
    for each edge (u, v):
        in_degree[v] += 1
    
    // Step 2: Initialize queue with in-degree 0 nodes
    queue = all nodes with in_degree == 0
    
    // Step 3: Process
    order = []
    while queue not empty:
        node = queue.dequeue()
        order.append(node)
        
        for each neighbor of node:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.enqueue(neighbor)
    
    // Step 4: Check for cycle
    if length(order) != V:
        return "CYCLE EXISTS â€” no valid topological order"
    
    return order
```

---

## 6. Reusable C++ Template

```cpp
// Template: Topological Sort (Kahn's BFS)
// Time: O(V + E), Space: O(V + E)

#include <vector>
#include <queue>
using namespace std;

vector<int> topologicalSort(int n, vector<vector<int>>& adj) {
    vector<int> inDegree(n, 0);
    
    // Compute in-degrees
    for (int u = 0; u < n; u++) {
        for (int v : adj[u]) {
            inDegree[v]++;
        }
    }
    
    // Initialize queue with in-degree 0 nodes
    queue<int> q;
    for (int i = 0; i < n; i++) {
        if (inDegree[i] == 0) {
            q.push(i);
        }
    }
    
    // Process
    vector<int> order;
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        order.push_back(node);
        
        for (int neighbor : adj[node]) {
            inDegree[neighbor]--;
            if (inDegree[neighbor] == 0) {
                q.push(neighbor);
            }
        }
    }
    
    // Cycle check
    if ((int)order.size() != n) {
        return {};  // cycle exists â€” no valid ordering
    }
    
    return order;
}

// Can finish all courses? (LeetCode #207)
bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
    vector<vector<int>> adj(numCourses);
    for (auto& p : prerequisites) {
        adj[p[1]].push_back(p[0]);  // p[1] â†’ p[0] (prereq â†’ course)
    }
    return (int)topologicalSort(numCourses, adj).size() == numCourses;
}
```

---

## 7. Dry Run

**Problem**: Course Schedule with prereqs `[[1,0], [2,0], [3,1], [3,2]]`.

```
Graph: 0â†’1, 0â†’2, 1â†’3, 2â†’3
In-degrees: [0:0, 1:1, 2:1, 3:2]

Queue: [0]  (only node with in-degree 0)

Pop 0. Order: [0]. Reduce neighbors:
  1: inDeg 1â†’0 â†’ push 1
  2: inDeg 1â†’0 â†’ push 2
Queue: [1, 2]

Pop 1. Order: [0, 1]. Reduce neighbors:
  3: inDeg 2â†’1
Queue: [2]

Pop 2. Order: [0, 1, 2]. Reduce neighbors:
  3: inDeg 1â†’0 â†’ push 3
Queue: [3]

Pop 3. Order: [0, 1, 2, 3].
Queue: empty.

Order has 4 nodes = V â†’ no cycle âœ“
Valid course order: 0, 1, 2, 3 âœ“
```

---

## 8. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Building edges in wrong direction | For prereqs, `[course, prereq]` means `prereq â†’ course` |
| Not detecting cycles | If `order.size() != n`, cycle exists |
| Using undirected graph | Topological sort only works on DIRECTED graphs |
| Assuming unique topological order | Multiple valid orders can exist |

---

## 9. When NOT to Use

| Situation | Why Not | Use Instead |
|---|---|---|
| Undirected graph | Topological sort requires directed edges | DFS/BFS components |
| Graph has cycles | No valid topological order exists | Detect cycle and report |
| Need shortest path | Topological sort gives ordering, not distances | BFS (unweighted) or Dijkstra (weighted) |

---

## 10. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "Find all valid orderings?" | Backtracking with multiple choices at each step |
| "What if there's a cycle?" | Return empty order; Kahn's detects this automatically |
| "Course Schedule II (return the order)?" | Same algorithm, return the order vector |
| "Alien dictionary?" | Build graph from character ordering, then topological sort |

---

## 11â€“19. Practice, Memory Trick

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #207 â€” Course Schedule | Medium | Cycle detection in DAG |
| 2 | LeetCode #210 â€” Course Schedule II | Medium | Return topological order |
| 3 | LeetCode #269 â€” Alien Dictionary | Hard | Build graph + topo sort |
| 4 | LeetCode #310 â€” Minimum Height Trees | Medium | Iterative leaf removal (topo-like) |
| 5 | LeetCode #329 â€” Longest Increasing Path in Matrix | Hard | DFS + topo sort / memoization |

**Memory Trick:**

> ðŸ§  **"The Course Registration Line"**
>
> Imagine registering for courses. You can only register for a course when ALL its prerequisites are completed (in-degree = 0). When you complete a course, it "unlocks" dependent courses (decrease their in-degree). Courses with no remaining prerequisites join the registration queue.

---
---

*Previous chapter: [10 â€” Recursion & Backtracking](10_Recursion_and_Backtracking.md)*
*Next chapter: [12 â€” Advanced Graphs](12_Advanced_Graphs.md)*
<!--
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DSA PATTERN HANDBOOK â€” ADVANCED GRAPHS
  Chapter 12
  
  Patterns in this chapter:
    Pattern 32: Union Find (DSU)
    Pattern 33: Dijkstra
    Pattern 34: Floyd-Warshall
    Pattern 35: Bellman-Ford
    Pattern 36: Minimum Spanning Tree
  
  Formatting Rules (internal reference for consistency):
  - Heading hierarchy: # (chapter) â†’ ## (pattern) â†’ ### (section)
  - 20 sections per pattern
  - Complexity notation: O(n), O(n log n), O(nÂ²)
  - Star rating: â˜… (filled), â˜† (empty), scale 1â€“5
  - Code style: C++17, 4-space indent, STL preferred
  - Tables: GitHub-flavored markdown pipe tables
  - Diagrams: ASCII art in code blocks
  - Problem references: "LeetCode #ID â€” Name" format
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-->

# Chapter 12: Advanced Graphs

This chapter covers five essential graph algorithms that handle more complex scenarios than basic DFS/BFS: **dynamic connectivity** (Union Find), **weighted shortest paths** (Dijkstra, Bellman-Ford, Floyd-Warshall), and **minimum spanning trees** (Kruskal/Prim).

---
---

# Pattern 32: Union Find (Disjoint Set Union)

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  UNION FIND (DSU)
  OA Frequency: â˜…â˜…â˜…â˜…â˜†
  Companies: Amazon | Google | Microsoft | Meta | Uber | Flipkart
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Are two elements in the **same group**?" and "**Merge two groups** into one." Union Find handles dynamic connectivity â€” efficiently answering whether two nodes are connected, and merging sets.

**Why was this pattern invented?**

DFS/BFS can check connectivity, but they must traverse the entire graph each time. Union Find maintains a data structure that answers "are X and Y connected?" in nearly O(1) time, even as edges are added dynamically.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem involve **grouping/merging** elements?
- [ ] Does it ask "are X and Y in the **same group/component**?"
- [ ] Are elements being **dynamically connected** over time?
- [ ] Does it ask for the **number of connected components**?
- [ ] Is it used with **Kruskal's MST** algorithm?

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Connected components" (dynamic) | Union-Find tracks components |
| "Same group" / "same team" | Check if same set |
| "Merge groups" | Union operation |
| "Number of components after adding edges" | Count components dynamically |
| "Accounts merge" / "synonymous sentences" | Group by equivalence |
| "Redundant connection" | Detect cycle (edge connecting same component) |

---

## 4. Reusable C++ Template

```cpp
// Template: Union Find with Path Compression + Union by Rank
// Time: O(Î±(n)) â‰ˆ O(1) per operation (amortized)
// Space: O(n)

#include <vector>
using namespace std;

class UnionFind {
    vector<int> parent, rank_;
    int components;
public:
    UnionFind(int n) : parent(n), rank_(n, 0), components(n) {
        for (int i = 0; i < n; i++) parent[i] = i;
    }

    // Find root with path compression
    int find(int x) {
        if (parent[x] != x)
            parent[x] = find(parent[x]);  // path compression
        return parent[x];
    }

    // Union by rank. Returns true if merged (were in different sets)
    bool unite(int x, int y) {
        int rx = find(x), ry = find(y);
        if (rx == ry) return false;  // already connected

        // Union by rank
        if (rank_[rx] < rank_[ry]) swap(rx, ry);
        parent[ry] = rx;
        if (rank_[rx] == rank_[ry]) rank_[rx]++;
        
        components--;
        return true;
    }

    bool connected(int x, int y) { return find(x) == find(y); }
    int getComponents() { return components; }
};
```

---

## 5. Complexity Analysis

| Operation | Time (amortized) | Why |
|---|---|---|
| find() | O(Î±(n)) â‰ˆ O(1) | Path compression flattens the tree |
| unite() | O(Î±(n)) â‰ˆ O(1) | Union by rank keeps trees short |
| Build | O(n) | Initialize n singletons |
| m operations | O(m Â· Î±(n)) â‰ˆ O(m) | Î±(n) â‰¤ 5 for all practical n |

**Î±(n)** = inverse Ackermann function. It grows so slowly that it's effectively constant (â‰¤ 5 for n â‰¤ 10â¸â°).

---

## 6. Medium-Level Example

**LeetCode #547 â€” Number of Provinces** (or Friend Circles)

```cpp
int findCircleNum(vector<vector<int>>& isConnected) {
    int n = isConnected.size();
    UnionFind uf(n);
    
    for (int i = 0; i < n; i++)
        for (int j = i + 1; j < n; j++)
            if (isConnected[i][j])
                uf.unite(i, j);
    
    return uf.getComponents();
}
```

---

## 7. When NOT to Use

| Situation | Why Not | Use Instead |
|---|---|---|
| Need shortest path | Union Find tracks connectivity, not distances | BFS / Dijkstra |
| Need to DISCONNECT elements | Standard Union Find doesn't support splits | DFS/BFS per query |
| Directed graph | Union Find is for undirected equivalence | Topological Sort, DFS |

---

## 8. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #547 â€” Number of Provinces | Medium | Basic Union Find |
| 2 | LeetCode #684 â€” Redundant Connection | Medium | Detect cycle |
| 3 | LeetCode #721 â€” Accounts Merge | Medium | Group by equivalence |
| 4 | LeetCode #1319 â€” Number of Operations to Make Network Connected | Medium | Component counting |
| 5 | LeetCode #839 â€” Similar String Groups | Hard | Union Find on strings |

**Memory Trick:**

> ðŸ§  **"The Club System"** â€” Each person starts as their own club president. When two clubs merge, one president becomes the leader. `find()` asks "who's my leader?" and `unite()` merges two clubs. Path compression: everyone directly points to the top leader after the first inquiry.

---
---

# Pattern 33: Dijkstra's Algorithm

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DIJKSTRA'S ALGORITHM
  OA Frequency: â˜…â˜…â˜…â˜…â˜†
  Companies: Google | Amazon | Microsoft | Uber | Flipkart
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find the **shortest path** from a source to all other nodes in a graph with **non-negative edge weights**."

BFS handles unweighted shortest paths. When edges have different weights, BFS fails because it explores by number of edges, not total weight. Dijkstra's algorithm uses a **priority queue** to always process the node with the smallest known distance first.

---

## 2. Pattern Identification Checklist

- [ ] Is the graph **weighted with non-negative weights**?
- [ ] Does the problem ask for **shortest path** or **minimum cost**?
- [ ] Is there a **single source** for shortest paths?
- [ ] Do edges have varying costs?

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Shortest path" (weighted, non-negative) | Direct application |
| "Minimum cost path" | Same as shortest path |
| "Network delay time" | Shortest path to all nodes |
| "Cheapest flights within k stops" | Modified Dijkstra |
| "Minimum effort path" | Dijkstra on max-edge-weight metric |

---

## 4. Reusable C++ Template

```cpp
// Template: Dijkstra's Algorithm
// Time: O((V + E) log V), Space: O(V + E)

#include <vector>
#include <queue>
#include <climits>
using namespace std;

vector<long long> dijkstra(int source, vector<vector<pair<int,int>>>& adj) {
    int n = adj.size();
    vector<long long> dist(n, LLONG_MAX);
    // Min-heap: (distance, node)
    priority_queue<pair<long long,int>, vector<pair<long long,int>>, 
                   greater<pair<long long,int>>> pq;

    dist[source] = 0;
    pq.push({0, source});

    while (!pq.empty()) {
        auto [d, u] = pq.top();
        pq.pop();

        if (d > dist[u]) continue;  // skip outdated entries (lazy deletion)

        for (auto [v, w] : adj[u]) {
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                pq.push({dist[v], v});
            }
        }
    }

    return dist;  // dist[i] = shortest distance from source to i
}
```

---

## 5. Complexity Analysis

| Metric | Value | Why |
|---|---|---|
| Time | O((V + E) log V) | Each edge relaxation: O(log V) heap push |
| Space | O(V + E) | Distance array + adjacency list + heap |

With Fibonacci heap: O(V log V + E), but not practical for interviews.

---

## 6. Dry Run

```
Graph: 0â†’1(4), 0â†’2(1), 2â†’1(2), 1â†’3(1), 2â†’3(5)

dist = [0, âˆž, âˆž, âˆž]   PQ: [(0,0)]

Pop (0,0). Neighbors: 1(w=4), 2(w=1)
  dist[1] = min(âˆž, 0+4) = 4. Push (4,1).
  dist[2] = min(âˆž, 0+1) = 1. Push (1,2).
  dist = [0, 4, 1, âˆž]   PQ: [(1,2), (4,1)]

Pop (1,2). Neighbors: 1(w=2), 3(w=5)
  dist[1] = min(4, 1+2) = 3. Push (3,1).
  dist[3] = min(âˆž, 1+5) = 6. Push (6,3).
  dist = [0, 3, 1, 6]   PQ: [(3,1), (4,1), (6,3)]

Pop (3,1). Neighbors: 3(w=1)
  dist[3] = min(6, 3+1) = 4. Push (4,3).
  dist = [0, 3, 1, 4]   PQ: [(4,1), (4,3), (6,3)]

Pop (4,1). d=4 > dist[1]=3 â†’ skip (outdated).

Pop (4,3). d=4 == dist[3] â†’ process. No new improvements.

Pop (6,3). d=6 > dist[3]=4 â†’ skip.

Final: dist = [0, 3, 1, 4]
  0â†’2â†’1 costs 3, 0â†’2â†’1â†’3 costs 4 âœ“
```

---

## 7. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Using with negative weights | Dijkstra FAILS with negative weights â€” use Bellman-Ford |
| Not skipping outdated PQ entries | Always check `if (d > dist[u]) continue;` |
| Using BFS for weighted graph | BFS assumes equal weights |
| Integer overflow in distance addition | Use `long long` for distances |

---

## 8. When NOT to Use

| Situation | Why Not | Use Instead |
|---|---|---|
| Negative edge weights | Greedy approach fails | Bellman-Ford |
| All edges weight 1 | Dijkstra's overhead unnecessary | BFS |
| All-pairs shortest path (small V) | Running Dijkstra V times is expensive | Floyd-Warshall |

---

## 9. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #743 â€” Network Delay Time | Medium | Standard Dijkstra |
| 2 | LeetCode #1631 â€” Path With Minimum Effort | Medium | Dijkstra on max-edge |
| 3 | LeetCode #787 â€” Cheapest Flights Within K Stops | Medium | Modified Dijkstra/BFS |
| 4 | LeetCode #1514 â€” Path with Maximum Probability | Medium | Max-probability Dijkstra |
| 5 | LeetCode #778 â€” Swim in Rising Water | Hard | Binary search + BFS or Dijkstra |

**Memory Trick:**

> ðŸ§  **"The Greedy GPS"** â€” Dijkstra's algorithm is like a GPS that always explores the closest unexplored intersection first. It guarantees finding the shortest route because it never passes a closer intersection without checking it.

---
---

# Pattern 34: Floyd-Warshall

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  FLOYD-WARSHALL
  OA Frequency: â˜…â˜…â˜†â˜†â˜†
  Companies: Google | Amazon | Microsoft
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find shortest paths between **ALL pairs** of nodes." While Dijkstra finds shortest paths from one source, Floyd-Warshall finds them for every possible source and destination simultaneously.

---

## 2. When to Use

| Scenario | Floyd-Warshall | Alternative |
|---|---|---|
| All-pairs, V â‰¤ 400 | O(VÂ³) âœ… | â€” |
| All-pairs, V â‰¤ 1000 | O(VÂ³) âš ï¸ tight | Run Dijkstra V times: O(VÂ·(V+E)logV) |
| All-pairs, V > 1000 | âŒ too slow | Dijkstra from each source |
| Can have negative weights | âœ… works! | Bellman-Ford from each source |
| Detect negative cycles | âœ… check `dist[i][i] < 0` | Bellman-Ford |

---

## 3. Reusable C++ Template

```cpp
// Template: Floyd-Warshall â€” All Pairs Shortest Path
// Time: O(VÂ³), Space: O(VÂ²)

#include <vector>
#include <climits>
using namespace std;

void floydWarshall(vector<vector<long long>>& dist, int n) {
    // dist[i][j] initialized to edge weight or LLONG_MAX (no edge)
    // dist[i][i] = 0
    
    for (int k = 0; k < n; k++) {            // intermediate node
        for (int i = 0; i < n; i++) {          // source
            for (int j = 0; j < n; j++) {      // destination
                if (dist[i][k] != LLONG_MAX && dist[k][j] != LLONG_MAX) {
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
                }
            }
        }
    }
    
    // Negative cycle detection: dist[i][i] < 0 for some i
}
```

**Core idea**: "Can going through node k improve the path from i to j?" Try every possible intermediate node k.

---

## 4. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #1334 â€” Find the City With Fewest Reachable Neighbors | Medium | Floyd-Warshall application |
| 2 | LeetCode #399 â€” Evaluate Division | Medium | Graph as distances |
| 3 | LeetCode #1462 â€” Course Schedule IV | Medium | Reachability (transitive closure) |

**Memory Trick:**

> ðŸ§  **"The Stopover Check"** â€” Floyd-Warshall asks: "For every pair of cities (i,j), is there a stopover city k that makes the trip shorter?" It tries every possible stopover.

---
---

# Pattern 35: Bellman-Ford

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  BELLMAN-FORD
  OA Frequency: â˜…â˜…â˜†â˜†â˜†
  Companies: Google | Amazon | Microsoft | Goldman Sachs
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Shortest path from a source in a graph that may have **negative edge weights**." Dijkstra can't handle negatives. Bellman-Ford can, and also detects **negative cycles**.

---

## 2. When to Use

| Scenario | Dijkstra | Bellman-Ford |
|---|---|---|
| Non-negative weights | âœ… Faster | âœ… Works but slower |
| Negative weights | âŒ Fails | âœ… Correct |
| Detect negative cycle | âŒ Can't | âœ… Run V-th iteration |
| "At most k edges" constraint | âŒ Hard to modify | âœ… Stop after k iterations |

---

## 3. Reusable C++ Template

```cpp
// Template: Bellman-Ford â€” Shortest Path with Negative Weights
// Time: O(V Ã— E), Space: O(V)

#include <vector>
#include <climits>
using namespace std;

struct Edge { int u, v, w; };

vector<long long> bellmanFord(int n, int source, vector<Edge>& edges) {
    vector<long long> dist(n, LLONG_MAX);
    dist[source] = 0;

    // Relax all edges V-1 times
    for (int i = 0; i < n - 1; i++) {
        for (auto& [u, v, w] : edges) {
            if (dist[u] != LLONG_MAX && dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
            }
        }
    }

    // V-th iteration: detect negative cycle
    for (auto& [u, v, w] : edges) {
        if (dist[u] != LLONG_MAX && dist[u] + w < dist[v]) {
            // NEGATIVE CYCLE detected!
            return {};  // or handle accordingly
        }
    }

    return dist;
}
```

---

## 4. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #787 â€” Cheapest Flights Within K Stops | Medium | Bellman-Ford with k iterations |
| 2 | LeetCode #743 â€” Network Delay Time | Medium | Can use Dijkstra or Bellman-Ford |

**Memory Trick:**

> ðŸ§  **"The Patient Relaxer"** â€” Bellman-Ford patiently relaxes every edge, V-1 times. Each pass ensures paths with one more edge are optimized. If the V-th pass still improves something, there's a negative cycle â€” an infinite discount loop!

---
---

# Pattern 36: Minimum Spanning Tree

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  MINIMUM SPANNING TREE (MST)
  OA Frequency: â˜…â˜…â˜…â˜†â˜†
  Companies: Google | Amazon | Microsoft | Uber
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Connect all nodes with the **minimum total edge weight**." An MST is a subset of edges that connects all nodes with no cycles and minimum total weight.

---

## 2. Two Algorithms

| Algorithm | Approach | Best For | Complexity |
|---|---|---|---|
| **Kruskal's** | Sort edges, add if no cycle (Union Find) | Sparse graphs (E â‰ˆ V) | O(E log E) |
| **Prim's** | Grow tree from a node, always add cheapest edge | Dense graphs (E â‰ˆ VÂ²) | O(E log V) with heap |

**Kruskal's is more common in interviews** due to its simplicity and natural pairing with Union Find.

---

## 3. Reusable C++ Template (Kruskal's)

```cpp
// Template: Kruskal's MST with Union Find
// Time: O(E log E), Space: O(V)

#include <vector>
#include <algorithm>
using namespace std;

struct Edge { int u, v, w; };

// Uses UnionFind class from Pattern 32
long long kruskalMST(int n, vector<Edge>& edges) {
    sort(edges.begin(), edges.end(), 
         [](const Edge& a, const Edge& b) { return a.w < b.w; });

    UnionFind uf(n);
    long long totalWeight = 0;
    int edgesUsed = 0;

    for (auto& [u, v, w] : edges) {
        if (uf.unite(u, v)) {           // if u and v are in different components
            totalWeight += w;
            edgesUsed++;
            if (edgesUsed == n - 1) break;  // MST complete
        }
    }

    return edgesUsed == n - 1 ? totalWeight : -1;  // -1 if not connected
}
```

---

## 4. Kruskal's Dry Run

```
Nodes: 4   Edges: (0,1,10), (0,2,6), (0,3,5), (1,3,15), (2,3,4)

Sorted by weight: (2,3,4), (0,3,5), (0,2,6), (0,1,10), (1,3,15)

Process (2,3,4): not connected â†’ add. Weight=4. Components: 3.
Process (0,3,5): not connected â†’ add. Weight=9. Components: 2.
Process (0,2,6): already connected (0-3-2) â†’ skip.
Process (0,1,10): not connected â†’ add. Weight=19. Components: 1.

MST edges: (2,3), (0,3), (0,1). Total weight: 19.
edgesUsed = 3 = n-1 âœ“
```

---

## 5. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #1584 â€” Min Cost to Connect All Points | Medium | Kruskal's / Prim's |
| 2 | LeetCode #1135 â€” Connecting Cities With Minimum Cost | Medium | Standard MST |
| 3 | LeetCode #1489 â€” Find Critical and Pseudo-Critical Edges in MST | Hard | MST analysis |

**Memory Trick:**

> ðŸ§  **"The Cheapskate Builder"** â€” Kruskal's builds a road network by always choosing the cheapest road next. But skip it if both towns are already connected (that would create a cycle). Use Union Find to quickly check connectivity.

---

## Summary: When to Use Which Graph Algorithm

| Problem | Algorithm |
|---|---|
| Connected components (static) | DFS / BFS |
| Connected components (dynamic edges) | Union Find |
| Shortest path (unweighted) | BFS |
| Shortest path (weighted, non-negative) | Dijkstra |
| Shortest path (negative weights) | Bellman-Ford |
| All-pairs shortest path (V â‰¤ 400) | Floyd-Warshall |
| Minimum cost to connect all nodes | MST (Kruskal / Prim) |
| Dependency ordering | Topological Sort |
| Cycle detection (directed) | DFS 3-color / Topological Sort |
| Cycle detection (undirected) | Union Find / DFS |

---
---

*Previous chapter: [11 â€” Graph Traversal](11_Graph_Traversal.md)*
*Next chapter: [13 â€” Trees](13_Trees.md)*
<!--
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DSA PATTERN HANDBOOK â€” TREES
  Chapter 13
  
  Patterns in this chapter:
    Pattern 37: Binary Tree DFS
    Pattern 38: Binary Tree BFS
    Pattern 39: BST Problems
    Pattern 40: Lowest Common Ancestor (LCA)
    Pattern 41: Trie (Prefix Tree)
  
  Formatting Rules (internal reference for consistency):
  - Heading hierarchy: # (chapter) â†’ ## (pattern) â†’ ### (section)
  - 20 sections per pattern
  - Complexity notation: O(n), O(n log n), O(nÂ²)
  - Star rating: â˜… (filled), â˜† (empty), scale 1â€“5
  - Code style: C++17, 4-space indent, STL preferred
  - Tables: GitHub-flavored markdown pipe tables
  - Diagrams: ASCII art in code blocks
  - Problem references: "LeetCode #ID â€” Name" format
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-->

# Chapter 13: Trees

Trees are the single most tested topic in interviews. They appear in nearly every OA and onsite round. This chapter covers five essential tree patterns that form the foundation for solving tree problems.

**Key definitions:**
- **Binary Tree**: Each node has at most 2 children (left, right).
- **BST (Binary Search Tree)**: Left subtree < node < right subtree.
- **Height/Depth**: Height = longest root-to-leaf path. Depth = distance from root.
- **Balanced**: Height difference of left and right subtrees â‰¤ 1.

```cpp
// Standard TreeNode definition (used throughout)
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```

---
---

# Pattern 37: Binary Tree DFS

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  BINARY TREE DFS
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Samsung | Flipkart
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Compute properties of a binary tree by visiting every node using **depth-first traversal**." DFS on trees comes in three flavors (preorder, inorder, postorder), each suited for different problems.

**Why was this pattern invented?**

Trees are recursive structures â€” each subtree is itself a tree. DFS leverages this by solving the problem for left and right subtrees, then combining results. This naturally maps to recursive code.

---

## 2. Pattern Identification Checklist

- [ ] Is the input a **binary tree**?
- [ ] Does the problem ask for a **property** of the tree (height, diameter, sum, path)?
- [ ] Can the answer be computed by **combining results from left and right subtrees**?
- [ ] Does the problem involve **tree traversal** (preorder, inorder, postorder)?
- [ ] Does it ask about **paths** from root to leaf or node to node?

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Maximum depth" | Recursion: 1 + max(left, right) |
| "Diameter of tree" | Postorder: track longest path through each node |
| "Path sum" | Preorder: accumulate sum from root |
| "Invert binary tree" | Swap left/right at each node |
| "Same tree" / "symmetric tree" | Compare nodes recursively |
| "Serialize / deserialize" | Preorder traversal encoding |
| "Flatten to linked list" | Preorder traversal + pointer manipulation |

---

## 4. The Three DFS Traversal Orders

```
        1
       / \
      2   3
     / \
    4   5

Preorder  (Root-Left-Right): 1, 2, 4, 5, 3    â†’ "Process root BEFORE children"
Inorder   (Left-Root-Right): 4, 2, 5, 1, 3    â†’ "Process root BETWEEN children"
Postorder (Left-Right-Root): 4, 5, 2, 3, 1    â†’ "Process root AFTER children"
```

| Order | When to Use |
|---|---|
| **Preorder** | When you need to **process root before children** (copying, serializing, top-down computation) |
| **Inorder** | When you need **sorted order** (BST traversal gives sorted output) |
| **Postorder** | When you need **information from children first** (height, diameter, subtree sums) |

---

## 5. Evolution of Thought

### The Two Approaches for Tree DFS Problems

**Approach 1: Return value (Bottom-up)**
> "Compute the answer for left subtree, compute for right subtree, combine."
> Best for: height, size, diameter, subtree sum.

**Approach 2: Pass value down (Top-down)**
> "Pass information from parent to children. Check condition at leaves."
> Best for: path sum, root-to-leaf paths, depth.

---

## 6. Reusable C++ Templates

```cpp
// Template A: Bottom-Up (Postorder) â€” return info from subtrees
// Example: Maximum Depth

int maxDepth(TreeNode* root) {
    if (!root) return 0;                        // base case
    int leftDepth = maxDepth(root->left);       // solve left
    int rightDepth = maxDepth(root->right);     // solve right
    return 1 + max(leftDepth, rightDepth);      // combine
}
```

```cpp
// Template B: Top-Down (Preorder) â€” pass info from parent
// Example: Has Path Sum

bool hasPathSum(TreeNode* root, int targetSum) {
    if (!root) return false;                              // null node
    if (!root->left && !root->right)                      // leaf node
        return targetSum == root->val;
    return hasPathSum(root->left, targetSum - root->val)  // go left
        || hasPathSum(root->right, targetSum - root->val);// go right
}
```

```cpp
// Template C: Global Variable Pattern â€” track answer across recursive calls
// Example: Diameter of Binary Tree

int diameter;  // global result

int height(TreeNode* root) {
    if (!root) return 0;
    int left = height(root->left);
    int right = height(root->right);
    diameter = max(diameter, left + right);  // update global answer
    return 1 + max(left, right);            // return height for parent
}

int diameterOfBinaryTree(TreeNode* root) {
    diameter = 0;
    height(root);
    return diameter;
}
```

```cpp
// Template D: Iterative DFS using explicit stack
// Example: Preorder Traversal

#include <stack>
#include <vector>

vector<int> preorderIterative(TreeNode* root) {
    vector<int> result;
    if (!root) return result;
    
    stack<TreeNode*> stk;
    stk.push(root);
    
    while (!stk.empty()) {
        TreeNode* node = stk.top();
        stk.pop();
        result.push_back(node->val);
        if (node->right) stk.push(node->right);  // right first (LIFO)
        if (node->left) stk.push(node->left);     // left pops first
    }
    
    return result;
}
```

---

## 7. Complexity Analysis

| Metric | Value | Why |
|---|---|---|
| Time | O(n) | Visit each node exactly once |
| Space | O(h) | Recursion stack depth = tree height |
| Balanced tree | O(log n) space | Height = log n |
| Skewed tree | O(n) space | Height = n (worst case) |

---

## 8. Dry Run

**Problem**: Diameter of `[1, 2, 3, 4, 5]`.

```
        1
       / \
      2   3
     / \
    4   5

height(4) = 1 (leaf)
height(5) = 1 (leaf)
height(2): left=1, right=1, diameter = max(0, 1+1) = 2. Return 1+1=2.
height(3) = 1 (leaf)
height(1): left=2, right=1, diameter = max(2, 2+1) = 3. Return 1+2=3.

Answer: diameter = 3 (path: 4â†’2â†’1â†’3 or 5â†’2â†’1â†’3) âœ“
```

---

## 9. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Confusing height (edges) vs depth (nodes) | LeetCode uses nodes: height of single node = 1, not 0 |
| Not handling null nodes | Always check `if (!root) return base_value;` |
| Diameter = 2 Ã— height (wrong) | Diameter passes THROUGH a node: left_height + right_height |
| Modifying tree when you shouldn't | Use a separate data structure if tree must stay intact |
| Forgetting to return values from recursive calls | Always capture and use return values |

---

## 10. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Level-order traversal | DFS goes deep, not level by level | BFS (Pattern 38) |
| Shortest path in tree | DFS may find longer path first | BFS |
| Need to process by levels | DFS mixes levels | BFS with level tracking |

---

## 11. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #104 â€” Maximum Depth of Binary Tree | Easy | Basic bottom-up |
| 2 | LeetCode #112 â€” Path Sum | Easy | Top-down |
| 3 | LeetCode #543 â€” Diameter of Binary Tree | Easy | Global variable pattern |
| 4 | LeetCode #236 â€” Lowest Common Ancestor | Medium | Bottom-up with condition |
| 5 | LeetCode #124 â€” Binary Tree Maximum Path Sum | Hard | Global variable + complex combine |

**Memory Trick:**

> ðŸ§  **"Ask Your Children"**
>
> In postorder (bottom-up): "Hey left child, what's your height? Hey right child, what's your height? OK, I'm 1 + max of you two."
>
> In preorder (top-down): "Parent says the remaining sum is 7. I'm 3, so I tell my children the remaining sum is 4."

---
---

# Pattern 38: Binary Tree BFS (Level-Order Traversal)

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  BINARY TREE BFS
  OA Frequency: â˜…â˜…â˜…â˜…â˜†
  Companies: Amazon | Google | Microsoft | Meta | Adobe
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Process a tree **level by level**." BFS on a tree visits all nodes at depth d before any nodes at depth d+1. The key trick: process the queue in **batches** (one batch per level).

---

## 2. Pattern Identification Checklist

- [ ] Does the problem ask for **level-order** traversal or processing?
- [ ] Does it mention "each level" or "per level"?
- [ ] Does it ask for **zigzag**, **right view**, **left view**, or **average of levels**?
- [ ] Does it need **minimum depth** (shortest root-to-leaf path)?

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Level order traversal" | Direct application |
| "Right side view" | Last node of each level |
| "Zigzag order" | Alternate direction per level |
| "Average of each level" | Aggregate per level |
| "Minimum depth" | BFS finds shallowest leaf first |
| "Connect next right pointers" | Level-by-level linking |

---

## 4. Reusable C++ Template

```cpp
// Template: Binary Tree BFS â€” Level-Order Traversal
// Time: O(n), Space: O(w) where w = max width of tree

#include <vector>
#include <queue>
using namespace std;

vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> result;
    if (!root) return result;

    queue<TreeNode*> q;
    q.push(root);

    while (!q.empty()) {
        int levelSize = q.size();  // KEY: process one level at a time
        vector<int> level;

        for (int i = 0; i < levelSize; i++) {
            TreeNode* node = q.front();
            q.pop();
            level.push_back(node->val);

            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }

        result.push_back(level);
    }

    return result;
}
```

---

## 5. Dry Run

```
        3
       / \
      9  20
         / \
        15  7

Queue: [3]
Level 0: size=1 â†’ process 3. Push 9, 20.
  result: [[3]]

Queue: [9, 20]
Level 1: size=2 â†’ process 9 (no children), 20 (push 15, 7).
  result: [[3], [9, 20]]

Queue: [15, 7]
Level 2: size=2 â†’ process 15, 7 (no children).
  result: [[3], [9, 20], [15, 7]]

Queue: empty â†’ done âœ“
```

---

## 6. Variations

| Variation | Modification |
|---|---|
| **Right Side View** | Take only the LAST element of each level |
| **Zigzag** | Reverse alternate levels |
| **Minimum Depth** | Return depth of first LEAF encountered |
| **Max Width** | Track level size, return maximum |
| **Level Averages** | Sum each level, divide by count |

---

## 7. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #102 â€” Binary Tree Level Order Traversal | Medium | Classic BFS |
| 2 | LeetCode #199 â€” Binary Tree Right Side View | Medium | Last node per level |
| 3 | LeetCode #103 â€” Binary Tree Zigzag Level Order | Medium | Alternate direction |
| 4 | LeetCode #111 â€” Minimum Depth of Binary Tree | Easy | BFS finds min depth |
| 5 | LeetCode #662 â€” Maximum Width of Binary Tree | Medium | Track positions |

**Memory Trick:**

> ðŸ§  **"Floor by Floor"** â€” BFS processes a tree like reading a building floor by floor, starting from the ground floor (root). You finish everyone on floor 1 before moving to floor 2. The trick is counting how many nodes are on the current floor (`levelSize = q.size()`).

---
---

# Pattern 39: BST (Binary Search Tree) Problems

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  BST PROBLEMS
  OA Frequency: â˜…â˜…â˜…â˜…â˜†
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Goldman Sachs
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

BST problems leverage the **ordering property**: for every node, all values in the left subtree are smaller and all values in the right subtree are larger. This enables O(h) search, insert, and delete operations, and inorder traversal gives sorted output.

---

## 2. Pattern Identification Checklist

- [ ] Is the tree explicitly a **BST**?
- [ ] Does the problem involve **search, insert, delete** in a BST?
- [ ] Does it ask for the **kth smallest/largest** element?
- [ ] Does it ask to **validate** a BST?
- [ ] Can you exploit the **sorted property** (inorder = sorted)?

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Validate BST" | Range-based DFS |
| "Kth smallest in BST" | Inorder traversal (sorted) |
| "Insert/Delete in BST" | Standard BST operations |
| "Inorder successor/predecessor" | BST navigation |
| "Convert sorted array to BST" | Divide and conquer |
| "Recover BST" | Two swapped nodes in inorder |

---

## 4. Key BST Properties

```
BST Property: left.val < node.val < right.val (for all nodes)

Inorder traversal of BST = sorted array:
        5
       / \
      3   7
     / \ / \
    2  4 6  8

Inorder: 2, 3, 4, 5, 6, 7, 8  â†’ sorted! âœ“
```

---

## 5. Reusable C++ Templates

```cpp
// Template: Validate BST (range-based)
// Time: O(n), Space: O(h)

bool isValidBST(TreeNode* root, long long lo = LLONG_MIN, long long hi = LLONG_MAX) {
    if (!root) return true;
    if (root->val <= lo || root->val >= hi) return false;
    return isValidBST(root->left, lo, root->val) &&
           isValidBST(root->right, root->val, hi);
}
```

```cpp
// Template: Kth Smallest in BST (inorder traversal)
// Time: O(h + k), Space: O(h)

int kthSmallest(TreeNode* root, int k) {
    stack<TreeNode*> stk;
    TreeNode* curr = root;

    while (curr || !stk.empty()) {
        while (curr) {
            stk.push(curr);
            curr = curr->left;
        }
        curr = stk.top();
        stk.pop();
        if (--k == 0) return curr->val;
        curr = curr->right;
    }

    return -1;  // shouldn't reach here
}
```

```cpp
// Template: Search in BST
// Time: O(h)

TreeNode* searchBST(TreeNode* root, int val) {
    if (!root || root->val == val) return root;
    if (val < root->val) return searchBST(root->left, val);
    return searchBST(root->right, val);
}
```

```cpp
// Template: Convert Sorted Array to Balanced BST
// Time: O(n), Space: O(log n)

TreeNode* sortedArrayToBST(vector<int>& nums, int lo, int hi) {
    if (lo > hi) return nullptr;
    int mid = lo + (hi - lo) / 2;
    TreeNode* root = new TreeNode(nums[mid]);
    root->left = sortedArrayToBST(nums, lo, mid - 1);
    root->right = sortedArrayToBST(nums, mid + 1, hi);
    return root;
}
```

---

## 6. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Checking only immediate children for BST validity | Must check against RANGE (min, max), not just parent |
| Using `int` for range bounds | Use `long long` to handle INT_MIN/INT_MAX edge cases |
| Assuming BST is always balanced | BST can be skewed â€” height can be O(n) |
| Inorder without stack in iterative version | Morris Traversal exists but is complex; stack is fine for interviews |

---

## 7. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #700 â€” Search in a BST | Easy | Basic BST search |
| 2 | LeetCode #98 â€” Validate Binary Search Tree | Medium | Range validation |
| 3 | LeetCode #230 â€” Kth Smallest Element in BST | Medium | Inorder traversal |
| 4 | LeetCode #108 â€” Convert Sorted Array to BST | Easy | Divide & conquer |
| 5 | LeetCode #99 â€” Recover Binary Search Tree | Medium | Two swapped nodes |

**Memory Trick:**

> ðŸ§  **"The Sorted Filing Cabinet"** â€” A BST is like a filing cabinet where every drawer's label is greater than all labels to its left and less than all to its right. Inorder traversal reads labels left to right â€” giving you sorted order. Validation checks: "Is every label in its allowed range?"

---
---

# Pattern 40: Lowest Common Ancestor (LCA)

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  LOWEST COMMON ANCESTOR
  OA Frequency: â˜…â˜…â˜…â˜…â˜†
  Companies: Amazon | Google | Microsoft | Meta | Adobe
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Given two nodes in a tree, find their **deepest common ancestor**." The LCA of nodes p and q is the deepest node that has both p and q as descendants (a node can be a descendant of itself).

**Why was this pattern invented?**

LCA is a fundamental operation in trees used for computing distances between nodes, finding paths, and building other algorithms (like Tarjan's offline LCA, binary lifting for O(log n) queries).

---

## 2. Pattern Identification Checklist

- [ ] Does the problem ask for the **lowest/deepest common ancestor**?
- [ ] Does it ask for the **distance** between two nodes in a tree?
- [ ] Does it involve **path between two nodes**?

---

## 3. Two LCA Algorithms

| Tree Type | Algorithm | Time | Why |
|---|---|---|---|
| **Binary Tree** | Recursive: "which subtree contains p? which contains q?" | O(n) | Must search all nodes |
| **BST** | Exploit ordering: split point where p < node < q | O(h) | BST property guides search |

---

## 4. Reusable C++ Templates

```cpp
// Template A: LCA in Binary Tree
// Time: O(n), Space: O(h)

TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if (!root || root == p || root == q) return root;

    TreeNode* left = lowestCommonAncestor(root->left, p, q);
    TreeNode* right = lowestCommonAncestor(root->right, p, q);

    if (left && right) return root;  // p and q are in different subtrees â†’ root is LCA
    return left ? left : right;      // both are in the same subtree
}
```

**Why this works:**
- If the current node IS p or q, return it.
- Recurse left and right.
- If both return non-null, the current node is where p and q "meet" â€” it's the LCA.
- If only one returns non-null, both p and q are in that subtree.

```cpp
// Template B: LCA in BST (exploit ordering)
// Time: O(h), Space: O(1) iterative

TreeNode* lcaBST(TreeNode* root, TreeNode* p, TreeNode* q) {
    while (root) {
        if (p->val < root->val && q->val < root->val) {
            root = root->left;    // both in left subtree
        } else if (p->val > root->val && q->val > root->val) {
            root = root->right;   // both in right subtree
        } else {
            return root;          // split point â€” this is the LCA
        }
    }
    return nullptr;
}
```

---

## 5. Dry Run

```
        3
       / \
      5   1
     / \ / \
    6  2 0  8
      / \
     7   4

LCA(5, 1):
  root=3: left returns 5 (found p=5), right returns 1 (found q=1).
  Both non-null â†’ return 3. âœ“

LCA(5, 4):
  root=3: left returns 5 (contains both 5 and 4), right returns null.
  Return 5. âœ“ (5 is ancestor of 4)

LCA(6, 4):
  root=3â†’leftâ†’root=5: left returns 6 (found), rightâ†’root=2â†’leftâ†’7(null),rightâ†’4(found).
  right returns 2 (found 4 through 2's subtree? No â€” actually 4 is child of 2).
  Wait, let's trace carefully:
  
  root=3: recurse left(5) and right(1)
    root=5: recurse left(6) and right(2)
      root=6: not p or q, no children â†’ null, null â†’ return null.
      Actually 6 IS p â†’ return 6. âœ“
      root=2: recurse left(7) and right(4)
        root=7: not p or q â†’ return null
        root=4: IS q â†’ return 4
      left=null, right=4 â†’ return 4
    left=6 (non-null), right=4 (non-null) â†’ BOTH found â†’ return 5 âœ“
    
LCA(6, 4) = 5 âœ“
```

---

## 6. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #235 â€” LCA of a BST | Medium | BST-specific O(h) solution |
| 2 | LeetCode #236 â€” LCA of a Binary Tree | Medium | General binary tree |
| 3 | LeetCode #1644 â€” LCA of Binary Tree II | Medium | Nodes may not exist |
| 4 | LeetCode #1650 â€” LCA of Binary Tree III | Medium | With parent pointers |
| 5 | LeetCode #1123 â€” LCA of Deepest Leaves | Medium | LCA variation |

**Memory Trick:**

> ðŸ§  **"The Family Reunion"** â€” Two relatives (p, q) are looking for their closest common ancestor. Start from the family patriarch (root). If p and q are on different sides of the family tree, the current person is the meeting point (LCA). If both are on the same side, go deeper into that branch.

---
---

# Pattern 41: Trie (Prefix Tree)

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  TRIE (PREFIX TREE)
  OA Frequency: â˜…â˜…â˜…â˜…â˜†
  Companies: Google | Amazon | Microsoft | Meta | Adobe | Uber
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Efficiently store and search **strings by their prefixes**." A Trie stores strings character by character in a tree where each edge represents a character and each path from root represents a prefix.

**Why was this pattern invented?**

Hash maps can check if a word exists in O(L) time, but they can't efficiently answer: "How many words start with this prefix?" or "What's the longest common prefix of all words?" A Trie naturally supports prefix queries by traversing the tree.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem involve **prefix matching** or **autocomplete**?
- [ ] Does it ask about **word search** or **dictionary lookup**?
- [ ] Does it ask for the **longest common prefix**?
- [ ] Does it involve **word break** or **stream of characters** matching?
- [ ] Are there **multiple string queries** against a dictionary?

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Implement Trie" / "prefix tree" | Direct implementation |
| "Autocomplete" / "search suggestions" | Trie prefix traversal |
| "Word search in board" (advanced) | Trie for dictionary lookup during backtracking |
| "Replace words" / "prefix replacement" | Trie for prefix matching |
| "XOR maximum" (advanced) | Bitwise Trie |
| "Longest word with all prefixes" | Check each prefix exists |

---

## 4. Reusable C++ Template

```cpp
// Template: Trie (Prefix Tree)
// Time: O(L) per operation where L = word length
// Space: O(total characters across all words)

#include <string>
#include <unordered_map>
using namespace std;

struct TrieNode {
    unordered_map<char, TrieNode*> children;
    bool isEnd = false;        // marks end of a complete word
    int prefixCount = 0;       // number of words with this prefix (optional)
};

class Trie {
    TrieNode* root;
public:
    Trie() { root = new TrieNode(); }

    // Insert a word: O(L)
    void insert(const string& word) {
        TrieNode* node = root;
        for (char c : word) {
            if (!node->children.count(c)) {
                node->children[c] = new TrieNode();
            }
            node = node->children[c];
            node->prefixCount++;
        }
        node->isEnd = true;
    }

    // Search for exact word: O(L)
    bool search(const string& word) {
        TrieNode* node = findNode(word);
        return node && node->isEnd;
    }

    // Check if any word starts with prefix: O(L)
    bool startsWith(const string& prefix) {
        return findNode(prefix) != nullptr;
    }

    // Count words with given prefix: O(L)
    int countPrefix(const string& prefix) {
        TrieNode* node = findNode(prefix);
        return node ? node->prefixCount : 0;
    }

private:
    TrieNode* findNode(const string& prefix) {
        TrieNode* node = root;
        for (char c : prefix) {
            if (!node->children.count(c)) return nullptr;
            node = node->children[c];
        }
        return node;
    }
};
```

```cpp
// Alternative: Array-based TrieNode (faster, for lowercase a-z only)

struct TrieNodeArray {
    TrieNodeArray* children[26] = {};  // null-initialized
    bool isEnd = false;
};
```

---

## 5. Complexity Analysis

| Operation | Time | Space |
|---|---|---|
| Insert | O(L) | O(L) new nodes in worst case |
| Search | O(L) | O(1) |
| Prefix check | O(L) | O(1) |
| Total space for n words | â€” | O(n Ã— L) worst, often much less due to shared prefixes |

---

## 6. Dry Run

**Insert**: "apple", "app", "apricot", "bat"

```
root
â”œâ”€â”€ 'a'
â”‚   â””â”€â”€ 'p'
â”‚       â”œâ”€â”€ 'p' (prefixCount=2)
â”‚       â”‚   â”œâ”€â”€ 'l'
â”‚       â”‚   â”‚   â””â”€â”€ 'e' [isEnd=true]  â†’ "apple"
â”‚       â”‚   â””â”€â”€ [isEnd=true]          â†’ "app"
â”‚       â””â”€â”€ 'r'
â”‚           â””â”€â”€ 'i'
â”‚               â””â”€â”€ 'c'
â”‚                   â””â”€â”€ 'o'
â”‚                       â””â”€â”€ 't' [isEnd=true]  â†’ "apricot"
â””â”€â”€ 'b'
    â””â”€â”€ 'a'
        â””â”€â”€ 't' [isEnd=true]  â†’ "bat"

search("app") â†’ traverse aâ†’pâ†’p, isEnd=true â†’ true âœ“
search("ap") â†’ traverse aâ†’p, isEnd=false â†’ false âœ“
startsWith("ap") â†’ traverse aâ†’p, node exists â†’ true âœ“
startsWith("ba") â†’ traverse bâ†’a, node exists â†’ true âœ“
startsWith("ca") â†’ 'c' not in root â†’ false âœ“
```

---

## 7. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Confusing `search` and `startsWith` | `search` checks `isEnd`; `startsWith` only checks node existence |
| Memory leaks (no destructor) | In interviews, mention but don't implement (time pressure) |
| Using array `[26]` for Unicode | Use `unordered_map` for arbitrary character sets |
| Not marking `isEnd` during insert | Must mark the final node as word-ending |

---

## 8. When NOT to Use

| Situation | Why Not | Use Instead |
|---|---|---|
| Exact word lookup only (no prefix queries) | Hash set is simpler and faster | `unordered_set<string>` |
| Very few words | Overhead of Trie not worthwhile | Hash set or vector |
| Need sorted iteration | Trie doesn't give sorted order naturally | Sorted array or BST |

---

## 9. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #208 â€” Implement Trie | Medium | Core implementation |
| 2 | LeetCode #211 â€” Design Add and Search Words | Medium | Trie with wildcard DFS |
| 3 | LeetCode #648 â€” Replace Words | Medium | Prefix replacement |
| 4 | LeetCode #212 â€” Word Search II | Hard | Trie + backtracking on grid |
| 5 | LeetCode #421 â€” Maximum XOR of Two Numbers | Medium | Bitwise Trie |

**Memory Trick:**

> ðŸ§  **"The Autocomplete Tree"** â€” A Trie works like your phone's autocomplete. Each letter you type follows a branch down the tree. All words sharing the same prefix share the same path. When you type "app", the Trie shows you the branch and all its descendants: "apple", "application", "appreciate".

---
---

*Previous chapter: [12 â€” Advanced Graphs](12_Advanced_Graphs.md)*
*Next chapter: [14 â€” Dynamic Programming](14_Dynamic_Programming.md)*
<!--
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DSA PATTERN HANDBOOK â€” DYNAMIC PROGRAMMING
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
  - Heading hierarchy: # (chapter) â†’ ## (pattern) â†’ ### (section)
  - 20 sections per pattern
  - Star rating: â˜… (filled), â˜† (empty), scale 1â€“5
  - Code style: C++17, 4-space indent, STL preferred
  - Tables: GitHub-flavored markdown pipe tables
  - Problem references: "LeetCode #ID â€” Name" format
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-->

# Chapter 14: Dynamic Programming

Dynamic Programming (DP) is the most feared pattern in interviews â€” and the most tested one. This chapter provides a systematic framework for tackling any DP problem.

**Core idea**: DP = Recursion + Memoization. If you can write a recursive solution with overlapping subproblems, you can convert it to DP.

---
---

# Pattern 42: DP Introduction & Framework

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DP INTRODUCTION & FRAMEWORK
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: ALL major companies
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
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
| Time | Usually O(nÂ²) or O(nÂ·W) | Usually O(n) or O(n log n) |
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

> ðŸ§  **"DP = Smart Brute Force"**
>
> Brute force tries all combinations. DP says: "I've already computed this combination before â€” let me reuse the answer." It's not a new algorithm â€” it's recursion that remembers.
>
> **The 4-Step Mantra**: "State â†’ Recurrence â†’ Base â†’ Order."

---
---

# Pattern 43: 1D DP (Linear DP)

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  1D DP (LINEAR)
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Goldman Sachs | Flipkart
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
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
> Try all subsets of houses where no two are adjacent â†’ O(2â¿).

**Step 2: Define State**
> `dp[i]` = maximum loot considering houses 0..i.

**Step 3: Recurrence**
> At house i, two choices:
> - **Rob house i**: gain `nums[i]`, can't use i-1 â†’ `dp[i-2] + nums[i]`
> - **Skip house i**: keep `dp[i-1]`
> `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`

**Step 4: Base Cases**
> `dp[0] = nums[0]`, `dp[1] = max(nums[0], nums[1])`

---

## 5. Reusable C++ Templates

```cpp
// Template A: 1D DP â€” Choose or Skip (House Robber)
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
// Template B: 1D DP â€” Kadane's Algorithm (Max Subarray Sum)
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
// Template C: 1D DP â€” Coin Change (Unbounded)
// Time: O(n Ã— amount), Space: O(amount)

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

â†’ prev2 = dp[i-2]
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

Answer: 12 (rob houses 0, 2, 4: 2+9+1=12) âœ“
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
| 1 | LeetCode #70 â€” Climbing Stairs | Easy | dp[i] = dp[i-1] + dp[i-2] |
| 2 | LeetCode #198 â€” House Robber | Medium | Choose or skip |
| 3 | LeetCode #53 â€” Maximum Subarray | Medium | Kadane's algorithm |
| 4 | LeetCode #322 â€” Coin Change | Medium | Unbounded DP |
| 5 | LeetCode #139 â€” Word Break | Medium | String DP with dictionary |

---
---

# Pattern 44: 2D DP (Grid / Two-Sequence)

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  2D DP (GRID / TWO-SEQUENCE)
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Goldman Sachs
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
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
// Template A: Grid DP â€” Minimum Path Sum
// Time: O(m Ã— n), Space: O(n) with optimization

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
// Template B: Two-Sequence DP â€” Longest Common Subsequence
// Time: O(m Ã— n), Space: O(m Ã— n) [O(n) with optimization]

int longestCommonSubsequence(string text1, string text2) {
    int m = text1.size(), n = text2.size();
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (text1[i-1] == text2[j-1]) {
                dp[i][j] = dp[i-1][j-1] + 1;     // match â†’ extend
            } else {
                dp[i][j] = max(dp[i-1][j], dp[i][j-1]);  // skip one
            }
        }
    }
    
    return dp[m][n];
}
```

```cpp
// Template C: Two-Sequence DP â€” Edit Distance
// Time: O(m Ã— n), Space: O(m Ã— n)

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

## 5. Dry Run â€” LCS

**LCS("abcde", "ace")**

```
       ""  a  c  e
  ""  [ 0  0  0  0 ]
   a  [ 0  1  1  1 ]    a==a â†’ dp[1][1] = dp[0][0]+1 = 1
   b  [ 0  1  1  1 ]    bâ‰ a,c,e â†’ max of neighbors
   c  [ 0  1  2  2 ]    c==c â†’ dp[3][2] = dp[2][1]+1 = 2
   d  [ 0  1  2  2 ]    dâ‰ a,c,e â†’ max of neighbors
   e  [ 0  1  2  3 ]    e==e â†’ dp[5][3] = dp[4][2]+1 = 3

Answer: LCS = 3 ("ace") âœ“
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
| 1 | LeetCode #62 â€” Unique Paths | Medium | Grid counting DP |
| 2 | LeetCode #64 â€” Minimum Path Sum | Medium | Grid optimization DP |
| 3 | LeetCode #1143 â€” Longest Common Subsequence | Medium | Classic two-sequence |
| 4 | LeetCode #72 â€” Edit Distance | Medium | Three-operation recurrence |
| 5 | LeetCode #10 â€” Regular Expression Matching | Hard | Complex two-sequence |

---
---

# Pattern 45: Knapsack Variants

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  KNAPSACK VARIANTS
  OA Frequency: â˜…â˜…â˜…â˜…â˜…
  Companies: Amazon | Google | Microsoft | Goldman Sachs | Flipkart
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Given items with costs and values, and a capacity constraint, maximize value (or count ways, or check feasibility)." The knapsack is the quintessential DP problem because the greedy approach fails â€” you must consider all combinations.

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
| "Coin change â€” minimum coins" | Unbounded Knapsack (optimization) |
| "Coin change â€” number of ways" | Unbounded Knapsack (counting) |
| "Target sum with +/-" | 0/1 Knapsack (transformed) |
| "Rod cutting" | Unbounded Knapsack |

---

## 4. Reusable C++ Templates

```cpp
// Template A: 0/1 Knapsack
// Time: O(n Ã— W), Space: O(W) with 1D optimization

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
// Time: O(n Ã— W), Space: O(W)

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
| Inner loop: **reverse** (target â†’ 0) | Inner loop: **forward** (0 â†’ target) |
| Ensures each item used once | Allows reusing items |
| Outer: items, Inner: capacity â†“ | Can vary, but forward allows reuse |

**Why reverse for 0/1?** Processing in reverse ensures `dp[w - weight[i]]` hasn't been updated with item `i` yet in this iteration. Forward would let item `i` be used multiple times.

---

## 6. Dry Run â€” 0/1 Knapsack

**Subset Sum**: Can `[3, 1, 5, 2]` form sum 6?

```
dp = [T, F, F, F, F, F, F]   (dp[0] = true)

num=3: w=6â†’3 (reverse)
  dp[6] = dp[6]||dp[3] = F||F = F
  dp[5] = dp[5]||dp[2] = F||F = F
  dp[4] = dp[4]||dp[1] = F||F = F
  dp[3] = dp[3]||dp[0] = F||T = T
dp = [T, F, F, T, F, F, F]

num=1: w=6â†’1
  dp[4] = dp[4]||dp[3] = F||T = T
  dp[1] = dp[1]||dp[0] = F||T = T
dp = [T, T, F, T, T, F, F]

num=5: w=6â†’5
  dp[6] = dp[6]||dp[1] = F||T = T  â† Found!
dp = [T, T, F, T, T, F, T]

dp[6] = true â†’ Yes, subset sum 6 exists (3+1+2 or 1+5) âœ“
```

---

## 7. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #416 â€” Partition Equal Subset Sum | Medium | 0/1 Knapsack: target = sum/2 |
| 2 | LeetCode #322 â€” Coin Change | Medium | Unbounded: min coins |
| 3 | LeetCode #518 â€” Coin Change II | Medium | Unbounded: count ways |
| 4 | LeetCode #494 â€” Target Sum | Medium | 0/1 Knapsack transform |
| 5 | LeetCode #474 â€” Ones and Zeroes | Medium | 2D 0/1 Knapsack |

**Memory Trick:**

> ðŸ§  **"The Suitcase Packer"** â€” 0/1 Knapsack: You're packing a suitcase. Each item can be taken once. Reverse iteration = "I haven't decided about this item yet." Unbounded: It's a store â€” buy as many copies as you want. Forward iteration = "I can buy another one."

---
---

# Pattern 46: Longest Increasing Subsequence (LIS)

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  LONGEST INCREASING SUBSEQUENCE
  OA Frequency: â˜…â˜…â˜…â˜…â˜†
  Companies: Google | Amazon | Microsoft | Meta | Goldman Sachs
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find the longest subsequence where elements are strictly increasing." This is a classic DP problem with an elegant O(n log n) optimization using binary search.

---

## 2. Two Approaches

| Approach | Time | Space | How |
|---|---|---|---|
| **DP** | O(nÂ²) | O(n) | `dp[i] = max(dp[j] + 1)` for all `j < i` where `nums[j] < nums[i]` |
| **Patience Sorting** | O(n log n) | O(n) | Maintain sorted "tails" array, binary search for insertion point |

---

## 3. Reusable C++ Templates

```cpp
// Template A: LIS â€” O(nÂ²) DP
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
// Template B: LIS â€” O(n log n) with Binary Search
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
  10 â†’ tails: [10]
   9 â†’ 9 < 10, replace â†’ tails: [9]
   2 â†’ 2 < 9, replace â†’ tails: [2]
   5 â†’ 5 > 2, append â†’ tails: [2, 5]
   3 â†’ 3 > 2, 3 < 5, replace â†’ tails: [2, 3]
   7 â†’ 7 > 3, append â†’ tails: [2, 3, 7]
 101 â†’ 101 > 7, append â†’ tails: [2, 3, 7, 101]
  18 â†’ 18 > 7, 18 < 101, replace â†’ tails: [2, 3, 7, 18]

Length = 4. (LIS: 2, 3, 7, 18 or 2, 5, 7, 101, etc.) âœ“

Note: tails is NOT the actual LIS â€” it's the smallest possible "ending" for each length.
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
| 1 | LeetCode #300 â€” Longest Increasing Subsequence | Medium | Classic LIS |
| 2 | LeetCode #673 â€” Number of Longest Increasing Subsequence | Medium | Count LIS |
| 3 | LeetCode #354 â€” Russian Doll Envelopes | Hard | 2D LIS |
| 4 | LeetCode #1048 â€” Longest String Chain | Medium | LIS with word matching |
| 5 | LeetCode #368 â€” Largest Divisible Subset | Medium | LIS variant |

**Memory Trick:**

> ðŸ§  **"Patience Solitaire"** â€” The O(n log n) LIS is exactly the patience sorting card game. Place each card on the leftmost pile where it fits (binary search). The number of piles = LIS length.

---
---

# Pattern 47: Digit DP* (Advanced)

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DIGIT DP (ADVANCED â€” OPTIONAL)
  OA Frequency: â˜…â˜…â˜†â˜†â˜†
  Companies: Google | Microsoft | Atlassian
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Count numbers in range [L, R] satisfying a digit-based property." Examples: count numbers with digit sum â‰¤ K, count numbers without repeated digits, count numbers divisible by X.

**Why Digit DP?**

Brute force checks every number in [1, R] â€” up to 10Â¹â¸. Digit DP processes one digit at a time (18 digits max), considering: "What can I place at the current position while staying within bounds?"

---

## 2. Pattern Identification Checklist

- [ ] Does the problem involve counting numbers in a **range [L, R]**?
- [ ] Is the constraint on individual **digits** (sum, uniqueness, pattern)?
- [ ] Is R very large (up to 10Â¹â¸)?

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
| 1 | LeetCode #233 â€” Number of Digit One | Hard | Count 1s in range |
| 2 | LeetCode #357 â€” Count Numbers with Unique Digits | Medium | Digit DP or math |
| 3 | LeetCode #902 â€” Numbers At Most N Given Digit Set | Hard | Digit DP |

---
---

# Pattern 48: State Compression DP (Bitmask DP)*

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  STATE COMPRESSION DP (BITMASK DP) â€” ADVANCED
  OA Frequency: â˜…â˜…â˜†â˜†â˜†
  Companies: Google | Amazon | Microsoft
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Optimize over **subsets** when n is small (â‰¤ 20)." The state is a bitmask where each bit represents whether an element is included. This compresses the "which elements have been used" information into a single integer.

---

## 2. Pattern Identification Checklist

- [ ] Is n â‰¤ 20?
- [ ] Does the problem involve **visiting all items** in an optimal order (like TSP)?
- [ ] Is the state "which items have been used" important?

---

## 3. Reusable C++ Template

```cpp
// Template: Bitmask DP â€” Traveling Salesman variant
// Time: O(2â¿ Ã— nÂ²), Space: O(2â¿ Ã— n)

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
| 1 | LeetCode #698 â€” Partition to K Equal Sum Subsets | Medium | Bitmask DP |
| 2 | LeetCode #1125 â€” Smallest Sufficient Team | Hard | Set cover with bitmask |
| 3 | LeetCode #943 â€” Find Shortest Superstring | Hard | TSP variant |

**Memory Trick:**

> ðŸ§  **"The Binary Checklist"** â€” Each bit is a checkbox. Bit 0 = item 0 visited? Bit 1 = item 1 visited? The entire checklist fits in one integer. With n â‰¤ 20, there are only 2Â²â° â‰ˆ 10â¶ possible checklists â€” small enough to DP over.

---
---

*Previous chapter: [13 â€” Trees](13_Trees.md)*
*Next chapter: [15 â€” String Algorithms](15_String_Algorithms.md)*
<!--
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DSA PATTERN HANDBOOK â€” STRING ALGORITHMS
  Chapter 15
  
  Patterns in this chapter:
    Pattern 49: KMP (Knuth-Morris-Pratt)
    Pattern 50: Z Algorithm
    Pattern 51: Rabin-Karp (Rolling Hash)
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-->

# Chapter 15: String Algorithms

These are specialized string-matching algorithms. While many string problems are solved with sliding window, hashing, or DP, some require dedicated pattern-matching algorithms.

---
---

# Pattern 49: KMP (Knuth-Morris-Pratt)

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  KMP ALGORITHM
  OA Frequency: â˜…â˜…â˜…â˜†â˜†
  Companies: Google | Amazon | Microsoft | Goldman Sachs
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find all occurrences of pattern P in text T." Brute force is O(n Ã— m). KMP achieves O(n + m) by **never re-comparing characters** â€” when a mismatch occurs, it uses pre-computed information to skip ahead.

**The key insight**: Build a "failure function" (LPS array â€” Longest Proper Prefix that is also a Suffix) for the pattern. When a mismatch happens at position j of the pattern, instead of restarting from 0, jump to `lps[j-1]` â€” the longest prefix of the pattern that matches the current suffix.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem involve **substring matching**?
- [ ] Does it ask for **all occurrences** of a pattern in a text?
- [ ] Is n or m large enough that O(n Ã— m) is too slow?
- [ ] Does the problem involve finding **repeated patterns** or **period of a string**?

---

## 3. Reusable C++ Template

```cpp
// Template: KMP â€” String Pattern Matching
// Time: O(n + m), Space: O(m) for LPS array

#include <string>
#include <vector>
using namespace std;

// Build LPS (Longest Proper Prefix which is also Suffix) array
vector<int> buildLPS(const string& pattern) {
    int m = pattern.size();
    vector<int> lps(m, 0);
    int len = 0;  // length of previous longest prefix suffix
    int i = 1;

    while (i < m) {
        if (pattern[i] == pattern[len]) {
            len++;
            lps[i] = len;
            i++;
        } else {
            if (len != 0) {
                len = lps[len - 1];  // don't increment i â€” try shorter prefix
            } else {
                lps[i] = 0;
                i++;
            }
        }
    }

    return lps;
}

// KMP search: find all occurrences of pattern in text
vector<int> kmpSearch(const string& text, const string& pattern) {
    int n = text.size(), m = pattern.size();
    vector<int> lps = buildLPS(pattern);
    vector<int> matches;

    int i = 0;  // index in text
    int j = 0;  // index in pattern

    while (i < n) {
        if (text[i] == pattern[j]) {
            i++;
            j++;
        }

        if (j == m) {
            matches.push_back(i - m);  // match found at index i-m
            j = lps[j - 1];           // continue searching
        } else if (i < n && text[i] != pattern[j]) {
            if (j != 0) {
                j = lps[j - 1];       // skip using LPS
            } else {
                i++;
            }
        }
    }

    return matches;
}
```

---

## 4. Dry Run â€” LPS Array

```
Pattern: "ABCABD"

i=1: Bâ‰ A(len=0) â†’ lps[1]=0
i=2: Câ‰ A(len=0) â†’ lps[2]=0
i=3: A==A(len=0) â†’ len=1, lps[3]=1
i=4: B==B(len=1) â†’ len=2, lps[4]=2
i=5: Dâ‰ C(len=2) â†’ len=lps[1]=0, Dâ‰ A(len=0) â†’ lps[5]=0

LPS: [0, 0, 0, 1, 2, 0]

Meaning: if mismatch at position 5 ('D'), jump back to position lps[4]=2 ('C')
instead of restarting from 0. We know "AB" already matched.
```

---

## 5. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #28 â€” Find Index of First Occurrence | Easy | Basic KMP |
| 2 | LeetCode #459 â€” Repeated Substring Pattern | Easy | LPS trick: if `n % (n - lps[n-1]) == 0` |
| 3 | LeetCode #214 â€” Shortest Palindrome | Hard | KMP on reversed string |
| 4 | LeetCode #1392 â€” Longest Happy Prefix | Hard | Direct LPS application |

**Memory Trick:**

> ðŸ§  **"The Bookmark"** â€” KMP's LPS array is like a bookmark. When you're reading and hit a wrong word, instead of going back to the start of the sentence, you jump back to the bookmark (last matching prefix). You never re-read text you've already processed.

---
---

# Pattern 50: Z Algorithm

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  Z ALGORITHM
  OA Frequency: â˜…â˜…â˜†â˜†â˜†
  Companies: Google | Microsoft
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Given a string S, compute Z[i] = the length of the longest substring starting at position i that matches a prefix of S. This is useful for string matching, repeated pattern detection, and prefix-based queries.

**For pattern matching**: Concatenate `pattern + "$" + text`. If `Z[i] == len(pattern)`, there's a match at position `i - len(pattern) - 1`.

---

## 2. Reusable C++ Template

```cpp
// Template: Z Algorithm
// Time: O(n), Space: O(n)

#include <string>
#include <vector>
using namespace std;

vector<int> zFunction(const string& s) {
    int n = s.size();
    vector<int> z(n, 0);
    int l = 0, r = 0;

    for (int i = 1; i < n; i++) {
        if (i < r) {
            z[i] = min(r - i, z[i - l]);
        }
        while (i + z[i] < n && s[z[i]] == s[i + z[i]]) {
            z[i]++;
        }
        if (i + z[i] > r) {
            l = i;
            r = i + z[i];
        }
    }

    return z;
}

// Pattern matching using Z
vector<int> zSearch(const string& text, const string& pattern) {
    string concat = pattern + "$" + text;
    vector<int> z = zFunction(concat);
    int m = pattern.size();
    vector<int> matches;

    for (int i = m + 1; i < (int)concat.size(); i++) {
        if (z[i] == m) {
            matches.push_back(i - m - 1);
        }
    }

    return matches;
}
```

---

## 3. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #28 â€” Find Index of First Occurrence | Easy | Z algorithm alternative |
| 2 | LeetCode #796 â€” Rotate String | Easy | Concatenate and match |

---
---

# Pattern 51: Rabin-Karp (Rolling Hash)*

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  RABIN-KARP (ROLLING HASH) â€” ADVANCED
  OA Frequency: â˜…â˜…â˜†â˜†â˜†
  Companies: Google | Amazon | Microsoft
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Pattern matching using **hashing**. Instead of comparing characters, compare hash values. If hashes match, verify with actual comparison (to handle collisions). The "rolling" part: update the hash in O(1) as the window slides.

---

## 2. When to Use Over KMP

| Scenario | KMP | Rabin-Karp |
|---|---|---|
| Single pattern matching | âœ… Better | âœ… Works |
| Multiple pattern matching | Run KMP per pattern | âœ… Better (hash all patterns) |
| Longest duplicate substring | âŒ Can't directly | âœ… Binary search + rolling hash |

---

## 3. Reusable C++ Template

```cpp
// Template: Rabin-Karp Rolling Hash
// Time: O(n + m) average, O(n Ã— m) worst (hash collisions)

#include <string>
#include <vector>
using namespace std;

vector<int> rabinKarp(const string& text, const string& pattern) {
    int n = text.size(), m = pattern.size();
    if (m > n) return {};

    const long long MOD = 1e9 + 7;
    const long long BASE = 31;
    vector<int> matches;

    // Compute pattern hash and initial window hash
    long long patHash = 0, winHash = 0, power = 1;
    for (int i = 0; i < m; i++) {
        patHash = (patHash * BASE + (pattern[i] - 'a' + 1)) % MOD;
        winHash = (winHash * BASE + (text[i] - 'a' + 1)) % MOD;
        if (i > 0) power = (power * BASE) % MOD;
    }

    for (int i = 0; i <= n - m; i++) {
        if (winHash == patHash) {
            // Verify (handle collisions)
            if (text.substr(i, m) == pattern) {
                matches.push_back(i);
            }
        }
        // Slide window: remove text[i], add text[i+m]
        if (i + m < n) {
            winHash = (winHash - (text[i] - 'a' + 1) * power % MOD + MOD) % MOD;
            winHash = (winHash * BASE + (text[i + m] - 'a' + 1)) % MOD;
        }
    }

    return matches;
}
```

---

## 4. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #187 â€” Repeated DNA Sequences | Medium | Rolling hash for fixed-length substrings |
| 2 | LeetCode #1044 â€” Longest Duplicate Substring | Hard | Binary search + rolling hash |
| 3 | LeetCode #718 â€” Maximum Length of Repeated Subarray | Medium | Rolling hash or DP |

---
---

*Previous chapter: [14 â€” Dynamic Programming](14_Dynamic_Programming.md)*
*Next chapter: [16 â€” Advanced Data Structures](16_Advanced_Data_Structures.md)*
<!--
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DSA PATTERN HANDBOOK â€” ADVANCED DATA STRUCTURES
  Chapter 16
  
  Patterns in this chapter:
    Pattern 52: Segment Tree*
    Pattern 53: Fenwick Tree (Binary Indexed Tree)*
    Pattern 54: Binary Lifting*
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-->

# Chapter 16: Advanced Data Structures

These data structures appear less frequently in standard placement OAs but are crucial for competitive programming and top-tier company interviews (Google, Meta). They handle **range queries** and **tree ancestor queries** efficiently.

> **Note**: Patterns marked with * are advanced. If you're short on time, master Chapters 2â€“14 first.

---
---

# Pattern 52: Segment Tree*

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  SEGMENT TREE (ADVANCED)
  OA Frequency: â˜…â˜…â˜†â˜†â˜†
  Companies: Google | Meta | Uber
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Answer **range queries** (sum, min, max over a range) and **point/range updates** efficiently on a mutable array."

| Operation | Array | Prefix Sum | Segment Tree |
|---|---|---|---|
| Range query | O(n) | O(1) | **O(log n)** |
| Point update | O(1) | O(n) rebuild | **O(log n)** |
| Range update | O(n) | O(n) | **O(log n) with lazy** |

Segment Tree handles both queries and updates in O(log n) â€” the best of both worlds.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem involve **range queries** (sum, min, max)?
- [ ] Does the array get **updated** between queries?
- [ ] Are there **many queries** (up to 10âµ)?
- [ ] Do prefix sums fail because of updates?

---

## 3. Reusable C++ Template

```cpp
// Template: Segment Tree â€” Range Sum Query + Point Update
// Time: O(log n) per query/update, O(n) build
// Space: O(4n)

#include <vector>
using namespace std;

class SegmentTree {
    vector<long long> tree;
    int n;

    void build(vector<int>& arr, int node, int start, int end) {
        if (start == end) {
            tree[node] = arr[start];
        } else {
            int mid = (start + end) / 2;
            build(arr, 2 * node, start, mid);
            build(arr, 2 * node + 1, mid + 1, end);
            tree[node] = tree[2 * node] + tree[2 * node + 1];  // MODIFY: operation
        }
    }

    void update(int node, int start, int end, int idx, int val) {
        if (start == end) {
            tree[node] = val;
        } else {
            int mid = (start + end) / 2;
            if (idx <= mid) update(2 * node, start, mid, idx, val);
            else update(2 * node + 1, mid + 1, end, idx, val);
            tree[node] = tree[2 * node] + tree[2 * node + 1];
        }
    }

    long long query(int node, int start, int end, int l, int r) {
        if (r < start || end < l) return 0;      // MODIFY: identity for operation
        if (l <= start && end <= r) return tree[node];
        int mid = (start + end) / 2;
        return query(2 * node, start, mid, l, r) +
               query(2 * node + 1, mid + 1, end, l, r);
    }

public:
    SegmentTree(vector<int>& arr) : n(arr.size()), tree(4 * arr.size(), 0) {
        build(arr, 1, 0, n - 1);
    }

    void update(int idx, int val) { update(1, 0, n - 1, idx, val); }
    long long query(int l, int r) { return query(1, 0, n - 1, l, r); }
};
```

---

## 4. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #307 â€” Range Sum Query - Mutable | Medium | Classic segment tree |
| 2 | LeetCode #315 â€” Count of Smaller Numbers After Self | Hard | Merge sort or segment tree |
| 3 | LeetCode #2407 â€” Longest Increasing Subsequence II | Hard | Segment tree + DP |

---
---

# Pattern 53: Fenwick Tree (Binary Indexed Tree)*

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  FENWICK TREE (BIT) â€” ADVANCED
  OA Frequency: â˜…â˜…â˜†â˜†â˜†
  Companies: Google | Meta | Amazon
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

A Fenwick Tree (BIT) is a simpler, more memory-efficient alternative to Segment Tree for **prefix queries + point updates**. It uses clever bit manipulation to achieve O(log n) for both operations.

**Key limitation**: Only works for **associative, commutative** operations with an inverse (like addition). Can't directly do range min/max (use Segment Tree for those).

---

## 2. Reusable C++ Template

```cpp
// Template: Fenwick Tree (BIT) â€” Prefix Sum + Point Update
// Time: O(log n) per operation, Space: O(n)

#include <vector>
using namespace std;

class FenwickTree {
    vector<long long> tree;
    int n;
public:
    FenwickTree(int n) : n(n), tree(n + 1, 0) {}

    // Add val to index i (1-indexed)
    void update(int i, long long val) {
        for (; i <= n; i += i & (-i))
            tree[i] += val;
    }

    // Prefix sum [1..i]
    long long query(int i) {
        long long sum = 0;
        for (; i > 0; i -= i & (-i))
            sum += tree[i];
        return sum;
    }

    // Range sum [l..r]
    long long query(int l, int r) {
        return query(r) - query(l - 1);
    }
};
```

---

## 3. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #307 â€” Range Sum Query - Mutable | Medium | BIT alternative to segment tree |
| 2 | LeetCode #493 â€” Reverse Pairs | Hard | BIT for counting |
| 3 | LeetCode #315 â€” Count of Smaller Numbers After Self | Hard | BIT with coordinate compression |

**When to use Fenwick vs Segment Tree:**

| Feature | Fenwick Tree | Segment Tree |
|---|---|---|
| Code complexity | Simple (10 lines) | Complex (40+ lines) |
| Operations | Sum, count (with inverse) | Sum, min, max, GCD (any) |
| Lazy propagation | Not supported | Supported |
| Memory | n+1 | 4n |

---
---

# Pattern 54: Binary Lifting*

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  BINARY LIFTING â€” ADVANCED
  OA Frequency: â˜…â˜…â˜†â˜†â˜†
  Companies: Google | Meta
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Answer **kth ancestor** queries and **LCA queries** on a tree in O(log n) per query." Without binary lifting, finding the kth ancestor requires O(k) time. Binary lifting pre-computes 2^j-th ancestors, enabling jumps of any size in O(log n).

---

## 2. Reusable C++ Template

```cpp
// Template: Binary Lifting â€” Kth Ancestor / LCA in O(log n)
// Preprocessing: O(n log n), Query: O(log n)

#include <vector>
using namespace std;

class BinaryLifting {
    int LOG;
    vector<vector<int>> up;  // up[v][j] = 2^j-th ancestor of v
    vector<int> depth;

public:
    BinaryLifting(int n, vector<vector<int>>& adj, int root = 0) {
        LOG = 1;
        while ((1 << LOG) <= n) LOG++;
        up.assign(n, vector<int>(LOG, -1));
        depth.assign(n, 0);

        // BFS to compute depth and parent
        vector<bool> visited(n, false);
        queue<int> q;
        q.push(root);
        visited[root] = true;

        while (!q.empty()) {
            int v = q.front(); q.pop();
            for (int u : adj[v]) {
                if (!visited[u]) {
                    visited[u] = true;
                    depth[u] = depth[v] + 1;
                    up[u][0] = v;  // direct parent
                    q.push(u);
                }
            }
        }

        // Fill binary lifting table
        for (int j = 1; j < LOG; j++) {
            for (int v = 0; v < n; v++) {
                if (up[v][j-1] != -1)
                    up[v][j] = up[up[v][j-1]][j-1];
            }
        }
    }

    int kthAncestor(int v, int k) {
        for (int j = 0; j < LOG && v != -1; j++) {
            if (k & (1 << j)) v = up[v][j];
        }
        return v;
    }

    int lca(int u, int v) {
        if (depth[u] < depth[v]) swap(u, v);
        u = kthAncestor(u, depth[u] - depth[v]);  // same depth
        if (u == v) return u;
        for (int j = LOG - 1; j >= 0; j--) {
            if (up[u][j] != up[v][j]) {
                u = up[u][j];
                v = up[v][j];
            }
        }
        return up[u][0];
    }
};
```

---

## 3. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #1483 â€” Kth Ancestor of a Tree Node | Hard | Direct binary lifting |
| 2 | LeetCode #2846 â€” Minimum Edge Weight Equilibrium | Hard | LCA with binary lifting |

---
---

*Previous chapter: [15 â€” String Algorithms](15_String_Algorithms.md)*
*Next chapter: [17 â€” Miscellaneous](17_Miscellaneous.md)*
<!--
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DSA PATTERN HANDBOOK â€” MISCELLANEOUS
  Chapter 17
  
  Patterns in this chapter:
    Pattern 55: Line Sweep (2D)
    Pattern 56: Coordinate Compression
    Pattern 57: Mathematical Observation
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-->

# Chapter 17: Miscellaneous

This chapter covers patterns that don't fit neatly into other categories but appear frequently enough in OAs to warrant coverage.

---
---

# Pattern 55: Line Sweep (2D)

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  LINE SWEEP (2D)
  OA Frequency: â˜…â˜…â˜†â˜†â˜†
  Companies: Google | Amazon | Microsoft
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

While we covered the basic sweep line in Chapter 7 (intervals), the 2D line sweep extends this to rectangles and 2D regions. A vertical line sweeps left-to-right, maintaining an active set of vertical segments using a balanced BST or segment tree.

**Classic problem**: The Skyline Problem (LeetCode #218).

---

## 2. When to Use

- Rectangle union area
- Skyline computation
- 2D event processing
- Geometric problems with rectangles

---

## 3. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #218 â€” The Skyline Problem | Hard | Classic 2D sweep |
| 2 | LeetCode #850 â€” Rectangle Area II | Hard | Sweep + compressed coordinates |
| 3 | LeetCode #391 â€” Perfect Rectangle | Hard | Sweep line validation |

---
---

# Pattern 56: Coordinate Compression

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  COORDINATE COMPRESSION
  OA Frequency: â˜…â˜…â˜†â˜†â˜†
  Companies: Google | Amazon
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Work with coordinates/values up to 10â¹, but only use n distinct values (n â‰¤ 10âµ)." Coordinate compression maps sparse values to a dense range [0, n-1], enabling array-based techniques (BIT, counting sort) on large value ranges.

---

## 2. When to Use

- Values up to 10â¹ but only n â‰¤ 10âµ distinct values
- Need BIT/Segment Tree indexed by values
- Counting inversions, rank queries

---

## 3. Reusable C++ Template

```cpp
// Template: Coordinate Compression
// Map large values to [0, n-1] preserving relative order

#include <vector>
#include <algorithm>
using namespace std;

vector<int> compress(vector<int>& arr) {
    vector<int> sorted_unique = arr;
    sort(sorted_unique.begin(), sorted_unique.end());
    sorted_unique.erase(unique(sorted_unique.begin(), sorted_unique.end()), 
                        sorted_unique.end());

    vector<int> compressed(arr.size());
    for (int i = 0; i < (int)arr.size(); i++) {
        compressed[i] = lower_bound(sorted_unique.begin(), sorted_unique.end(), 
                                     arr[i]) - sorted_unique.begin();
    }

    return compressed;  // values now in [0, distinct_count - 1]
}
```

---

## 4. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #315 â€” Count of Smaller Numbers After Self | Hard | BIT + coordinate compression |
| 2 | LeetCode #493 â€” Reverse Pairs | Hard | Merge sort or BIT + compression |

---
---

# Pattern 57: Mathematical Observation

```
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  MATHEMATICAL OBSERVATION
  OA Frequency: â˜…â˜…â˜…â˜†â˜†
  Companies: All (especially Goldman Sachs, Google, Amazon)
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Some problems have no standard algorithm â€” they require spotting a mathematical pattern, formula, or property. These problems test mathematical thinking and the ability to derive solutions from examples.

---

## 2. Common Mathematical Tricks

| Trick | When to Use | Example |
|---|---|---|
| **Modular arithmetic** | Large number computations | `(a * b) % MOD` |
| **GCD / LCM** | Divisibility, fractions | `__gcd(a, b)` in C++ |
| **Combinatorics** | Counting arrangements | nCr, nPr |
| **Pigeonhole principle** | Existence proofs | If n+1 items in n boxes, some box has 2+ |
| **Parity** | Even/odd arguments | Nim game, tiling |
| **Sum formulas** | Arithmetic/geometric series | Sum 1..n = n(n+1)/2 |
| **Number theory** | Primes, factorization | Sieve of Eratosthenes |

---

## 3. Essential Formulas

```cpp
// Modular exponentiation: compute (base^exp) % mod
long long modPow(long long base, long long exp, long long mod) {
    long long result = 1;
    base %= mod;
    while (exp > 0) {
        if (exp & 1) result = result * base % mod;
        base = base * base % mod;
        exp >>= 1;
    }
    return result;
}

// Sieve of Eratosthenes: all primes up to n
vector<bool> sieve(int n) {
    vector<bool> isPrime(n + 1, true);
    isPrime[0] = isPrime[1] = false;
    for (int i = 2; i * i <= n; i++) {
        if (isPrime[i]) {
            for (int j = i * i; j <= n; j += i)
                isPrime[j] = false;
        }
    }
    return isPrime;
}

// nCr with modular inverse (for large n, mod = prime)
long long factorial[200001];
long long modInverse(long long a, long long mod) { return modPow(a, mod - 2, mod); }

void precomputeFactorials(int n, long long mod) {
    factorial[0] = 1;
    for (int i = 1; i <= n; i++)
        factorial[i] = factorial[i-1] * i % mod;
}

long long nCr(int n, int r, long long mod) {
    if (r > n) return 0;
    return factorial[n] % mod * modInverse(factorial[r], mod) % mod 
           * modInverse(factorial[n-r], mod) % mod;
}
```

---

## 4. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #204 â€” Count Primes | Medium | Sieve of Eratosthenes |
| 2 | LeetCode #50 â€” Pow(x, n) | Medium | Fast exponentiation |
| 3 | LeetCode #172 â€” Factorial Trailing Zeroes | Medium | Math: count factors of 5 |
| 4 | LeetCode #400 â€” Nth Digit | Medium | Mathematical pattern |
| 5 | LeetCode #829 â€” Consecutive Numbers Sum | Hard | Number theory |

---
---

*Previous chapter: [16 â€” Advanced Data Structures](16_Advanced_Data_Structures.md)*
*Next chapter: [18 â€” Cheat Sheets](18_Cheat_Sheets.md)*
<!--
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DSA PATTERN HANDBOOK â€” CHEAT SHEETS
  Chapter 18
  
  Sections:
    - Complexity Cheat Sheet
    - C++ STL Quick Reference
    - Interview Tips & Strategy
    - Common OA Mistakes
    - 30-Minute Revision Guide
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-->

# Chapter 18: Cheat Sheets

---

## 18.1 â€” Complexity Cheat Sheet

### Time Complexity Ranking (Fastest to Slowest)

| Complexity | Name | Max n (1s) | Example Algorithm |
|---|---|---|---|
| O(1) | Constant | âˆž | Hash table lookup |
| O(log n) | Logarithmic | 10Â¹â¸ | Binary search |
| O(âˆšn) | Square root | 10Â¹â´ | Prime checking |
| O(n) | Linear | 10â¸ | Array traversal |
| O(n log n) | Linearithmic | 10â¶ | Merge sort, Dijkstra |
| O(nâˆšn) | â€” | 10âµ | Mo's algorithm |
| O(nÂ²) | Quadratic | 5000 | Nested loops, 2D DP |
| O(nÂ² log n) | â€” | 2000 | Sort within nested loops |
| O(nÂ³) | Cubic | 400 | Floyd-Warshall |
| O(2â¿) | Exponential | 20-25 | Subsets, bitmask DP |
| O(n!) | Factorial | 10-12 | Permutations |

### Constraint â†’ Expected Complexity

| Constraint | Expected Time | Typical Pattern |
|---|---|---|
| n â‰¤ 10 | O(n!) | Backtracking, brute force |
| n â‰¤ 20 | O(2â¿) | Bitmask DP, backtracking |
| n â‰¤ 500 | O(nÂ³) | Floyd-Warshall, 3 nested loops |
| n â‰¤ 5000 | O(nÂ²) | 2D DP, LIS O(nÂ²) |
| n â‰¤ 10âµ | O(n log n) | Sorting, Dijkstra, Segment Tree |
| n â‰¤ 10â¶ | O(n) | Linear scan, sliding window |
| n â‰¤ 10â¸ | O(n) careful | Simple loop, prefix sum |
| n â‰¤ 10Â¹â¸ | O(log n) | Binary search, math |

### Space Complexity Reference

| Structure | Space | Notes |
|---|---|---|
| Adjacency list | O(V + E) | Standard for graphs |
| Adjacency matrix | O(VÂ²) | Dense graphs only |
| 2D DP table | O(m Ã— n) | Often optimizable to O(n) |
| Segment tree | O(4n) | Array-based |
| Trie | O(Î£ word_lengths) | Shared prefixes save space |

---

## 18.2 â€” C++ STL Quick Reference

### Containers

```cpp
// === SEQUENCE CONTAINERS ===
vector<int> v;                    // dynamic array
v.push_back(x);                  // O(1) amortized
v.pop_back();                    // O(1)
v[i];                            // O(1) access
v.size();                        // O(1)
sort(v.begin(), v.end());        // O(n log n)

deque<int> dq;                   // double-ended queue
dq.push_front(x);               // O(1)
dq.push_back(x);                // O(1)
dq.pop_front();                  // O(1)

// === ASSOCIATIVE CONTAINERS (SORTED) ===
set<int> s;                      // sorted unique elements
s.insert(x);                    // O(log n)
s.erase(x);                     // O(log n)
s.count(x);                     // O(log n): 0 or 1
*s.begin();                      // smallest element
*s.rbegin();                     // largest element
s.lower_bound(x);               // iterator to first element >= x
s.upper_bound(x);               // iterator to first element > x

multiset<int> ms;                // sorted with duplicates
map<int, int> m;                 // sorted key-value pairs
m[key] = val;                    // O(log n)

// === UNORDERED CONTAINERS (HASH-BASED) ===
unordered_set<int> us;           // hash set
us.insert(x);                   // O(1) average
us.count(x);                    // O(1) average

unordered_map<int, int> um;      // hash map
um[key] = val;                   // O(1) average

// === ADAPTORS ===
stack<int> stk;                  // LIFO
stk.push(x); stk.top(); stk.pop();

queue<int> q;                    // FIFO
q.push(x); q.front(); q.pop();

priority_queue<int> maxPQ;       // MAX-heap
maxPQ.push(x); maxPQ.top(); maxPQ.pop();

priority_queue<int, vector<int>, greater<int>> minPQ;  // MIN-heap
```

### Algorithms

```cpp
#include <algorithm>
#include <numeric>

sort(v.begin(), v.end());                    // ascending
sort(v.begin(), v.end(), greater<int>());    // descending
sort(v.begin(), v.end(), [](int a, int b){ return a < b; });  // custom

reverse(v.begin(), v.end());
unique(v.begin(), v.end());           // removes consecutive dups (sort first)
next_permutation(v.begin(), v.end()); // next lexicographic permutation

lower_bound(v.begin(), v.end(), x);  // iterator to first >= x
upper_bound(v.begin(), v.end(), x);  // iterator to first > x
binary_search(v.begin(), v.end(), x); // true/false

*min_element(v.begin(), v.end());
*max_element(v.begin(), v.end());
accumulate(v.begin(), v.end(), 0);    // sum (include <numeric>)
accumulate(v.begin(), v.end(), 0LL);  // sum with long long

__gcd(a, b);                         // GCC built-in GCD
__builtin_popcount(x);               // count set bits (int)
__builtin_popcountll(x);             // count set bits (long long)
```

### Useful Patterns

```cpp
// Read until EOF
while (cin >> x) { ... }

// Initialize 2D vector
vector<vector<int>> grid(m, vector<int>(n, 0));

// String to number
int x = stoi("123");
long long y = stoll("123456789");

// Number to string
string s = to_string(42);

// Split by delimiter (no built-in, use stringstream)
#include <sstream>
stringstream ss("hello world");
string token;
while (ss >> token) { ... }

// Infinity
int INF = 1e9;           // for int
long long LINF = 1e18;   // for long long
```

---

## 18.3 â€” Interview Tips & Strategy

### Before the OA

1. **Read ALL problems first** â€” solve the easiest one first for confidence.
2. **Estimate time per problem** â€” for a 90-min OA with 3 problems: ~30 min each.
3. **Have your templates ready** â€” BFS, DFS, Union Find, Segment Tree, etc.

### During Problem Solving

```
The 5-Minute Framework:
1. READ carefully (2 min) â€” don't miss constraints!
2. IDENTIFY the pattern (1 min) â€” use the flowchart from Chapter 1
3. THINK about edge cases (1 min)
4. CODE the solution (1 min planning)
5. TEST with examples before submitting
```

### Pattern Recognition Decision Tree (Quick)

```
Is the input sorted?
  â†’ Binary Search

Is it asking for shortest path?
  â†’ Unweighted: BFS | Weighted: Dijkstra

Is it a string problem?
  â†’ Sliding Window, DP, or Trie

Does it say "all permutations/combinations"?
  â†’ Backtracking

Does it say "minimum/maximum of something"?
  â†’ DP, Greedy, or Binary Search on Answer

Is it about intervals/scheduling?
  â†’ Sort + Greedy or Sweep Line

Is the constraint n â‰¤ 20?
  â†’ Bitmask DP or Backtracking

Is the constraint very large (10â¹+)?
  â†’ Binary Search, Math, or O(log n) approach
```

### Communication Tips (Live Interviews)

1. **Think aloud** â€” explain your reasoning as you work.
2. **Start with brute force** â€” "The brute force is O(nÂ²)..." then optimize.
3. **Confirm understanding** â€” restate the problem in your own words.
4. **State complexity** â€” always mention time and space complexity.
5. **Handle edge cases explicitly** â€” empty input, single element, all same values.

---

## 18.4 â€” Common OA Mistakes

| # | Mistake | Fix |
|---|---|---|
| 1 | **Integer overflow** | Use `long long` for sums, products, and intermediate calculations |
| 2 | **Off-by-one errors** | Trace through with n=1, n=2 manually |
| 3 | **Not reading constraints** | The constraint IS the hint â€” it tells you the expected complexity |
| 4 | **Submitting without testing** | Test with the given examples AND edge cases |
| 5 | **TLE on brute force** | Check if n â‰¤ 10âµ â€” you probably need O(n log n) or better |
| 6 | **Modifying input when you shouldn't** | Use a copy or separate visited array |
| 7 | **Wrong comparison in sort** | `a < b` = ascending, `a > b` = descending, NOT `a <= b` |
| 8 | **Infinite loop in binary search** | Check: `lo <= hi` vs `lo < hi`, and `mid` calculation |
| 9 | **Not handling "not found" / -1 cases** | Always consider: what if the answer doesn't exist? |
| 10 | **Ignoring the "modulo 10â¹+7" requirement** | Apply mod at every addition/multiplication step |

### The Modular Arithmetic Trap

```cpp
// WRONG: overflow before mod
int result = (a * b) % MOD;  // a*b can overflow int!

// CORRECT: cast to long long
long long result = (1LL * a * b) % MOD;

// CORRECT: apply mod at each step
long long result = ((a % MOD) * (b % MOD)) % MOD;

// For subtraction (can go negative):
long long result = ((a - b) % MOD + MOD) % MOD;
```

---

## 18.5 â€” 30-Minute Revision Guide

> **Use this the night before or morning of your OA.**

### Core Patterns (5 minutes)

| # | Pattern | Key Idea | Template |
|---|---|---|---|
| 1 | **Sliding Window** | Expand right, shrink left | `while (invalid) shrink left` |
| 2 | **Two Pointers** | Start from both ends | `while (lo < hi)` |
| 3 | **Binary Search** | Halve the search space | `while (lo < hi) mid = lo+(hi-lo)/2` |
| 4 | **BS on Answer** | Guess + verify | `canAchieve(mid) â†’ adjust range` |
| 5 | **Greedy** | Locally best = globally best | Sort + iterate |
| 6 | **Monotonic Stack** | Next greater/smaller | Pop while top < current |
| 7 | **BFS** | Shortest path (unweighted) | Queue + visited |
| 8 | **DFS** | Connected components, paths | Recursion + visited |
| 9 | **Topological Sort** | Dependency ordering | In-degree + queue |
| 10 | **DP** | State â†’ Recurrence â†’ Base â†’ Order | `dp[i] = f(dp[i-1], ...)` |

### Quick Template Recall (5 minutes)

```
Binary Search:       lo, hi, while lo < hi, mid = lo+(hi-lo)/2
BFS on Grid:         queue + direction arrays + visited
DFS on Grid:         recursion + bounds check + mark visited
Union Find:          find(x) with path compression, unite with rank
Dijkstra:            min-heap of (dist, node), skip if d > dist[u]
Kahn's Topo Sort:    in-degree array, queue of in-degree 0 nodes
Knapsack 0/1:        for w = W to weight[i] (REVERSE)
Knapsack Unbounded:  for w = coin to amount (FORWARD)
LIS O(n log n):      tails array + lower_bound
Tree DFS:            if (!root) return base; left = solve(left); right = solve(right); combine
```

### Edge Cases Checklist (3 minutes)

```
âœ“ Empty input (n = 0)
âœ“ Single element (n = 1)
âœ“ All same elements
âœ“ Already sorted / reverse sorted
âœ“ Negative numbers
âœ“ Maximum values (INT_MAX, 10â¹)
âœ“ Overflow potential (use long long)
âœ“ Disconnected graph
âœ“ Tree with only left or only right children
âœ“ String with special characters or empty string
```

### Complexity Quick Check (2 minutes)

```
n â‰¤ 10   â†’ O(n!) OK          â†’ Backtracking
n â‰¤ 20   â†’ O(2â¿) OK         â†’ Bitmask DP
n â‰¤ 500  â†’ O(nÂ³) OK          â†’ 3 loops / Floyd
n â‰¤ 5000 â†’ O(nÂ²) OK          â†’ 2D DP
n â‰¤ 10âµ  â†’ O(n log n) needed â†’ Sorting / BS / Heap
n â‰¤ 10â¶  â†’ O(n) needed       â†’ Linear / Sliding Window
n â‰¤ 10â¹  â†’ O(log n) needed   â†’ Binary Search / Math
```

---

*Previous chapter: [17 â€” Miscellaneous](17_Miscellaneous.md)*
*Next chapter: [19 â€” Pattern Atlas](19_Pattern_Atlas.md)*
<!--
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
  DSA PATTERN HANDBOOK â€” PATTERN ATLAS
  Chapter 19
  
  Master index of all patterns with cross-references
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
-->

# Chapter 19: Pattern Atlas

A complete master index of all patterns in this handbook, organized for quick lookup.

---

## Master Pattern Table

| # | Pattern Name | Chapter | OA Freq | Key Idea |
|---|---|---|---|---|
| 1 | Brute Force | 02 | â˜…â˜…â˜…â˜…â˜… | Try all possibilities |
| 2 | Prefix Sum | 02 | â˜…â˜…â˜…â˜…â˜… | Precompute cumulative sums for O(1) range queries |
| 3 | Difference Array | 02 | â˜…â˜…â˜…â˜†â˜† | O(1) range updates with prefix sum to reconstruct |
| 4 | Hashing | 02 | â˜…â˜…â˜…â˜…â˜… | O(1) lookup/insert with hash maps |
| 5 | Frequency Counting | 02 | â˜…â˜…â˜…â˜…â˜… | Count occurrences to detect patterns |
| 6 | Fixed Sliding Window | 03 | â˜…â˜…â˜…â˜…â˜† | Maintain window of fixed size k |
| 7 | Variable Sliding Window | 03 | â˜…â˜…â˜…â˜…â˜… | Expand right, shrink left until valid |
| 8 | Two Pointers | 03 | â˜…â˜…â˜…â˜…â˜… | Two indices converging or diverging |
| 9 | Fast & Slow Pointer | 03 | â˜…â˜…â˜…â˜…â˜† | Cycle detection, middle finding |
| 10 | Binary Search (Sorted) | 04 | â˜…â˜…â˜…â˜…â˜… | Halve search space on sorted data |
| 11 | Binary Search on Answer | 04 | â˜…â˜…â˜…â˜…â˜… | Guess answer + feasibility check |
| 12 | Sorting + Greedy | 05 | â˜…â˜…â˜…â˜…â˜… | Sort to enable greedy decisions |
| 13 | Greedy Algorithms | 05 | â˜…â˜…â˜…â˜…â˜… | Locally optimal = globally optimal |
| 14 | Stack | 06 | â˜…â˜…â˜…â˜…â˜… | Matching, nesting, LIFO |
| 15 | Queue | 06 | â˜…â˜…â˜…â˜…â˜† | FIFO processing, BFS backbone |
| 16 | Deque | 06 | â˜…â˜…â˜…â˜†â˜† | Double-ended access |
| 17 | Monotonic Stack | 06 | â˜…â˜…â˜…â˜…â˜† | Next/previous greater/smaller |
| 18 | Monotonic Queue | 06 | â˜…â˜…â˜…â˜†â˜† | Sliding window max/min in O(n) |
| 19 | Heap / Priority Queue | 07 | â˜…â˜…â˜…â˜…â˜… | Dynamic max/min, top-K |
| 20 | Interval Problems | 07 | â˜…â˜…â˜…â˜…â˜† | Range-based scheduling |
| 21 | Merge Intervals | 07 | â˜…â˜…â˜…â˜…â˜… | Sort by start, merge overlapping |
| 22 | Sweep Line | 07 | â˜…â˜…â˜…â˜…â˜† | +1/-1 events, max concurrent |
| 23 | Bit Manipulation | 08 | â˜…â˜…â˜…â˜†â˜† | XOR, masks, power-of-2 |
| 24 | Matrix Traversal | 09 | â˜…â˜…â˜…â˜…â˜† | Spiral, rotation, direction arrays |
| 25 | Simulation | 09 | â˜…â˜…â˜…â˜†â˜† | Follow rules step by step |
| 26 | Recursion | 10 | â˜…â˜…â˜…â˜…â˜† | Solve smaller sub-problems |
| 27 | Backtracking | 10 | â˜…â˜…â˜…â˜…â˜… | Choose â†’ Explore â†’ Unchoose |
| 28 | DFS | 11 | â˜…â˜…â˜…â˜…â˜… | Deep exploration, components |
| 29 | BFS | 11 | â˜…â˜…â˜…â˜…â˜… | Shortest path (unweighted) |
| 30 | Multi-Source BFS | 11 | â˜…â˜…â˜…â˜…â˜† | Simultaneous spread from multiple sources |
| 31 | Topological Sort | 11 | â˜…â˜…â˜…â˜…â˜† | Dependency ordering (Kahn's) |
| 32 | Union Find (DSU) | 12 | â˜…â˜…â˜…â˜…â˜† | Dynamic connectivity |
| 33 | Dijkstra | 12 | â˜…â˜…â˜…â˜…â˜† | Shortest path (weighted, non-negative) |
| 34 | Floyd-Warshall | 12 | â˜…â˜…â˜†â˜†â˜† | All-pairs shortest path (V â‰¤ 400) |
| 35 | Bellman-Ford | 12 | â˜…â˜…â˜†â˜†â˜† | Negative weights, cycle detection |
| 36 | Minimum Spanning Tree | 12 | â˜…â˜…â˜…â˜†â˜† | Connect all nodes, min total weight |
| 37 | Binary Tree DFS | 13 | â˜…â˜…â˜…â˜…â˜… | Preorder/inorder/postorder |
| 38 | Binary Tree BFS | 13 | â˜…â˜…â˜…â˜…â˜† | Level-order traversal |
| 39 | BST Problems | 13 | â˜…â˜…â˜…â˜…â˜† | Exploit sorted property |
| 40 | LCA | 13 | â˜…â˜…â˜…â˜…â˜† | Deepest common ancestor |
| 41 | Trie | 13 | â˜…â˜…â˜…â˜…â˜† | Prefix matching, autocomplete |
| 42 | DP Framework | 14 | â˜…â˜…â˜…â˜…â˜… | State â†’ Recurrence â†’ Base â†’ Order |
| 43 | 1D DP | 14 | â˜…â˜…â˜…â˜…â˜… | Linear sequence decisions |
| 44 | 2D DP | 14 | â˜…â˜…â˜…â˜…â˜… | Grid paths, two-sequence matching |
| 45 | Knapsack | 14 | â˜…â˜…â˜…â˜…â˜… | 0/1 and unbounded variants |
| 46 | LIS | 14 | â˜…â˜…â˜…â˜…â˜† | O(n log n) with patience sorting |
| 47 | Digit DP* | 14 | â˜…â˜…â˜†â˜†â˜† | Count numbers in range with digit constraints |
| 48 | Bitmask DP* | 14 | â˜…â˜…â˜†â˜†â˜† | Subset optimization (n â‰¤ 20) |
| 49 | KMP | 15 | â˜…â˜…â˜…â˜†â˜† | O(n+m) string matching |
| 50 | Z Algorithm | 15 | â˜…â˜…â˜†â˜†â˜† | Prefix matching array |
| 51 | Rabin-Karp* | 15 | â˜…â˜…â˜†â˜†â˜† | Rolling hash matching |
| 52 | Segment Tree* | 16 | â˜…â˜…â˜†â˜†â˜† | Range queries + updates |
| 53 | Fenwick Tree* | 16 | â˜…â˜…â˜†â˜†â˜† | Prefix queries + point updates |
| 54 | Binary Lifting* | 16 | â˜…â˜…â˜†â˜†â˜† | O(log n) ancestor queries |
| 55 | Line Sweep (2D)* | 17 | â˜…â˜…â˜†â˜†â˜† | Skyline, rectangle union |
| 56 | Coordinate Compression | 17 | â˜…â˜…â˜†â˜†â˜† | Map sparse values to dense range |
| 57 | Math Observation | 17 | â˜…â˜…â˜…â˜†â˜† | Spot patterns, derive formulas |

---

## Pattern by Problem Type

### "Find / Search" Problems

| What You're Finding | Pattern |
|---|---|
| Element in sorted array | Binary Search (#10) |
| First/last occurrence | Lower/Upper Bound (#10) |
| Optimal numeric answer | BS on Answer (#11) |
| Element in hash map | Hashing (#4) |
| kth largest/smallest | Heap (#19) |
| Substring in string | KMP (#49), Z (#50), Rabin-Karp (#51) |

### "Count" Problems

| What You're Counting | Pattern |
|---|---|
| Subarrays with property | Sliding Window (#6, #7) |
| Paths in grid | 2D DP (#44) |
| Subsets with sum | Knapsack (#45) |
| Connected components | DFS (#28), Union Find (#32) |
| Numbers in range | Digit DP (#47) |
| Frequency/occurrences | Hashing (#4), Frequency (#5) |

### "Optimize" Problems

| What You're Optimizing | Pattern |
|---|---|
| Min/max subarray sum | Kadane's / 1D DP (#43) |
| Min/max path cost | DP (#44), Dijkstra (#33) |
| Min coins / items | Knapsack (#45) |
| Max non-overlapping intervals | Sorting + Greedy (#12) |
| Min operations to transform | DP (#44 â€” Edit Distance) |
| Min/max of an answer | BS on Answer (#11) |

### "Generate All" Problems

| What You're Generating | Pattern |
|---|---|
| Permutations | Backtracking (#27) |
| Combinations | Backtracking (#27) |
| Subsets | Backtracking (#27) or Bitmask (#48) |
| Valid configurations | Backtracking (#27) |

### "Shortest Path" Problems

| Graph Type | Pattern |
|---|---|
| Unweighted | BFS (#29) |
| Weighted, non-negative | Dijkstra (#33) |
| Negative weights | Bellman-Ford (#35) |
| All pairs | Floyd-Warshall (#34) |
| Grid, unweighted | BFS on Grid (#29) |

### "Tree" Problems

| What About the Tree | Pattern |
|---|---|
| Height, diameter, path sum | Tree DFS (#37) |
| Level-order, right view | Tree BFS (#38) |
| kth smallest, validate | BST (#39) |
| Common ancestor | LCA (#40) |
| Prefix search | Trie (#41) |

---

## Company-Specific High-Frequency Patterns

| Company | Top Patterns |
|---|---|
| **Amazon** | Sliding Window, Two Pointers, BFS/DFS, Greedy, Heap, DP |
| **Google** | Binary Search, Graph (all), DP, Trie, Segment Tree, Math |
| **Microsoft** | Array/String manipulation, BFS/DFS, DP, Stack, BST |
| **Meta** | Graph traversal, DP, Sliding Window, Backtracking, Tree |
| **Adobe** | Two Pointers, Binary Search, Stack, DP, Greedy |
| **Goldman Sachs** | DP, Greedy, Math, Sorting, Binary Search |
| **Flipkart** | Sliding Window, BS on Answer, Greedy, DP, Graph |
| **Samsung** | Matrix, BFS/DFS, Backtracking, Bit Manipulation |

---

## Priority Study Order (Limited Time)

### If you have 1 week:
1. Sliding Window & Two Pointers (Ch. 3)
2. Binary Search (Ch. 4)
3. DFS & BFS (Ch. 11)
4. 1D DP & Knapsack (Ch. 14)
5. Stack & Greedy (Ch. 5, 6)

### If you have 2 weeks:
Add: Trees (Ch. 13), 2D DP (Ch. 14), Heap & Intervals (Ch. 7), Backtracking (Ch. 10)

### If you have 1 month:
Add: Topological Sort, Union Find, Dijkstra (Ch. 11-12), Trie, String Algorithms (Ch. 13, 15), Bit Manipulation (Ch. 8), All DP variants

### If you have 2+ months:
Cover everything including advanced patterns (*).

---

*Previous chapter: [18 â€” Cheat Sheets](18_Cheat_Sheets.md)*

---

**END OF HANDBOOK**

> *"The goal is not to memorize solutions, but to recognize patterns. When you see a problem and immediately know which tool to reach for, you've mastered DSA."*
