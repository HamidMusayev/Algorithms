### **Aho-Corasick Algorithm 🧵**

Searches for **many patterns simultaneously** in a single pass over the text. Builds a **trie of patterns**, then adds **failure links** (like KMP, generalized).

---

#### **Summary**
- **Preprocessing:** O(Σ|patterns|)
- **Search:** O(n + total matches)
- **Space Complexity:** O(Σ|patterns| · σ)

Used in antivirus signature matching, DNA scanning, lexers.

---

#### **Python Implementation**
```python
from collections import deque

class Aho:
    def __init__(self, patterns):
        self.goto = [{}]            # trie children
        self.fail = [0]
        self.output = [[]]          # patterns ending at this node
        for p in patterns:
            self._add(p)
        self._build()

    def _add(self, word):
        node = 0
        for ch in word:
            if ch not in self.goto[node]:
                self.goto.append({})
                self.fail.append(0)
                self.output.append([])
                self.goto[node][ch] = len(self.goto) - 1
            node = self.goto[node][ch]
        self.output[node].append(word)

    def _build(self):
        q = deque()
        for ch, nxt in self.goto[0].items():
            self.fail[nxt] = 0
            q.append(nxt)
        while q:
            r = q.popleft()
            for ch, u in self.goto[r].items():
                q.append(u)
                f = self.fail[r]
                while f and ch not in self.goto[f]:
                    f = self.fail[f]
                self.fail[u] = self.goto[f].get(ch, 0) if f or ch in self.goto[0] else 0
                self.output[u] += self.output[self.fail[u]]

    def search(self, text):
        node = 0
        hits = []
        for i, ch in enumerate(text):
            while node and ch not in self.goto[node]:
                node = self.fail[node]
            node = self.goto[node].get(ch, 0)
            for w in self.output[node]:
                hits.append((i - len(w) + 1, w))
        return hits

# Example
ac = Aho(["he", "she", "his", "hers"])
print(ac.search("ushers"))   # [(1,'she'), (2,'he'), (2,'hers')]
```

---

#### **When to Use**
✅ Many patterns scanned over the same text
✅ Antivirus, NLP keyword spotting, intrusion detection
🚫 Single pattern → KMP/Boyer-Moore are simpler
