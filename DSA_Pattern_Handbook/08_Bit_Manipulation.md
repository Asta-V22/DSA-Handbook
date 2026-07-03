<!--
═══════════════════════════════════════════════════════════════════════════════
  DSA PATTERN HANDBOOK — BIT MANIPULATION
  Chapter 08
  
  Patterns in this chapter:
    Pattern 23: Bit Manipulation
  
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

# Chapter 8: Bit Manipulation

Bit manipulation uses low-level binary operations to solve problems with extreme efficiency — O(1) per operation, no extra space. While it may seem intimidating, the number of bit tricks needed for interviews is small and learnable.

---
---

# Pattern 23: Bit Manipulation

```
═══════════════════════════════════════════════════════════════
  BIT MANIPULATION
  OA Frequency: ★★★☆☆
  Companies: Amazon | Google | Microsoft | Apple | Adobe | Samsung
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Problems involving **binary representations, subsets, power of 2, XOR properties, or bitwise flags**. Bits let you encode, manipulate, and query information at the hardware level — making operations constant time and zero space.

**Why was this pattern invented?**

Every integer is stored as bits. Operations like AND, OR, XOR, and shifts map directly to CPU instructions — they're the fastest operations a computer can perform. Certain mathematical properties (XOR cancellation, power-of-two check, subset enumeration) have elegant bitwise solutions.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem mention **binary representation**, bits, or powers of 2?
- [ ] Does it ask to find a **single/unique element** among duplicates?
- [ ] Does it involve **subset enumeration** (n ≤ 20)?
- [ ] Can the problem benefit from **XOR properties** (a ⊕ a = 0, a ⊕ 0 = a)?
- [ ] Does it ask for counting **set bits** or toggling specific bits?

---

## 3. Recognition Keywords

| Keyword / Phrase | Why It Points Here |
|---|---|
| "Single number" / "element appearing once" | XOR all elements |
| "Power of 2" | `n & (n-1) == 0` |
| "Count set bits" / "Hamming weight" | Brian Kernighan's algorithm |
| "Hamming distance" | XOR + count bits |
| "Subset enumeration" (n ≤ 20) | Bitmask iteration |
| "Missing number" | XOR of indices and values |
| "Reverse bits" | Bit-by-bit reversal |
| "Binary representation" | Direct bit operations |

---

## 4. Constraint Analysis

| Constraint | Why Bits Work |
|---|---|
| n ≤ 20 | Bitmask 2²⁰ ≈ 10⁶ — feasible for subset enumeration |
| Need O(1) space for uniqueness | XOR achieves this |
| Integer operations | Bitwise ops are O(1) |

---

## 5. Evolution of Thought

**Example: Find the single non-duplicate element**

### Step 1: Brute Force
> "For each element, count its occurrences." → O(n²) or O(n) with hash map.

### Step 2: Key Observation
> "XOR has magical properties: `a ⊕ a = 0` and `a ⊕ 0 = a`. XOR-ing all elements cancels out duplicates."

### Step 3: Optimal
> "XOR all elements: duplicates cancel, leaving the unique one." → O(n) time, O(1) space.

---

## 6. Essential Bit Operations Reference

```
Operation           C++ Syntax          Effect
─────────────────────────────────────────────────────────
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

// XOR all elements — find single unique
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
| Enumerate all subsets (bitmask) | O(2ⁿ) | O(1) per subset |

---

## 9. Dry Run

**Problem**: Find single number in `[4, 1, 2, 1, 2]`.

```
XOR all elements:
  result = 0
  result ^= 4 = 4         (binary: 100)
  result ^= 1 = 5         (binary: 101)
  result ^= 2 = 7         (binary: 111)
  result ^= 1 = 6         (binary: 110)  ← 1 cancelled
  result ^= 2 = 4         (binary: 100)  ← 2 cancelled

Answer: 4 ✓

Why? 1 ⊕ 1 = 0, 2 ⊕ 2 = 0, leaving only 4.
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
| n > 20 for subset enumeration | 2²⁰ ≈ 10⁶ is the practical limit | DP, greedy |
| Need to store more than 64 items in a bitmask | `long long` has 64 bits | Use `bitset<N>` or hash set |
| Problem has no binary/XOR structure | Bit tricks only help when the problem is "binary" | Other patterns |

---

## 12. Medium-Level Example Problem

**LeetCode #260 — Single Number III**

> Given an array where exactly two elements appear once and all others appear twice, find those two elements.

**Why this represents the pattern well**: It extends the XOR trick. XOR-ing everything gives `a ⊕ b`. To separate `a` and `b`, find a set bit in `a ⊕ b` (where they differ), then partition elements by that bit.

---

## 13. Solution Walkthrough

1. "XOR all elements → get `xorAll = a ⊕ b`."
2. "Find any set bit in `xorAll` — this bit is 1 in `a` and 0 in `b` (or vice versa)."
3. "Partition all elements into two groups by this bit."
4. "XOR each group separately → duplicates cancel, leaving `a` and `b`."

---

## 14. C++ Implementation

```cpp
#include <vector>
using namespace std;

// Single Number III — two unique numbers
// Pattern: Bit Manipulation (XOR + partition)
// Time: O(n), Space: O(1)
vector<int> singleNumberIII(vector<int>& nums) {
    // Step 1: XOR all → get a ^ b
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
| "What if THREE elements appear once?" | Much harder — use modular counting of bits (count each bit position mod 3) |
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
| **Hashing** | Alternative for "find unique" — uses O(n) space vs O(1) for XOR |
| **State Compression DP** | Uses bitmasks to represent DP states |
| **Backtracking** | Bitmasks can replace backtracking for subset enumeration when n ≤ 20 |

---

## 18. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #136 — Single Number | Easy | XOR all |
| 2 | LeetCode #191 — Number of 1 Bits | Easy | Kernighan's algorithm |
| 3 | LeetCode #260 — Single Number III | Medium | XOR + partition |
| 4 | LeetCode #338 — Counting Bits | Medium | DP with bits |
| 5 | LeetCode #137 — Single Number II | Medium | Bit counting mod 3 |

---

## 19. Memory Trick

> 🧠 **"XOR = The Toggle Switch"**
>
> XOR is like a toggle switch: flip it once (a ⊕ b) to turn ON, flip it again (result ⊕ b) to turn OFF — back to `a`. Duplicates toggle twice, cancelling out. Only the unique element remains.
>
> **"Power of 2 = Lonely 1"** — A power of 2 has exactly one bit set. `n & (n-1)` removes it, leaving 0. If anything remains, it's not a power of 2.

---
---

*Previous chapter: [07 — Heap & Intervals](07_Heap_and_Intervals.md)*
*Next chapter: [09 — Matrix & Simulation](09_Matrix_and_Simulation.md)*
