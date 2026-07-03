<!--
═══════════════════════════════════════════════════════════════════════════════
  DSA PATTERN HANDBOOK — MISCELLANEOUS
  Chapter 17
  
  Patterns in this chapter:
    Pattern 55: Line Sweep (2D)
    Pattern 56: Coordinate Compression
    Pattern 57: Mathematical Observation
═══════════════════════════════════════════════════════════════════════════════
-->

# Chapter 17: Miscellaneous

This chapter covers patterns that don't fit neatly into other categories but appear frequently enough in OAs to warrant coverage.

---
---

# Pattern 55: Line Sweep (2D)

```
═══════════════════════════════════════════════════════════════
  LINE SWEEP (2D)
  OA Frequency: ★★☆☆☆
  Companies: Google | Amazon | Microsoft
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

While we covered the basic sweep line in Chapter 7 (intervals), the 2D line sweep extends this to rectangles and 2D regions. A vertical line sweeps left-to-right, maintaining an active set of vertical segments using a balanced BST or segment tree.

**Classic problem**: The Skyline Problem (LeetCode #218).

---

## 2. When to Use

- Rectangle union area
- Skyline computation
- 2D event processing
- Geometric problems with rectangles

---

## 3. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #218 — The Skyline Problem | Hard | Classic 2D sweep |
| 2 | LeetCode #850 — Rectangle Area II | Hard | Sweep + compressed coordinates |
| 3 | LeetCode #391 — Perfect Rectangle | Hard | Sweep line validation |

---
---

# Pattern 56: Coordinate Compression

```
═══════════════════════════════════════════════════════════════
  COORDINATE COMPRESSION
  OA Frequency: ★★☆☆☆
  Companies: Google | Amazon
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Work with coordinates/values up to 10⁹, but only use n distinct values (n ≤ 10⁵)." Coordinate compression maps sparse values to a dense range [0, n-1], enabling array-based techniques (BIT, counting sort) on large value ranges.

---

## 2. When to Use

- Values up to 10⁹ but only n ≤ 10⁵ distinct values
- Need BIT/Segment Tree indexed by values
- Counting inversions, rank queries

---

## 3. Reusable C++ Template

```cpp
// Template: Coordinate Compression
// Map large values to [0, n-1] preserving relative order

#include <vector>
#include <algorithm>
using namespace std;

vector<int> compress(vector<int>& arr) {
    vector<int> sorted_unique = arr;
    sort(sorted_unique.begin(), sorted_unique.end());
    sorted_unique.erase(unique(sorted_unique.begin(), sorted_unique.end()), 
                        sorted_unique.end());

    vector<int> compressed(arr.size());
    for (int i = 0; i < (int)arr.size(); i++) {
        compressed[i] = lower_bound(sorted_unique.begin(), sorted_unique.end(), 
                                     arr[i]) - sorted_unique.begin();
    }

    return compressed;  // values now in [0, distinct_count - 1]
}
```

---

## 4. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #315 — Count of Smaller Numbers After Self | Hard | BIT + coordinate compression |
| 2 | LeetCode #493 — Reverse Pairs | Hard | Merge sort or BIT + compression |

---
---

# Pattern 57: Mathematical Observation

```
═══════════════════════════════════════════════════════════════
  MATHEMATICAL OBSERVATION
  OA Frequency: ★★★☆☆
  Companies: All (especially Goldman Sachs, Google, Amazon)
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Some problems have no standard algorithm — they require spotting a mathematical pattern, formula, or property. These problems test mathematical thinking and the ability to derive solutions from examples.

---

## 2. Common Mathematical Tricks

| Trick | When to Use | Example |
|---|---|---|
| **Modular arithmetic** | Large number computations | `(a * b) % MOD` |
| **GCD / LCM** | Divisibility, fractions | `__gcd(a, b)` in C++ |
| **Combinatorics** | Counting arrangements | nCr, nPr |
| **Pigeonhole principle** | Existence proofs | If n+1 items in n boxes, some box has 2+ |
| **Parity** | Even/odd arguments | Nim game, tiling |
| **Sum formulas** | Arithmetic/geometric series | Sum 1..n = n(n+1)/2 |
| **Number theory** | Primes, factorization | Sieve of Eratosthenes |

---

## 3. Essential Formulas

```cpp
// Modular exponentiation: compute (base^exp) % mod
long long modPow(long long base, long long exp, long long mod) {
    long long result = 1;
    base %= mod;
    while (exp > 0) {
        if (exp & 1) result = result * base % mod;
        base = base * base % mod;
        exp >>= 1;
    }
    return result;
}

// Sieve of Eratosthenes: all primes up to n
vector<bool> sieve(int n) {
    vector<bool> isPrime(n + 1, true);
    isPrime[0] = isPrime[1] = false;
    for (int i = 2; i * i <= n; i++) {
        if (isPrime[i]) {
            for (int j = i * i; j <= n; j += i)
                isPrime[j] = false;
        }
    }
    return isPrime;
}

// nCr with modular inverse (for large n, mod = prime)
long long factorial[200001];
long long modInverse(long long a, long long mod) { return modPow(a, mod - 2, mod); }

void precomputeFactorials(int n, long long mod) {
    factorial[0] = 1;
    for (int i = 1; i <= n; i++)
        factorial[i] = factorial[i-1] * i % mod;
}

long long nCr(int n, int r, long long mod) {
    if (r > n) return 0;
    return factorial[n] % mod * modInverse(factorial[r], mod) % mod 
           * modInverse(factorial[n-r], mod) % mod;
}
```

---

## 4. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #204 — Count Primes | Medium | Sieve of Eratosthenes |
| 2 | LeetCode #50 — Pow(x, n) | Medium | Fast exponentiation |
| 3 | LeetCode #172 — Factorial Trailing Zeroes | Medium | Math: count factors of 5 |
| 4 | LeetCode #400 — Nth Digit | Medium | Mathematical pattern |
| 5 | LeetCode #829 — Consecutive Numbers Sum | Hard | Number theory |

---
---

*Previous chapter: [16 — Advanced Data Structures](16_Advanced_Data_Structures.md)*
*Next chapter: [18 — Cheat Sheets](18_Cheat_Sheets.md)*
