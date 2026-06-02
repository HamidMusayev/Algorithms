# Aho-Corasick Algorithm 🕸️

---

## 🧠 Intuition

KMP searches for ONE pattern in a text. What if you have 1000 patterns to find simultaneously? Running KMP 1000 times is O(1000 × n). Aho-Corasick does it in ONE pass over the text — O(n + total matches).

The idea: build a **trie** (prefix tree) of all patterns. Then add **failure links** (like KMP's failure function but generalized to the trie) so that when you fail to match at a trie node, you instantly know where to continue without re-reading the text.

**Mental model:** "A trie of all patterns + KMP-style failure links = search for all patterns in one text scan."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Preprocessing | O(Σ|patterns| × σ) — σ = alphabet size |
| Search | O(n + total output matches) |
| Space | O(Σ|patterns| × σ) |

---

## ⚙️ How It Works

**Build phase:**
1. Insert all patterns into a **trie**.
2. Compute **failure links** (via BFS): for each node, the failure link points to the longest proper suffix of the current path that is also a prefix in the trie.
3. Compute **output links**: propagate pattern hits through failure links (a node might be the end of multiple patterns via failure links).

**Search phase:**
4. Walk the text character by character through the automaton.
5. At each position, follow the current node's character edge (or failure links until you find one).
6. Collect all patterns ending at the current position (via output links).

---

## 🔢 Step-by-Step Trace

Patterns: `["he", "she", "his", "hers"]`, Text: `"ushers"`

Trie built:
```
root → h → e → [OUTPUT: "he"]
             → r → s → [OUTPUT: "hers"]
     → s → h → e → [OUTPUT: "she"]
     → h → i → s → [OUTPUT: "his"]
```

Scan `"ushers"`:
- u: no match → stay at root
- s: root → s node
- h: s → sh node
- e: sh → she node → **OUTPUT: "she"** (and via failure link → he node → **OUTPUT: "he"**)
- r: she → ... → hers path
- s: → **OUTPUT: "hers"**

Results: `[(1, "she"), (2, "he"), (2, "hers")]` ✅

---

## 🐍 Python Implementation

```python
from collections import deque

class AhoCorasick:
    def __init__(self, patterns):
        # Trie structure: list of dicts (children), failure links, and output
        self.goto = [{}]         # goto[node][char] = next_node
        self.fail = [0]          # Failure link for each node
        self.output = [[]]       # Patterns that end at each node

        # Build trie
        for pattern in patterns:
            self._insert(pattern)

        # Build failure links and propagate outputs
        self._build_failure_links()

    def _insert(self, word):
        """Insert a pattern into the trie."""
        node = 0
        for char in word:
            if char not in self.goto[node]:
                self.goto.append({})       # Create new node
                self.fail.append(0)
                self.output.append([])
                self.goto[node][char] = len(self.goto) - 1
            node = self.goto[node][char]
        self.output[node].append(word)     # Mark end of pattern

    def _build_failure_links(self):
        """BFS to compute failure links and propagate output sets."""
        q = deque()
        # Initialize: all depth-1 nodes fail back to root
        for char, child in self.goto[0].items():
            self.fail[child] = 0
            q.append(child)

        while q:
            node = q.popleft()
            for char, child in self.goto[node].items():
                q.append(child)
                # Follow failure links to find where this child's suffix leads
                fail = self.fail[node]
                while fail and char not in self.goto[fail]:
                    fail = self.fail[fail]
                self.fail[child] = self.goto[fail].get(char, 0)
                if self.fail[child] == child:    # Avoid self-loops
                    self.fail[child] = 0
                # Propagate outputs from failure link
                self.output[child] += self.output[self.fail[child]]

    def search(self, text):
        """
        Search text for all patterns simultaneously.
        Returns list of (end_index, matched_pattern) tuples.
        """
        node = 0
        results = []

        for i, char in enumerate(text):
            # Follow failure links until we find a match or reach root
            while node and char not in self.goto[node]:
                node = self.fail[node]
            node = self.goto[node].get(char, 0)

            # Collect all patterns ending at this position
            for pattern in self.output[node]:
                start = i - len(pattern) + 1
                results.append((start, pattern))

        return results


# Example
ac = AhoCorasick(["he", "she", "his", "hers"])
matches = ac.search("ushers")
print("Matches:", matches)
# [(1, 'she'), (2, 'he'), (2, 'hers')]
```

---

## 🎯 Recognize This Problem When...

- You need to search for **many patterns simultaneously** in a single text scan.
- Keywords: "find all keywords in document", "antivirus scanning", "multi-pattern search".
- The text is very long and running KMP for each pattern separately would be too slow.
- You're building a keyword filter, intrusion detection system, or DNA scanner.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Many patterns (10+) in a long text | ✅ O(n + matches) regardless of pattern count |
| Antivirus, intrusion detection, keyword spotting | ✅ Industry standard |
| Preprocessing patterns once, scanning many texts | ✅ Build automaton once, reuse |
| Single pattern | ❌ Use KMP or Z-algorithm — simpler |
| Patterns of equal length | ❌ Rabin-Karp with hash set may be simpler |
| Approximate/fuzzy matching | ❌ Aho-Corasick only finds exact matches |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [KMP](KMP.md) | Aho-Corasick generalizes KMP to multiple patterns |
| [Z-Algorithm](ZAlgorithm.md) | Single pattern only; simpler |
| [Rabin-Karp](RabinKarp.md) | Multi-pattern of equal length; hash-based |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Multi Pattern Search](https://www.hackerrank.com/challenges/aho-corasick/problem) | HackerRank | 🔴 Hard |
| [Stream of Characters](https://leetcode.com/problems/stream-of-characters/) | LeetCode | 🔴 Hard |
| [Word Search II](https://leetcode.com/problems/word-search-ii/) | LeetCode | 🔴 Hard |
