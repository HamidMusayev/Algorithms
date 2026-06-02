# Kosaraju's Algorithm 🔃

---

## 🧠 Intuition

A **Strongly Connected Component (SCC)** is a group where every node can reach every other. Kosaraju's key insight: if you can reach node B from A in the original graph, and also reach A from B in the **reversed** graph, they're in the same SCC.

Two passes: first find which nodes finish last in DFS (these are the SCC "roots"), then run DFS on the reversed graph in that order.

**Mental model:** "Nodes that finish DFS last in the original graph are roots of SCCs. In the reversed graph, DFS from each root visits exactly its own SCC."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(V + E) — two DFS passes |
| Space Complexity | O(V + E) — stores the transposed graph |
| Output | List of all Strongly Connected Components |

---

## ⚙️ How It Works

**Pass 1 — Finish-time ordering:**
1. Run DFS on the **original** graph.
2. When a node finishes (all descendants explored), push it onto a **stack**.
3. The node on top of the stack finishes last — it's the "most powerful" node in its SCC.

**Pass 2 — SCC discovery:**
4. **Transpose** the graph (reverse all edge directions).
5. While the stack is not empty:
   - Pop the top node. If unvisited, run DFS from it in the **transposed** graph.
   - Every node reached in this DFS is part of the same SCC.

---

## 🔢 Step-by-Step Trace

Graph: `0→1, 1→2, 2→0, 2→3, 3→4, 4→5, 5→3`

**Pass 1 (DFS, track finish order):**
- DFS order: 0→1→2 (back to 0), then 3→4→5 (back to 3)
- Finish order (stack, bottom to top): `[0, 1, 2, 3, 4, 5]`  
  Stack top = 5 (finished last)

**Transposed graph:** `1→0, 2→1, 0→2, 3→2, 4→3, 5→4`

**Pass 2 (DFS on transposed, pop from stack):**
- Pop 5: DFS on transposed from 5 → reaches 4, 3 → SCC = {3, 4, 5}
- Pop 2: DFS on transposed from 2 → reaches 1, 0 → SCC = {0, 1, 2}

SCCs: `{3,4,5}` and `{0,1,2}` ✅

---

## 🐍 Python Implementation

```python
def kosaraju(graph):
    """
    graph: { node: [neighbors] } — integer node IDs
    Returns list of SCCs.
    """
    V = len(graph)
    visited = [False] * V
    finish_order = []         # Nodes ordered by finish time

    def dfs_pass1(u):
        """DFS on original graph — collect finish order."""
        visited[u] = True
        for v in graph[u]:
            if not visited[v]:
                dfs_pass1(v)
        finish_order.append(u)   # Push after all descendants finished

    # Step 1: Run DFS on original graph
    for u in range(V):
        if not visited[u]:
            dfs_pass1(u)

    # Step 2: Build the transposed graph (reverse all edges)
    transposed = {i: [] for i in range(V)}
    for u in range(V):
        for v in graph[u]:
            transposed[v].append(u)   # Reverse direction

    # Step 3: DFS on transposed graph in reverse finish order
    visited = [False] * V
    sccs = []

    def dfs_pass2(u, component):
        """DFS on transposed graph — collect all nodes in one SCC."""
        visited[u] = True
        component.append(u)
        for v in transposed[u]:
            if not visited[v]:
                dfs_pass2(v, component)

    # Process nodes in reverse finish order (stack = top = last finished)
    for u in reversed(finish_order):
        if not visited[u]:
            component = []
            dfs_pass2(u, component)
            sccs.append(component)

    return sccs


# Example
graph = {
    0: [1], 1: [2], 2: [0, 3],   # 0-1-2 form an SCC
    3: [4], 4: [5], 5: [3]        # 3-4-5 form an SCC
}
sccs = kosaraju(graph)
print("SCCs:", sccs)
# e.g., [[0, 1, 2], [3, 4, 5]]
```

---

## 🎯 Recognize This Problem When...

- You need to find **groups of mutually reachable nodes** in a directed graph.
- You prefer a **simpler, more intuitive** implementation over Tarjan's.
- Keywords: "strongly connected", "mutual reachability", "condensation graph".
- You need to identify **cycles in a directed graph** and the nodes involved.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                  | Verdict                                       |
|--------------------------------------------|-----------------------------------------------|
| Finding SCCs with simple, clear code       | ✅ Easier to understand than Tarjan           |
| All SCC use cases (2-SAT, condensation)    | ✅ Same output as Tarjan                      |
| Memory is constrained                      | ❌ Needs transposed graph (extra O(V+E) space)|
| Maximum efficiency in one pass             | ❌ Tarjan uses one DFS; Kosaraju uses two     |
| Undirected graphs                          | ❌ Use simple BFS/DFS for connected components|

---

## 🔗 Related Algorithms

| Algorithm                              | How it relates                                                |
|----------------------------------------|---------------------------------------------------------------|
| [Tarjan](Tarjan.md)                    | Also finds SCCs; single DFS pass (more efficient, less intuitive)|
| [DFS](DFS.md)                          | Both passes of Kosaraju are standard DFS                      |
| [Topological Sort](TopologicalSort.md) | Condensation graph (SCC as nodes) is a DAG                    |

---

## 📝 Practice Problems

| Problem                              | Platform   | Difficulty |
|-------------------------------------|------------|------------|
| Critical Connections in a Network   | [LeetCode 1192](https://leetcode.com/problems/critical-connections-in-a-network/) | 🔴 Hard |
| Strongly Connected Components       | [HackerEarth](https://www.hackerearth.com/practice/algorithms/graphs/strongly-connected-components/tutorial/) | 🟡 Medium |
| Number of Provinces                 | [LeetCode 547](https://leetcode.com/problems/number-of-provinces/) | 🟡 Medium |
