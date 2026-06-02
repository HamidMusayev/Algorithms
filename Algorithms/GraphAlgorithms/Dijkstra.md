# Dijkstra's Algorithm 🗺️

---

## 🧠 Intuition

Imagine water flowing outward from a source in a weighted pipe network. The water always spreads through the cheapest pipe first, gradually filling all reachable nodes in order of their total cost from the source.

Dijkstra works the same way: it maintains a running "best known distance" to every node and always processes the currently cheapest-to-reach node next. Once a node is processed, its shortest distance is finalized forever.

**Mental model:** A spreading wave that always expands along the cheapest edge — the wave front is a priority queue sorted by distance.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O((V + E) log V) with binary heap |
| Space Complexity | O(V) |
| Works with negative edges | ❌ No — use Bellman-Ford |
| Works with negative cycles | ❌ No |
| Finds all-pairs shortest paths | ❌ No — use Floyd-Warshall |

> V = vertices, E = edges. With a Fibonacci heap, time improves to O(E + V log V) but is rarely used in practice.

---

## ⚙️ How It Works

1. Initialize distances: `dist[source] = 0`, `dist[all others] = ∞`.
2. Push `(0, source)` onto a **min-heap** (priority queue).
3. While the heap is not empty:
   - Pop the node `u` with the **smallest current distance** `d`.
   - If `d > dist[u]`, skip it (a shorter path was already found).
   - For each neighbor `v` of `u` with edge weight `w`:
     - If `dist[u] + w < dist[v]`, **relax the edge**: update `dist[v]` and push `(dist[v], v)` onto the heap.
4. Return the `dist` array (shortest distances from source to all nodes).

---

## 🔢 Step-by-Step Trace

Graph:
```
A --(1)--> B --(2)--> C
|                     ^
+--(4)-----------(1)--+
```
Source: **A**

| Step | Pop       | Relax Neighbors                    | dist         |
|------|-----------|------------------------------------|--------------|
| Init | —         | —                                  | A=0, B=∞, C=∞|
| 1    | (0, A)    | B→0+1=1, C→0+4=4                  | A=0, B=1, C=4|
| 2    | (1, B)    | C→1+2=3 < 4 → update C            | A=0, B=1, C=3|
| 3    | (3, C)    | No neighbors                       | A=0, B=1, C=3|

Shortest distances from A: **A=0, B=1, C=3** ✅

---

## 🐍 Python Implementation

```python
import heapq

def dijkstra(graph, start):
    """
    Find shortest distances from 'start' to all nodes.
    graph = { node: [(neighbor, weight), ...] }
    """
    # Initialize all distances to infinity
    dist = {node: float('inf') for node in graph}
    dist[start] = 0

    pq = [(0, start)]    # Min-heap: (distance, node)

    while pq:
        d, node = heapq.heappop(pq)   # Pop node with smallest distance

        if d > dist[node]:             # Outdated entry — skip
            continue

        for neighbor, weight in graph[node]:
            new_dist = d + weight      # Try to relax the edge
            if new_dist < dist[neighbor]:
                dist[neighbor] = new_dist            # Update shortest distance
                heapq.heappush(pq, (new_dist, neighbor))  # Push to heap

    return dist


def dijkstra_with_path(graph, start, end):
    """Returns (distance, path) from start to end."""
    dist = {node: float('inf') for node in graph}
    dist[start] = 0
    prev = {node: None for node in graph}    # Track path
    pq = [(0, start)]

    while pq:
        d, node = heapq.heappop(pq)
        if d > dist[node]:
            continue
        for neighbor, weight in graph[node]:
            new_dist = d + weight
            if new_dist < dist[neighbor]:
                dist[neighbor] = new_dist
                prev[neighbor] = node            # Record where we came from
                heapq.heappush(pq, (new_dist, neighbor))

    # Reconstruct path
    path = []
    current = end
    while current is not None:
        path.append(current)
        current = prev[current]
    path.reverse()

    return dist[end], path


# Example
graph = {
    'A': [('B', 1), ('C', 4)],
    'B': [('C', 2), ('D', 5)],
    'C': [('D', 1)],
    'D': []
}

print(dijkstra(graph, 'A'))
# {'A': 0, 'B': 1, 'C': 3, 'D': 4}

dist, path = dijkstra_with_path(graph, 'A', 'D')
print(f"Shortest path A→D: {path}, distance: {dist}")
# Shortest path A→D: ['A', 'B', 'C', 'D'], distance: 4
```

---

## 🎯 Recognize This Problem When...

- The problem asks for **shortest path** or **minimum cost** from one node to others.
- Edge weights are **non-negative**.
- Keywords: "minimum distance", "cheapest route", "network latency", "road map".
- The graph represents a **road network, game map, network topology**.
- You see phrases like "find the fastest path" or "minimum total weight".

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                  | Verdict                                       |
|--------------------------------------------|-----------------------------------------------|
| Single-source shortest paths, non-negative weights | ✅ Gold standard                    |
| Road networks, GPS routing                 | ✅ Classic application                        |
| All edges have weight 1 (unweighted)       | ❌ Use BFS — simpler and same complexity      |
| Graph has negative edge weights            | ❌ Use Bellman-Ford                           |
| Need all-pairs shortest paths              | ❌ Use Floyd-Warshall                         |
| Very dense graph (E ≈ V²)                  | ❌ Consider matrix-based Dijkstra O(V²)       |

---

## 🔗 Related Algorithms

| Algorithm                              | How it relates                                                |
|----------------------------------------|---------------------------------------------------------------|
| [BFS](BFS.md)                          | Dijkstra with all weights = 1                                 |
| [Bellman-Ford](BellmanFord.md)         | Handles negative edges; slower O(VE)                          |
| [Floyd-Warshall](FloydWarshall.md)     | All-pairs shortest paths; O(V³)                               |
| [A*](AStar.md)                         | Dijkstra + heuristic; faster when destination is known        |
| [Prim's Algorithm](Prim.md)            | Same min-heap structure but builds MST instead of shortest paths |

---

## 📝 Practice Problems

| Problem                              | Platform   | Difficulty |
|-------------------------------------|------------|------------|
| Network Delay Time                  | [LeetCode 743](https://leetcode.com/problems/network-delay-time/) | 🟡 Medium |
| Path With Minimum Effort            | [LeetCode 1631](https://leetcode.com/problems/path-with-minimum-effort/) | 🟡 Medium |
| Cheapest Flights Within K Stops     | [LeetCode 787](https://leetcode.com/problems/find-the-cheapest-flights-within-k-stops/) | 🟡 Medium |
| Shortest Path in a Grid with Obstacles | [LeetCode 1293](https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/) | 🔴 Hard |
| Dijkstra's Shortest Reach           | [HackerRank](https://www.hackerrank.com/challenges/dijkstrashortreach/problem) | 🟡 Medium |
