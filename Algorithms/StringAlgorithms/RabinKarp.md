### **Rabin-Karp Algorithm 🧵**

Uses **rolling hashes** to find a pattern in text. Compares hashes first; only checks the actual characters on hash match.

---

#### **Summary**
- **Average / Best:** O(n + m)
- **Worst:** O(n × m) (lots of hash collisions)
- **Space Complexity:** O(1) extra

Particularly strong for **multi-pattern search of equal length** (hash multiple patterns into a set).

---

#### **Python Implementation**
```python
def rabin_karp(text, pattern, base=256, mod=10**9 + 7):
    n, m = len(text), len(pattern)
    if m > n: return []

    h = pow(base, m - 1, mod)
    pat_hash = 0
    txt_hash = 0
    for i in range(m):
        pat_hash = (pat_hash * base + ord(pattern[i])) % mod
        txt_hash = (txt_hash * base + ord(text[i])) % mod

    matches = []
    for i in range(n - m + 1):
        if pat_hash == txt_hash and text[i:i + m] == pattern:
            matches.append(i)
        if i < n - m:
            txt_hash = (txt_hash - ord(text[i]) * h) * base + ord(text[i + m])
            txt_hash %= mod
    return matches

# Example
print(rabin_karp("GEEKS FOR GEEKS", "GEEK"))   # [0, 10]
```

---

#### **When to Use**
✅ Multiple patterns of equal length (hash set)
✅ Plagiarism / duplicate detection (large text)
🚫 Single pattern, worst-case guarantee → use KMP
