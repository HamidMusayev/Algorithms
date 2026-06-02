# A* Search Algorithm ⭐

---

## 🧠 Intuition

Dijkstra explores nodes in all directions equally, like an expanding circle. A* is smarter — it uses a **heuristic** (a guess of the remaining distance) to focus the search toward the goal. It's like navigating to a destination: instead of exploring every road equally, you favor roads that lead roughly in the right direction.

**Mental model:** Dijkstra + a compass. The heuristic `h(n)` is the compass — it tells the algorithm which explored nodes are most likely on the way to the goal.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(E) in best case; O(b^d) worst case (b = branching factor, d = depth) |
| Space Complexity | O(V) — stores all open/closed nodes |
| Optimal | ✅ Yes, if heuristic is **admissible** (never overestimates) |
| Complete | ✅ Yes, if branching factor is finite |

> With a perfect heuristic, A* explores only the optimal path. With h=0, A* degenerates into Dijkstra.

---

## ⚙️ How It Works

For each node `n`, A* tracks:
- `g(n)` = actual cost from start to n
- `h(n)` = heuristic estimate from n to goal
- `f(n) = g(n) + h(n)` = estimated total path cost through n

1. Add the start node to the **open set** (priority queue ordered by f).
2. While the open set is not empty:
   - Pop the node with the **smallest f(n)**.
   - If it's the goal → reconstruct and return the path.
   - For each neighbor, compute tentative `g = g(current) + edge_weight`.
   - If this is a better path, update `g` and push to the open set.
3. If open set is empty without finding goal → no path exists.

**Common heuristics:**
- **Grid / maze:** Manhattan distance `|x₁-x₂| + |y₁-y₂|`
- **Euclidean space:** Euclidean distance `√((x₁-x₂)² + (y₁-y₂)²)`

---

## 🔢 Step-by-Step Trace

Graph (nodes with (x,y) positions, goal=D):
```
A(0,0) --1-- B(1,0) --2-- C(2,0)
                              |
                              1
                              |
                             D(2,1)   ← goal
```
Heuristic h = Manhattan distance to D=(2,1):
- h(A)=3, h(B)=2, h(C)=1, h(D)=0

| Step | Pop (f,node) | g(node) | Explore neighbors | f values pushed |
|------|-------------|---------|-------------------|-----------------| 
| 1    | (3, A)      | 0       | B: g=1, f=1+2=3   | (3,B)           |
| 2    | (3, B)      | 1       | C: g=3, f=3+1=4   | (4,C)           |
| 3    | (4, C)      | 3       | D: g=4, f=4+0=4   | (4,D)           |
| 4    | (4, D)      | 4       | **Goal reached!** | —               |

Path: A → B → C → D, cost = 4 ✅

---

## 🐍 Python Implementation

```python
import heapq

def a_star(graph, start, goal, heuristic):
    """
    graph: { node: [(neighbor, cost), ...] }
    heuristic: function(node) → estimated cost to goal
    Returns (total_cost, path) or (inf, []) if no path
    """
    # (f_score, g_score, node, path)
    open_set = [(heuristic(start), 0, start, [start])]
    best_g = {}   # Best known g-score for each node

    while open_set:
        f, g, node, path = heapq.heappop(open_set)

        if node == goal:
            return g, path   # Found the optimal path

        # Skip if we've already found a better path to this node
        if node in best_g and best_g[node] <= g:
            continue
        best_g[node] = g

        for neighbor, cost in graph[node]:
            new_g = g + cost                          # Actual cost to reach neighbor
            new_f = new_g + heuristic(neighbor)       # Estimated total cost through neighbor
            heapq.heappush(open_set, (new_f, new_g, neighbor, path + [neighbor]))

    return float('inf'), []   # Goal not reachable


# Example: simple weighted graph with Manhattan-inspired heuristic
graph = {
    'A': [('B', 1), ('C', 4)],
    'B': [('C', 2), ('D', 5)],
    'C': [('D', 1)],
    'D': []
}

# Heuristic: remaining hops to D (admissible estimate)
h = {'A': 3, 'B': 2, 'C': 1, 'D': 0}
cost, path = a_star(graph, 'A', 'D', lambda n: h[n])
print(f"Cost: {cost}, Path: {path}")
# Cost: 4, Path: ['A', 'B', 'C', 'D']
```

---

## 🎯 Recognize This Problem When...

- You need **shortest path to a specific goal** (not all nodes).
- You have a **spatial map or grid** where distance to goal can be estimated.
- You want to be **faster than Dijkstra** by focusing on the goal direction.
- Keywords: "find path in grid", "game pathfinding", "robot navigation", "maze solver".
- A good **admissible heuristic** exists (Manhattan, Euclidean, etc.).

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                  | Verdict                                       |
|--------------------------------------------|-----------------------------------------------|
| Pathfinding to a specific goal in a grid   | ✅ Faster than Dijkstra with good heuristic   |
| Game AI movement, GPS routing              | ✅ Industry standard for single-target paths  |
| Heuristic is perfect                       | ✅ Explores only the optimal path             |
| Need shortest paths to ALL nodes           | ❌ Use Dijkstra (A* only finds one target)    |
| No good heuristic (h=0 everywhere)         | ❌ Degrades to Dijkstra — just use Dijkstra   |
| Heuristic overestimates (inadmissible)     | ❌ May return suboptimal path                 |

---

## 🔗 Related Algorithms

| Algorithm                              | How it relates                                                |
|----------------------------------------|---------------------------------------------------------------|
| [Dijkstra](Dijkstra.md)                | A* with h=0; explores in all directions equally               |
| [BFS](BFS.md)                          | A* on unweighted graph with h=0; same as BFS                  |
| [Greedy Best-First Search](Dijkstra.md)| Uses only h(n), ignoring g(n); faster but not optimal         |

---

## 📝 Practice Problems

| Problem                              | Platform   | Difficulty |
|-------------------------------------|------------|------------|
| Shortest Path in Binary Matrix      | [LeetCode 1091](https://leetcode.com/problems/shortest-path-in-binary-matrix/) | 🟡 Medium |
| Path With Minimum Effort            | [LeetCode 1631](https://leetcode.com/problems/path-with-minimum-effort/) | 🟡 Medium |
| Sliding Puzzle                      | [LeetCode 773](https://leetcode.com/problems/sliding-puzzle/) | 🔴 Hard |
