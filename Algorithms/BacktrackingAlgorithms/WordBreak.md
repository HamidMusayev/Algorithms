### **Word Break Problem 🔄**

Given a string `s` and a dictionary of words, decide if `s` can be **segmented** into a sequence of dictionary words. A variant returns **all such segmentations**.

---

#### **Summary**
- **Pure backtracking:** O(2ⁿ) worst case
- **With memoization:** O(n² · L) where L is max word length
- **Space Complexity:** O(n) (memo)

---

#### **Python Implementations**

**1. Feasibility (backtracking + memo)**
```python
def word_break(s, words):
    word_set = set(words)
    memo = {}

    def can_break(start):
        if start == len(s): return True
        if start in memo: return memo[start]
        for end in range(start + 1, len(s) + 1):
            if s[start:end] in word_set and can_break(end):
                memo[start] = True
                return True
        memo[start] = False
        return False

    return can_break(0)

print(word_break("leetcode", ["leet", "code"]))   # True
```

**2. All segmentations**
```python
def word_break_all(s, words):
    word_set = set(words)
    results = []

    def backtrack(start, path):
        if start == len(s):
            results.append(" ".join(path))
            return
        for end in range(start + 1, len(s) + 1):
            w = s[start:end]
            if w in word_set:
                path.append(w)
                backtrack(end, path)
                path.pop()

    backtrack(0, [])
    return results

print(word_break_all("catsanddog", ["cat","cats","and","sand","dog"]))
# ['cat sand dog', 'cats and dog']
```

---

#### **When to Use**
✅ Tokenization, NLP segmentation
✅ Foundation for trie-based segmentation
🚫 Without memo, exponential — always memoize
