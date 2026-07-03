<!--
═══════════════════════════════════════════════════════════════════════════════
  DSA PATTERN HANDBOOK — TREES
  Chapter 13
  
  Patterns in this chapter:
    Pattern 37: Binary Tree DFS
    Pattern 38: Binary Tree BFS
    Pattern 39: BST Problems
    Pattern 40: Lowest Common Ancestor (LCA)
    Pattern 41: Trie (Prefix Tree)
  
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

# Chapter 13: Trees

Trees are the single most tested topic in interviews. They appear in nearly every OA and onsite round. This chapter covers five essential tree patterns that form the foundation for solving tree problems.

**Key definitions:**
- **Binary Tree**: Each node has at most 2 children (left, right).
- **BST (Binary Search Tree)**: Left subtree < node < right subtree.
- **Height/Depth**: Height = longest root-to-leaf path. Depth = distance from root.
- **Balanced**: Height difference of left and right subtrees ≤ 1.

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
═══════════════════════════════════════════════════════════════
  BINARY TREE DFS
  OA Frequency: ★★★★★
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Samsung | Flipkart
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Compute properties of a binary tree by visiting every node using **depth-first traversal**." DFS on trees comes in three flavors (preorder, inorder, postorder), each suited for different problems.

**Why was this pattern invented?**

Trees are recursive structures — each subtree is itself a tree. DFS leverages this by solving the problem for left and right subtrees, then combining results. This naturally maps to recursive code.

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

Preorder  (Root-Left-Right): 1, 2, 4, 5, 3    → "Process root BEFORE children"
Inorder   (Left-Root-Right): 4, 2, 5, 1, 3    → "Process root BETWEEN children"
Postorder (Left-Right-Root): 4, 5, 2, 3, 1    → "Process root AFTER children"
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
// Template A: Bottom-Up (Postorder) — return info from subtrees
// Example: Maximum Depth

int maxDepth(TreeNode* root) {
    if (!root) return 0;                        // base case
    int leftDepth = maxDepth(root->left);       // solve left
    int rightDepth = maxDepth(root->right);     // solve right
    return 1 + max(leftDepth, rightDepth);      // combine
}
```

```cpp
// Template B: Top-Down (Preorder) — pass info from parent
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
// Template C: Global Variable Pattern — track answer across recursive calls
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

Answer: diameter = 3 (path: 4→2→1→3 or 5→2→1→3) ✓
```

---

## 9. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Confusing height (edges) vs depth (nodes) | LeetCode uses nodes: height of single node = 1, not 0 |
| Not handling null nodes | Always check `if (!root) return base_value;` |
| Diameter = 2 × height (wrong) | Diameter passes THROUGH a node: left_height + right_height |
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
| 1 | LeetCode #104 — Maximum Depth of Binary Tree | Easy | Basic bottom-up |
| 2 | LeetCode #112 — Path Sum | Easy | Top-down |
| 3 | LeetCode #543 — Diameter of Binary Tree | Easy | Global variable pattern |
| 4 | LeetCode #236 — Lowest Common Ancestor | Medium | Bottom-up with condition |
| 5 | LeetCode #124 — Binary Tree Maximum Path Sum | Hard | Global variable + complex combine |

**Memory Trick:**

> 🧠 **"Ask Your Children"**
>
> In postorder (bottom-up): "Hey left child, what's your height? Hey right child, what's your height? OK, I'm 1 + max of you two."
>
> In preorder (top-down): "Parent says the remaining sum is 7. I'm 3, so I tell my children the remaining sum is 4."

---
---

# Pattern 38: Binary Tree BFS (Level-Order Traversal)

```
═══════════════════════════════════════════════════════════════
  BINARY TREE BFS
  OA Frequency: ★★★★☆
  Companies: Amazon | Google | Microsoft | Meta | Adobe
═══════════════════════════════════════════════════════════════
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
// Template: Binary Tree BFS — Level-Order Traversal
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
Level 0: size=1 → process 3. Push 9, 20.
  result: [[3]]

Queue: [9, 20]
Level 1: size=2 → process 9 (no children), 20 (push 15, 7).
  result: [[3], [9, 20]]

Queue: [15, 7]
Level 2: size=2 → process 15, 7 (no children).
  result: [[3], [9, 20], [15, 7]]

Queue: empty → done ✓
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
| 1 | LeetCode #102 — Binary Tree Level Order Traversal | Medium | Classic BFS |
| 2 | LeetCode #199 — Binary Tree Right Side View | Medium | Last node per level |
| 3 | LeetCode #103 — Binary Tree Zigzag Level Order | Medium | Alternate direction |
| 4 | LeetCode #111 — Minimum Depth of Binary Tree | Easy | BFS finds min depth |
| 5 | LeetCode #662 — Maximum Width of Binary Tree | Medium | Track positions |

**Memory Trick:**

> 🧠 **"Floor by Floor"** — BFS processes a tree like reading a building floor by floor, starting from the ground floor (root). You finish everyone on floor 1 before moving to floor 2. The trick is counting how many nodes are on the current floor (`levelSize = q.size()`).

---
---

# Pattern 39: BST (Binary Search Tree) Problems

