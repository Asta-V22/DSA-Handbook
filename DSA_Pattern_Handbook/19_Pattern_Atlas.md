<!--
═══════════════════════════════════════════════════════════════════════════════
  DSA PATTERN HANDBOOK — PATTERN ATLAS
  Chapter 19
  
  Master index of all patterns with cross-references
═══════════════════════════════════════════════════════════════════════════════
-->

# Chapter 19: Pattern Atlas

A complete master index of all patterns in this handbook, organized for quick lookup.

---

## Master Pattern Table

| # | Pattern Name | Chapter | OA Freq | Key Idea |
|---|---|---|---|---|
| 1 | Brute Force | 02 | ★★★★★ | Try all possibilities |
| 2 | Prefix Sum | 02 | ★★★★★ | Precompute cumulative sums for O(1) range queries |
| 3 | Difference Array | 02 | ★★★☆☆ | O(1) range updates with prefix sum to reconstruct |
| 4 | Hashing | 02 | ★★★★★ | O(1) lookup/insert with hash maps |
| 5 | Frequency Counting | 02 | ★★★★★ | Count occurrences to detect patterns |
| 6 | Fixed Sliding Window | 03 | ★★★★☆ | Maintain window of fixed size k |
| 7 | Variable Sliding Window | 03 | ★★★★★ | Expand right, shrink left until valid |
| 8 | Two Pointers | 03 | ★★★★★ | Two indices converging or diverging |
| 9 | Fast & Slow Pointer | 03 | ★★★★☆ | Cycle detection, middle finding |
| 10 | Binary Search (Sorted) | 04 | ★★★★★ | Halve search space on sorted data |
| 11 | Binary Search on Answer | 04 | ★★★★★ | Guess answer + feasibility check |
| 12 | Sorting + Greedy | 05 | ★★★★★ | Sort to enable greedy decisions |
| 13 | Greedy Algorithms | 05 | ★★★★★ | Locally optimal = globally optimal |
| 14 | Stack | 06 | ★★★★★ | Matching, nesting, LIFO |
| 15 | Queue | 06 | ★★★★☆ | FIFO processing, BFS backbone |
| 16 | Deque | 06 | ★★★☆☆ | Double-ended access |
| 17 | Monotonic Stack | 06 | ★★★★☆ | Next/previous greater/smaller |
| 18 | Monotonic Queue | 06 | ★★★☆☆ | Sliding window max/min in O(n) |
| 19 | Heap / Priority Queue | 07 | ★★★★★ | Dynamic max/min, top-K |
| 20 | Interval Problems | 07 | ★★★★☆ | Range-based scheduling |
| 21 | Merge Intervals | 07 | ★★★★★ | Sort by start, merge overlapping |
| 22 | Sweep Line | 07 | ★★★★☆ | +1/-1 events, max concurrent |
| 23 | Bit Manipulation | 08 | ★★★☆☆ | XOR, masks, power-of-2 |
| 24 | Matrix Traversal | 09 | ★★★★☆ | Spiral, rotation, direction arrays |
| 25 | Simulation | 09 | ★★★☆☆ | Follow rules step by step |
| 26 | Recursion | 10 | ★★★★☆ | Solve smaller sub-problems |
| 27 | Backtracking | 10 | ★★★★★ | Choose → Explore → Unchoose |
| 28 | DFS | 11 | ★★★★★ | Deep exploration, components |
| 29 | BFS | 11 | ★★★★★ | Shortest path (unweighted) |
| 30 | Multi-Source BFS | 11 | ★★★★☆ | Simultaneous spread from multiple sources |
| 31 | Topological Sort | 11 | ★★★★☆ | Dependency ordering (Kahn's) |
| 32 | Union Find (DSU) | 12 | ★★★★☆ | Dynamic connectivity |
| 33 | Dijkstra | 12 | ★★★★☆ | Shortest path (weighted, non-negative) |
| 34 | Floyd-Warshall | 12 | ★★☆☆☆ | All-pairs shortest path (V ≤ 400) |
| 35 | Bellman-Ford | 12 | ★★☆☆☆ | Negative weights, cycle detection |
| 36 | Minimum Spanning Tree | 12 | ★★★☆☆ | Connect all nodes, min total weight |
| 37 | Binary Tree DFS | 13 | ★★★★★ | Preorder/inorder/postorder |
| 38 | Binary Tree BFS | 13 | ★★★★☆ | Level-order traversal |
| 39 | BST Problems | 13 | ★★★★☆ | Exploit sorted property |
| 40 | LCA | 13 | ★★★★☆ | Deepest common ancestor |
| 41 | Trie | 13 | ★★★★☆ | Prefix matching, autocomplete |
| 42 | DP Framework | 14 | ★★★★★ | State → Recurrence → Base → Order |
| 43 | 1D DP | 14 | ★★★★★ | Linear sequence decisions |
| 44 | 2D DP | 14 | ★★★★★ | Grid paths, two-sequence matching |
| 45 | Knapsack | 14 | ★★★★★ | 0/1 and unbounded variants |
| 46 | LIS | 14 | ★★★★☆ | O(n log n) with patience sorting |
| 47 | Digit DP* | 14 | ★★☆☆☆ | Count numbers in range with digit constraints |
| 48 | Bitmask DP* | 14 | ★★☆☆☆ | Subset optimization (n ≤ 20) |
| 49 | KMP | 15 | ★★★☆☆ | O(n+m) string matching |
| 50 | Z Algorithm | 15 | ★★☆☆☆ | Prefix matching array |
| 51 | Rabin-Karp* | 15 | ★★☆☆☆ | Rolling hash matching |
| 52 | Segment Tree* | 16 | ★★☆☆☆ | Range queries + updates |
| 53 | Fenwick Tree* | 16 | ★★☆☆☆ | Prefix queries + point updates |
| 54 | Binary Lifting* | 16 | ★★☆☆☆ | O(log n) ancestor queries |
| 55 | Line Sweep (2D)* | 17 | ★★☆☆☆ | Skyline, rectangle union |
| 56 | Coordinate Compression | 17 | ★★☆☆☆ | Map sparse values to dense range |
| 57 | Math Observation | 17 | ★★★☆☆ | Spot patterns, derive formulas |

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
| Min operations to transform | DP (#44 — Edit Distance) |
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

*Previous chapter: [18 — Cheat Sheets](18_Cheat_Sheets.md)*

---

**END OF HANDBOOK**

> *"The goal is not to memorize solutions, but to recognize patterns. When you see a problem and immediately know which tool to reach for, you've mastered DSA."*
