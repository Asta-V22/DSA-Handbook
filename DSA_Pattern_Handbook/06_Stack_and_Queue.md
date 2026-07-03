<!--
═══════════════════════════════════════════════════════════════════════════════
  DSA PATTERN HANDBOOK — STACK & QUEUE
  Chapter 06
  
  Patterns in this chapter:
    Pattern 14: Stack
    Pattern 15: Queue
    Pattern 16: Deque
    Pattern 17: Monotonic Stack
    Pattern 18: Monotonic Queue
  
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

# Chapter 6: Stack & Queue

Stacks and queues are fundamental data structures that support a specific access pattern. Their power in interviews comes from their constrained nature — by restricting how elements are accessed, they enable elegant O(n) solutions to problems that seem to require O(n²).

**Key insight**: A stack processes elements in **last-in, first-out** (LIFO) order — perfect for matching, nesting, and "nearest previous" problems. A queue processes in **first-in, first-out** (FIFO) order — perfect for level-order processing and BFS.

The **monotonic** variants (monotonic stack and monotonic queue) maintain elements in sorted order, enabling powerful range queries in O(n).

---
---

# Pattern 14: Stack

```
═══════════════════════════════════════════════════════════════
  STACK
  OA Frequency: ★★★★★
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Goldman Sachs
═══════════════════════════════════════════════════════════════
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

**If you checked 2+ of these → Stack.**

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
| n ≤ 10⁶ | O(n) ✅ | Each element pushed/popped at most once |
| Nested depth ≤ 10⁴ | O(depth) space ✅ | Stack depth = nesting depth |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each closing bracket, scan backward to find its matching opening bracket." → O(n²).

### Step 2: Key Observation
> "The most recent unmatched opening bracket is always the one that should match the current closing bracket. That's LIFO — a stack!"

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
// Template: Stack — Bracket / Pair Matching
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
// Template: Stack — Expression Evaluation
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

**Problem**: Valid Parentheses — `"({[]})"`.

```
String: ( { [ ] } )
Stack: []

char '(' → push → Stack: ['(']
char '{' → push → Stack: ['(', '{']
char '[' → push → Stack: ['(', '{', '[']
char ']' → matches top '[' → pop → Stack: ['(', '{']
char '}' → matches top '{' → pop → Stack: ['(']
char ')' → matches top '(' → pop → Stack: []

Stack empty → return true ✓
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

**LeetCode #394 — Decode String**

> Given an encoded string like `"3[a2[c]]"`, return the decoded string: `"accaccacc"`.

**Why this represents the pattern well**: The nested brackets create a natural stack problem. Each `[` pushes the current state, and each `]` pops and combines.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. "Nested structure → Stack."
2. "When I see a `[`, I save the current string and multiplier, then start fresh."
3. "When I see a `]`, I pop the saved state, repeat the current string, and append."
4. "I need two stacks or a stack of pairs: (previous_string, multiplier)."

---

## 14. C++ Implementation

```cpp
#include <string>
#include <stack>
using namespace std;

// Decode String: "3[a2[c]]" → "accaccacc"
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
| **Queue** | FIFO vs LIFO — different processing order |
| **Recursion** | Recursion uses the call stack implicitly; iterative stack solutions mirror recursion |
| **DFS** | DFS uses a stack (implicitly or explicitly) |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #20 — Valid Parentheses | Easy | Classic bracket matching |
| 2 | LeetCode #155 — Min Stack | Medium | Stack with auxiliary tracking |
| 3 | LeetCode #394 — Decode String | Medium | Nested structure processing |
| 4 | LeetCode #227 — Basic Calculator II | Medium | Expression evaluation |
| 5 | LeetCode #84 — Largest Rectangle in Histogram | Hard | Stack-based area calculation |

---

## 19. Memory Trick

> 🧠 **"Stack of Plates"**
>
> A stack is like a stack of plates: you always add to the top and remove from the top. When you encounter something that "opens" (a bracket, a function call, a new nesting level), put a plate on top. When you encounter something that "closes," take the top plate off and check if it matches.

---
---

# Pattern 15: Queue

```
═══════════════════════════════════════════════════════════════
  QUEUE
  OA Frequency: ★★★★☆
  Companies: Amazon | Google | Microsoft | Meta
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Problems involving **first-in, first-out (FIFO) processing**. When elements must be processed in the order they arrive — like a line at a grocery store — a queue is the natural choice.

**Why was this pattern invented?**

