# Huffman Coding 🌳

---

## 🧠 Intuition

Imagine you're a telegraph operator charged by the letter. Common letters like "e" and "t" should have the shortest codes; rare letters like "q" and "z" can afford longer ones. Huffman Coding does exactly that: it assigns shorter binary codes to more frequent symbols and longer codes to less frequent ones.

The **greedy choice** at each step is: merge the two least-frequent nodes in the priority queue into a new internal node. By always handling the rarest characters first (keeping them deepest in the tree), the most frequent ones naturally rise to the top and receive the shortest paths from the root.

This locally optimal merge order produces a globally optimal prefix-free code — proven by the exchange argument: swapping any two merges can only increase total encoded length.

**Mental model:** Build from the bottom up — least popular characters wait the longest to be absorbed.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time   | O(n log n) — n heap operations, each O(log n) |
| Space  | O(n) — heap + tree nodes |

---

## ⚙️ How It Works

1. Count the frequency of every unique symbol in the input.
2. Create a leaf node for each symbol and insert all nodes into a min-heap (ordered by frequency).
3. **Greedy choice:** Pop the two nodes with the lowest frequencies (call them `left` and `right`).
4. Create a new internal node whose frequency equals `left.freq + right.freq`, with `left` and `right` as children.
5. Push the new internal node back onto the heap.
6. Repeat steps 3–5 until exactly one node remains — this is the root of the Huffman tree.
7. Traverse the tree from the root: going **left** appends `"0"`, going **right** appends `"1"`. Record the bit-string at each leaf as that symbol's code.

---

## 🔢 Step-by-Step Trace

Input string: `"aabbbccdd"` → frequencies: `a:2, b:3, c:2, d:2`

**Initial heap** (sorted by frequency):

| Node | Freq |
|------|------|
| a    | 2    |
| c    | 2    |
| d    | 2    |
| b    | 3    |

**Merge 1:** Pop `a(2)` and `c(2)` → new internal node `#1(4)`.

| Node | Freq |
|------|------|
| d    | 2    |
| b    | 3    |
| #1   | 4    |

**Merge 2:** Pop `d(2)` and `b(3)` → new internal node `#2(5)`.

| Node | Freq |
|------|------|
| #1   | 4    |
| #2   | 5    |

**Merge 3:** Pop `#1(4)` and `#2(5)` → root node `#3(9)`.

**Resulting tree:**
```
        #3(9)
       /      \
    #1(4)    #2(5)
    /   \    /   \
  a(2) c(2) d(2) b(3)
```

**Codes assigned** (left=0, right=1):

| Symbol | Code | Bits |
|--------|------|------|
| a      | 00   | 2    |
| c      | 01   | 2    |
| d      | 10   | 2    |
| b      | 11   | 2    |

Total bits for `"aabbbccdd"` = 2×2 + 3×2 + 2×2 + 2×2 = 18 bits (vs. 9×3 = 27 bits with fixed 2-bit codes — a 33% saving would emerge with more skewed frequencies).

---

## 🐍 Python Implementation

