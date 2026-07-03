<!--
═══════════════════════════════════════════════════════════════════════════════
  DSA PATTERN HANDBOOK — CHEAT SHEETS
  Chapter 18
  
  Sections:
    - Complexity Cheat Sheet
    - C++ STL Quick Reference
    - Interview Tips & Strategy
    - Common OA Mistakes
    - 30-Minute Revision Guide
═══════════════════════════════════════════════════════════════════════════════
-->

# Chapter 18: Cheat Sheets

---

## 18.1 — Complexity Cheat Sheet

### Time Complexity Ranking (Fastest to Slowest)

| Complexity | Name | Max n (1s) | Example Algorithm |
|---|---|---|---|
| O(1) | Constant | ∞ | Hash table lookup |
| O(log n) | Logarithmic | 10¹⁸ | Binary search |
| O(√n) | Square root | 10¹⁴ | Prime checking |
| O(n) | Linear | 10⁸ | Array traversal |
| O(n log n) | Linearithmic | 10⁶ | Merge sort, Dijkstra |
| O(n√n) | — | 10⁵ | Mo's algorithm |
| O(n²) | Quadratic | 5000 | Nested loops, 2D DP |
| O(n² log n) | — | 2000 | Sort within nested loops |
| O(n³) | Cubic | 400 | Floyd-Warshall |
| O(2ⁿ) | Exponential | 20-25 | Subsets, bitmask DP |
| O(n!) | Factorial | 10-12 | Permutations |

### Constraint → Expected Complexity

| Constraint | Expected Time | Typical Pattern |
|---|---|---|
| n ≤ 10 | O(n!) | Backtracking, brute force |
| n ≤ 20 | O(2ⁿ) | Bitmask DP, backtracking |
| n ≤ 500 | O(n³) | Floyd-Warshall, 3 nested loops |
| n ≤ 5000 | O(n²) | 2D DP, LIS O(n²) |
| n ≤ 10⁵ | O(n log n) | Sorting, Dijkstra, Segment Tree |
| n ≤ 10⁶ | O(n) | Linear scan, sliding window |
| n ≤ 10⁸ | O(n) careful | Simple loop, prefix sum |
| n ≤ 10¹⁸ | O(log n) | Binary search, math |

### Space Complexity Reference

| Structure | Space | Notes |
|---|---|---|
| Adjacency list | O(V + E) | Standard for graphs |
| Adjacency matrix | O(V²) | Dense graphs only |
| 2D DP table | O(m × n) | Often optimizable to O(n) |
| Segment tree | O(4n) | Array-based |
| Trie | O(Σ word_lengths) | Shared prefixes save space |

---

## 18.2 — C++ STL Quick Reference

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

## 18.3 — Interview Tips & Strategy

### Before the OA

1. **Read ALL problems first** — solve the easiest one first for confidence.
2. **Estimate time per problem** — for a 90-min OA with 3 problems: ~30 min each.
3. **Have your templates ready** — BFS, DFS, Union Find, Segment Tree, etc.

### During Problem Solving

```
The 5-Minute Framework:
1. READ carefully (2 min) — don't miss constraints!
2. IDENTIFY the pattern (1 min) — use the flowchart from Chapter 1
3. THINK about edge cases (1 min)
4. CODE the solution (1 min planning)
5. TEST with examples before submitting
```

### Pattern Recognition Decision Tree (Quick)

```
Is the input sorted?
  → Binary Search

Is it asking for shortest path?
  → Unweighted: BFS | Weighted: Dijkstra

Is it a string problem?
  → Sliding Window, DP, or Trie

Does it say "all permutations/combinations"?
  → Backtracking

Does it say "minimum/maximum of something"?
  → DP, Greedy, or Binary Search on Answer

Is it about intervals/scheduling?
  → Sort + Greedy or Sweep Line

Is the constraint n ≤ 20?
  → Bitmask DP or Backtracking

Is the constraint very large (10⁹+)?
  → Binary Search, Math, or O(log n) approach
```

### Communication Tips (Live Interviews)

1. **Think aloud** — explain your reasoning as you work.
2. **Start with brute force** — "The brute force is O(n²)..." then optimize.
3. **Confirm understanding** — restate the problem in your own words.
4. **State complexity** — always mention time and space complexity.
5. **Handle edge cases explicitly** — empty input, single element, all same values.

---

## 18.4 — Common OA Mistakes

| # | Mistake | Fix |
|---|---|---|
| 1 | **Integer overflow** | Use `long long` for sums, products, and intermediate calculations |
| 2 | **Off-by-one errors** | Trace through with n=1, n=2 manually |
| 3 | **Not reading constraints** | The constraint IS the hint — it tells you the expected complexity |
| 4 | **Submitting without testing** | Test with the given examples AND edge cases |
| 5 | **TLE on brute force** | Check if n ≤ 10⁵ — you probably need O(n log n) or better |
| 6 | **Modifying input when you shouldn't** | Use a copy or separate visited array |
| 7 | **Wrong comparison in sort** | `a < b` = ascending, `a > b` = descending, NOT `a <= b` |
| 8 | **Infinite loop in binary search** | Check: `lo <= hi` vs `lo < hi`, and `mid` calculation |
| 9 | **Not handling "not found" / -1 cases** | Always consider: what if the answer doesn't exist? |
| 10 | **Ignoring the "modulo 10⁹+7" requirement** | Apply mod at every addition/multiplication step |

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

## 18.5 — 30-Minute Revision Guide

> **Use this the night before or morning of your OA.**

### Core Patterns (5 minutes)

| # | Pattern | Key Idea | Template |
|---|---|---|---|
| 1 | **Sliding Window** | Expand right, shrink left | `while (invalid) shrink left` |
| 2 | **Two Pointers** | Start from both ends | `while (lo < hi)` |
| 3 | **Binary Search** | Halve the search space | `while (lo < hi) mid = lo+(hi-lo)/2` |
| 4 | **BS on Answer** | Guess + verify | `canAchieve(mid) → adjust range` |
| 5 | **Greedy** | Locally best = globally best | Sort + iterate |
| 6 | **Monotonic Stack** | Next greater/smaller | Pop while top < current |
| 7 | **BFS** | Shortest path (unweighted) | Queue + visited |
| 8 | **DFS** | Connected components, paths | Recursion + visited |
| 9 | **Topological Sort** | Dependency ordering | In-degree + queue |
| 10 | **DP** | State → Recurrence → Base → Order | `dp[i] = f(dp[i-1], ...)` |

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
✓ Empty input (n = 0)
✓ Single element (n = 1)
✓ All same elements
✓ Already sorted / reverse sorted
✓ Negative numbers
✓ Maximum values (INT_MAX, 10⁹)
✓ Overflow potential (use long long)
✓ Disconnected graph
✓ Tree with only left or only right children
✓ String with special characters or empty string
```

### Complexity Quick Check (2 minutes)

```
n ≤ 10   → O(n!) OK          → Backtracking
n ≤ 20   → O(2ⁿ) OK         → Bitmask DP
n ≤ 500  → O(n³) OK          → 3 loops / Floyd
n ≤ 5000 → O(n²) OK          → 2D DP
n ≤ 10⁵  → O(n log n) needed → Sorting / BS / Heap
n ≤ 10⁶  → O(n) needed       → Linear / Sliding Window
n ≤ 10⁹  → O(log n) needed   → Binary Search / Math
```

---

*Previous chapter: [17 — Miscellaneous](17_Miscellaneous.md)*
*Next chapter: [19 — Pattern Atlas](19_Pattern_Atlas.md)*
