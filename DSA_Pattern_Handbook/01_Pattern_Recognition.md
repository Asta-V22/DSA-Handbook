<!--
═══════════════════════════════════════════════════════════════════════════════
  DSA PATTERN HANDBOOK — PATTERN RECOGNITION FLOWCHART
  Chapter 01
  
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

# Chapter 1: Pattern Recognition Flowchart

---

> *"An expert is someone who has made every possible mistake in a very narrow field. A fast expert is someone who recognizes the pattern before making the mistake."*

---

## Why a Flowchart?

During an OA, you have limited time. The most dangerous trap is **trying the wrong approach first** — spending 15 minutes on a brute force solution only to realize you need a sliding window, or attempting DP when a greedy approach works.

This chapter provides a systematic **decision framework** that you can follow in the first 2–3 minutes after reading a problem. It won't always give you the exact pattern, but it will **narrow your search to 2–3 candidates** instantly.

---

## The 5-Step Recognition Process

Before looking at the flowchart, internalize this 5-step process:

### Step 1: Read the Constraints

| Constraint | What It Tells You |
|---|---|
| n ≤ 15–20 | Bitmask / brute force / backtracking is expected |
| n ≤ 1000–5000 | O(n²) is acceptable — consider DP, nested loops |
| n ≤ 10⁵ | O(n log n) — sorting, binary search, divide & conquer |
| n ≤ 10⁶–10⁷ | O(n) — single pass, sliding window, prefix sum |
| n ≤ 10⁹ | O(log n) or O(1) — math formula, binary search on answer |

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
┌──────────────────────────────────────────────────────────────────────┐
│                     READ THE PROBLEM                                 │
│              What is the input? What is asked?                       │
└──────────────────────────┬───────────────────────────────────────────┘
                           │
                           ▼
              ┌────────────────────────┐
              │  Is the input a GRAPH  │
              │  or TREE?              │
              └─────┬──────────┬───────┘
                YES │          │ NO
                    ▼          ▼
        ┌───────────────┐   ┌──────────────────────────┐
        │ Go to GRAPH / │   │ Is the input an ARRAY    │
        │ TREE section  │   │ or STRING?               │
        │ (below)       │   └─────┬──────────────┬─────┘
        └───────────────┘     YES │              │ NO
                                  ▼              ▼
                    ┌──────────────────┐  ┌──────────────────────┐
                    │ Go to ARRAY /    │  │ Is it about MATH,    │
                    │ STRING section   │  │ BITS, or SIMULATION? │
                    │ (below)          │  └────┬────────────┬────┘
                    └──────────────────┘   YES │            │ NO
                                               ▼            ▼
                                 ┌───────────────┐  ┌──────────────┐
                                 │ Math/Bit/Sim  │  │ INTERVALS?   │
                                 │ section       │  │ → Merge/Sweep│
                                 └───────────────┘  └──────────────┘
```

---

## Branch 1: Array / String Problems

```
                        ARRAY / STRING INPUT
                              │
              ┌───────────────┼───────────────────┐
              │               │                   │
              ▼               ▼                   ▼
     "Subarray / Substring"  "Find target"    "All subsets /
      with some property      or "Sorted"      permutations"
              │               │                   │
              ▼               ▼                   ▼
  ┌───────────────────┐  ┌──────────┐     ┌──────────────┐
  │ Is the window     │  │ Binary   │     │ Backtracking │
  │ size fixed?       │  │ Search   │     │ / Recursion  │
  └──┬──────────┬─────┘  └──────────┘     └──────────────┘
  YES│          │NO
     ▼          ▼
