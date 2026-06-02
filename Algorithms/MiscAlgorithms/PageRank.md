# PageRank Algorithm 📊

---

## 🧠 Intuition

Imagine a random web surfer: they start on a random page, then keep clicking random links. Sometimes (with probability 1-d) they get bored and jump to a completely random page. The probability that the surfer is on any given page after a very long time is that page's **PageRank** — its importance.

Pages linked from many important pages get high scores. A link from Google's homepage is worth more than a link from an obscure blog.

**Mental model:** "Important pages are pointed to by other important pages. Run a random walk simulation — where the surfer spends most of their time = that page's rank."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time per iteration | O(V + E) |
| Total time | O(k × (V + E)) — k iterations until convergence |
| Space Complexity | O(V) |
| Convergence | Typically 20-100 iterations for ε = 10⁻⁸ |

> V = pages, E = links, k = iterations until convergence.

---

## ⚙️ How It Works

**PageRank formula:**
```
PR(u) = (1 - d) / N  +  d × Σ (PR(v) / out_degree(v))
         [teleport]              v→u [from all pages linking to u]
```

- `d` = damping factor (typically 0.85) — probability of following a link
- `1 - d` = probability of random teleport to any page
- `N` = total number of pages

**Algorithm (Power Iteration):**
1. Initialize all PR to `1/N`.
2. Repeatedly apply the formula to update each page's PR.
3. Stop when changes fall below threshold ε.

---

## 🔢 Step-by-Step Trace

4-page graph: A→B, A→C, B→C, C→A, D→C

d=0.85, N=4, start PR=[0.25, 0.25, 0.25, 0.25]

**Iteration 1:**
- PR(A) = 0.15/4 + 0.85 × (PR(C)/1) = 0.0375 + 0.85×0.25 = 0.25
- PR(B) = 0.15/4 + 0.85 × (PR(A)/2) = 0.0375 + 0.85×0.125 = 0.144
- PR(C) = 0.15/4 + 0.85 × (PR(A)/2 + PR(B)/1 + PR(D)/1) = 0.0375 + 0.85×(0.125+0.25+0.25) = 0.556
- PR(D) = 0.15/4 + 0 = 0.0375

Iterate until convergence → C gets highest rank (linked from A, B, and D).

---

## 🐍 Python Implementation

```python
def pagerank(graph, d=0.85, epsilon=1e-8, max_iter=100):
    """
    Compute PageRank for all nodes in a directed graph.
    graph: {node: [list of nodes it links TO]}
    d: damping factor (probability of following a link)
    Returns: dict {node: pagerank_score}
    """
    nodes = list(graph.keys())
    N = len(nodes)

    # Initialize equal PageRank for all nodes
    pr = {node: 1.0 / N for node in nodes}

    # Precompute in-links and out-degrees for efficiency
    in_links = {node: [] for node in nodes}
    out_degree = {node: len(graph[node]) for node in nodes}
    for node in nodes:
        for target in graph[node]:
            in_links[target].append(node)

    # Handle dangling nodes (nodes with no outgoing links)
    # They distribute their rank equally to all other nodes
    teleport = (1 - d) / N

    for iteration in range(max_iter):
        new_pr = {}
        dangling_sum = sum(pr[v] for v in nodes if out_degree[v] == 0)

        for node in nodes:
            # Contribution from in-links
            link_sum = sum(pr[v] / out_degree[v] for v in in_links[node] if out_degree[v] > 0)
            # Dangling node contribution
            dangling_contribution = d * dangling_sum / N
            new_pr[node] = teleport + d * link_sum + dangling_contribution

        # Check convergence (L1 norm of change)
        change = sum(abs(new_pr[v] - pr[v]) for v in nodes)
        pr = new_pr

        if change < epsilon:
            print(f"Converged after {iteration + 1} iterations")
            break

    # Normalize so scores sum to 1
    total = sum(pr.values())
    return {node: score / total for node, score in pr.items()}


# Example
graph = {
    'A': ['B', 'C'],
    'B': ['C'],
    'C': ['A'],
    'D': ['C'],    # D links to C but no one links to D
}

ranks = pagerank(graph)
print("\nPageRank scores:")
for page, score in sorted(ranks.items(), key=lambda x: -x[1]):
    print(f"  {page}: {score:.4f}")

# Expected: C has highest rank (linked from A, B, D)
# A has medium rank (linked from C, which is important)
# B has lower rank
# D has lowest rank (no incoming links)
```

---

## 🎯 Recognize This Problem When...

- You need to **rank nodes by their importance** in a directed graph.
- Importance is determined by who links to you AND how important those linkers are.
- Keywords: "web page ranking", "influence", "authority score", "random walk".
- Building a **recommendation system** or citation network analysis.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Web page / document ranking | ✅ Original use case |
| Social network influence scoring | ✅ Well-established application |
| Citation network analysis (academic papers) | ✅ Works on any directed graph |
| Unweighted, undirected graphs | ❌ Use simpler centrality measures |
| Real-time ranking (updates frequent) | ❌ PageRank is expensive to recompute |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [BFS](../GraphAlgorithms/BFS.md) | BFS explores graph breadth-first; PageRank assigns probability by link structure |
| [DFS](../GraphAlgorithms/DFS.md) | Both explore graph structure; different objectives |
| [Monte Carlo / Las Vegas](MonteCarloLasVegas.md) | PageRank simulates a random walk — Monte Carlo interpretation |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Find the Town Judge](https://leetcode.com/problems/find-the-town-judge/) | LeetCode | 🟢 Easy |
| [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/) | LeetCode | 🟡 Medium |
| [PageRank](https://www.hackerrank.com/challenges/pagerank/problem) | HackerRank | 🔴 Hard |
