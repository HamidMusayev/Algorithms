### **Karger's Algorithm 🛠**

A **randomized** algorithm to find the **minimum cut** of an undirected graph — the smallest set of edges whose removal disconnects the graph.

**Repeatedly contract a random edge** until only 2 super-nodes remain. The edges between them form a cut (the min cut with high probability after enough trials).

---

#### **Summary**
- **One run:** O(V²)
- **Success probability per run:** ≥ 2 / (V·(V−1))
- **Run O(V² log V)** times for high probability → O(V⁴ log V)
- **Karger-Stein** variant: O(V² log³ V)

---

#### **Python Implementation**
```python
import random, copy

def karger_min_cut(graph):
    # graph = {node: [neighbors with multiplicity]}
    g = copy.deepcopy(graph)
    while len(g) > 2:
        u = random.choice(list(g.keys()))
        v = random.choice(g[u])
        # contract v into u
        g[u].extend(g[v])
        for n in g[v]:
            g[n] = [u if x == v else x for x in g[n]]
        g[u] = [x for x in g[u] if x != u]    # remove self-loops
        del g[v]
    u, v = g.keys()
    return len(g[next(iter(g))])

def min_cut(graph, trials=100):
    return min(karger_min_cut(graph) for _ in range(trials))

# Example
graph = {
    1: [2, 3, 4],
    2: [1, 3],
    3: [1, 2, 4],
    4: [1, 3]
}
print(min_cut(graph))   # 2
```

---

#### **When to Use**
✅ Network reliability, image segmentation
✅ Approximation when exact polynomial cut algorithms (Stoer-Wagner) are unavailable
🚫 Need a single deterministic answer → use Stoer-Wagner (O(V·E + V² log V))