Queues model real-world fairness: "whoever arrived first gets served first." In algorithms, queues are the backbone of **BFS** (breadth-first search) — processing nodes level by level.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem require processing elements **in arrival order**?
- [ ] Is there a **level-order** or **breadth-first** traversal?
- [ ] Does the problem involve a **simulation** with a line/queue?
- [ ] Do elements wait for their turn to be processed?

**If you checked 2+ of these → Queue.**

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

Queues are O(1) per operation (push, pop, front). They're rarely the bottleneck — the algorithm using the queue determines complexity.

---

## 5. Evolution of Thought

Queue is a data structure, not an algorithmic pattern per se. The evolution is:

1. "I need to process elements in the order they arrive."
2. "I need to explore a graph/tree level by level."
3. "I need FIFO behavior." → Use a queue.

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
// Template: Queue — BFS / Level-order Processing
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
Adjacency: 0→[1,2], 1→[3], 2→[3,4], 3→[], 4→[]

Queue: [0]  Visited: {0}
  Pop 0 → process → push 1, 2
Queue: [1, 2]  Visited: {0, 1, 2}
  Pop 1 → process → push 3
Queue: [2, 3]  Visited: {0, 1, 2, 3}
  Pop 2 → process → 3 already visited, push 4
Queue: [3, 4]  Visited: {0, 1, 2, 3, 4}
  Pop 3 → process → no new neighbors
  Pop 4 → process → no new neighbors
Queue: empty → done

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

**LeetCode #933 — Number of Recent Calls**

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

## 15–19. (Abbreviated for Queue — core usage is covered in BFS chapters)

Queue is primarily a building block for BFS (Chapter 11), Multi-Source BFS (Chapter 11), and Tree BFS (Chapter 13). See those chapters for full treatment.

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #933 — Number of Recent Calls | Easy | Queue as sliding window |
| 2 | LeetCode #622 — Design Circular Queue | Medium | Queue implementation |
| 3 | LeetCode #649 — Dota2 Senate | Medium | Queue simulation |
| 4 | LeetCode #950 — Reveal Cards in Increasing Order | Medium | Queue simulation |
| 5 | LeetCode #239 — Sliding Window Maximum | Hard | Queue/Deque application |

**Memory Trick:**

> 🧠 **"The Grocery Line"** — Queue = grocery store line. First person in line gets served first. No cutting. No going backward. Fair and orderly.

---
---

# Pattern 16: Deque

```
═══════════════════════════════════════════════════════════════
  DEQUE (DOUBLE-ENDED QUEUE)
  OA Frequency: ★★★☆☆
  Companies: Google | Amazon | Microsoft | Meta
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

When you need **efficient insertion and removal from BOTH ends** — something neither a stack (back only) nor a queue (front remove, back insert) can do alone. Deque = Stack + Queue in one structure.

**Why was this pattern invented?**

Problems like "Sliding Window Maximum" require adding new elements at one end and removing old elements from the other, while also removing elements from the same end when they become irrelevant. A deque supports all these operations in O(1).

---

## 2. Pattern Identification Checklist

- [ ] Do you need to add/remove elements from **both ends** efficiently?
- [ ] Is the problem a **sliding window** variant that needs more than just sum tracking?
- [ ] Do you need a structure that acts as both a stack and a queue?

**If you checked any of these → Consider Deque.**

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
| Push front | O(1) ✅ | O(n) ❌ | ❌ | ❌ |
| Push back | O(1) ✅ | O(1) ✅ | O(1) ✅ | O(1) ✅ |
| Pop front | O(1) ✅ | O(n) ❌ | O(1) ✅ | ❌ |
| Pop back | O(1) ✅ | O(1) ✅ | ❌ | O(1) ✅ |
| Access front | O(1) ✅ | O(1) ✅ | O(1) ✅ | ❌ |
| Access back | O(1) ✅ | O(1) ✅ | ❌ | O(1) ✅ |
| Random access | O(1) ✅ | O(1) ✅ | ❌ | ❌ |

---

## 5–7. Generic Algorithm and Template

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

## 8–19. (Deque's full power is demonstrated in Pattern 18: Monotonic Queue)

Deque as a standalone pattern is primarily a data structure enabling the Monotonic Queue pattern and 0-1 BFS. See Pattern 18 below for the full treatment.

**Practice Problems:**

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #641 — Design Circular Deque | Medium | Deque implementation |
| 2 | LeetCode #239 — Sliding Window Maximum | Hard | Monotonic deque |
| 3 | LeetCode #862 — Shortest Subarray with Sum ≥ K | Hard | Monotonic deque with prefix sums |

**Memory Trick:**

> 🧠 **"The Double Door"** — A deque is a hallway with doors on both ends. People can enter and exit from either door. A stack has only one door (back). A queue has an entrance (back) and an exit (front) but you can't go the other way. The deque has no restrictions.

---
---

# Pattern 17: Monotonic Stack

```
═══════════════════════════════════════════════════════════════
  MONOTONIC STACK
  OA Frequency: ★★★★☆
  Companies: Amazon | Google | Microsoft | Meta | Adobe | Uber
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"For each element, find the **nearest greater** (or nearest smaller) element to its left or right."

