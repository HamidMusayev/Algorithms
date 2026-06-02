# Hamiltonian Cycle 🔁

---

## 🧠 Intuition

A Hamiltonian Cycle visits **every vertex exactly once** and returns to the starting vertex. Think of a traveling salesman trying to visit every city once and return home — the Hamiltonian Cycle problem asks: does such a route exist?

Unlike Eulerian circuits (which traverse every **edge** once), Hamiltonian cycles require visiting every **vertex** once. This is NP-complete — no polynomial algorithm is known, so we use backtracking.

**Mental model:** "Build the path vertex by vertex. At each step, try adding an adjacent unvisited vertex. If stuck (no valid next vertex), backtrack."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(N!) worst case |
| Space Complexity | O(N) for the path |

---

## ⚙️ How It Works

1. Start at vertex 0 (WLOG — all cycles include vertex 0).
2. For each position in the path (1 to N-1):
   - Try each vertex `v` that:
     - Has an edge from the previous vertex.
     - Has not been visited yet.
   - **Place** `v` in the path.
   - **Recurse** to fill the next position.
   - If we've placed all N vertices AND there's an edge back to vertex 0 → **cycle found!**
   - Otherwise → **backtrack**: remove `v` and try the next candidate.

---

## 🔢 Step-by-Step Trace

5-vertex graph adjacency matrix (1 = edge exists):

```
   0  1  2  3  4
0 [0, 1, 0, 1, 0]
1 [1, 0, 1, 1, 1]
2 [0, 1, 0, 0, 1]
3 [1, 1, 0, 0, 1]
4 [0, 1, 1, 1, 0]
```

Path building:
```
pos=1: try v=1 (edge 0→1 ✅) → path=[0,1]
  pos=2: try v=2 (edge 1→2 ✅) → path=[0,1,2]
    pos=3: try v=4 (edge 2→4 ✅) → path=[0,1,2,4]
      pos=4: try v=3 (edge 4→3 ✅) → path=[0,1,2,4,3]
        All N vertices placed! Edge 3→0? ✅ → CYCLE FOUND!
        Cycle: 0 → 1 → 2 → 4 → 3 → 0 ✅
```

---

## 🐍 Python Implementation

```python
def hamiltonian_cycle(graph):
    """
    graph: N×N adjacency matrix (graph[u][v] = 1 if edge u→v exists)
    Returns the Hamiltonian cycle path, or None if no cycle exists.
    """
    n = len(graph)
    path = [-1] * n
    path[0] = 0   # Start at vertex 0

    if backtrack(graph, path, 1):
        return path + [path[0]]   # Append start to show cycle
    return None


def is_safe(graph, path, pos, v):
    """Check if vertex v can be added at position pos in the path."""
    if graph[path[pos - 1]][v] == 0:    # No edge from last vertex to v
        return False
    if v in path:                        # v already in path
        return False
    return True


def backtrack(graph, path, pos):
    n = len(graph)
    if pos == n:
        # All vertices placed — check if there's an edge back to start
        return graph[path[pos - 1]][path[0]] == 1

    for v in range(1, n):               # Try each vertex (skip 0, it's the start)
        if is_safe(graph, path, pos, v):
            path[pos] = v               # Place vertex v at position pos

            if backtrack(graph, path, pos + 1):
                return True

            path[pos] = -1              # Backtrack: remove v

    return False   # No valid vertex found at this position


# Example
graph = [
    [0, 1, 0, 1, 0],
    [1, 0, 1, 1, 1],
    [0, 1, 0, 0, 1],
    [1, 1, 0, 0, 1],
    [0, 1, 1, 1, 0],
]
cycle = hamiltonian_cycle(graph)
print("Hamiltonian Cycle:", cycle)
# [0, 1, 2, 4, 3, 0]
```

---

## 🎯 Recognize This Problem When...

- You need to visit **every node exactly once** and return to start.
- Keywords: "Hamiltonian cycle/path", "visit all cities once", "route visiting all nodes".
- The graph is small (N ≤ 20) or you need existence check only.
- You see "does a cycle exist that visits every vertex?" — that's Hamiltonian.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Small graphs (N ≤ 15) — check existence | ✅ Backtracking is feasible |
| Theoretical/teaching constraint satisfaction | ✅ Classic NP-complete problem |
| Large graphs (N > 20) | ❌ Backtracking is exponential — use heuristics or DP (Held-Karp) |
| Minimum cost Hamiltonian cycle (TSP) | ❌ Use Held-Karp DP O(2^N × N²) for exact, or approximations |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Knight's Tour](KnightsTour.md) | Hamiltonian path on the knight-move graph |
| [N-Queens](NQueens.md) | Also backtracking constraint placement |
| [DFS](../GraphAlgorithms/DFS.md) | Hamiltonian backtracking is DFS with undo |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Hamiltonian Path](https://www.hackerrank.com/challenges/hamiltonian-path/problem) | HackerRank | 🔴 Hard |
| [Find if Path Exists in Graph](https://leetcode.com/problems/find-if-path-exists-in-graph/) | LeetCode | 🟢 Easy |
| Reconstruct Itinerary (Eulerian, related) | [LeetCode 332](https://leetcode.com/problems/reconstruct-itinerary/) | 🔴 Hard |
