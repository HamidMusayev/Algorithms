# Kruskal's Algorithm 🔗

---

## 🧠 Intuition

Imagine you have a list of all possible roads between cities and their construction costs. To connect all cities cheaply, you sort roads by cost and build the cheapest one first. If two cities being connected are already connected (directly or indirectly), you skip that road — it would just be redundant. You use a **Union-Find** data structure to efficiently track which cities are already connected.

**Mental model:** Sort all edges by weight. Greedily add the cheapest edge that doesn't create a cycle.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(E log E) — dominated by sorting |
| Space Complexity | O(V) — for the Union-Find structure |
| Works on | Undirected weighted graphs |
| Output | Minimum Spanning Tree (MST) |

> The Union-Find operations (find + union) are nearly O(1) amortized with path compression and union by rank.

---

## ⚙️ How It Works

1. Create a **DSU (Disjoint Set Union)** with V nodes, each in its own set.
2. **Sort all edges** by weight in ascending order.
3. For each edge `(u, v, w)` from cheapest to most expensive:
   - If `find(u) ≠ find(v)` → they are in different components → **safe to add**.
     - Union the two components.
     - Add edge to MST, accumulate weight.
   - If `find(u) == find(v)` → adding this edge would create a **cycle** → skip.
4. Stop when MST has V-1 edges.

---

## 🔢 Step-by-Step Trace

Edges (sorted): `(0,1,1), (1,2,1), (2,3,1), (0,2,3), (1,3,4)`

| Step | Edge    | find(u)==find(v)? | Action       | MST             |
|------|---------|--------------------|--------------|-----------------| 
| 1    | (0,1,1) | No                 | Add to MST   | {0-1}           |
| 2    | (1,2,1) | No                 | Add to MST   | {0-1, 1-2}      |
| 3    | (2,3,1) | No                 | Add to MST   | {0-1,1-2,2-3}   |
| 4    | (0,2,3) | Yes (0,1,2 connected)| Skip (cycle) | unchanged      |
| 5    | (1,3,4) | Yes (all connected)| Skip (cycle) | unchanged      |

MST: edges (0,1,1), (1,2,1), (2,3,1). Total weight = 3. ✅

---

## 🐍 Python Implementation

```python
class DSU:
    """Disjoint Set Union with path compression and union by rank."""
    def __init__(self, n):
        self.parent = list(range(n))   # Each node is its own parent initially
        self.rank = [0] * n

    def find(self, x):
        # Path compression: flatten the tree while finding root
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        """Merge sets of x and y. Returns False if already in same set (cycle)."""
        rx, ry = self.find(x), self.find(y)
        if rx == ry:
            return False          # Same component → edge would create a cycle
        # Union by rank: attach smaller tree under larger
        if self.rank[rx] < self.rank[ry]:
            rx, ry = ry, rx
        self.parent[ry] = rx
        if self.rank[rx] == self.rank[ry]:
            self.rank[rx] += 1
        return True


def kruskal(V, edges):
    """
    V: number of vertices
    edges: list of (u, v, weight)
    Returns (mst_edges, total_weight)
    """
    edges.sort(key=lambda e: e[2])    # Sort by edge weight (ascending)
    dsu = DSU(V)
    mst_edges = []
    total_weight = 0

    for u, v, w in edges:
        if dsu.union(u, v):           # If not creating a cycle
            mst_edges.append((u, v, w))
            total_weight += w
            if len(mst_edges) == V - 1:  # MST complete
                break

    return mst_edges, total_weight


# Example
edges = [(0, 1, 1), (0, 2, 3), (1, 2, 1), (1, 3, 4), (2, 3, 1)]
mst, total = kruskal(4, edges)
print("MST edges:", mst)
# [(0, 1, 1), (1, 2, 1), (2, 3, 1)]
print("Total weight:", total)
# 3
```

---

## 🎯 Recognize This Problem When...

- You need to **connect all nodes with minimum total edge weight**.
- The graph is represented as an **edge list** (Kruskal works directly on edge lists).
- The graph is **sparse** (few edges relative to nodes).
- Keywords: "minimum cost network", "minimum spanning tree", "connect all nodes cheaply".

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                  | Verdict                                       |
|--------------------------------------------|-----------------------------------------------|
| Sparse graphs (E close to V)               | ✅ Kruskal excels here                        |
| Graph given as edge list                   | ✅ No adjacency list conversion needed        |
| Need MST for disconnected graph check      | ✅ Number of MST edges < V-1 means disconnected|
| Dense graphs (E close to V²)               | ❌ Prim's is more efficient                   |
| Directed graphs                            | ❌ Use minimum spanning arborescence          |

---

## 🔗 Related Algorithms

| Algorithm                              | How it relates                                                |
|----------------------------------------|---------------------------------------------------------------|
| [Prim's Algorithm](Prim.md)            | Also finds MST; grows from a node instead of sorting all edges|
| [Union-Find / DSU](../MiscAlgorithms/HopcroftKarp.md) | The core data structure powering Kruskal  |
| [Dijkstra](Dijkstra.md)                | Shortest path, not MST; different objective                   |

---

## 📝 Practice Problems

| Problem                              | Platform   | Difficulty |
|-------------------------------------|------------|------------|
| Min Cost to Connect All Points      | [LeetCode 1584](https://leetcode.com/problems/min-cost-to-connect-all-points/) | 🟡 Medium |
| Connecting Cities With Minimum Cost | [LeetCode 1135](https://leetcode.com/problems/connecting-cities-with-minimum-cost/) | 🟡 Medium |
| Number of Operations to Make Network Connected | [LeetCode 1319](https://leetcode.com/problems/number-of-operations-to-make-network-connected/) | 🟡 Medium |
| Kruskal MST                         | [HackerRank](https://www.hackerrank.com/challenges/kruskalmstrsub/problem) | 🟡 Medium |
