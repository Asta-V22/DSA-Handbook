<!--
═══════════════════════════════════════════════════════════════════════════════
  DSA PATTERN HANDBOOK — ADVANCED GRAPHS
  Chapter 12
  
  Patterns in this chapter:
    Pattern 32: Union Find (DSU)
    Pattern 33: Dijkstra
    Pattern 34: Floyd-Warshall
    Pattern 35: Bellman-Ford
    Pattern 36: Minimum Spanning Tree
  
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

# Chapter 12: Advanced Graphs

This chapter covers five essential graph algorithms that handle more complex scenarios than basic DFS/BFS: **dynamic connectivity** (Union Find), **weighted shortest paths** (Dijkstra, Bellman-Ford, Floyd-Warshall), and **minimum spanning trees** (Kruskal/Prim).

---
---

# Pattern 32: Union Find (Disjoint Set Union)

```
═══════════════════════════════════════════════════════════════
  UNION FIND (DSU)
  OA Frequency: ★★★★☆
  Companies: Amazon | Google | Microsoft | Meta | Uber | Flipkart
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Are two elements in the **same group**?" and "**Merge two groups** into one." Union Find handles dynamic connectivity — efficiently answering whether two nodes are connected, and merging sets.

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
// Time: O(α(n)) ≈ O(1) per operation (amortized)
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
| find() | O(α(n)) ≈ O(1) | Path compression flattens the tree |
| unite() | O(α(n)) ≈ O(1) | Union by rank keeps trees short |
| Build | O(n) | Initialize n singletons |
| m operations | O(m · α(n)) ≈ O(m) | α(n) ≤ 5 for all practical n |

**α(n)** = inverse Ackermann function. It grows so slowly that it's effectively constant (≤ 5 for n ≤ 10⁸⁰).

---

## 6. Medium-Level Example

**LeetCode #547 — Number of Provinces** (or Friend Circles)

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
| 1 | LeetCode #547 — Number of Provinces | Medium | Basic Union Find |
| 2 | LeetCode #684 — Redundant Connection | Medium | Detect cycle |
| 3 | LeetCode #721 — Accounts Merge | Medium | Group by equivalence |
| 4 | LeetCode #1319 — Number of Operations to Make Network Connected | Medium | Component counting |
| 5 | LeetCode #839 — Similar String Groups | Hard | Union Find on strings |

**Memory Trick:**

> 🧠 **"The Club System"** — Each person starts as their own club president. When two clubs merge, one president becomes the leader. `find()` asks "who's my leader?" and `unite()` merges two clubs. Path compression: everyone directly points to the top leader after the first inquiry.

---
---

# Pattern 33: Dijkstra's Algorithm

```
═══════════════════════════════════════════════════════════════
  DIJKSTRA'S ALGORITHM
  OA Frequency: ★★★★☆
  Companies: Google | Amazon | Microsoft | Uber | Flipkart
═══════════════════════════════════════════════════════════════
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
Graph: 0→1(4), 0→2(1), 2→1(2), 1→3(1), 2→3(5)

dist = [0, ∞, ∞, ∞]   PQ: [(0,0)]

Pop (0,0). Neighbors: 1(w=4), 2(w=1)
  dist[1] = min(∞, 0+4) = 4. Push (4,1).
  dist[2] = min(∞, 0+1) = 1. Push (1,2).
  dist = [0, 4, 1, ∞]   PQ: [(1,2), (4,1)]

Pop (1,2). Neighbors: 1(w=2), 3(w=5)
  dist[1] = min(4, 1+2) = 3. Push (3,1).
  dist[3] = min(∞, 1+5) = 6. Push (6,3).
  dist = [0, 3, 1, 6]   PQ: [(3,1), (4,1), (6,3)]

Pop (3,1). Neighbors: 3(w=1)
  dist[3] = min(6, 3+1) = 4. Push (4,3).
  dist = [0, 3, 1, 4]   PQ: [(4,1), (4,3), (6,3)]

Pop (4,1). d=4 > dist[1]=3 → skip (outdated).

Pop (4,3). d=4 == dist[3] → process. No new improvements.

Pop (6,3). d=6 > dist[3]=4 → skip.

Final: dist = [0, 3, 1, 4]
  0→2→1 costs 3, 0→2→1→3 costs 4 ✓
```

---

## 7. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Using with negative weights | Dijkstra FAILS with negative weights — use Bellman-Ford |
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
| 1 | LeetCode #743 — Network Delay Time | Medium | Standard Dijkstra |
| 2 | LeetCode #1631 — Path With Minimum Effort | Medium | Dijkstra on max-edge |
| 3 | LeetCode #787 — Cheapest Flights Within K Stops | Medium | Modified Dijkstra/BFS |
| 4 | LeetCode #1514 — Path with Maximum Probability | Medium | Max-probability Dijkstra |
| 5 | LeetCode #778 — Swim in Rising Water | Hard | Binary search + BFS or Dijkstra |

**Memory Trick:**

> 🧠 **"The Greedy GPS"** — Dijkstra's algorithm is like a GPS that always explores the closest unexplored intersection first. It guarantees finding the shortest route because it never passes a closer intersection without checking it.

---
---

# Pattern 34: Floyd-Warshall

```
═══════════════════════════════════════════════════════════════
  FLOYD-WARSHALL
  OA Frequency: ★★☆☆☆
  Companies: Google | Amazon | Microsoft
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find shortest paths between **ALL pairs** of nodes." While Dijkstra finds shortest paths from one source, Floyd-Warshall finds them for every possible source and destination simultaneously.

