# Tarjan's Algorithm 🔄

---

## 🧠 Intuition

A **Strongly Connected Component (SCC)** is a group of nodes where you can reach every node from every other node. Think of it as an "island" of mutual reachability in a directed graph.

Tarjan's key insight: during a DFS, if a node `u` has `disc[u] == low[u]` (its "discovery time" equals the lowest time reachable from its subtree), then `u` is the **root** of an SCC — everything on the DFS stack down to `u` belongs to the same component.

**Mental model:** DFS + track "how far back can I reach?" — when you can't reach any further back than yourself, you've found an SCC root.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(V + E) — single DFS pass |
| Space Complexity | O(V) |
| Output | List of all Strongly Connected Components |

---

## ⚙️ How It Works

1. For each unvisited node, start a DFS.
2. Assign each node a **discovery time** `disc[u]` and **low value** `low[u]` (initially same as disc).
3. Push the node onto a **stack** and mark it `on_stack`.
4. For each neighbor `v`:
   - If unvisited: recurse; then update `low[u] = min(low[u], low[v])`.
   - If on stack: update `low[u] = min(low[u], disc[v])` (back edge → same SCC).
5. If `low[u] == disc[u]`: this is an **SCC root** — pop nodes from stack until `u` is popped. That group is one SCC.

---

## 🔢 Step-by-Step Trace

Graph: `0→1, 1→2, 2→0, 2→3, 3→4, 4→5, 5→3`

Two SCCs: `{0,1,2}` (mutual cycle) and `{3,4,5}` (mutual cycle)

```
DFS from 0: disc=[0,1,2,-1,-1,-1], low=[0,0,0,...]
- Visit 0→1→2→0 (back edge: low[2]=min(low[2],disc[0])=0)
- Back at 2: low[2]=0=disc[2]? No (disc[2]=2). Continue.
- Back at 1: low[1]=min(low[1],low[2])=0
- Back at 0: low[0]=min(low[0],low[1])=0; low[0]==disc[0]=0 → SCC ROOT!
  Pop: 2, 1, 0 → SCC = {0,1,2} ✅
- 2 also visits 3→4→5→3 (back edge: low[5]=min(disc[3])=3)
- At 3: low[3]=3=disc[3] → SCC ROOT! Pop: 5,4,3 → SCC = {3,4,5} ✅
```

---

## 🐍 Python Implementation

```python
def tarjan_scc(graph):
    """
    graph: { node: [neighbors] } — uses integer node IDs
    Returns list of SCCs, each SCC is a list of nodes.
    """
    V = len(graph)
    disc = [-1] * V           # Discovery time (-1 = not visited)
    low = [0] * V             # Lowest disc time reachable from subtree
    on_stack = [False] * V    # Whether node is currently on the DFS stack
    stack = []
    sccs = []
    timer = [0]               # Use list for mutability in nested function

    def dfs(u):
        disc[u] = low[u] = timer[0]   # Set discovery and low time
        timer[0] += 1
        stack.append(u)
        on_stack[u] = True

        for v in graph[u]:
            if disc[v] == -1:          # v not yet visited — recurse
                dfs(v)
                low[u] = min(low[u], low[v])   # Update low via tree edge
            elif on_stack[v]:          # v is on stack — back edge to same SCC
                low[u] = min(low[u], disc[v])

        # If u is the root of an SCC (can't reach further back)
        if low[u] == disc[u]:
            scc = []
            while True:
                w = stack.pop()
                on_stack[w] = False
                scc.append(w)
                if w == u:             # Stop when we've popped u (the SCC root)
                    break
            sccs.append(scc)

    for u in range(V):
        if disc[u] == -1:
            dfs(u)

    return sccs


# Example
graph = {
    0: [1], 1: [2], 2: [0, 3],   # 0-1-2 form an SCC
    3: [4], 4: [5], 5: [3]        # 3-4-5 form an SCC
}
sccs = tarjan_scc(graph)
print("SCCs:", sccs)
# e.g., [[5, 4, 3], [2, 1, 0]] (order may vary)
```

---

## 🎯 Recognize This Problem When...

- You need to find **groups of mutually reachable nodes** in a directed graph.
- The problem asks to **decompose a directed graph** into independent components.
- Keywords: "strongly connected", "cyclic dependencies", "condensation", "2-SAT".
- You need to check if a directed graph is **strongly connected** (one SCC = entire graph).
- You're solving a **2-SAT** problem (Tarjan finds SCCs to determine satisfiability).

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                  | Verdict                                       |
|--------------------------------------------|-----------------------------------------------|
| Finding SCCs in one DFS pass               | ✅ Most efficient SCC algorithm               |
| 2-SAT problem solving                      | ✅ SCCs reveal contradiction                  |
| Condensation graph (compress cycles)       | ✅ Each SCC becomes one node in DAG           |
| Undirected graphs (connected components)   | ❌ Use simple BFS/DFS instead                 |
| Prefer simpler implementation              | ❌ Kosaraju is easier to understand/code      |

---

## 🔗 Related Algorithms

| Algorithm                              | How it relates                                                |
|----------------------------------------|---------------------------------------------------------------|
| [Kosaraju](Kosaraju.md)                | Also finds SCCs; uses 2 DFS passes (easier to understand)     |
| [DFS](DFS.md)                          | Tarjan is a specialized DFS with extra bookkeeping            |
| [Topological Sort](TopologicalSort.md) | Condensation graph (SCCs as nodes) is always a DAG            |

---

## 📝 Practice Problems

| Problem                              | Platform   | Difficulty |
|-------------------------------------|------------|------------|
| Critical Connections in a Network   | [LeetCode 1192](https://leetcode.com/problems/critical-connections-in-a-network/) | 🔴 Hard |
| Number of Strongly Connected Components | Various competitive programming platforms | 🟡 Medium |
| 2-SAT Problem                       | [Codeforces EDU](https://codeforces.com/edu/course/2/lesson/3) | 🔴 Hard |
