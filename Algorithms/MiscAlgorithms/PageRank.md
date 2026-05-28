### **PageRank Algorithm 🛠**

Originally invented by Google: rank pages by how many "important" pages link to them. Models a random surfer who, with probability `d`, follows an outlink, and with probability `1−d` jumps to a random page.

---

#### **Formula**
```
PR(u) = (1 - d) / N  +  d · Σ ( PR(v) / outDeg(v) )      for v → u
```
`d` is the **damping factor** (typically 0.85).

---

#### **Summary**
- **Time Complexity:** O(k · (V + E)) where `k` is iterations to convergence
- **Space Complexity:** O(V)
- **Convergence:** until `||PR_new − PR_old||₁ < ε`

---

#### **Python Implementation**
```python
def pagerank(graph, d=0.85, eps=1e-8, max_iter=100):
    nodes = list(graph)
    N = len(nodes)
    pr = {n: 1.0 / N for n in nodes}

    # precompute in-links and out-degree
    in_links = {n: [] for n in nodes}
    for u in nodes:
        for v in graph[u]:
            in_links[v].append(u)
    out_deg = {u: len(graph[u]) for u in nodes}

    for _ in range(max_iter):
        new_pr = {}
        for n in nodes:
            s = sum(pr[u] / out_deg[u] for u in in_links[n] if out_deg[u] > 0)
            new_pr[n] = (1 - d) / N + d * s
        if sum(abs(new_pr[n] - pr[n]) for n in nodes) < eps:
            return new_pr
        pr = new_pr
    return pr

# Example
graph = {
    'A': ['B', 'C'],
    'B': ['C'],
    'C': ['A'],
    'D': ['C'],
}
print(pagerank(graph))
# e.g. {'A': 0.37..., 'B': 0.19..., 'C': 0.39..., 'D': 0.03...}
```

---

#### **When to Use**
✅ Ranking web pages, citations, social-network influence
✅ Recommender systems, importance scoring in graphs
✅ Foundation for personalized PageRank (random walks with restart)
