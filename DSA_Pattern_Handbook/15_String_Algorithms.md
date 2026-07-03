<!--
═══════════════════════════════════════════════════════════════════════════════
  DSA PATTERN HANDBOOK — STRING ALGORITHMS
  Chapter 15
  
  Patterns in this chapter:
    Pattern 49: KMP (Knuth-Morris-Pratt)
    Pattern 50: Z Algorithm
    Pattern 51: Rabin-Karp (Rolling Hash)
═══════════════════════════════════════════════════════════════════════════════
-->

# Chapter 15: String Algorithms

These are specialized string-matching algorithms. While many string problems are solved with sliding window, hashing, or DP, some require dedicated pattern-matching algorithms.

---
---

# Pattern 49: KMP (Knuth-Morris-Pratt)

```
═══════════════════════════════════════════════════════════════
  KMP ALGORITHM
  OA Frequency: ★★★☆☆
  Companies: Google | Amazon | Microsoft | Goldman Sachs
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

"Find all occurrences of pattern P in text T." Brute force is O(n × m). KMP achieves O(n + m) by **never re-comparing characters** — when a mismatch occurs, it uses pre-computed information to skip ahead.

**The key insight**: Build a "failure function" (LPS array — Longest Proper Prefix that is also a Suffix) for the pattern. When a mismatch happens at position j of the pattern, instead of restarting from 0, jump to `lps[j-1]` — the longest prefix of the pattern that matches the current suffix.

---

## 2. Pattern Identification Checklist

- [ ] Does the problem involve **substring matching**?
- [ ] Does it ask for **all occurrences** of a pattern in a text?
- [ ] Is n or m large enough that O(n × m) is too slow?
- [ ] Does the problem involve finding **repeated patterns** or **period of a string**?

---

## 3. Reusable C++ Template

```cpp
// Template: KMP — String Pattern Matching
// Time: O(n + m), Space: O(m) for LPS array

#include <string>
#include <vector>
using namespace std;

// Build LPS (Longest Proper Prefix which is also Suffix) array
vector<int> buildLPS(const string& pattern) {
    int m = pattern.size();
    vector<int> lps(m, 0);
    int len = 0;  // length of previous longest prefix suffix
    int i = 1;

    while (i < m) {
        if (pattern[i] == pattern[len]) {
            len++;
            lps[i] = len;
            i++;
        } else {
            if (len != 0) {
                len = lps[len - 1];  // don't increment i — try shorter prefix
            } else {
                lps[i] = 0;
                i++;
            }
        }
    }

    return lps;
}

// KMP search: find all occurrences of pattern in text
vector<int> kmpSearch(const string& text, const string& pattern) {
    int n = text.size(), m = pattern.size();
    vector<int> lps = buildLPS(pattern);
    vector<int> matches;

    int i = 0;  // index in text
    int j = 0;  // index in pattern

    while (i < n) {
        if (text[i] == pattern[j]) {
            i++;
            j++;
        }

        if (j == m) {
            matches.push_back(i - m);  // match found at index i-m
            j = lps[j - 1];           // continue searching
        } else if (i < n && text[i] != pattern[j]) {
            if (j != 0) {
                j = lps[j - 1];       // skip using LPS
            } else {
                i++;
            }
        }
    }

    return matches;
}
```

---

## 4. Dry Run — LPS Array

```
Pattern: "ABCABD"

i=1: B≠A(len=0) → lps[1]=0
i=2: C≠A(len=0) → lps[2]=0
i=3: A==A(len=0) → len=1, lps[3]=1
i=4: B==B(len=1) → len=2, lps[4]=2
i=5: D≠C(len=2) → len=lps[1]=0, D≠A(len=0) → lps[5]=0

LPS: [0, 0, 0, 1, 2, 0]

Meaning: if mismatch at position 5 ('D'), jump back to position lps[4]=2 ('C')
instead of restarting from 0. We know "AB" already matched.
```

---

## 5. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #28 — Find Index of First Occurrence | Easy | Basic KMP |
| 2 | LeetCode #459 — Repeated Substring Pattern | Easy | LPS trick: if `n % (n - lps[n-1]) == 0` |
| 3 | LeetCode #214 — Shortest Palindrome | Hard | KMP on reversed string |
| 4 | LeetCode #1392 — Longest Happy Prefix | Hard | Direct LPS application |

**Memory Trick:**

> 🧠 **"The Bookmark"** — KMP's LPS array is like a bookmark. When you're reading and hit a wrong word, instead of going back to the start of the sentence, you jump back to the bookmark (last matching prefix). You never re-read text you've already processed.

---
---

# Pattern 50: Z Algorithm

```
═══════════════════════════════════════════════════════════════
  Z ALGORITHM
  OA Frequency: ★★☆☆☆
  Companies: Google | Microsoft
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Given a string S, compute Z[i] = the length of the longest substring starting at position i that matches a prefix of S. This is useful for string matching, repeated pattern detection, and prefix-based queries.