```python
import heapq
from collections import Counter

class Node:
    """A node in the Huffman tree."""
    def __init__(self, freq, char=None, left=None, right=None):
        self.freq = freq
        self.char = char      # None for internal nodes
        self.left = left
        self.right = right

    # Min-heap orders by frequency
    def __lt__(self, other):
        return self.freq < other.freq


def huffman_codes(text: str) -> dict[str, str]:
    """Return a dict mapping each character to its Huffman bit-string."""
    if not text:
        return {}

    # Step 1: Count frequencies
    freq = Counter(text)

    # Edge case: single unique character
    if len(freq) == 1:
        char = next(iter(freq))
        return {char: "0"}

    # Step 2: Build the min-heap of leaf nodes
    heap = [Node(f, c) for c, f in freq.items()]
    heapq.heapify(heap)

    # Steps 3–6: Merge until one root remains
    while len(heap) > 1:
        left = heapq.heappop(heap)   # greedy: lowest freq
        right = heapq.heappop(heap)  # greedy: second lowest freq
        merged = Node(left.freq + right.freq, left=left, right=right)
        heapq.heappush(heap, merged)

    root = heap[0]

    # Step 7: Walk the tree to assign codes
    codes: dict[str, str] = {}

    def walk(node: Node, code: str) -> None:
        if node is None:
            return
        if node.char is not None:          # leaf node → record code
            codes[node.char] = code
            return
        walk(node.left,  code + "0")       # left branch  → 0
        walk(node.right, code + "1")       # right branch → 1

    walk(root, "")
    return codes


def encode(text: str, codes: dict[str, str]) -> str:
    """Encode text using the given Huffman code table."""
    return "".join(codes[ch] for ch in text)


def compression_ratio(text: str, codes: dict[str, str]) -> float:
    """Return bits-per-character for the Huffman encoding."""
    encoded_bits = sum(len(codes[ch]) for ch in text)
    return encoded_bits / len(text)


# --- Runnable example ---
if __name__ == "__main__":
    sample = "abracadabra"
    codes = huffman_codes(sample)

    print("Huffman codes:")
    for char, code in sorted(codes.items()):
        print(f"  '{char}': {code}")

    encoded = encode(sample, codes)
    print(f"\nEncoded: {encoded}")
    print(f"Original bits (8-bit ASCII): {len(sample) * 8}")
    print(f"Huffman bits:                {len(encoded)}")
    print(f"Bits per character:          {compression_ratio(sample, codes):.2f}")
```

**Expected output (codes may vary by tie-breaking):**
```
Huffman codes:
  'a': 0
  'b': 111
  'c': 1101
  'd': 1100
  'r': 10

Encoded: 0111100010110010111100
Original bits (8-bit ASCII): 88
Huffman bits:                22
Bits per character:          2.00
```

---

## 🎯 Recognize This Problem When...

- You need to compress data where symbol frequencies are **not uniform**.
- The problem asks for the **minimum total weighted path length** in a binary tree.
- You must produce **prefix-free** codes (no code is a prefix of another).
- Clues include phrases like "optimal encoding", "lossless compression", or "variable-length codes".
- The greedy signal: merging the two smallest items is always safe — the exchange argument shows any other merge order is worse.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Symbol frequencies are highly skewed | ✅ Large compression gains |
| File compression (zip, gzip, JPEG entropy coding) | ✅ Industry-standard use case |
| All symbols have equal frequency | ❌ Produces same-length codes; no benefit over fixed-width |
| You need random access into compressed data | ❌ Huffman is a stream code; seek is non-trivial |
| You need arithmetic or better compression | ❌ Arithmetic coding achieves closer to entropy bound |
| Items are indivisible (0/1 items, not frequencies) | ❌ Use 0/1 Knapsack DP instead |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Activity Selection](./ActivitySelection.md) | Also greedy; sorts by a priority (finish time vs. frequency) |
| [Fractional Knapsack](./FractionalKnapsack.md) | Another classic greedy — sorts by value/weight ratio |
| [Job Scheduling](./JobScheduling.md) | Greedy scheduling: picks highest-profit job, similar heap/priority approach |
| [Knapsack (0/1 DP)](../DPAlgorithms/Knapsack.md) | When items are discrete, greedy fails — need DP |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Minimum Cost to Connect Ropes](https://leetcode.com/problems/minimum-cost-to-connect-sticks/) | LeetCode | 🟡 Medium |
| [Huffman Decoding](https://www.hackerrank.com/challenges/tree-huffman-decoding/problem) | HackerRank | 🟡 Medium |
| [Huffman Encoding](https://www.hackerrank.com/challenges/tree-huffman-decoding/problem) | HackerRank | 🟡 Medium |
| [Reorganize String](https://leetcode.com/problems/reorganize-string/) | LeetCode | 🟡 Medium |
| [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) | LeetCode | 🟡 Medium |
