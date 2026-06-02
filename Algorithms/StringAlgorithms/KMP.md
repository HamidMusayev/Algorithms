# KMP Algorithm (Knuth–Morris–Pratt) 🧵

---

## 🧠 Intuition

Imagine you are reading a book looking for the phrase "ABCABD". You find "ABCAB" and then the next character is wrong. A naive reader starts over from the beginning. KMP realizes that the "AB" at the end of what you already matched is identical to the "AB" at the start of the pattern — so you only need to back up to position 2, not position 0.

The key insight is precomputing for every prefix of the pattern the longest proper prefix that is also a suffix (called the LPS — Longest Proper Prefix which is also Suffix). On a mismatch at position `j` in the pattern, instead of resetting `j` to 0, you jump to `lps[j-1]`, skipping all the characters you know must match.

**Mental model:** The LPS table is a "get out of jail free" card — it tells you the furthest you can safely rewind without missing a match.

---

## 📊 Complexity

| Metric | Value |
|---|---|
| Time — preprocessing (build LPS) | O(m) |
| Time — search | O(n) |
| Time — total | O(n + m) |
| Space | O(m) |

*n = text length, m = pattern length*

---

## ⚙️ How It Works

**Phase 1 — Build the LPS (failure function) table:**

1. Initialize `lps[0] = 0` (a single character has no proper prefix/suffix).
2. Use two pointers: `length = 0` (length of previous longest prefix-suffix) and `i = 1`.
3. If `pattern[i] == pattern[length]`, set `lps[i] = length + 1`, advance both.
4. If they differ and `length > 0`, fall back: `length = lps[length - 1]` (do NOT advance `i`).
5. If they differ and `length == 0`, set `lps[i] = 0` and advance `i`.

**Phase 2 — Search:**

1. Use pointer `i` over text and `j` over pattern.
2. If `text[i] == pattern[j]`, advance both.
3. When `j == m` (full match), record the start index `i - j`, then set `j = lps[j - 1]` to continue searching.
4. On a mismatch with `j > 0`, set `j = lps[j - 1]` (do NOT move `i`).
5. On a mismatch with `j == 0`, advance only `i`.

---

## 🔢 Step-by-Step Trace

Pattern: `"ABAB"`, Text: `"ABABCABABAB"`

**LPS table for "ABAB":**

| Index | 0 | 1 | 2 | 3 |
|---|---|---|---|---|
| Pattern | A | B | A | B |
| LPS | 0 | 0 | 1 | 2 |

*LPS[2]=1 because "A" is both a prefix and suffix of "ABA". LPS[3]=2 because "AB" is both a prefix and suffix of "ABAB".*

**Search trace:**

| Text index (i) | Text char | Pattern index (j) | Pattern char | Action |
|---|---|---|---|---|
| 0 | A | 0 | A | Match, advance both |
| 1 | B | 1 | B | Match, advance both |
| 2 | A | 2 | A | Match, advance both |
| 3 | B | 3 | B | Match, j==m → **Match at index 0**, j = lps[3] = 2 |
| 4 | C | 2 | A | Mismatch, j = lps[1] = 0 |
| 4 | C | 0 | A | Mismatch, advance i |
| 5 | A | 0 | A | Match, advance both |
| 6 | B | 1 | B | Match, advance both |
| 7 | A | 2 | A | Match, advance both |
| 8 | B | 3 | B | Match, j==m → **Match at index 5**, j = lps[3] = 2 |
| 9 | A | 2 | A | Match, advance both |
| 10 | B | 3 | B | Match, j==m → **Match at index 7**, j = lps[3] = 2 |

Matches at indices: **0, 5, 7**

---

## 🐍 Python Implementation

```python
def build_lps(pattern):
    """Build the Longest Proper Prefix which is also Suffix table."""
    m = len(pattern)
    lps = [0] * m       # lps[0] is always 0
    length = 0           # length of previous longest prefix-suffix
    i = 1

    while i < m:
        if pattern[i] == pattern[length]:
            # Characters match — extend the current prefix-suffix
            length += 1
            lps[i] = length
            i += 1
        elif length != 0:
            # Fall back using the LPS table itself (don't move i)
            length = lps[length - 1]
        else:
            # No prefix-suffix of any length — lps[i] stays 0
            lps[i] = 0
            i += 1

    return lps


def kmp_search(text, pattern):
    """Return all start indices where pattern occurs in text."""
    if not pattern:
        return []

    n, m = len(text), len(pattern)
    lps = build_lps(pattern)

    matches = []
    i = 0   # index into text
    j = 0   # index into pattern

    while i < n:
        if text[i] == pattern[j]:
            i += 1
            j += 1

        if j == m:
            # Full pattern matched; record start index
            matches.append(i - j)
            # Don't start from scratch — reuse the suffix we already matched
            j = lps[j - 1]
        elif i < n and text[i] != pattern[j]:
            if j != 0:
                # Use LPS to skip redundant comparisons
                j = lps[j - 1]
            else:
                i += 1

    return matches


# --- Example ---
text = "ababcababab"
pattern = "abab"

lps = build_lps(pattern)
print(f"Pattern : {list(pattern)}")
print(f"LPS     : {lps}")
# LPS: [0, 0, 1, 2]

result = kmp_search(text, pattern)
print(f"Matches at indices: {result}")
# Matches at indices: [0, 5, 7]
```

---

## 🎯 Recognize This Problem When...

- You need to find all occurrences of a fixed pattern inside a text.
- The problem asks for substring search with a linear-time guarantee.
- The pattern or text contains many repeated substrings (naive approach would be slow).
- You need to check whether one string is a rotation of another (search in `s + s`).
- The problem involves finding the shortest period of a string.
- You see "find needle in haystack" with constraints where O(n×m) would TLE.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|---|---|
| Single pattern, long text, linear-time required | ✅ Ideal choice |
| Pattern appears many times and you want all occurrences | ✅ Handles overlapping matches cleanly |
| Large alphabet, very long pattern, practical speed matters most | ❌ Boyer-Moore is faster in practice |
| Searching for many different patterns simultaneously | ❌ Use Aho-Corasick instead |
| Multiple substring queries on the same text | ❌ Suffix array gives O(m log n) per query |
| You need to detect near-matches / fuzzy matching | ❌ KMP only finds exact matches |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|---|---|
| [Z-Algorithm](./ZAlgorithm.md) | Also O(n+m) pattern matching; builds a different table (Z-array); often simpler to code |
| [Rabin-Karp](./RabinKarp.md) | Hash-based alternative; great for multi-pattern of equal length |
| [Boyer-Moore](./BoyerMoore.md) | Faster in practice for large alphabets; scans right-to-left |
| [Aho-Corasick](./AhoCorasick.md) | Generalizes KMP's failure links to a trie for multi-pattern search |
| [Suffix Array](./SuffixArray.md) | Better when you have many distinct patterns to search in the same text |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---|---|---|
| [Find the Index of the First Occurrence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/) | LeetCode | 🟢 Easy |
| [Repeated Substring Pattern](https://leetcode.com/problems/repeated-substring-pattern/) | LeetCode | 🟢 Easy |
| [Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/) | LeetCode | 🔴 Hard |
| [Longest Happy Prefix](https://leetcode.com/problems/longest-happy-prefix/) | LeetCode | 🔴 Hard |
| [String Matching in an Array](https://leetcode.com/problems/string-matching-in-an-array/) | LeetCode | 🟢 Easy |
| [Find Beautiful Indices in the Given Array II](https://leetcode.com/problems/find-beautiful-indices-in-the-given-array-ii/) | LeetCode | 🔴 Hard |
