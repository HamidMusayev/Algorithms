### **Boyer-Moore Algorithm 🧵**

A **fast pattern-matching** algorithm that scans the pattern **right-to-left** and uses two heuristics — **bad character** and **good suffix** — to skip large portions of text.

In practice, often the **fastest substring search** for large alphabets (used by GNU `grep`).

---

#### **Summary**
- **Best / Average:** O(n / m) — sublinear in practice!
- **Worst:** O(n × m) (without good-suffix rule); O(n + m) with full Galil rule
- **Space:** O(m + σ) for tables (σ = alphabet size)

---

#### **Python Implementation (Bad-Character Heuristic)**
```python
def boyer_moore(text, pattern):
    n, m = len(text), len(pattern)
    if m == 0: return []

    # bad-character table
    bad = {c: i for i, c in enumerate(pattern)}

    matches = []
    s = 0
    while s <= n - m:
        j = m - 1
        while j >= 0 and pattern[j] == text[s + j]:
            j -= 1
        if j < 0:
            matches.append(s)
            s += 1
        else:
            s += max(1, j - bad.get(text[s + j], -1))
    return matches

# Example
print(boyer_moore("ABAAABCDABCDABDE", "ABCD"))   # [4, 8]
```

---

#### **When to Use**
✅ Long text, long alphabet (English, code) → very fast
✅ Search tools like `grep`
🚫 Tiny alphabet (binary, DNA) → KMP/Z may be steadier
