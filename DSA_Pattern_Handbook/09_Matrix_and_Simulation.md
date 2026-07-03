<!--
═══════════════════════════════════════════════════════════════════════════════
  DSA PATTERN HANDBOOK — MATRIX & SIMULATION
  Chapter 09
  
  Patterns in this chapter:
    Pattern 24: Matrix Traversal
    Pattern 25: Simulation
  
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

# Chapter 9: Matrix & Simulation

These patterns handle 2D arrays (matrices) and problems that require you to follow a set of rules step by step. While conceptually simpler than graph algorithms, they test your ability to handle indices, boundaries, and edge cases precisely.

---
---

# Pattern 24: Matrix Traversal

```
═══════════════════════════════════════════════════════════════
  MATRIX TRAVERSAL
  OA Frequency: ★★★★☆
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Samsung
═══════════════════════════════════════════════════════════════
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
| "Rotate matrix 90°" | Transpose + reverse |
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

// Rotate 90° clockwise: transpose, then reverse each row
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
// Time: O(m × n), Space: O(1) extra

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

## 6–10. Complexity, Dry Run, Mistakes

**Complexity**: O(m × n) time — visit each cell once. O(1) extra space.

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

## 12–19. Example, Practice

**Example**: LeetCode #54 — Spiral Matrix. See template above.

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #867 — Transpose Matrix | Easy | Basic matrix operation |
| 2 | LeetCode #48 — Rotate Image | Medium | In-place rotation |
| 3 | LeetCode #54 — Spiral Matrix | Medium | Classic spiral traversal |
| 4 | LeetCode #73 — Set Matrix Zeroes | Medium | In-place flag management |
| 5 | LeetCode #59 — Spiral Matrix II | Medium | Generate spiral-filled matrix |

**Memory Trick:**

> 🧠 **"Peeling the Onion"** — Spiral traversal peels the matrix layer by layer, like an onion. Process the outer border (top row → right column → bottom row → left column), then move inward.

---
---

# Pattern 25: Simulation

```
═══════════════════════════════════════════════════════════════
  SIMULATION
  OA Frequency: ★★★☆☆
  Companies: Amazon | Google | Microsoft | Apple
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Implement the exact process described in the problem, step by step." No clever algorithm — just translate the rules into code accurately.

Simulation problems test **implementation precision**: can you handle edge cases, boundary conditions, and complex state transitions without bugs?

---

## 2. Pattern Identification Checklist

- [ ] Does the problem describe a **process** or **game** with clear rules?
- [ ] Can you directly **translate the rules into code**?
- [ ] Is there no known algorithmic shortcut — you must "just do it"?
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
// Template: Simulation — Step-by-step processing
// Time: O(steps × work_per_step), Space: O(state_size)

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

## 5–10. Key Principles

**Key principles for simulation problems:**

1. **Separate read and write**: Don't modify the state you're reading from. Use a copy or encode both states.
2. **Boundary checking**: Always validate indices before accessing neighbors.
3. **State encoding**: Game of Life trick — encode current and next state in same cell using bit manipulation.
4. **Early termination**: If state repeats (cycle detection), you can skip ahead.

**Common Mistakes:**

| Mistake | How to Avoid |
|---|---|
| Modifying state while iterating | Use a copy or double-buffering |
| Missing boundary checks | Always check `r >= 0 && r < rows && c >= 0 && c < cols` |
| Off-by-one in step counting | Clarify: does step 1 mean after 1 update or the initial state? |
| Not detecting cycles (for large step counts) | If state space is finite, cycles exist — detect with hash set |

---

## 11. When NOT to Use This Pattern

| Situation | Why Not | Use Instead |
|---|---|---|
| Step count is very large (10⁹) | Direct simulation is too slow | Math / cycle detection / matrix exponentiation |
| There's a known formula | Simulation is needlessly slow | Direct computation |
| The process has a greedy or DP structure | More efficient approaches exist | Greedy / DP |

---

## 12–19. Example, Practice

**Example**: LeetCode #289 — Game of Life.

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #657 — Robot Return to Origin | Easy | Simple movement simulation |
| 2 | LeetCode #289 — Game of Life | Medium | Classic grid simulation with state encoding |
| 3 | LeetCode #874 — Walking Robot Simulation | Medium | Obstacle-aware movement |
| 4 | LeetCode #68 — Text Justification | Hard | String formatting simulation |
| 5 | LeetCode #65 — Valid Number | Hard | State machine parsing |

**Memory Trick:**

> 🧠 **"Be the Computer"** — In simulation problems, YOU are the CPU. Read the instructions, execute them exactly, track the state, and don't optimize what doesn't need optimizing. The challenge is accuracy, not cleverness.

---
---

*Previous chapter: [08 — Bit Manipulation](08_Bit_Manipulation.md)*
*Next chapter: [10 — Recursion & Backtracking](10_Recursion_and_Backtracking.md)*
