# Boyer-Moore Algorithm 🔍

---

## 🧠 Intuition

Most pattern matching algorithms scan left-to-right. Boyer-Moore scans the **pattern right-to-left** against the text window. When a mismatch occurs, two heuristics tell you how far to jump:

1. **Bad Character Rule:** The mismatched text character wasn't in the pattern at all (or was last seen far back) → jump past it.
2. **Good Suffix Rule:** The suffix of the pattern that DID match has this exact suffix elsewhere in the pattern → align that occurrence.

The result: in practice, Boyer-Moore often skips large chunks of text, making it **sublinear on average** — the fastest practical string search for large alphabets.

**Mental model:** "When you mismatch, learn from both the bad character AND the good suffix to jump as far forward as possible."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Preprocessing | O(m + σ) — σ = alphabet size |
| Search (best/average) | O(n / m) — sublinear! |
| Search (worst) | O(n × m) — bad char only; O(n) with full Galil rule |
| Space | O(m + σ) |

> n = text length, m = pattern length, σ = alphabet size (256 for ASCII).

---

## ⚙️ How It Works

**Bad Character Heuristic (simplified):**
1. Build a table: for each character `c`, `bad[c]` = last position it appears in the pattern (-1 if absent).
2. When pattern[j] ≠ text[s+j] at position j in pattern:
   - Let `shift = j - bad.get(text[s+j], -1)`.
   - Move the pattern window forward by `max(1, shift)`.

**Good Suffix Heuristic** (more complex — full version):
- Also considers how much of the already-matched suffix can be realigned.
- The implementation below uses only the bad character heuristic (simpler but still very fast in practice).

---

## 🔢 Step-by-Step Trace

Text: `"ABAAABCDABCDABDE"`, Pattern: `"ABCD"`

Bad character table for "ABCD": A→0, B→1, C→2, D→3

| s  | Align window | Check right-to-left | Mismatch | Shift | Action |
|----|-------------|---------------------|----------|-------|--------|
| 0  | ABAA | D≠A (j=3) | bad['A']=0, shift=3-0=3 | 3 | jump 3 |
| 3  | AABC | D≠A (j=3) | bad['A']=0, shift=3 | 3 | jump 1 (max(1,3)... check again) |
| 4  | ABCD | DCBA match! | — | 1 | **MATCH at 4** |
| ... | ... | ... | ... | ... | ... |

---

## 🐍 Python Implementation

```python
def boyer_moore(text, pattern):
    """
    Find all occurrences of pattern in text using Boyer-Moore (bad char heuristic).
    Returns list of starting indices.
    """
    n, m = len(text), len(pattern)
    if m == 0 or m > n:
        return []

    # Build bad character table: for each char, its last position in pattern
    bad = {}
    for i, c in enumerate(pattern):
        bad[c] = i    # Last occurrence wins (rightmost position)

    matches = []
    s = 0    # s = shift of pattern relative to text

    while s <= n - m:
        j = m - 1    # Start matching from right end of pattern

        # Scan right-to-left while characters match
        while j >= 0 and pattern[j] == text[s + j]:
            j -= 1

        if j < 0:
            # Full match found at position s
            matches.append(s)
            # Shift to look for next match: align with next occurrence of text[s+m] in pattern
            s += (m - bad[text[s + m]] if s + m < n and text[s + m] in bad else 1)
        else:
            # Mismatch at position j: use bad character heuristic
            bc_shift = j - bad.get(text[s + j], -1)
            s += max(1, bc_shift)    # Jump at least 1 to avoid infinite loop

    return matches


def preprocess_bad_char(pattern):
    """Build the full bad character table (last occurrence of each char)."""
    bad = {}
    for i, c in enumerate(pattern):
        bad[c] = i
    return bad


# Example
text = "ABAAABCDABCDABDE"
pattern = "ABCD"
print(f"Pattern '{pattern}' found at:", boyer_moore(text, pattern))
# [4, 8]

# Verify
for i in boyer_moore(text, pattern):
    print(f"  text[{i}:{i+len(pattern)}] = '{text[i:i+len(pattern)]}'")
```

---

## 🎯 Recognize This Problem When...

- You're searching a **large text** with a **large alphabet** (English, source code, binary).
- You need **practical speed** over theoretical guarantees.
- The pattern is long (the longer the pattern, the bigger the skips).
- You're implementing `grep`-style tools.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Large alphabet (ASCII, Unicode) text search | ✅ Typically the fastest practical choice |
| Long patterns in natural language text | ✅ Long patterns → large jumps → fast |
| Small alphabet (DNA A/C/G/T, binary) | ❌ Jumps are small; KMP or Z-algo may be better |
| Multi-pattern search | ❌ Use Aho-Corasick |
| Need strict O(n+m) guarantee | ❌ Use KMP (BM can be O(nm) without Galil rule) |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [KMP](KMP.md) | Left-to-right; guaranteed O(n+m); better for small alphabets |
| [Z-Algorithm](ZAlgorithm.md) | Also O(n+m); simpler code |
| [Rabin-Karp](RabinKarp.md) | Hash-based; great for multi-pattern of equal length |
| [Aho-Corasick](AhoCorasick.md) | Multi-pattern matching |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Find the Index of the First Occurrence](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/) | LeetCode | 🟢 Easy |
| [Implement strStr()](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/) | LeetCode | 🟢 Easy |