```
═══════════════════════════════════════════════════════════════
  BST PROBLEMS
  OA Frequency: ★★★★☆
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Goldman Sachs
═══════════════════════════════════════════════════════════════
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

Inorder: 2, 3, 4, 5, 6, 7, 8  → sorted! ✓
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
| Assuming BST is always balanced | BST can be skewed — height can be O(n) |
| Inorder without stack in iterative version | Morris Traversal exists but is complex; stack is fine for interviews |

---

## 7. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #700 — Search in a BST | Easy | Basic BST search |
| 2 | LeetCode #98 — Validate Binary Search Tree | Medium | Range validation |
| 3 | LeetCode #230 — Kth Smallest Element in BST | Medium | Inorder traversal |
| 4 | LeetCode #108 — Convert Sorted Array to BST | Easy | Divide & conquer |
| 5 | LeetCode #99 — Recover Binary Search Tree | Medium | Two swapped nodes |

**Memory Trick:**

> 🧠 **"The Sorted Filing Cabinet"** — A BST is like a filing cabinet where every drawer's label is greater than all labels to its left and less than all to its right. Inorder traversal reads labels left to right — giving you sorted order. Validation checks: "Is every label in its allowed range?"

---
---

# Pattern 40: Lowest Common Ancestor (LCA)

```
═══════════════════════════════════════════════════════════════
  LOWEST COMMON ANCESTOR
  OA Frequency: ★★★★☆
  Companies: Amazon | Google | Microsoft | Meta | Adobe
═══════════════════════════════════════════════════════════════
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

    if (left && right) return root;  // p and q are in different subtrees → root is LCA
    return left ? left : right;      // both are in the same subtree
}
```

**Why this works:**
- If the current node IS p or q, return it.
- Recurse left and right.
- If both return non-null, the current node is where p and q "meet" — it's the LCA.
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
            return root;          // split point — this is the LCA
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
  Both non-null → return 3. ✓

LCA(5, 4):
  root=3: left returns 5 (contains both 5 and 4), right returns null.
  Return 5. ✓ (5 is ancestor of 4)

LCA(6, 4):
  root=3→left→root=5: left returns 6 (found), right→root=2→left→7(null),right→4(found).
  right returns 2 (found 4 through 2's subtree? No — actually 4 is child of 2).
  Wait, let's trace carefully:
  
  root=3: recurse left(5) and right(1)
    root=5: recurse left(6) and right(2)
      root=6: not p or q, no children → null, null → return null.
      Actually 6 IS p → return 6. ✓
      root=2: recurse left(7) and right(4)
        root=7: not p or q → return null
        root=4: IS q → return 4
      left=null, right=4 → return 4
    left=6 (non-null), right=4 (non-null) → BOTH found → return 5 ✓
    
LCA(6, 4) = 5 ✓
```

---

## 6. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #235 — LCA of a BST | Medium | BST-specific O(h) solution |
| 2 | LeetCode #236 — LCA of a Binary Tree | Medium | General binary tree |
| 3 | LeetCode #1644 — LCA of Binary Tree II | Medium | Nodes may not exist |
| 4 | LeetCode #1650 — LCA of Binary Tree III | Medium | With parent pointers |
| 5 | LeetCode #1123 — LCA of Deepest Leaves | Medium | LCA variation |

**Memory Trick:**

> 🧠 **"The Family Reunion"** — Two relatives (p, q) are looking for their closest common ancestor. Start from the family patriarch (root). If p and q are on different sides of the family tree, the current person is the meeting point (LCA). If both are on the same side, go deeper into that branch.

---
---

# Pattern 41: Trie (Prefix Tree)

```
═══════════════════════════════════════════════════════════════
  TRIE (PREFIX TREE)
  OA Frequency: ★★★★☆
  Companies: Google | Amazon | Microsoft | Meta | Adobe | Uber
═══════════════════════════════════════════════════════════════
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
| Total space for n words | — | O(n × L) worst, often much less due to shared prefixes |

---

## 6. Dry Run

**Insert**: "apple", "app", "apricot", "bat"

```
root
├── 'a'
│   └── 'p'
│       ├── 'p' (prefixCount=2)
│       │   ├── 'l'
│       │   │   └── 'e' [isEnd=true]  → "apple"
│       │   └── [isEnd=true]          → "app"
│       └── 'r'
│           └── 'i'
│               └── 'c'
│                   └── 'o'
│                       └── 't' [isEnd=true]  → "apricot"
└── 'b'
    └── 'a'
        └── 't' [isEnd=true]  → "bat"

search("app") → traverse a→p→p, isEnd=true → true ✓
search("ap") → traverse a→p, isEnd=false → false ✓
startsWith("ap") → traverse a→p, node exists → true ✓
startsWith("ba") → traverse b→a, node exists → true ✓
startsWith("ca") → 'c' not in root → false ✓
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
| 1 | LeetCode #208 — Implement Trie | Medium | Core implementation |
| 2 | LeetCode #211 — Design Add and Search Words | Medium | Trie with wildcard DFS |
| 3 | LeetCode #648 — Replace Words | Medium | Prefix replacement |
| 4 | LeetCode #212 — Word Search II | Hard | Trie + backtracking on grid |
| 5 | LeetCode #421 — Maximum XOR of Two Numbers | Medium | Bitwise Trie |

**Memory Trick:**

> 🧠 **"The Autocomplete Tree"** — A Trie works like your phone's autocomplete. Each letter you type follows a branch down the tree. All words sharing the same prefix share the same path. When you type "app", the Trie shows you the branch and all its descendants: "apple", "application", "appreciate".

---
---

*Previous chapter: [12 — Advanced Graphs](12_Advanced_Graphs.md)*
*Next chapter: [14 — Dynamic Programming](14_Dynamic_Programming.md)*
