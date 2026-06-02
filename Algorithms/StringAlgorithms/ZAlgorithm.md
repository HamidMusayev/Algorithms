# Z-Algorithm 🔤

---

## 🧠 Intuition

The Z-array for a string `s` tells you: at every position `i`, how many characters starting at `i` match the beginning of `s`? So `Z[i]` = length of the longest substring starting at `s[i]` that is also a prefix of `s`.

For pattern matching: concatenate `pattern + '$' + text` (the `$` is a separator not in either string). Then Z values equal to `len(pattern)` indicate a match.

**Mental model:** "Z[i] measures how much of the string starting at position i looks like the beginning of the string."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time — build Z-array | O(n) |
| Time — search | O(n + m) total |
| Space | O(n + m) for the combined string |

> n = text length, m = pattern length.

---

## ⚙️ How It Works

**Building the Z-array:**
1. Maintain a window `[l, r]` where `s[l..r]` matches `s[0..r-l]`.
2. For each position `i`:
   - If `i < r`: use previously computed Z values to skip (copy `min(r-i, Z[i-l])`).
   - Extend naively: compare `s[i + Z[i]]` with `s[Z[i]]` until mismatch.
   - Update `[l, r]` if the window extended beyond r.

**Pattern search:**
1. Build combined string: `P + '$' + T`.
2. Compute Z-array.
3. Positions where `Z[i] == len(P)` are matches in the text.

---

## 🔢 Step-by-Step Trace

String: `"aabxaa"`

| i | Z[i] | Reasoning |
|---|------|-----------|
| 0 | 0    | Z[0] undefined (or = n by convention) |
| 1 | 1    | s[1]='a'=s[0], s[2]='b'≠s[1] → Z[1]=1 |
| 2 | 0    | s[2]='b'≠s[0]='a' |
| 3 | 0    | s[3]='x'≠s[0]='a' |
| 4 | 2    | s[4]='a'=s[0], s[5]='a'=s[1], s[6] out of bounds → Z[4]=2 |
| 5 | 1    | s[5]='a'=s[0], s[6] out of bounds → Z[5]=1 |

Z-array: `[0, 1, 0, 0, 2, 1]`

**Search:** pattern=`"aa"`, text=`"aabxaa"`, combined=`"aa$aabxaa"`

Z-values at positions where Z[i]==2 indicate matches at text indices 0 and 5.

---

## 🐍 Python Implementation

```python
def z_function(s):
    """
    Compute Z-array for string s in O(n) time.
    Z[i] = length of longest substring starting at s[i] that matches a prefix of s.
    """
    n = len(s)
    z = [0] * n
    l, r = 0, 0    # Current matching window [l, r]

    for i in range(1, n):
        if i < r:
            # We're inside the window — use previously computed value
            z[i] = min(r - i, z[i - l])

        # Extend beyond the window (or from scratch if i >= r)
        while i + z[i] < n and s[z[i]] == s[i + z[i]]:
            z[i] += 1

        # Update window if we've extended beyond r
        if i + z[i] > r:
            l, r = i, i + z[i]

    return z


def z_search(text, pattern):
    """
    Find all occurrences of pattern in text using Z-algorithm.
    Returns list of starting indices.
    """
    if not pattern or not text:
        return []

    # Combine: pattern + separator + text
    combined = pattern + '$' + text
    z = z_function(combined)

    m = len(pattern)
    matches = []

    # Matches occur where Z[i] == len(pattern)
    for i in range(m + 1, len(combined)):
        if z[i] == m:
            matches.append(i - m - 1)   # Convert back to text index

    return matches


# Examples
print(z_function("aabxaa"))
# [0, 1, 0, 0, 2, 1]

print(z_search("abxabcabcaby", "abcaby"))
# [6]

print(z_search("aaaa", "aa"))
# [0, 1, 2]
```

---

## 🎯 Recognize This Problem When...

- You need **pattern matching** and want a simpler alternative to KMP.
- You need to find all positions where a pattern occurs in a string.
- The problem involves **finding the shortest period** of a string.
- You need to find the **longest palindromic prefix** (Z-array on reversed string).
- You want to check if one string is a **rotation** of another (search in `s + s`).

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Pattern matching, simpler code than KMP | ✅ Often preferred for its simplicity |
| Finding string periods | ✅ Period = smallest p where Z[p] = n - p |
| Rotation check (is s1 a rotation of s2?) | ✅ Search s1 in s2+s2 |
| Multiple different patterns | ❌ Use Aho-Corasick |
| Approximate/fuzzy matching | ❌ Use edit distance-based approaches |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [KMP](KMP.md) | Also O(n+m); uses LPS table instead of Z-array; KMP for overlapping matches |
| [Rabin-Karp](RabinKarp.md) | Hash-based; better for multi-pattern of equal length |
| [Boyer-Moore](BoyerMoore.md) | Right-to-left; faster in practice for large alphabets |
| [Aho-Corasick](AhoCorasick.md) | Multi-pattern matching |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Repeated Substring Pattern](https://leetcode.com/problems/repeated-substring-pattern/) | LeetCode | 🟢 Easy |
| [Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/) | LeetCode | 🔴 Hard |
| [Find the Index of the First Occurrence](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/) | LeetCode | 🟢 Easy |
