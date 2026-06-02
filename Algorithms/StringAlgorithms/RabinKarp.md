# Rabin-Karp Algorithm #️⃣

---

## 🧠 Intuition

Comparing strings character by character takes O(m) per check. Rabin-Karp's insight: compute a **hash** of the pattern and a hash of each text window. If hashes differ, skip (no comparison needed!). If hashes match, verify with actual comparison (to rule out hash collisions).

The magic is the **rolling hash**: when the window slides one character right, you can compute the new hash in O(1) by removing the leftmost character and adding the new rightmost one — like updating a running total.

**Mental model:** Instead of comparing all characters, compare fingerprints (hashes). Only do the expensive comparison when fingerprints match.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Preprocessing | O(m) — compute pattern hash |
| Search (average) | O(n + m) — rolling hash makes each step O(1) |
| Search (worst) | O(n × m) — with many hash collisions |
| Space | O(1) extra |

> n = text length, m = pattern length. Hash collisions are extremely rare with a good hash function.

---

## ⚙️ How It Works

1. Compute `hash(pattern)` using polynomial rolling hash:
   ```
   hash("abc") = ord('a')·base² + ord('b')·base¹ + ord('c')·base⁰  (mod prime)
   ```
2. Compute `hash(text[0..m-1])` (initial window).
3. Slide the window one character at a time:
   - If `hash(window) == hash(pattern)`: verify with actual comparison.
   - Update window hash: `hash_new = (hash_old - ord(removed) × base^(m-1)) × base + ord(added)`.
4. Collect all match positions.

---

## 🔢 Step-by-Step Trace

Text: `"ABCABD"`, Pattern: `"ABD"`, base=256, mod=101

| i | Window | Window Hash | Pattern Hash | Match? |
|---|--------|-------------|--------------|--------|
| 0 | "ABC"  | (65×256²+66×256+67)%101 = 4 | (65×256²+66×256+68)%101 = 5 | 4≠5 ❌ |
| 1 | "BCA"  | roll → different | 5 | ❌ |
| 2 | "CAB"  | roll → different | 5 | ❌ |
| 3 | "ABD"  | roll → 5 | 5 | ✅ → compare "ABD"=="ABD" → **MATCH at index 3** |

---

## 🐍 Python Implementation

```python
def rabin_karp(text, pattern, base=256, mod=10**9 + 7):
    """
    Find all occurrences of pattern in text using rolling hash.
    Returns list of starting indices.
    """
    n, m = len(text), len(pattern)
    if m > n:
        return []

    # Precompute base^(m-1) mod p — used to remove leftmost character
    high_power = pow(base, m - 1, mod)

    # Compute initial hashes for pattern and first window
    pat_hash = 0
    win_hash = 0
    for i in range(m):
        pat_hash = (pat_hash * base + ord(pattern[i])) % mod
        win_hash = (win_hash * base + ord(text[i])) % mod

    matches = []

    for i in range(n - m + 1):
        # Hash match → do exact comparison to confirm (rule out collisions)
        if win_hash == pat_hash:
            if text[i:i + m] == pattern:     # O(m) verification
                matches.append(i)

        # Roll the window: remove leftmost char, add next char
        if i < n - m:
            win_hash = (win_hash - ord(text[i]) * high_power) * base + ord(text[i + m])
            win_hash %= mod

    return matches


def rabin_karp_multi(text, patterns):
    """
    Search for multiple patterns of the SAME LENGTH simultaneously.
    This is where Rabin-Karp shines over KMP!
    """
    if not patterns:
        return {}
    m = len(patterns[0])
    assert all(len(p) == m for p in patterns), "All patterns must have equal length"

    base, mod = 256, 10**9 + 7
    pattern_hashes = {}
    for p in patterns:
        h = 0
        for c in p:
            h = (h * base + ord(c)) % mod
        pattern_hashes.setdefault(h, []).append(p)

    high_power = pow(base, m - 1, mod)
    results = {p: [] for p in patterns}
    n = len(text)

    win_hash = 0
    for c in text[:m]:
        win_hash = (win_hash * base + ord(c)) % mod

    for i in range(n - m + 1):
        if win_hash in pattern_hashes:
            window = text[i:i + m]
            for p in pattern_hashes[win_hash]:
                if window == p:
                    results[p].append(i)

        if i < n - m:
            win_hash = (win_hash - ord(text[i]) * high_power) * base + ord(text[i + m])
            win_hash %= mod

    return results


# Examples
print(rabin_karp("GEEKS FOR GEEKS", "GEEK"))    # [0, 10]
print(rabin_karp("aaaa", "aa"))                  # [0, 1, 2]

results = rabin_karp_multi("abcabdabc", ["ab", "bc"])
print(results)  # {'ab': [0, 3, 6], 'bc': [1, 7]}
```

---

## 🎯 Recognize This Problem When...

- You need to search for **multiple patterns of equal length** simultaneously.
- You're detecting **plagiarism** or duplicate content across documents.
- You need a string search algorithm that's easy to extend to 2D patterns.
- The pattern set is large — Rabin-Karp can hash all patterns into a set for O(1) lookup.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Multiple equal-length patterns in one pass | ✅ Hash all patterns into a set |
| Plagiarism / duplicate substring detection | ✅ Classic use case |
| 2D pattern matching | ✅ Extend rolling hash to 2D |
| Single pattern, worst-case guarantee needed | ❌ Use KMP (O(n+m) guaranteed) |
| Very long pattern with complex repeats | ❌ KMP or Z-algorithm are more reliable |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [KMP](KMP.md) | O(n+m) guaranteed; better for single pattern |
| [Z-Algorithm](ZAlgorithm.md) | Also O(n+m); simpler code than KMP |
| [Boyer-Moore](BoyerMoore.md) | Faster in practice for long patterns |
| [Aho-Corasick](AhoCorasick.md) | Multi-pattern but for different-length patterns |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Repeated String Match](https://leetcode.com/problems/repeated-string-match/) | LeetCode | 🟡 Medium |
| [Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring/) | LeetCode | 🔴 Hard |
| [Rabin-Karp](https://www.hackerrank.com/challenges/sherlock-and-valid-string/problem) | HackerRank | 🟡 Medium |
