# Bellman-Ford Algorithm ⚖️

---

## 🧠 Intuition

Imagine finding the cheapest flight route, but some airlines offer promotions that make certain legs effectively free or even cost-negative. Dijkstra would fail here. Bellman-Ford instead **tries to improve (relax) every single edge V-1 times**. After the k-th round, all nodes reachable in at most k hops have their correct shortest distance.

**Mental model:** Hammer every edge V-1 times — the shortest paths "settle" round by round, like water finding its level.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(V × E) |
| Space Complexity | O(V) |
| Works with negative edges | ✅ Yes |
| Detects negative cycles | ✅ Yes |
| Single-source only | ✅ (use Floyd-Warshall for all-pairs) |

> Slower than Dijkstra but handles cases Dijkstra cannot — negative weights and cycle detection.

---

## ⚙️ How It Works

1. Initialize: `dist[source] = 0`, `dist[everything else] = ∞`.
2. **Relax all edges V-1 times:**
   - For every edge `(u, v, w)`: if `dist[u] + w < dist[v]`, update `dist[v] = dist[u] + w`.
3. **Negative cycle check — one more full pass:**
   - If any edge can still be relaxed, a negative cycle exists and shortest paths are undefined.

> **Why V-1?** The shortest path in a graph with V nodes visits at most V-1 edges (more would mean revisiting a node = a cycle).

---

## 🔢 Step-by-Step Trace

Graph edges: `A→B(4), A→C(5), B→C(-3), C→D(4)`, source = A

| Round | Edge        | Action            | dist: A | B  | C  | D  |
|-------|-------------|-------------------|---------|----|----|----|
| Init  | —           | —                 | 0       | ∞  | ∞  | ∞  |
| 1     | A→B (4)     | 0+4=4 < ∞        | 0       | 4  | ∞  | ∞  |
| 1     | A→C (5)     | 0+5=5 < ∞        | 0       | 4  | 5  | ∞  |
| 1     | B→C (-3)    | 4-3=1 < 5        | 0       | 4  | 1  | ∞  |
| 1     | C→D (4)     | 1+4=5 < ∞        | 0       | 4  | 1  | 5  |
| 2-3   | No changes  | Stable            | 0       | 4  | 1  | 5  |

Final: **A=0, B=4, C=1, D=5** ✅

---

## 🐍 Python Implementation

```python
def bellman_ford(vertices, edges, source):
    """
    vertices: list of node names
    edges: list of (u, v, weight) tuples
    Returns (dist dict, has_negative_cycle bool)
    """
    dist = {v: float('inf') for v in vertices}
    dist[source] = 0
    pred = {v: None for v in vertices}    # Track predecessors for path reconstruction

    # Relax all edges V-1 times
    for _ in range(len(vertices) - 1):
        updated = False
        for u, v, w in edges:
            if dist[u] != float('inf') and dist[u] + w < dist[v]:
                dist[v] = dist[u] + w    # Relax edge (u, v)
                pred[v] = u
                updated = True
        if not updated:
            break                        # Early exit — no changes means we're done

    # Negative cycle check: one more full pass
    has_negative_cycle = any(
        dist[u] != float('inf') and dist[u] + w < dist[v]
        for u, v, w in edges
    )

    return dist, has_negative_cycle, pred


def get_path(pred, source, target):
    """Reconstruct path from source to target using predecessor map."""
    path, curr = [], target
    while curr is not None:
        path.append(curr)
        curr = pred[curr]
    path.reverse()
    return path if path[0] == source else []


# Example
vertices = ['A', 'B', 'C', 'D']
edges = [('A', 'B', 4), ('A', 'C', 5), ('B', 'C', -3), ('C', 'D', 4)]

dist, neg_cycle, pred = bellman_ford(vertices, edges, 'A')
print("Distances:", dist)
# {'A': 0, 'B': 4, 'C': 1, 'D': 5}
print("Negative cycle:", neg_cycle)
# False
print("Path A→D:", get_path(pred, 'A', 'D'))
# ['A', 'B', 'C', 'D']
```

---

## 🎯 Recognize This Problem When...

- The graph has **negative edge weights** (Dijkstra would give wrong answers).
- You need to **detect a negative cycle** (arbitrage, contradiction in constraints).
- Keywords: "currency exchange", "arbitrage", "some edges have negative cost".
- The graph is **sparse** (E is small), making O(VE) manageable.
- Constraint satisfaction problems modeled as graphs (e.g., difference constraints).

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                  | Verdict                                       |
|--------------------------------------------|-----------------------------------------------|
| Graph with negative edge weights           | ✅ Only option among simple algorithms        |
| Need to detect negative cycles             | ✅ One extra pass reveals it                  |
| Currency arbitrage detection               | ✅ Log-transform prices → negative cycle = profit|
| All edges non-negative                     | ❌ Dijkstra is O((V+E)log V) — much faster    |
| All-pairs shortest paths needed            | ❌ Use Floyd-Warshall                         |
| Very dense graphs (E ≈ V²)                 | ❌ O(V³) performance, very slow               |

---

## 🔗 Related Algorithms

| Algorithm                              | How it relates                                                |
|----------------------------------------|---------------------------------------------------------------|
| [Dijkstra](Dijkstra.md)                | Faster for non-negative edges; Bellman-Ford is the fallback   |
| [Floyd-Warshall](FloydWarshall.md)     | All-pairs shortest paths; also handles negative edges         |
| [BFS](BFS.md)                          | Shortest path in unweighted graphs                            |

---

## 📝 Practice Problems

| Problem                              | Platform   | Difficulty |
|-------------------------------------|------------|------------|
| Network Delay Time                  | [LeetCode 743](https://leetcode.com/problems/network-delay-time/) | 🟡 Medium |
| Cheapest Flights Within K Stops     | [LeetCode 787](https://leetcode.com/problems/find-the-cheapest-flights-within-k-stops/) | 🟡 Medium |
| Bellman Ford                        | [HackerRank](https://www.hackerrank.com/challenges/bellman-ford-mssp/problem) | 🟡 Medium |
