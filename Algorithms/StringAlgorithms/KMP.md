### **KMP Algorithm (Knuth–Morris–Pratt) 🧵**

Efficient **substring search** that avoids re-checking characters by using a precomputed **failure / LPS table** (longest proper prefix that is also a suffix).

---

#### **Summary**
- **Preprocessing:** O(m) (pattern length)
- **Search:** O(n) (text length)
- **Space Complexity:** O(m)

---

#### **How It Works**
1. Build `lps[i]` = length of the longest proper prefix of `pattern[0..i]` that is also a suffix.
2. Slide the pattern over text; on a mismatch, jump using `lps` instead of restarting from the next character.

---

#### **Python Implementation**
```python
def build_lps(pattern):
    lps = [0] * len(pattern)
    length = 0
    i = 1
    while i < len(pattern):
        if pattern[i] == pattern[length]:
            length += 1
            lps[i] = length
            i += 1
        elif length:
            length = lps[length - 1]
        else:
            lps[i] = 0
            i += 1
    return lps

def kmp_search(text, pattern):
    if not pattern: return []
    lps = build_lps(pattern)
    matches = []
    i = j = 0
    while i < len(text):
        if text[i] == pattern[j]:
            i += 1; j += 1
            if j == len(pattern):
                matches.append(i - j)
                j = lps[j - 1]
        elif j:
            j = lps[j - 1]
        else:
            i += 1
    return matches

# Example
print(kmp_search("ababcabcabababd", "ababd"))   # [10]
```

---

#### **When to Use**
✅ Repeated single-pattern search in long text
✅ Linear-time guarantee, no hashing
🚫 Multi-pattern → Aho-Corasick
