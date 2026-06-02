# Reservoir Sampling 🎲

---

## 🧠 Intuition
Imagine you're watching a never-ending parade of people and you need to hand out exactly 3 raffle tickets so that every person who ever walks by has an equal chance of holding one — but you can't go back and you don't know how long the parade will last.

Reservoir sampling solves exactly this: maintain a "reservoir" of `k` items; as each new item arrives, decide probabilistically whether it earns a spot, bumping out a random current holder.

**Mental model:** Every item `i` (1-indexed) gets a fair lottery ticket: its probability of being in the final reservoir is always exactly `k / i` — proven by induction.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time | O(n) — one pass through the stream |
| Space | O(k) — only the reservoir is kept |
| Guarantee | Exactly uniform: every element has probability `k/n` of selection |
| Variant (Vitter Algorithm Z) | O(k + k · log(n/k)) skips using geometric distribution |

---

## ⚙️ How It Works

1. Fill the reservoir with the first `k` items from the stream.
2. For each subsequent item at (0-indexed) position `i` where `i >= k`:
   a. Generate a random integer `j` in `[0, i]` (inclusive).
   b. If `j < k`, replace `reservoir[j]` with the current item.
3. After the stream is exhausted, return the reservoir.

**Why it works:** When item `i` arrives, the probability it is selected is `k / (i+1)`. The probability any existing reservoir item survives is `1 - 1/(i+1) = i/(i+1)`. Combined across all steps, every item ends up with probability `k/n`.

---

## 🔢 Step-by-Step Trace

Stream: `[A, B, C, D, E, F]`, reservoir size `k = 3`

| Position (i) | Item | Action | Reservoir |
|---|---|---|---|
| 0 | A | i < k, add directly | [A, -, -] |
| 1 | B | i < k, add directly | [A, B, -] |
| 2 | C | i < k, add directly | [A, B, C] |
| 3 | D | j = rand(0..3); if j < 3, replace reservoir[j] | e.g. j=1 → [A, D, C] |
| 4 | E | j = rand(0..4); if j < 3, replace reservoir[j] | e.g. j=4 → no change → [A, D, C] |
| 5 | F | j = rand(0..5); if j < 3, replace reservoir[j] | e.g. j=2 → [A, D, F] |

Final reservoir: `[A, D, F]` (varies each run — all outcomes equally likely)

---

## 🐍 Python Implementation

```python
import random

def reservoir_sample(stream, k):
    """
    Uniformly sample k items from an iterable stream of unknown length.
    Uses Vitter's Algorithm R.
    """
    reservoir = []

    for i, item in enumerate(stream):
        if i < k:
            # Phase 1: fill the reservoir with the first k items
            reservoir.append(item)
        else:
            # Phase 2: each new item replaces a random slot with prob k/(i+1)
            j = random.randrange(i + 1)   # random int in [0, i]
            if j < k:
                reservoir[j] = item       # item earns a spot, evicting the old one

    return reservoir


# --- Runnable example ---
stream = range(1, 101)   # numbers 1..100, pretend we don't know length
sample = reservoir_sample(stream, 5)
print("Sample of 5 from 1..100:", sample)
# e.g. [73, 12, 55, 8, 91]  — changes every run, always exactly 5
```

---

## 🎯 Recognize This Problem When...

- You must select `k` random items from a **stream or iterator** whose total length is unknown or too large to store.
- Memory is constrained — you cannot buffer the entire input.
- The problem asks for a **uniform random sample** with a single pass.
- Data arrives continuously (log files, network packets, sensor feeds) and you need an up-to-date sample at any moment.
- The problem says "randomly pick from a list" and calls `random.choice` on a stream — that's a signal to use reservoir sampling instead.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|---|---|
| Sampling from a huge file that doesn't fit in memory | ✅ Perfect use case |
| Streaming data of unknown length (Kafka, stdin pipe) | ✅ Exactly what it's designed for |
| Distributed random sampling across shards | ✅ Each shard runs its own reservoir; merge with weighted sampling |
| You know `n` in advance and can afford O(n) memory | ❌ Simpler to use `random.sample(population, k)` |
| Weighted sampling (items have different probabilities) | ❌ Use weighted reservoir sampling (Algorithm A-Chao) instead |
| You need sampling without replacement from a small list | ❌ `random.sample()` is cleaner and faster |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|---|---|
| [Fisher-Yates Shuffle](../SortingAlgorithms/FisherYates.md) | Reservoir sampling of size `n` on a known list is equivalent to a partial Fisher-Yates shuffle |
| [Monte Carlo / Las Vegas](./MonteCarloLasVegas.md) | Reservoir sampling is a Las Vegas-style randomized algorithm (output always correct, time fixed) |
| [Skip Sampling (Vitter Algorithm Z)](https://en.wikipedia.org/wiki/Reservoir_sampling) | Optimization: skip items using geometric distribution instead of coin-flipping each one |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---|---|---|
| [Random Pick Index](https://leetcode.com/problems/random-pick-index/) | [LeetCode #398](https://leetcode.com/problems/random-pick-index/) | 🟡 Medium |
| [Linked List Random Node](https://leetcode.com/problems/linked-list-random-node/) | [LeetCode #382](https://leetcode.com/problems/linked-list-random-node/) | 🟡 Medium |
| [Random Pick with Weight](https://leetcode.com/problems/random-pick-with-weight/) | [LeetCode #528](https://leetcode.com/problems/random-pick-with-weight/) | 🟡 Medium |
| Sample k elements from a stream | Various competitive programming platforms | 🟡 Medium |