---

## 2. When to Use

| Scenario | Floyd-Warshall | Alternative |
|---|---|---|
| All-pairs, V ≤ 400 | O(V³) ✅ | — |
| All-pairs, V ≤ 1000 | O(V³) ⚠️ tight | Run Dijkstra V times: O(V·(V+E)logV) |
| All-pairs, V > 1000 | ❌ too slow | Dijkstra from each source |
| Can have negative weights | ✅ works! | Bellman-Ford from each source |
| Detect negative cycles | ✅ check `dist[i][i] < 0` | Bellman-Ford |

---

## 3. Reusable C++ Template

```cpp
// Template: Floyd-Warshall — All Pairs Shortest Path
// Time: O(V³), Space: O(V²)

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
| 1 | LeetCode #1334 — Find the City With Fewest Reachable Neighbors | Medium | Floyd-Warshall application |
| 2 | LeetCode #399 — Evaluate Division | Medium | Graph as distances |
| 3 | LeetCode #1462 — Course Schedule IV | Medium | Reachability (transitive closure) |

**Memory Trick:**

> 🧠 **"The Stopover Check"** — Floyd-Warshall asks: "For every pair of cities (i,j), is there a stopover city k that makes the trip shorter?" It tries every possible stopover.

---
---

# Pattern 35: Bellman-Ford

```
═══════════════════════════════════════════════════════════════
  BELLMAN-FORD
  OA Frequency: ★★☆☆☆
  Companies: Google | Amazon | Microsoft | Goldman Sachs
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Shortest path from a source in a graph that may have **negative edge weights**." Dijkstra can't handle negatives. Bellman-Ford can, and also detects **negative cycles**.

---

## 2. When to Use

| Scenario | Dijkstra | Bellman-Ford |
|---|---|---|
| Non-negative weights | ✅ Faster | ✅ Works but slower |
| Negative weights | ❌ Fails | ✅ Correct |
| Detect negative cycle | ❌ Can't | ✅ Run V-th iteration |
| "At most k edges" constraint | ❌ Hard to modify | ✅ Stop after k iterations |

---

## 3. Reusable C++ Template

```cpp
// Template: Bellman-Ford — Shortest Path with Negative Weights
// Time: O(V × E), Space: O(V)

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
| 1 | LeetCode #787 — Cheapest Flights Within K Stops | Medium | Bellman-Ford with k iterations |
| 2 | LeetCode #743 — Network Delay Time | Medium | Can use Dijkstra or Bellman-Ford |

**Memory Trick:**

> 🧠 **"The Patient Relaxer"** — Bellman-Ford patiently relaxes every edge, V-1 times. Each pass ensures paths with one more edge are optimized. If the V-th pass still improves something, there's a negative cycle — an infinite discount loop!

---
---

# Pattern 36: Minimum Spanning Tree

```
═══════════════════════════════════════════════════════════════
  MINIMUM SPANNING TREE (MST)
  OA Frequency: ★★★☆☆
  Companies: Google | Amazon | Microsoft | Uber
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Connect all nodes with the **minimum total edge weight**." An MST is a subset of edges that connects all nodes with no cycles and minimum total weight.

---

## 2. Two Algorithms

| Algorithm | Approach | Best For | Complexity |
|---|---|---|---|
| **Kruskal's** | Sort edges, add if no cycle (Union Find) | Sparse graphs (E ≈ V) | O(E log E) |
| **Prim's** | Grow tree from a node, always add cheapest edge | Dense graphs (E ≈ V²) | O(E log V) with heap |

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

Process (2,3,4): not connected → add. Weight=4. Components: 3.
Process (0,3,5): not connected → add. Weight=9. Components: 2.
Process (0,2,6): already connected (0-3-2) → skip.
Process (0,1,10): not connected → add. Weight=19. Components: 1.

MST edges: (2,3), (0,3), (0,1). Total weight: 19.
edgesUsed = 3 = n-1 ✓
```

---

## 5. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #1584 — Min Cost to Connect All Points | Medium | Kruskal's / Prim's |
| 2 | LeetCode #1135 — Connecting Cities With Minimum Cost | Medium | Standard MST |
| 3 | LeetCode #1489 — Find Critical and Pseudo-Critical Edges in MST | Hard | MST analysis |

**Memory Trick:**

> 🧠 **"The Cheapskate Builder"** — Kruskal's builds a road network by always choosing the cheapest road next. But skip it if both towns are already connected (that would create a cycle). Use Union Find to quickly check connectivity.

---

## Summary: When to Use Which Graph Algorithm

| Problem | Algorithm |
|---|---|
| Connected components (static) | DFS / BFS |
| Connected components (dynamic edges) | Union Find |
| Shortest path (unweighted) | BFS |
| Shortest path (weighted, non-negative) | Dijkstra |
| Shortest path (negative weights) | Bellman-Ford |
| All-pairs shortest path (V ≤ 400) | Floyd-Warshall |
| Minimum cost to connect all nodes | MST (Kruskal / Prim) |
| Dependency ordering | Topological Sort |
| Cycle detection (directed) | DFS 3-color / Topological Sort |
| Cycle detection (undirected) | Union Find / DFS |

---
---

*Previous chapter: [11 — Graph Traversal](11_Graph_Traversal.md)*
*Next chapter: [13 — Trees](13_Trees.md)*
