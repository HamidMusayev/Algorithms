# Topological Sort 📋

---

## 🧠 Intuition

Imagine you need to get dressed: you can't put on your shoes before your socks, or your jacket before your shirt. Topological sort arranges tasks so that every prerequisite always comes before the task that depends on it — like ordering steps in a recipe or compilation dependencies.

**Mental model:** Order the nodes of a DAG so every arrow points "left to right" — dependencies always come before their dependents.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(V + E) |
| Space Complexity | O(V) |
| Works on | Directed Acyclic Graphs (DAGs) only |
| Cycle detection | ✅ Built-in — cycle = no valid ordering |

---

## ⚙️ How It Works

**Kahn's Algorithm (BFS-based):**
1. Compute **in-degree** (number of incoming edges) for every node.
2. Add all nodes with in-degree = 0 to a queue (no prerequisites).
3. While the queue is not empty:
   - Dequeue a node, add it to the result.
   - For each neighbor, decrement its in-degree.
   - If a neighbor's in-degree becomes 0, enqueue it.
4. If result has fewer than V nodes → **cycle exists** (some nodes were never enqueued).

**DFS-based:**
1. DFS the graph. After fully exploring all descendants of node u, **push u onto a stack**.
2. Reverse the stack — this gives topological order.

---

## 🔢 Step-by-Step Trace

Graph (directed): `5→2, 5→0, 4→0, 4→1, 2→3, 3→1`

**In-degrees:** 0=2, 1=2, 2=1, 3=1, 4=0, 5=0

| Step | Queue  | Process | Decrement neighbors | Result     |
|------|--------|---------|---------------------|------------|
| 1    | [4, 5] | 4       | in[0]--, in[1]--    | [4]        |
| 2    | [5]    | 5       | in[2]--, in[0]--    | [4, 5]     |
| 3    | [0, 2] | 0       | (no outgoing edges) | [4, 5, 0] |
| 4    | [2]    | 2       | in[3]--             | [4,5,0,2]  |
| 5    | [3]    | 3       | in[1]--             | [4,5,0,2,3]|
| 6    | [1]    | 1       | —                   | [4,5,0,2,3,1]|

Valid topological order: **4 → 5 → 0 → 2 → 3 → 1** ✅

---

## 🐍 Python Implementation

```python
from collections import deque, defaultdict

def topological_sort_kahn(V, edges):
    """
    Kahn's Algorithm (BFS-based).
    V: number of vertices (0 to V-1)
    edges: list of (u, v) meaning u must come before v
    Returns topological order, or raises if cycle exists.
    """
    graph = defaultdict(list)
    in_degree = [0] * V

    for u, v in edges:
        graph[u].append(v)
        in_degree[v] += 1          # Track how many prerequisites v has

    # Start with all nodes that have no prerequisites
    queue = deque([i for i in range(V) if in_degree[i] == 0])
    order = []

    while queue:
        node = queue.popleft()
        order.append(node)

        for neighbor in graph[node]:
            in_degree[neighbor] -= 1     # One prerequisite satisfied
            if in_degree[neighbor] == 0:
                queue.append(neighbor)   # All prerequisites met — add to queue

    if len(order) != V:
        raise ValueError("Graph contains a cycle — no topological ordering possible")

    return order


def topological_sort_dfs(V, edges):
    """DFS-based topological sort using post-order traversal."""
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)

    visited = set()
    stack = []
    in_stack = set()    # For cycle detection

    def dfs(u):
        in_stack.add(u)
        visited.add(u)
        for v in graph[u]:
            if v in in_stack:
                raise ValueError("Cycle detected")
            if v not in visited:
                dfs(v)
        in_stack.remove(u)
        stack.append(u)   # Push AFTER all descendants are processed

    for i in range(V):
        if i not in visited:
            dfs(i)

    return stack[::-1]   # Reverse gives topological order


# Example
edges = [(5, 2), (5, 0), (4, 0), (4, 1), (2, 3), (3, 1)]
print("Kahn's:", topological_sort_kahn(6, edges))
# e.g., [4, 5, 0, 2, 3, 1]
print("DFS:", topological_sort_dfs(6, edges))
# e.g., [5, 4, 2, 3, 1, 0]
```

---

## 🎯 Recognize This Problem When...

- The problem involves **ordering tasks with dependencies**.
- Keywords: "prerequisites", "course schedule", "build order", "dependency resolution".
- You need to determine if a **set of constraints is satisfiable** (cycle = contradiction).
- The graph is directed and you need a **linear ordering**.
- You see "you must complete X before Y" style constraints.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                  | Verdict                                       |
|--------------------------------------------|-----------------------------------------------|
| Task scheduling with dependencies          | ✅ Classic use case                           |
| Build systems (Makefile, package managers) | ✅ Determines compile/install order           |
| Detecting cycles in directed graphs        | ✅ Cycle = topological sort impossible        |
| Undirected graphs                          | ❌ No concept of direction = no topo order    |
| Graphs with cycles                         | ❌ No valid topological order exists          |

---

## 🔗 Related Algorithms

| Algorithm                              | How it relates                                                |
|----------------------------------------|---------------------------------------------------------------|
| [DFS](DFS.md)                          | DFS post-order gives reverse topological order                |
| [BFS](BFS.md)                          | Kahn's algorithm is essentially BFS on in-degrees             |
| [Tarjan](Tarjan.md)                    | Finds SCCs; if any SCC has >1 node, there's a cycle           |

---

## 📝 Practice Problems

| Problem                              | Platform   | Difficulty |
|-------------------------------------|------------|------------|
| Course Schedule                     | [LeetCode 207](https://leetcode.com/problems/course-schedule/) | 🟡 Medium |
| Course Schedule II                  | [LeetCode 210](https://leetcode.com/problems/course-schedule-ii/) | 🟡 Medium |
| Alien Dictionary                    | [LeetCode 269](https://leetcode.com/problems/alien-dictionary/) | 🔴 Hard |
| Sequence Reconstruction             | [LeetCode 444](https://leetcode.com/problems/sequence-reconstruction/) | 🟡 Medium |
