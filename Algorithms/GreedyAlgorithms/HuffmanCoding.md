### **Huffman Coding 💰**

A **greedy** algorithm for **lossless data compression**. Frequent characters get shorter binary codes; rare ones get longer codes — based on a binary tree built from a min-heap.

---

#### **Summary**
- **Time Complexity:** O(n log n) where n = number of unique symbols
- **Space Complexity:** O(n)
- **Output:** prefix-free binary codes (no code is a prefix of another)

---

#### **How It Works**
1. Count frequency of each symbol.
2. Insert all symbols as nodes in a min-heap by frequency.
3. Pop the two smallest; merge them into a new node (sum of frequencies). Push back.
4. Repeat until one node remains — the root of the Huffman tree.
5. Traverse the tree: left = `0`, right = `1` → codes.

---

#### **Python Implementation**
```python
import heapq
from collections import Counter

class Node:
    def __init__(self, freq, char=None, left=None, right=None):
        self.freq, self.char, self.left, self.right = freq, char, left, right
    def __lt__(self, other): return self.freq < other.freq

def huffman_codes(text):
    freq = Counter(text)
    heap = [Node(f, c) for c, f in freq.items()]
    heapq.heapify(heap)

    while len(heap) > 1:
        a = heapq.heappop(heap)
        b = heapq.heappop(heap)
        heapq.heappush(heap, Node(a.freq + b.freq, None, a, b))

    root = heap[0]
    codes = {}
    def walk(node, code=""):
        if node is None: return
        if node.char is not None:
            codes[node.char] = code or "0"
            return
        walk(node.left, code + "0")
        walk(node.right, code + "1")
    walk(root)
    return codes

# Example
print(huffman_codes("abracadabra"))
# e.g. {'a': '0', 'b': '111', 'r': '10', 'c': '1101', 'd': '1100'}
```

---

#### **When to Use**
✅ File compression (used in zip, JPEG, MP3)
✅ Whenever symbol frequencies are skewed
🚫 Equal frequencies → no real gain
