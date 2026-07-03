<!--
═══════════════════════════════════════════════════════════════════════════════
  DSA PATTERN HANDBOOK — GRAPH TRAVERSAL
  Chapter 11
  
  Patterns in this chapter:
    Pattern 28: DFS (Depth-First Search)
    Pattern 29: BFS (Breadth-First Search)
    Pattern 30: Multi-Source BFS
    Pattern 31: Topological Sort
  
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
═══════════════════════════════════════════════════════════════
  DFS (DEPTH-FIRST SEARCH)
  OA Frequency: ★★★★★
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Uber | Flipkart
═══════════════════════════════════════════════════════════════
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
| V + E ≤ 10⁵ | O(V + E) ✅ | O(V) ✅ | Standard DFS |
| Grid m × n ≤ 10⁶ | O(m × n) ✅ | O(m × n) ✅ | Grid DFS |
| V ≤ 10⁴ (recursive) | O(V + E) | O(V) stack depth | Watch for stack overflow |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each node, check if it can reach every other node." → O(V²) per query.

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
// Time: O(m × n), Space: O(m × n) for recursion stack

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
// Template C: DFS Cycle Detection (Directed Graph — 3-color)
// Colors: 0 = unvisited, 1 = in-progress, 2 = completed

bool hasCycleDFS(int node, vector<vector<int>>& adj, vector<int>& color) {
    color[node] = 1;  // mark in-progress
    
    for (int neighbor : adj[node]) {
        if (color[neighbor] == 1) return true;   // back edge → cycle!
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
| Grid | O(m × n) | Each cell visited once |

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

Answer: 3 islands ✓
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Stack overflow on large grids | Use iterative DFS with explicit stack, or increase stack size |
| Forgetting to mark visited BEFORE recursion | Mark as visited when you first encounter, not after |
| Modifying grid when you shouldn't | Use a separate visited array if the grid shouldn't change |
| Wrong bounds checking | Check ALL four conditions: r ≥ 0, r < rows, c ≥ 0, c < cols |
| Using DFS for shortest path | DFS finds A path, not the SHORTEST — use BFS instead |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Shortest path (unweighted) | DFS doesn't guarantee shortest path | BFS |
| Level-order processing | DFS goes deep, not level-by-level | BFS |
| Very deep graphs with limited stack | Stack overflow risk | Iterative BFS |

---

## 12. Medium-Level Example Problem

**LeetCode #200 — Number of Islands**

> Given a 2D grid of '1's (land) and '0's (water), count the number of islands.

---

## 13–14. (See Template B above for full implementation)

---

## 15. Interview Follow-up Questions

| Follow-up | How the Solution Changes |
|---|---|
| "What about the number of distinct islands (by shape)?" | LeetCode #694. Normalize DFS path and use hash set |
| "What if we can't modify the grid?" | Use a separate `visited` 2D array |
| "Max area of island?" | Track DFS return value = number of cells visited |

---

## 16–19. Practice, Memory Trick

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #200 — Number of Islands | Medium | Classic grid DFS |
| 2 | LeetCode #695 — Max Area of Island | Medium | DFS with counting |
| 3 | LeetCode #130 — Surrounded Regions | Medium | DFS from border |
| 4 | LeetCode #207 — Course Schedule | Medium | Cycle detection DFS |
| 5 | LeetCode #332 — Reconstruct Itinerary | Hard | Eulerian path DFS |

**Memory Trick:**

> 🧠 **"The Deep Diver"** — DFS dives as deep as possible into the graph before coming back up. Like a cave explorer who always takes the first unexplored tunnel, goes as deep as it goes, then backtracks to try the next tunnel.

---
---

# Pattern 29: BFS (Breadth-First Search)

```
═══════════════════════════════════════════════════════════════
  BFS (BREADTH-FIRST SEARCH)
  OA Frequency: ★★★★★
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Uber | Flipkart
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find the **shortest path** in an unweighted graph or grid." BFS explores nodes in order of their distance from the source — all nodes at distance 1 before distance 2, and so on.

**Why was this pattern invented?**

BFS guarantees that the first time you visit a node, you've arrived via the **shortest path** (in terms of edges). This makes it the go-to algorithm for unweighted shortest path problems.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem ask for the **shortest path** in an unweighted graph/grid?
- [ ] Does it require **level-order** traversal?
- [ ] Does it ask for the **minimum number of steps/moves**?
- [ ] Does the problem involve **spreading** (fire, rotten oranges)?
- [ ] Is it about finding the **nearest** something?

**If you checked 2+ of these → BFS.**

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
| V + E ≤ 10⁵ | O(V + E) ✅ | O(V) ✅ |
| Grid m × n ≤ 10⁶ | O(m × n) ✅ | O(m × n) ✅ |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "Try all possible paths from source to target, track the shortest." → Exponential.

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
// Template: BFS — Shortest Path in Unweighted Graph/Grid
// Time: O(V + E) or O(m × n), Space: O(V) or O(m × n)

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

Shortest path length: 3 ✓ (0→1→3→4 or 0→2→3→4)
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Marking visited when popping, not when pushing | Mark when PUSHING to avoid duplicates in queue |
| Using DFS for shortest path | DFS finds a path, not the shortest — use BFS |
| Forgetting that BFS works on UNWEIGHTED graphs only | For weighted: use Dijkstra |
| Not handling the "no path" case | Return -1 if queue empties without reaching target |

---

## 11–19. Example, Practice

**Example**: LeetCode #1091 — Shortest Path in Binary Matrix (BFS on grid).

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #733 — Flood Fill | Easy | BFS/DFS on grid |
| 2 | LeetCode #994 — Rotting Oranges | Medium | Multi-source BFS |
| 3 | LeetCode #127 — Word Ladder | Hard | BFS on word graph |
| 4 | LeetCode #1091 — Shortest Path in Binary Matrix | Medium | Grid BFS |
| 5 | LeetCode #542 — 01 Matrix | Medium | Multi-source BFS |

**Memory Trick:**

> 🧠 **"The Ripple Effect"** — BFS spreads like ripples in water. Drop a stone (source). The first ring hits all nodes at distance 1. The second ring hits distance 2. The first ring to reach the shore (target) is the shortest path.

---
---

# Pattern 30: Multi-Source BFS

```
═══════════════════════════════════════════════════════════════
  MULTI-SOURCE BFS
  OA Frequency: ★★★★☆
  Companies: Amazon | Google | Microsoft | Meta | Uber
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find the **shortest distance from any one of multiple sources** to each cell." Instead of running BFS from each source separately (O(k × V)), start BFS from ALL sources simultaneously — O(V + E) total.

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
// Time: O(m × n), Space: O(m × n)

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

**LeetCode #994 — Rotting Oranges**

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

## 6–19. Practice, Memory Trick

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #542 — 01 Matrix | Medium | Distance to nearest 0 |
| 2 | LeetCode #994 — Rotting Oranges | Medium | Multi-source spread |
| 3 | LeetCode #286 — Walls and Gates | Medium | Distance from gates |
| 4 | LeetCode #1162 — As Far from Land as Possible | Medium | Max distance from any land |
| 5 | LeetCode #934 — Shortest Bridge | Medium | BFS between two islands |

**Memory Trick:**

> 🧠 **"Multiple Fires"** — Multi-source BFS is like multiple fires starting simultaneously. Each fire spreads at the same speed. The time each cell catches fire is its distance to the nearest fire source. You don't run one fire simulation per source — you let them all burn together.

---
---

# Pattern 31: Topological Sort

```
═══════════════════════════════════════════════════════════════
  TOPOLOGICAL SORT
  OA Frequency: ★★★★☆
  Companies: Amazon | Google | Microsoft | Meta | Uber | Atlassian
═══════════════════════════════════════════════════════════════
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

Kahn's algorithm (BFS) is generally preferred in interviews — it's iterative and handles cycle detection naturally.

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
        return "CYCLE EXISTS — no valid topological order"
    
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
        return {};  // cycle exists — no valid ordering
    }
    
    return order;
}

// Can finish all courses? (LeetCode #207)
bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
    vector<vector<int>> adj(numCourses);
    for (auto& p : prerequisites) {
        adj[p[1]].push_back(p[0]);  // p[1] → p[0] (prereq → course)
    }
    return (int)topologicalSort(numCourses, adj).size() == numCourses;
}
```

---

## 7. Dry Run

**Problem**: Course Schedule with prereqs `[[1,0], [2,0], [3,1], [3,2]]`.

```
Graph: 0→1, 0→2, 1→3, 2→3
In-degrees: [0:0, 1:1, 2:1, 3:2]

Queue: [0]  (only node with in-degree 0)

Pop 0. Order: [0]. Reduce neighbors:
  1: inDeg 1→0 → push 1
  2: inDeg 1→0 → push 2
Queue: [1, 2]

Pop 1. Order: [0, 1]. Reduce neighbors:
  3: inDeg 2→1
Queue: [2]

Pop 2. Order: [0, 1, 2]. Reduce neighbors:
  3: inDeg 1→0 → push 3
Queue: [3]

Pop 3. Order: [0, 1, 2, 3].
Queue: empty.

Order has 4 nodes = V → no cycle ✓
Valid course order: 0, 1, 2, 3 ✓
```

---

## 8. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Building edges in wrong direction | For prereqs, `[course, prereq]` means `prereq → course` |
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

## 11–19. Practice, Memory Trick

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #207 — Course Schedule | Medium | Cycle detection in DAG |
| 2 | LeetCode #210 — Course Schedule II | Medium | Return topological order |
| 3 | LeetCode #269 — Alien Dictionary | Hard | Build graph + topo sort |
| 4 | LeetCode #310 — Minimum Height Trees | Medium | Iterative leaf removal (topo-like) |
| 5 | LeetCode #329 — Longest Increasing Path in Matrix | Hard | DFS + topo sort / memoization |

**Memory Trick:**

> 🧠 **"The Course Registration Line"**
>
> Imagine registering for courses. You can only register for a course when ALL its prerequisites are completed (in-degree = 0). When you complete a course, it "unlocks" dependent courses (decrease their in-degree). Courses with no remaining prerequisites join the registration queue.

---
---

*Previous chapter: [10 — Recursion & Backtracking](10_Recursion_and_Backtracking.md)*
*Next chapter: [12 — Advanced Graphs](12_Advanced_Graphs.md)*