**For pattern matching**: Concatenate `pattern + "$" + text`. If `Z[i] == len(pattern)`, there's a match at position `i - len(pattern) - 1`.

---

## 2. Reusable C++ Template

```cpp
// Template: Z Algorithm
// Time: O(n), Space: O(n)

#include <string>
#include <vector>
using namespace std;

vector<int> zFunction(const string& s) {
    int n = s.size();
    vector<int> z(n, 0);
    int l = 0, r = 0;

    for (int i = 1; i < n; i++) {
        if (i < r) {
            z[i] = min(r - i, z[i - l]);
        }
        while (i + z[i] < n && s[z[i]] == s[i + z[i]]) {
            z[i]++;
        }
        if (i + z[i] > r) {
            l = i;
            r = i + z[i];
        }
    }

    return z;
}

// Pattern matching using Z
vector<int> zSearch(const string& text, const string& pattern) {
    string concat = pattern + "$" + text;
    vector<int> z = zFunction(concat);
    int m = pattern.size();
    vector<int> matches;

    for (int i = m + 1; i < (int)concat.size(); i++) {
        if (z[i] == m) {
            matches.push_back(i - m - 1);
        }
    }

    return matches;
}
```

---

## 3. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #28 — Find Index of First Occurrence | Easy | Z algorithm alternative |
| 2 | LeetCode #796 — Rotate String | Easy | Concatenate and match |

---
---

# Pattern 51: Rabin-Karp (Rolling Hash)*

```
═══════════════════════════════════════════════════════════════
  RABIN-KARP (ROLLING HASH) — ADVANCED
  OA Frequency: ★★☆☆☆
  Companies: Google | Amazon | Microsoft
═══════════════════════════════════════════════════════════════
```

---

## 1. Intuition

**What problem is this pattern trying to solve?**

Pattern matching using **hashing**. Instead of comparing characters, compare hash values. If hashes match, verify with actual comparison (to handle collisions). The "rolling" part: update the hash in O(1) as the window slides.

---

## 2. When to Use Over KMP

| Scenario | KMP | Rabin-Karp |
|---|---|---|
| Single pattern matching | ✅ Better | ✅ Works |
| Multiple pattern matching | Run KMP per pattern | ✅ Better (hash all patterns) |
| Longest duplicate substring | ❌ Can't directly | ✅ Binary search + rolling hash |

---

## 3. Reusable C++ Template

```cpp
// Template: Rabin-Karp Rolling Hash
// Time: O(n + m) average, O(n × m) worst (hash collisions)

#include <string>
#include <vector>
using namespace std;

vector<int> rabinKarp(const string& text, const string& pattern) {
    int n = text.size(), m = pattern.size();
    if (m > n) return {};

    const long long MOD = 1e9 + 7;
    const long long BASE = 31;
    vector<int> matches;

    // Compute pattern hash and initial window hash
    long long patHash = 0, winHash = 0, power = 1;
    for (int i = 0; i < m; i++) {
        patHash = (patHash * BASE + (pattern[i] - 'a' + 1)) % MOD;
        winHash = (winHash * BASE + (text[i] - 'a' + 1)) % MOD;
        if (i > 0) power = (power * BASE) % MOD;
    }

    for (int i = 0; i <= n - m; i++) {
        if (winHash == patHash) {
            // Verify (handle collisions)
            if (text.substr(i, m) == pattern) {
                matches.push_back(i);
            }
        }
        // Slide window: remove text[i], add text[i+m]
        if (i + m < n) {
            winHash = (winHash - (text[i] - 'a' + 1) * power % MOD + MOD) % MOD;
            winHash = (winHash * BASE + (text[i + m] - 'a' + 1)) % MOD;
        }
    }

    return matches;
}
```

---

## 4. Practice Problems

| # | Problem | Difficulty | Why |
|---|---|---|---|
| 1 | LeetCode #187 — Repeated DNA Sequences | Medium | Rolling hash for fixed-length substrings |
| 2 | LeetCode #1044 — Longest Duplicate Substring | Hard | Binary search + rolling hash |
| 3 | LeetCode #718 — Maximum Length of Repeated Subarray | Medium | Rolling hash or DP |

---
---

*Previous chapter: [14 — Dynamic Programming](14_Dynamic_Programming.md)*
*Next chapter: [16 — Advanced Data Structures](16_Advanced_Data_Structures.md)*
