### **Z-Algorithm 🧵**

Computes the **Z-array** of a string in **O(n)** time, where `Z[i]` = length of the longest substring starting at `i` that matches a prefix of the string.

Used for **pattern matching**: concatenate `pattern + '$' + text` and find positions where `Z[i] == len(pattern)`.

---

#### **Summary**
- **Time Complexity:** O(n + m)
- **Space Complexity:** O(n + m)

---

#### **Python Implementation**
```python
def z_function(s):
    n = len(s)
    z = [0] * n
    l = r = 0
    for i in range(1, n):
        if i < r:
            z[i] = min(r - i, z[i - l])
        while i + z[i] < n and s[z[i]] == s[i + z[i]]:
            z[i] += 1
        if i + z[i] > r:
            l, r = i, i + z[i]
    return z

def z_search(text, pattern):
    combined = pattern + '$' + text
    z = z_function(combined)
    m = len(pattern)
    return [i - m - 1 for i, v in enumerate(z) if v == m]

# Example
print(z_search("abxabcabcaby", "abcaby"))   # [6]
```

---

#### **When to Use**
✅ Pattern matching with simpler code than KMP
✅ Period finding, palindromes (Manacher's), etc.
🚫 Multi-pattern → Aho-Corasick