Brute force: For each element, scan left/right → O(n²). Monotonic stack: Process all elements in O(n) by maintaining a stack of elements in **increasing or decreasing order**.

**Why was this pattern invented?**

When you look at element `arr[i]`, the "nearest greater element to the left" is somewhere in `arr[0..i-1]`. But you don't need to check all of them. If `arr[j] ≤ arr[k]` and `j < k`, then `arr[j]` will never be the answer for any element to the right of `k` — `arr[k]` "shadows" `arr[j]`. The monotonic stack maintains only the unshadowed elements, processing each element exactly once.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem ask for the **next/previous greater/smaller** element?
- [ ] Does it involve **spans** (how far back can you go while maintaining a property)?
- [ ] Does the problem mention **stock span**, **daily temperatures**, or **histogram**?
- [ ] Can the problem be modeled as "for each element, find the nearest element satisfying a condition"?
- [ ] Is there an O(n²) brute force where the inner loop searches for a boundary?

**If you checked 2+ of these → Monotonic Stack.**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Next greater element" | Direct application |
| "Previous smaller element" | Direct application |
| "Daily temperatures" / "days until warmer" | Next greater element in temperature |
| "Stock span" | How many consecutive days price was ≤ today |
| "Largest rectangle in histogram" | Stack-based boundary finding |
| "Trapping rain water" (stack approach) | Find bounding walls |
| "Remove k digits to make smallest number" | Greedy removal with monotonic stack |

---

## 4. Constraint Analysis

| Constraint | Brute Force O(n²) | Monotonic Stack O(n) | Verdict |
|---|---|---|---|
| n ≤ 5000 | 2.5 × 10⁷ ⚠️ | 5000 ✅ | Stack preferred |
| n ≤ 10⁵ | 10¹⁰ ❌ | 10⁵ ✅ | Stack required |
| n ≤ 10⁶ | 10¹² ❌ | 10⁶ ✅ | Stack required |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each element i, scan right to find the first element greater than arr[i]." → O(n²).

### Step 2: Key Observation
> "When I find that arr[j] > arr[i], then arr[j] is the answer for arr[i]. But it might also be the answer for elements between i and j that are smaller than arr[j]."

```
Array: [4, 2, 1, 5, 3]

For arr[0]=4: scan right → arr[3]=5 is next greater.
For arr[1]=2: scan right → arr[3]=5 is next greater.
For arr[2]=1: scan right → arr[3]=5 is next greater.

When we reach 5, it "answers" all three pending elements at once!
```

### Step 3: Optimization
> "Maintain a stack of elements waiting for their 'next greater.' When a new element is larger than the stack's top, pop and record the answer. Repeat until the stack's top is larger or empty."

### Step 4: Optimal Solution
> O(n) — each element is pushed once and popped at most once. Total operations = 2n.

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
| Next Greater | Decreasing | `arr[top] < arr[i]` | Left → Right |
| Next Smaller | Increasing | `arr[top] > arr[i]` | Left → Right |
| Previous Greater | Decreasing | `arr[top] ≤ arr[i]` | Left → Right (store before push) |
| Previous Smaller | Increasing | `arr[top] ≥ arr[i]` | Left → Right (store before push) |

---

## 7. Reusable C++ Template

```cpp
// Template: Monotonic Stack — Next Greater Element
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

**Amortized argument**: Total pushes = n. Total pops ≤ n (can't pop more than pushed). Total = 2n = O(n).

---

## 9. Dry Run

**Problem**: Next Greater Element for `[4, 2, 1, 5, 3]`.

```
Array: [4, 2, 1, 5, 3]
Stack: [] (stores indices)

i=0: arr[0]=4
  Stack empty → push 0
  Stack: [0(4)]

i=1: arr[1]=2
  2 < 4 → don't pop → push 1
  Stack: [0(4), 1(2)]

i=2: arr[2]=1
  1 < 2 → don't pop → push 2
  Stack: [0(4), 1(2), 2(1)]

i=3: arr[3]=5
  5 > 1 → pop 2, result[2] = 5
  5 > 2 → pop 1, result[1] = 5
  5 > 4 → pop 0, result[0] = 5
  Stack empty → push 3
  Stack: [3(5)]

