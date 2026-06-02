# Word Break Problem 🔤

---

## 🧠 Intuition

Given a string like `"leetcode"` and a dictionary `["leet", "code"]`, can you split the string into valid dictionary words? Backtracking tries every possible split point, checks if the prefix is a valid word, and recurses on the rest.

The key optimization: **memoize** which starting positions lead to dead ends, so we never re-explore them.

**Mental model:** "At each position, try all dictionary words as the next segment. If the word matches, recurse from where it ends. Undo if recursion fails."

---

## 📊 Complexity

| Approach | Time | Space |
|----------|------|-------|
| Pure backtracking | O(2ⁿ) worst case | O(n) |
| With memoization | O(n² × L) — L = max word length | O(n) |

---

## ⚙️ How It Works

**Feasibility (with memoization):**
1. Try every possible split of `s[start:]` — look for `s[start:end]` in the word set.
2. If it's a valid word, recurse on `s[end:]`.
3. Cache starting positions that lead to failure.
4. Return True when `start == len(s)` (processed entire string).

**Enumerate all segmentations (without memoization):**
- Same approach but collect all valid paths, not just the first one.

---

## 🔢 Step-by-Step Trace

s = `"catsanddog"`, words = `["cat", "cats", "and", "sand", "dog"]`

```
start=0: try "c"? No. "ca"? No. "cat"? YES!
  start=3: try "s"? No. "sa"? No. "san"? No. "sand"? YES!
    start=7: try "d"? No. "do"? No. "dog"? YES!
      start=10: = len(s) → FOUND! → "cat sand dog" ✅
  start=3: try "cats"? YES!
    start=7: try "and"? YES!
      start=10: = len(s) → FOUND! → "cats and dog" ✅
```

All valid segmentations: `["cat sand dog", "cats and dog"]`

---

## 🐍 Python Implementation

```python
def word_break(s, words):
    """
    Check if s can be segmented into dictionary words.
    Returns True or False.
    """
    word_set = set(words)    # O(1) lookup
    memo = {}                # Cache: start position → can we break from here?

    def can_break(start):
        if start == len(s):
            return True          # Processed entire string successfully
        if start in memo:
            return memo[start]   # Already computed — return cached result

        for end in range(start + 1, len(s) + 1):
            word = s[start:end]
            if word in word_set and can_break(end):
                memo[start] = True
                return True

        memo[start] = False  # Dead end from this position
        return False

    return can_break(0)


def word_break_all(s, words):
    """
    Find ALL ways to segment s into dictionary words.
    Returns list of space-separated segmentations.
    """
    word_set = set(words)
    results = []

    def backtrack(start, path):
        if start == len(s):
            results.append(" ".join(path))  # Full string consumed → valid segmentation
            return
        for end in range(start + 1, len(s) + 1):
            word = s[start:end]
            if word in word_set:
                path.append(word)
                backtrack(end, path)     # Recurse on remainder
                path.pop()              # Backtrack: undo word choice

    backtrack(0, [])
    return results


# Examples
print(word_break("leetcode", ["leet", "code"]))
# True

print(word_break("applepenapple", ["apple", "pen"]))
# True ("apple pen apple")

print(word_break("catsandog", ["cats", "dog", "sand", "and", "cat"]))
# False

print(word_break_all("catsanddog", ["cat", "cats", "and", "sand", "dog"]))
# ['cat sand dog', 'cats and dog']
```

---

## 🎯 Recognize This Problem When...

- You need to split a string into valid words from a dictionary.
- Keywords: "word segmentation", "split string into words", "tokenization".
- The problem asks "can s be broken" or "find all ways to break s".
- Common in NLP preprocessing and compiler lexer design.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Check if string can be segmented | ✅ Memoized backtracking |
| Find all valid segmentations | ✅ Plain backtracking |
| Very long strings (millions of chars) | ❌ Need trie-based optimization |
| Approximate matching (typos allowed) | ❌ Use edit distance / fuzzy matching |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Subset Sum Backtracking](SubsetSumBacktracking.md) | Same try→recurse→undo pattern |
| [N-Queens](NQueens.md) | Same backtracking structure |
| [KMP](../StringAlgorithms/KMP.md) | Efficient substring matching (used in word lookup) |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Word Break](https://leetcode.com/problems/word-break/) | LeetCode | 🟡 Medium |
| [Word Break II](https://leetcode.com/problems/word-break-ii/) | LeetCode | 🔴 Hard |
| [Concatenated Words](https://leetcode.com/problems/concatenated-words/) | LeetCode | 🔴 Hard |