┌──────────┐  ┌──────────────────────────────────────────┐
│ Fixed    │  │ Does the window shrink/grow based         │
│ Sliding  │  │ on a condition?                           │
│ Window   │  └──────┬──────────────────────┬─────────────┘
└──────────┘     YES │                      │ NO
                     ▼                      ▼
            ┌──────────────┐    ┌──────────────────────────┐
            │ Variable     │    │ Can you precompute        │
            │ Sliding      │    │ cumulative information?   │
            │ Window       │    └────┬───────────────┬──────┘
            └──────────────┘     YES │               │ NO
                                     ▼               ▼
                            ┌──────────────┐  ┌─────────────┐
                            │ Prefix Sum / │  │ Is the array │
                            │ Hashing      │  │ sorted?      │
                            └──────────────┘  └──┬───────┬───┘
                                              YES│       │NO
                                                 ▼       ▼
                                         ┌──────────┐ ┌──────────┐
                                         │ Two      │ │ Consider │
                                         │ Pointers │ │ Sorting  │
                                         └──────────┘ │ first,   │
                                                      │ then Two │
                                                      │ Pointers │
                                                      │ or Greedy│
                                                      └──────────┘
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
                              │
              ┌───────────────┼───────────────────┐
              │               │                   │
              ▼               ▼                   ▼
        Unweighted?      Weighted?          "Connected
              │               │              components?"
              │               │                   │
              ▼               ▼                   ▼
    ┌──────────────┐  ┌──────────────┐    ┌──────────────┐
    │ Shortest     │  │ Shortest     │    │ Union Find   │
    │ path?        │  │ path?        │    │ or DFS/BFS   │
    │ → BFS        │  │              │    └──────────────┘
    │              │  │ Non-negative │
    │ Traversal?   │  │ → Dijkstra   │
    │ → DFS/BFS    │  │              │
    │              │  │ Negative     │
    │ Dependencies │  │ → Bellman-F  │
    │ → Topo Sort  │  │              │
    │              │  │ All pairs    │
    │ Cycle?       │  │ → Floyd-W    │
    │ → DFS color  │  │              │
    └──────────────┘  │ Min tree     │
                      │ → MST        │
                      └──────────────┘
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
                              │
              ┌───────────────┼───────────────────┐
              │               │                   │
              ▼               ▼                   ▼
        "Path / depth /   "Level-order /     "Ancestor /
         subtree sum"      width / zigzag"    distance"
              │               │                   │
              ▼               ▼                   ▼
    ┌──────────────┐  ┌──────────────┐    ┌──────────────┐
    │ Binary Tree  │  │ Binary Tree  │    │ LCA          │
    │ DFS          │  │ BFS          │    │ (Binary      │
    │ (recursion)  │  │ (queue)      │    │  Lifting)    │
    └──────────────┘  └──────────────┘    └──────────────┘
              │
              ▼
    ┌──────────────┐
    │ Is it a BST? │
    │ → BST-specific│
    │   properties  │
    │   (inorder =  │
    │    sorted)    │
    └──────────────┘
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
                              │
              ┌───────────────┼───────────────────┐
              │               │                   │
              ▼               ▼                   ▼
     "Does a greedy     "Overlapping         "Check if X
      choice lead to     subproblems?"         is feasible
      global optimum?"        │                for value V?"
              │               ▼                      │
              ▼       ┌──────────────┐               ▼
    ┌──────────────┐  │ Dynamic      │    ┌──────────────────┐
    │ Greedy       │  │ Programming  │    │ Binary Search    │
    │ Algorithm    │  │              │    │ on Answer        │
    └──────────────┘  │ 1D? 2D?     │    │                  │
                      │ Knapsack?   │    │ + Greedy/Check   │
                      │ LIS?        │    │   function       │
                      └──────────────┘    └──────────────────┘
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
                          │
          ┌───────────────┼──────────────┐
          │               │              │
          ▼               ▼              ▼
   "Merge / combine"  "Count events   "Schedule non-
    overlapping        at time t"       overlapping"
          │               │              │
          ▼               ▼              ▼
  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐
  │ Merge        │ │ Sweep Line / │ │ Greedy       │
  │ Intervals    │ │ Difference   │ │ (sort by end)│
  │ (sort+merge) │ │ Array        │ │              │
  └──────────────┘ └──────────────┘ └──────────────┘
