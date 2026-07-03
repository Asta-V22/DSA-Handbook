<!--
═══════════════════════════════════════════════════════════════════════════════
  DSA PATTERN HANDBOOK — ADVANCED DATA STRUCTURES
  Chapter 16
  
  Patterns in this chapter:
    Pattern 52: Segment Tree*
    Pattern 53: Fenwick Tree (Binary Indexed Tree)*
    Pattern 54: Binary Lifting*
═══════════════════════════════════════════════════════════════════════════════
-->

# Chapter 16: Advanced Data Structures

These data structures appear less frequently in standard placement OAs but are crucial for competitive programming and top-tier company interviews (Google, Meta). They handle **range queries** and **tree ancestor queries** efficiently.

> **Note**: Patterns marked with * are advanced. If you're short on time, master Chapters 2–14 first.

---
---

# Pattern 52: Segment Tree*

```
═══════════════════════════════════════════════════════════════
  SEGMENT TREE (ADVANCED)
  OA Frequency: ★★☆☆☆
  Companies: Google | Meta | Uber
═══════════════════════════════════════════════════════════════
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

Segment Tree handles both queries and updates in O(log n) — the best of both worlds.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem involve **range queries** (sum, min, max)?
- [ ] Does the array get **updated** between queries?
- [ ] Are there **many queries** (up to 10⁵)?
- [ ] Do prefix sums fail because of updates?

---

## 3. Reusable C++ Template

```cpp
// Template: Segment Tree — Range Sum Query + Point Update
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
| 1 | LeetCode #307 — Range Sum Query - Mutable | Medium | Classic segment tree |
| 2 | LeetCode #315 — Count of Smaller Numbers After Self | Hard | Merge sort or segment tree |
| 3 | LeetCode #2407 — Longest Increasing Subsequence II | Hard | Segment tree + DP |

---
---

# Pattern 53: Fenwick Tree (Binary Indexed Tree)*

```
═══════════════════════════════════════════════════════════════
  FENWICK TREE (BIT) — ADVANCED
  OA Frequency: ★★☆☆☆
  Companies: Google | Meta | Amazon
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

A Fenwick Tree (BIT) is a simpler, more memory-efficient alternative to Segment Tree for **prefix queries + point updates**. It uses clever bit manipulation to achieve O(log n) for both operations.

**Key limitation**: Only works for **associative, commutative** operations with an inverse (like addition). Can't directly do range min/max (use Segment Tree for those).

---

## 2. Reusable C++ Template

```cpp
// Template: Fenwick Tree (BIT) — Prefix Sum + Point Update
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
| 1 | LeetCode #307 — Range Sum Query - Mutable | Medium | BIT alternative to segment tree |
| 2 | LeetCode #493 — Reverse Pairs | Hard | BIT for counting |
| 3 | LeetCode #315 — Count of Smaller Numbers After Self | Hard | BIT with coordinate compression |

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
═══════════════════════════════════════════════════════════════
  BINARY LIFTING — ADVANCED
  OA Frequency: ★★☆☆☆
  Companies: Google | Meta
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Answer **kth ancestor** queries and **LCA queries** on a tree in O(log n) per query." Without binary lifting, finding the kth ancestor requires O(k) time. Binary lifting pre-computes 2^j-th ancestors, enabling jumps of any size in O(log n).

---

## 2. Reusable C++ Template

```cpp
// Template: Binary Lifting — Kth Ancestor / LCA in O(log n)
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
| 1 | LeetCode #1483 — Kth Ancestor of a Tree Node | Hard | Direct binary lifting |
| 2 | LeetCode #2846 — Minimum Edge Weight Equilibrium | Hard | LCA with binary lifting |

---
---

*Previous chapter: [15 — String Algorithms](15_String_Algorithms.md)*
*Next chapter: [17 — Miscellaneous](17_Miscellaneous.md)*
