# Floyd-Warshall Algorithm 🌍

---

## 🧠 Intuition

Imagine you have a table of direct flight prices between every pair of cities. You want to know the cheapest way to get between any two cities, possibly with layovers. Floyd-Warshall systematically considers each city as a potential **intermediate stop** and asks: "Is flying through city k cheaper than the current best route from i to j?"

**Mental model:** For each possible intermediate node k, check if routing through k improves any i→j path. After considering all V nodes as intermediates, all shortest paths are found.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(V³) |
| Space Complexity | O(V²) |
| Works with negative edges | ✅ Yes (no negative cycles) |
| Detects negative cycles | ✅ Yes (diagonal < 0 after running) |
| Single-source or all-pairs | All-pairs ✅ |

> V = number of vertices. O(V³) makes it practical only for small to medium graphs (V ≤ 500 or so).

---

## ⚙️ How It Works

1. Initialize a V×V distance matrix `dist` where:
   - `dist[i][i] = 0` (no cost from a node to itself)
   - `dist[i][j] = weight(i, j)` if edge exists
   - `dist[i][j] = ∞` if no direct edge
2. For each intermediate node **k** from 0 to V-1:
   - For each pair **(i, j)**:
     - If `dist[i][k] + dist[k][j] < dist[i][j]`:
       - Update `dist[i][j] = dist[i][k] + dist[k][j]`
3. After all k iterations, `dist[i][j]` holds the shortest path from i to j.
4. **Negative cycle detection:** if `dist[i][i] < 0` for any i, a negative cycle exists.

---

## 🔢 Step-by-Step Trace

Graph (4 nodes, INF = no direct edge):
```
     0    1    2    3
0  [ 0,   3, INF,   7]
1  [ 8,   0,   2, INF]
2  [ 5, INF,   0,   1]
3  [ 2, INF, INF,   0]
```

**k=0** (route through node 0):
- `dist[1][3]`: 8+7=15 vs INF → update to 15
- `dist[3][1]`: 2+3=5 vs INF → update to 5
- ...

**k=1** (route through node 1):
- `dist[0][2]`: 3+2=5 vs INF → update to 5
- ...

After all k: `dist[0][3]` = 6 (path 0→2→3), `dist[1][0]` = 7 (path 1→2→3→0)

---

## 🐍 Python Implementation

```python
def floyd_warshall(graph):
    """
    graph: V×V adjacency matrix
           graph[i][j] = weight of edge i→j, float('inf') if no edge
    Returns the all-pairs shortest path matrix.
    """
    V = len(graph)
    # Copy the input matrix — don't modify the original
    dist = [row[:] for row in graph]

    # Set self-distances to 0
    for i in range(V):
        dist[i][i] = 0

    # Try every node k as a potential intermediate stop
    for k in range(V):
        for i in range(V):
            for j in range(V):
                # Is going i→k→j shorter than the current i→j?
                if dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]

    # Check for negative cycles (any node can reach itself with negative cost)
    for i in range(V):
        if dist[i][i] < 0:
            raise ValueError(f"Negative cycle detected involving node {i}")

    return dist


# Example
INF = float('inf')
graph = [
    [0,   3, INF, 7],
    [8,   0,   2, INF],
    [5, INF,   0,   1],
    [2, INF, INF,   0],
]

result = floyd_warshall(graph)
print("All-pairs shortest paths:")
for row in result:
    print([x if x != INF else '∞' for x in row])
# [0, 3, 5, 6]
# [7, 0, 2, 3]
# [3, 6, 0, 1]  (corrected)
# [2, 5, 7, 0]
```

---

## 🎯 Recognize This Problem When...

- You need **shortest paths between ALL pairs** of nodes, not just from one source.
- The graph is **small** (V ≤ 200–500 is typical).
- You need to check **transitive closure** (can node i reach node j?).
- The graph may have **negative edge weights** (but no negative cycles).
- Keywords: "for every pair of cities", "all-pairs", "any two nodes".

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                  | Verdict                                       |
|--------------------------------------------|-----------------------------------------------|
| All-pairs shortest paths needed            | ✅ Gold standard for small graphs             |
| Graph has negative edge weights            | ✅ Handles them (unlike Dijkstra)             |
| Transitive closure check                   | ✅ Modify to boolean matrix                   |
| Large graph (V > 1000)                     | ❌ O(V³) is too slow                         |
| Only single-source shortest paths needed   | ❌ Dijkstra or Bellman-Ford is faster         |
| Graph has negative cycles                  | ❌ Algorithm doesn't terminate correctly      |

---

## 🔗 Related Algorithms

| Algorithm                              | How it relates                                                |
|----------------------------------------|---------------------------------------------------------------|
| [Dijkstra](Dijkstra.md)                | Single-source only; faster for non-negative edges             |
| [Bellman-Ford](BellmanFord.md)         | Single-source; handles negative edges; O(VE)                  |
| [BFS](BFS.md)                          | All-pairs on unweighted graphs (run BFS from every node)      |

---

## 📝 Practice Problems

| Problem                              | Platform   | Difficulty |
|-------------------------------------|------------|------------|
| Find the City With the Smallest Number of Neighbors | [LeetCode 1334](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/) | 🟡 Medium |
| Network Delay Time                  | [LeetCode 743](https://leetcode.com/problems/network-delay-time/) | 🟡 Medium |
| All Pairs Shortest Path             | [HackerRank](https://www.hackerrank.com/challenges/floyd-city-of-blinding-lights/problem) | 🟡 Medium |
