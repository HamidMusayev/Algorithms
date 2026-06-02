# Prim's Algorithm 🌿

---

## 🧠 Intuition

Imagine you're building a cable TV network for a neighborhood. You start at one house and always connect the next **cheapest unconnected house** to your existing network. You never add a cable that creates a loop (since that would be wasteful). This is exactly Prim's algorithm.

**Mental model:** Grow the MST like a plant — always extend with the cheapest edge that connects a new node to the existing tree.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O((V + E) log V) with binary heap |
| Space Complexity | O(V + E) |
| Works on | Undirected weighted graphs |
| Output | Minimum Spanning Tree (MST) |

> With a Fibonacci heap, time improves to O(E + V log V). For dense graphs (E ≈ V²), a simple O(V²) matrix-based version is also practical.

---

## ⚙️ How It Works

1. Start from any arbitrary node (mark it as visited).
2. Push all edges from this node onto a **min-heap** (sorted by weight).
3. While the heap is not empty and the MST is incomplete:
   - Pop the cheapest edge `(weight, u, v)`.
   - If `v` is **already visited**, skip (would create a cycle).
   - Otherwise: add edge to MST, mark `v` visited, push all edges from `v` onto the heap.
4. Stop when all V nodes are in the MST (V-1 edges added).

---

## 🔢 Step-by-Step Trace

Graph:
```
A --(1)-- B --(4)-- D
|         |
(3)      (1)
|         |
C --------+
```
Start at **A**:

| Step | Heap (min)           | Edge Added  | MST Nodes    |
|------|---------------------|-------------|--------------|
| Init | [(1,A,B),(3,A,C)]   | —           | {A}          |
| 1    | [(1,B,C),(3,A,C),(4,B,D)] | A-B (1) | {A, B}    |
| 2    | [(1,B,C),(3,A,C),(4,B,D)] | B-C (1) | {A, B, C} |
| 3    | [(3,A,C),(4,B,D)]   | B-D (4)     | {A,B,C,D}   |

Note: A-C(3) is skipped because C is already in MST.

MST edges: A-B(1), B-C(1), B-D(4). Total weight = 6.

---

## 🐍 Python Implementation

```python
import heapq

def prim(graph, start):
    """
    graph: { node: [(neighbor, weight), ...] }
    Returns (mst_edges, total_weight)
    """
    visited = {start}
    # Heap stores (weight, from_node, to_node)
    heap = [(w, start, v) for v, w in graph[start]]
    heapq.heapify(heap)

    mst_edges = []
    total_weight = 0

    while heap and len(visited) < len(graph):
        w, u, v = heapq.heappop(heap)   # Cheapest edge in the heap

        if v in visited:                 # Skip if destination already in MST
            continue

        # Add this edge to MST
        visited.add(v)
        mst_edges.append((u, v, w))
        total_weight += w

        # Push all edges from the newly added node
        for neighbor, weight in graph[v]:
            if neighbor not in visited:
                heapq.heappush(heap, (weight, v, neighbor))

    return mst_edges, total_weight


# Example
graph = {
    'A': [('B', 1), ('C', 3)],
    'B': [('A', 1), ('C', 1), ('D', 4)],
    'C': [('A', 3), ('B', 1)],
    'D': [('B', 4)]
}
mst, total = prim(graph, 'A')
print("MST edges:", mst)
# [('A', 'B', 1), ('B', 'C', 1), ('B', 'D', 4)]
print("Total weight:", total)
# 6
```

---

## 🎯 Recognize This Problem When...

- You need to **connect all nodes with minimum total edge weight** (Minimum Spanning Tree).
- Keywords: "minimum cost to connect all cities", "minimum cable/wire/pipeline to connect all nodes".
- The graph is **dense** (Prim performs well on dense graphs).
- You want to **start from a specific node** and grow outward.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                  | Verdict                                       |
|--------------------------------------------|-----------------------------------------------|
| Dense graphs (E close to V²)               | ✅ Prim is efficient here                     |
| Need MST starting from a specific node     | ✅ Natural starting point                     |
| Sparse graphs (E close to V)               | ❌ Kruskal's is simpler and faster            |
| Directed graphs                            | ❌ Use minimum spanning arborescence instead  |
| Graph may be disconnected                  | ❌ MST requires a connected graph             |

---

## 🔗 Related Algorithms

| Algorithm                              | How it relates                                                |
|----------------------------------------|---------------------------------------------------------------|
| [Kruskal's Algorithm](Kruskal.md)      | Also finds MST; sorts edges globally instead of growing tree  |
| [Dijkstra](Dijkstra.md)                | Same min-heap structure but finds shortest paths, not MST     |

---

## 📝 Practice Problems

| Problem                              | Platform   | Difficulty |
|-------------------------------------|------------|------------|
| Min Cost to Connect All Points      | [LeetCode 1584](https://leetcode.com/problems/min-cost-to-connect-all-points/) | 🟡 Medium |
| Connecting Cities With Minimum Cost | [LeetCode 1135](https://leetcode.com/problems/connecting-cities-with-minimum-cost/) | 🟡 Medium |
| Minimum Spanning Tree               | [HackerRank](https://www.hackerrank.com/challenges/primsmstsub/problem) | 🟡 Medium |