```

---

## The "I'm Stuck" Recovery Flowchart

When you've been staring at a problem for 5+ minutes with no idea:

```
┌─────────────────────────────────────────────────────────────┐
│                        I'M STUCK                            │
└─────────────────────────┬───────────────────────────────────┘
                          │
                          ▼
              ┌───────────────────────┐
              │ 1. Re-read constraints│  ← Most common fix
              │    What complexity    │
              │    is expected?       │
              └───────────┬───────────┘
                          │
                          ▼
              ┌───────────────────────┐
              │ 2. Write brute force  │  ← Even if you know
              │    Can you see a      │     it won't pass,
              │    repeated pattern?  │     it reveals structure
              └───────────┬───────────┘
                          │
                          ▼
              ┌───────────────────────┐
              │ 3. Try small examples │  ← n=3, n=4, n=5
              │    Do you see         │     Look for patterns
              │    repeated work?     │     in the output
              └───────────┬───────────┘
                          │
                          ▼
              ┌───────────────────────┐
              │ 4. Think about what   │
              │    information you    │  ← "What if I knew
              │    WISH you had       │     the answer for
              │                       │     n-1 elements?"
              └───────────┬───────────┘
                          │
                          ▼
              ┌───────────────────────┐
              │ 5. Consider the       │
              │    OPPOSITE question  │  ← "Instead of finding
              │                       │     max, what if I
              │                       │     binary search it?"
              └───────────┬───────────┘
                          │
                          ▼
              ┌───────────────────────┐
              │ 6. Sort the input     │  ← Sorting often
              │    Does anything      │     reveals structure
              │    become clearer?    │     you couldn't see
              └───────────────────────┘
```

---

## Quick Pattern Selector by Constraint + Question Type

This is the most practical table in the handbook. Cross-reference the constraint size with the question type:

| Question Type ↓ \ Constraint → | n ≤ 20 | n ≤ 5000 | n ≤ 10⁵ | n ≤ 10⁶ |
|---|---|---|---|---|
| **Subarray sum/property** | Brute Force | Prefix Sum | Prefix Sum + Hash | Prefix Sum |
| **Longest substring** | Brute Force | O(n²) check | Variable Sliding Window | Variable Sliding Window |
| **Find target in sorted** | Linear | Binary Search | Binary Search | Binary Search |
| **Shortest path** | BFS/DFS | Dijkstra/BFS | Dijkstra | Dijkstra + optimized |
| **All subsets** | Bitmask | — | — | — |
| **Permutations** | Backtracking | — | — | — |
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
| **Prefix Sum + Hashing** | Subarray Sum = K (LeetCode #560) | Prefix sum converts to "find pair" → hash map |
| **Sorting + Greedy + Heap** | Meeting Rooms II (LeetCode #253) | Sort by start; heap tracks earliest end time |
| **DP + Binary Search** | LIS O(n log n) (LeetCode #300) | DP for structure; binary search for position |
| **Graph + DP** | Shortest path in DAG | Topological sort + DP relaxation |
| **Sliding Window + Hash Map** | Longest Substring Without Repeat (#3) | Window for range; hash map for tracking |
| **Union Find + Sorting** | Kruskal's MST | Sort edges; union find for cycle detection |

---

## Your First 3 Minutes on Any OA Problem

```
┌─────────────────────────────────────────┐
│         MINUTE 0:00 – 1:00              │
│                                         │
│  • Read the problem ONCE completely     │
│  • Identify: input type, output type    │
│  • Note the constraints (n = ?)         │
│  • Circle keywords (max, min, shortest, │
│    count, subarray, path, etc.)         │
└─────────────────────┬───────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────┐
│         MINUTE 1:00 – 2:00              │
│                                         │
│  • Determine expected complexity        │
│    from constraint table                │
│  • Match keywords to patterns           │
│    using Recognition Keywords Table     │
│  • Narrow down to 2-3 candidate        │
│    patterns                             │
└─────────────────────┬───────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────┐
│         MINUTE 2:00 – 3:00              │
│                                         │
│  • Think through brute force mentally   │
│  • Identify the bottleneck              │
│  • Confirm which pattern eliminates     │
│    the bottleneck                       │
│  • Start coding (template first)        │
└─────────────────────────────────────────┘
```

---

## What's Next?

Now that you have the framework for recognizing patterns, the rest of this handbook teaches you each pattern in depth. Starting from Chapter 02 (Fundamentals), every pattern follows the same comprehensive 20-section template so you can build both **understanding** and **muscle memory**.

The patterns are ordered from fundamental to advanced. Each builds on concepts from earlier chapters, but every chapter is also self-contained — you can jump to any pattern you need.

---

*Previous chapter: [00 — Front Matter](00_Front_Matter.md)*
*Next chapter: [02 — Fundamentals](02_Fundamentals.md)*
