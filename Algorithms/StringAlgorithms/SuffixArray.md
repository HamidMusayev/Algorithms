### **Suffix Array (and Suffix Tree) 🧵**

A **suffix array** is the sorted array of all suffixes of a string (stored as starting indices). Combined with the **LCP array** (longest common prefix), it answers many substring queries in O(log n) or O(1).

A **suffix tree** is a compressed trie of all suffixes — same power, more memory.

---

#### **Summary**
| Structure | Build Time | Space |
|-----------|-----------|-------|
| Naïve SA | O(n² log n) | O(n) |
| SA-IS / DC3 | **O(n)** | O(n) |
| Suffix Tree (Ukkonen) | O(n) | O(n · σ) |

---

#### **Use Cases**
- Substring search in O(m log n) by binary searching SA
- Longest repeated substring
- Number of distinct substrings
- Bioinformatics: BWT, FM-index

---

#### **Python Implementation (Simple O(n² log n))**
```python
def suffix_array(s):
    return sorted(range(len(s)), key=lambda i: s[i:])

def lcp_array(s, sa):
    n = len(s)
    rank = [0] * n
    for i, p in enumerate(sa):
        rank[p] = i
    lcp = [0] * (n - 1)
    h = 0
    for i in range(n):
        if rank[i] > 0:
            j = sa[rank[i] - 1]
            while i + h < n and j + h < n and s[i + h] == s[j + h]:
                h += 1
            lcp[rank[i] - 1] = h
            if h: h -= 1
    return lcp

# Example
s = "banana"
sa = suffix_array(s)
print(sa)                # [5, 3, 1, 0, 4, 2]
print(lcp_array(s, sa))  # [1, 3, 0, 0, 2]
```

> For production / contest use, implement SA-IS or use a library — the simple `sorted` approach is O(n² log n) due to slice comparison.

---

#### **When to Use**
✅ Many substring queries on the same text
✅ Bioinformatics (long sequences)
🚫 Single, ad-hoc substring search → KMP / Z / Boyer-Moore