i=4: arr[4]=3
  3 < 5 → don't pop → push 4
  Stack: [3(5), 4(3)]

Remaining in stack: indices 3, 4 → no next greater → result stays -1

Result: [5, 5, 5, -1, -1]

Verify:
  arr[0]=4 → next greater = 5 ✓
  arr[1]=2 → next greater = 5 ✓
  arr[2]=1 → next greater = 5 ✓
  arr[3]=5 → no next greater ✓
  arr[4]=3 → no next greater ✓
```

---

## 10. Common Mistakes

| Mistake | How to Avoid |
|---|---|
| Storing values instead of indices | Usually store indices — you can always get values from indices |
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

**LeetCode #739 — Daily Temperatures**

> Given daily temperatures, for each day find how many days you must wait until a warmer temperature. If no warmer day exists, return 0.

**Why this represents the pattern well**: It's "Next Greater Element" but returning the distance instead of the value. Perfectly demonstrates the monotonic stack's pop-and-record mechanism.

---

## 13. Solution Walkthrough

**Thought process during an interview:**

1. "For each day, find the next warmer day → Next Greater Element."
2. "Instead of recording the value, record the distance: `i - popped_index`."
3. "Stack stores indices of days waiting for a warmer day."
4. "When a warmer day arrives, pop all colder days and calculate wait times."

---

## 14. C++ Implementation

```cpp
#include <vector>
#include <stack>
using namespace std;

// Daily Temperatures — days until warmer temperature
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
| 1 | LeetCode #496 — Next Greater Element I | Easy | Basic next greater with hash map |
| 2 | LeetCode #739 — Daily Temperatures | Medium | Next greater with distance |
| 3 | LeetCode #503 — Next Greater Element II | Medium | Circular array variant |
| 4 | LeetCode #84 — Largest Rectangle in Histogram | Hard | Left/right boundaries |
| 5 | LeetCode #907 — Sum of Subarray Minimums | Hard | Both previous and next smaller |

---

## 19. Memory Trick

> 🧠 **"The Domino Effect"**
>
> Picture a row of dominoes of different heights. When a tall domino (new element) arrives, it knocks down all shorter dominoes in front of it (pop from stack). The tall domino is the "next greater" for all the short ones it knocked over. Dominoes that were too tall to be knocked down stay standing (remain in stack).

---
---

# Pattern 18: Monotonic Queue

