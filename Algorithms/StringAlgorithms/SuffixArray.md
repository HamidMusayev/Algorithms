# Suffix Array & LCP Array 📚

---

## 🧠 Intuition

A **suffix array** is the sorted order of all suffixes of a string. For `"banana"`, the suffixes are: "banana", "anana", "nana", "ana", "na", "a". Sorted: ["a", "ana", "anana", "banana", "na", "nana"] → suffix array = [5, 3, 1, 0, 4, 2].

Once you have the suffix array, you can **binary search** for any pattern in O(m log n) time. Combined with the **LCP (Longest Common Prefix) array**, it answers a huge range of string questions in O(1) or O(log n).

**Mental model:** "Sort all suffixes → get a phonebook of the string → binary search for anything."

---

## 📊 Complexity

| Operation | Time |
|-----------|------|
| Build suffix array (naive) | O(n² log n) |
| Build suffix array (SA-IS, DC3) | O(n) |
| Build LCP array (Kasai's algorithm) | O(n) |
| Pattern search with SA | O(m log n) |
| Space | O(n) |

---

## ⚙️ How It Works

**Suffix Array (simple O(n² log n)):**
1. Generate all suffixes as starting indices.
2. Sort them by the actual suffix string.
3. Return sorted indices.

**LCP Array (Kasai's Algorithm — O(n)):**
1. Build the inverse of the suffix array (`rank`).
2. For each suffix in the original order, compute LCP with the previous suffix in sorted order.
3. Key insight: if LCP between suffix `i` and its predecessor is `h > 0`, then LCP between `i+1` and its predecessor is `≥ h-1` (LCP can decrease by at most 1 per step).

---

## 🔢 Step-by-Step Trace

String: `"banana"` (length 6)

All suffixes sorted:

| SA index | Suffix       |
|----------|--------------|
| 5        | "a"          |
| 3        | "ana"        |
| 1        | "anana"      |
| 0        | "banana"     |
| 4        | "na"         |
| 2        | "nana"       |

Suffix array: `[5, 3, 1, 0, 4, 2]`

LCP array (LCP between adjacent sorted suffixes):
- LCP("a", "ana") = 1
- LCP("ana", "anana") = 3
- LCP("anana", "banana") = 0
- LCP("banana", "na") = 0
- LCP("na", "nana") = 2

LCP: `[1, 3, 0, 0, 2]`

---

## 🐍 Python Implementation

```python
def suffix_array(s):
    """
    Build suffix array for string s (O(n² log n) — simple version).
    Returns sorted list of starting indices.
    """
    return sorted(range(len(s)), key=lambda i: s[i:])


def build_lcp_array(s, sa):
    """
    Kasai's algorithm — O(n) LCP array construction.
    lcp[i] = LCP between sa[i] and sa[i-1] (adjacent in sorted order).
    """
    n = len(s)
    # rank[i] = position of suffix starting at i in the suffix array
    rank = [0] * n
    for i, suffix_start in enumerate(sa):
        rank[suffix_start] = i

    lcp = [0] * (n - 1)
    h = 0    # Current LCP length

    for i in range(n):
        if rank[i] > 0:           # If this suffix is not first in SA
            j = sa[rank[i] - 1]  # Previous suffix in sorted order
            # Extend LCP as much as possible
            while i + h < n and j + h < n and s[i + h] == s[j + h]:
                h += 1
            lcp[rank[i] - 1] = h
            if h > 0:
                h -= 1            # LCP can decrease by at most 1

    return lcp


def search_pattern(s, sa, pattern):
    """Binary search for pattern in text using suffix array. O(m log n)."""
    m = len(pattern)
    n = len(sa)

    # Find left boundary (first position where suffix >= pattern)
    lo, hi = 0, n
    while lo < hi:
        mid = (lo + hi) // 2
        if s[sa[mid]:sa[mid] + m] < pattern:
            lo = mid + 1
        else:
            hi = mid

    left = lo

    # Find right boundary (first position where suffix > pattern)
    hi = n
    while lo < hi:
        mid = (lo + hi) // 2
        if s[sa[mid]:sa[mid] + m] <= pattern:
            lo = mid + 1
        else:
            hi = mid

    return sorted(sa[left:lo])   # All starting positions of pattern


# Example
s = "banana"
sa = suffix_array(s)
lcp = build_lcp_array(s, sa)

print(f"String: {s}")
print(f"Suffix Array: {sa}")       # [5, 3, 1, 0, 4, 2]
print(f"LCP Array: {lcp}")         # [1, 3, 0, 0, 2]
print()

# Pattern search
for pat in ["ana", "an", "na", "xyz"]:
    positions = search_pattern(s, sa, pat)
    print(f"'{pat}' found at: {positions}")
# 'ana' found at: [1, 3]
# 'an' found at: [1, 3]
# 'na' found at: [2, 4]
# 'xyz' found at: []
```

---

## 🎯 Recognize This Problem When...

- You need to perform **many different substring queries** on the same string.
- Keywords: "longest repeated substring", "number of distinct substrings", "multiple pattern searches".
- You're in bioinformatics working with long DNA sequences.
- The problem asks for something like "for every query pattern, find all occurrences" — suffix array handles all queries after O(n log n) preprocessing.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Multiple pattern searches on the same text | ✅ O(m log n) per query after O(n log n) build |
| Longest repeated substring | ✅ Max value in LCP array |
| Number of distinct substrings | ✅ n(n+1)/2 - sum(LCP) |
| Bioinformatics (long sequences) | ✅ Standard tool |
| Single one-time pattern search | ❌ KMP/Z-algo are simpler |
| Need exact match (one pattern, one text) | ❌ KMP is simpler and sufficient |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [KMP](KMP.md) | Better for single pattern; no preprocessing of text |
| [Aho-Corasick](AhoCorasick.md) | Multi-pattern without text preprocessing |
| [Z-Algorithm](ZAlgorithm.md) | Also O(n+m) pattern match; no suffix array needed |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring/) | LeetCode | 🔴 Hard |
| [Number of Distinct Substrings](https://codeforces.com/problemset/problem/271/D) | Codeforces | 🔴 Hard |
| [Suffix Array](https://www.hackerrank.com/challenges/string-similarity/problem) | HackerRank | 🔴 Hard |
