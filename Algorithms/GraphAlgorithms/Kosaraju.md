### **Kosaraju's Algorithm 🌐**

Another algorithm for finding **Strongly Connected Components (SCCs)** in a directed graph, using **two DFS passes**.

---

#### **Summary**
- **Time Complexity:** O(V + E)
- **Space Complexity:** O(V + E) (transposed graph)
- **Easier to understand** than Tarjan, but does two passes.

---

#### **How It Works**
1. DFS the graph and push nodes onto a stack in **finish-time order**.
2. **Transpose** the graph (reverse all edges).
3. Pop from the stack; for each unvisited node, DFS in the transposed graph — every reached node is one SCC.

---

#### **Python Implementation**
```python
def kosaraju(graph):
    V = len(graph)
    visited = [False] * V
    order = []

    def dfs(u, g, container):
        visited[u] = True
        for v in g[u]:
            if not visited[v]:
                dfs(v, g, container)
        container.append(u)

    # 1. First pass: order by finish time
    for u in range(V):
        if not visited[u]:
            dfs(u, graph, order)

    # 2. Transpose graph
    rg = {i: [] for i in range(V)}
    for u in range(V):
        for v in graph[u]:
            rg[v].append(u)

    # 3. Second pass on transposed graph
    visited = [False] * V
    sccs = []
    for u in reversed(order):
        if not visited[u]:
            comp = []
            dfs(u, rg, comp)
            sccs.append(comp)
    return sccs

# Example
graph = {
    0: [1], 1: [2], 2: [0, 3],
    3: [4], 4: [5], 5: [3]
}
print(kosaraju(graph))   # [[0,1,2], [3,4,5]]
```

---

#### **When to Use**
✅ Same use cases as Tarjan
✅ Easier mental model (two DFS passes)
🚫 Two passes + transpose = slightly more overhead than Tarjan