```
═══════════════════════════════════════════════════════════════
  MONOTONIC QUEUE (MONOTONIC DEQUE)
  OA Frequency: ★★★☆☆
  Companies: Google | Amazon | Microsoft | Uber
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find the **maximum (or minimum) element in every sliding window** of size k." More generally: maintain access to the extreme value in a dynamic range.

Brute force: For each window, scan k elements → O(n·k).
Monotonic deque: Maintain elements in decreasing order. The front of the deque is always the maximum. → O(n).

**Why was this pattern invented?**

It's the sliding-window version of the monotonic stack. The stack can find "next greater" but can't handle elements leaving the window from the left. The **deque** supports removal from both ends: elements leave from the front (when they exit the window) and from the back (when they're dominated by a new element).

---

## 2. Pattern Identification Checklist

- [ ] Does the problem ask for **max or min in a sliding window**?
- [ ] Is there a **fixed-size window** moving across the array and you need an extreme value?
- [ ] Do you need to maintain a "best candidate" that can expire?
- [ ] Does the problem involve a dynamic range query where elements enter and exit from different ends?

**If you checked 2+ of these → Monotonic Queue (Deque).**

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Maximum in each window of size k" | Direct application |
| "Sliding window maximum/minimum" | Classic monotonic deque problem |
| "Shortest subarray with sum ≥ k" (with negatives) | Monotonic deque + prefix sums |
| "Max in subarray of bounded length" | Deque tracks candidates |

---

## 4. Constraint Analysis

| Constraint | Brute O(n·k) | Heap O(n log n) | Mono Deque O(n) | Verdict |
|---|---|---|---|---|
| n ≤ 10⁴, k ≤ 100 | 10⁶ ✅ | 10⁴ × 14 ✅ | 10⁴ ✅ | All work |
| n ≤ 10⁵, k ≤ 10⁵ | 10¹⁰ ❌ | 10⁵ × 17 ✅ | 10⁵ ✅ | Heap or deque |
| n ≤ 10⁶ | Impossible | 10⁶ × 20 ⚠️ | 10⁶ ✅ | Deque preferred |

---

## 5. Evolution of Thought

### Step 1: Brute Force
> "For each window, scan all k elements to find the max." → O(n·k).

### Step 2: Optimization Attempt (Heap)
> "Use a max-heap. Add entering elements, remove leaving elements." → But heap removal is O(n) unless using lazy deletion → O(n log n) with lazy.

### Step 3: Key Observation
> "In the window [l, r], if `arr[i] ≤ arr[j]` and `i < j`, then `arr[i]` can never be the max while `arr[j]` is in the window. We can discard `arr[i]`."

### Step 4: Monotonic Deque
> "Maintain a deque of indices in decreasing order of their values. When a new element enters:
> 1. Remove from the back all elements ≤ new (they're dominated).
> 2. Add new to back.
> 3. Remove from front if the front index is outside the window.
> 4. The front is always the maximum." → O(n).

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
        
        // Remove elements from back that are ≤ arr[i] (dominated)
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
// Template: Monotonic Deque — Sliding Window Maximum
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
  Back empty → push 0. Deque: [0(1)]
  Window not full yet.

i=1: nums[1]=3
  nums[back=0]=1 ≤ 3 → pop_back. Deque: []
  Push 1. Deque: [1(3)]
  Window not full yet.

i=2: nums[2]=-1
  nums[back=1]=3 > -1 → don't pop.
  Push 2. Deque: [1(3), 2(-1)]
  Window [0,1,2] full → result: nums[front=1] = 3

i=3: nums[3]=-3
  nums[back=2]=-1 > -3 → don't pop.
  Push 3. Deque: [1(3), 2(-1), 3(-3)]
  Check front: 1 > 3-3=0 → front stays.
  result: nums[front=1] = 3

i=4: nums[4]=5
  nums[back=3]=-3 ≤ 5 → pop. Deque: [1(3), 2(-1)]
  nums[back=2]=-1 ≤ 5 → pop. Deque: [1(3)]
  nums[back=1]=3 ≤ 5 → pop. Deque: []
  Push 4. Deque: [4(5)]
  Check front: 4 > 4-3=1 → front stays.
  result: nums[front=4] = 5

i=5: nums[5]=3
  nums[back=4]=5 > 3 → don't pop.
  Push 5. Deque: [4(5), 5(3)]
  result: nums[front=4] = 5

i=6: nums[6]=6
  nums[back=5]=3 ≤ 6 → pop. Deque: [4(5)]
  nums[back=4]=5 ≤ 6 → pop. Deque: []
  Push 6. Deque: [6(6)]
  result: nums[front=6] = 6

i=7: nums[7]=7
  nums[back=6]=6 ≤ 7 → pop. Deque: []
  Push 7. Deque: [7(7)]
  result: nums[front=7] = 7

Result: [3, 3, 5, 5, 6, 7] ✓
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

**LeetCode #239 — Sliding Window Maximum**

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
| "What if k varies per window?" | More complex — consider segment tree |
| "What about both max AND min in each window?" | Two deques: one for max (decreasing), one for min (increasing) |
| "Shortest subarray with sum ≥ k (with negatives)?" | LeetCode #862. Monotonic deque on prefix sums |

---

## 16. Variations

| Variation | Key Change |
|---|---|
| **Sliding Window Minimum** | Maintain increasing deque instead of decreasing |
| **Jump Game VI** | LeetCode #1696. DP + monotonic deque for sliding max |
| **Shortest Subarray with Sum ≥ K** | LeetCode #862. Deque on prefix sums |
| **Longest Subarray with abs diff ≤ limit** | LeetCode #1438. Two deques (max + min) |

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
| 1 | LeetCode #239 — Sliding Window Maximum | Hard | Classic monotonic deque |
| 2 | LeetCode #1438 — Longest Continuous Subarray with Abs Diff ≤ Limit | Medium | Two deques |
| 3 | LeetCode #1696 — Jump Game VI | Medium | DP + monotonic deque |
| 4 | LeetCode #862 — Shortest Subarray with Sum ≥ K | Hard | Deque on prefix sums |
| 5 | LeetCode #1499 — Max Value of Equation | Hard | Deque optimization |

---

## 19. Memory Trick

> 🧠 **"The VIP Lounge"**
>
> The monotonic deque is like a VIP lounge with strict rules:
> 1. **New VIPs** (big numbers) kick out all lesser VIPs already inside (pop from back).
> 2. **Old VIPs** whose passes expire (index outside window) are removed from the front.
> 3. **The most important VIP** is always at the front — that's your maximum.
>
> Nobody mediocre survives in the VIP lounge.

---
---

*Previous chapter: [05 — Sorting & Greedy](05_Sorting_and_Greedy.md)*
*Next chapter: [07 — Heap & Intervals](07_Heap_and_Intervals.md)*
