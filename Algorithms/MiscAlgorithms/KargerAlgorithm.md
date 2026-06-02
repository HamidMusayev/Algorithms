# Karger's Algorithm ✂️

---

## 🧠 Intuition

The **minimum cut** of a graph is the smallest set of edges whose removal disconnects the graph. Think of it as finding the weakest link in a network — the bottleneck that, if severed, splits the network.

Karger's randomized approach: repeatedly pick a random edge and **contract** it (merge the two endpoints into a supernode, keeping all other edges but removing self-loops). After n-2 contractions, exactly 2 supernodes remain. The edges between them form a cut. Repeat many times and take the minimum — probability guarantees that the true min-cut is found.

**Mental model:** "Randomly merge nodes until 2 remain. The remaining edges are a cut. Do this many times; the smallest cut seen is likely the true minimum."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Single run | O(V²) |
| Success probability per run | ≥ 2 / (V × (V-1)) |
| Runs needed for high confidence | O(V² log V) |
| Total time | O(V⁴ log V) — or O(V² log³ V) with Karger-Stein |

---

## ⚙️ How It Works

**One run:**
1. Start with the original graph.
2. While more than 2 supernodes remain:
   - Pick a **random edge** `(u, v)`.
   - **Contract**: merge v into u. All edges to v now point to u. Remove self-loops (edges within the merged supernode).
3. Count edges between the remaining 2 supernodes — this is the cut size.

**Multiple runs for correctness:**
- Run the algorithm many times (O(V² log V) times).
- Return the minimum cut found across all runs.

---

## 🔢 Step-by-Step Trace

Graph: 4 nodes `{1,2,3,4}`, edges: `1-2, 2-3, 3-4, 4-1, 1-3`

```
Start: {1,2,3,4} with 5 edges

Pick random edge 1-3 → Contract: merge 3 into 1
Supernodes: {1,3} has edges to: 2 (two edges: 1-2 and 3-2), 4 (two edges: 3-4 and 4-1)
Remove self-loop 1-3.

Pick random edge {1,3}-4 → Contract: merge 4 into {1,3}
Supernodes: {1,3,4} has edges to: 2 (two edges: 1-2 and 3-2, both still)
Remove self-loops (4-1 and 3-4 become self-loops).

2 supernodes remain: {1,3,4} and {2}
Edges between them: 2
→ Cut size = 2
```

True min-cut of this graph: 2 (cutting edges 1-2 and 3-2) ✅

---

## 🐍 Python Implementation

```python
import random
import copy

def karger_one_run(graph):
    """
    Run one iteration of Karger's algorithm.
    graph: dict {node: [neighbors]} — supports multi-edges as duplicates
    Returns: number of edges in the cut (between the 2 remaining supernodes)
    """
    g = copy.deepcopy(graph)

    while len(g) > 2:
        # Pick a random node u and a random neighbor v
        u = random.choice(list(g.keys()))
        v = random.choice(g[u])

        # Contract v into u: all of v's edges become u's edges
        g[u].extend(g[v])

        # Update all neighbors of v to point to u instead
        for neighbor in g[v]:
            g[neighbor] = [u if x == v else x for x in g[neighbor]]

        # Remove self-loops (edges within the merged supernode)
        g[u] = [x for x in g[u] if x != u]

        # Remove v from graph
        del g[v]

    # Return the number of edges between the 2 remaining supernodes
    remaining = list(g.keys())
    return len(g[remaining[0]])


def min_cut(graph, num_trials=None):
    """
    Find minimum cut using repeated Karger's algorithm.
    num_trials: number of runs (default: V² × ln(V) for good probability)
    """
    n = len(graph)
    if num_trials is None:
        import math
        num_trials = max(100, n * n * int(math.log(n) + 1))

    best = float('inf')
    for _ in range(num_trials):
        cut = karger_one_run(graph)
        best = min(best, cut)

    return best


# Example
graph = {
    1: [2, 3, 4],
    2: [1, 3],
    3: [1, 2, 4],
    4: [1, 3]
}
print("Minimum cut:", min_cut(graph, trials=200))
# Expected: 2 (cutting edges 1-2 and 1-4, or 2-3 and 3-4, etc.)
```

---

## 🎯 Recognize This Problem When...

- You need to find the **minimum cut** of an undirected graph.
- The problem involves finding the **weakest bottleneck** in a network.
- Keywords: "minimum cut", "network reliability", "graph partition", "separator".
- You need an approximate answer quickly (Karger is randomized, not exact per run).

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Min-cut in undirected graphs | ✅ Simple to implement, randomized |
| Network reliability analysis | ✅ Classic application |
| Image segmentation | ✅ Model as graph, find min-cut |
| Need deterministic result | ❌ Use Stoer-Wagner O(V³) or max-flow |
| Directed graphs | ❌ Use max-flow/min-cut (Ford-Fulkerson family) |
| Large graphs (V > 1000) | ❌ Too slow without Karger-Stein variant |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Floyd's Cycle Detection](FloydCycleDetection.md) | Both are randomized algorithms |
| [Monte Carlo / Las Vegas](MonteCarloLasVegas.md) | Karger is a Monte Carlo algorithm (may give wrong answer per run) |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| Min Cut | Various competitive programming | 🔴 Hard |
| [Network Flow](https://www.hackerrank.com/challenges/cut-the-tree/problem) | HackerRank | 🟡 Medium |
