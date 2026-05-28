### **Tarjan's Algorithm 🌐**

Finds all **Strongly Connected Components (SCCs)** of a directed graph in **a single DFS pass** using discovery times and low-link values.

---

#### **Summary**
- **Time Complexity:** O(V + E)
- **Space Complexity:** O(V)
- **Output:** list of SCCs

An SCC is a maximal set of vertices where every node can reach every other node.

---

#### **How It Works**
1. DFS the graph, assigning each node a discovery time `disc[u]`.
2. Track `low[u]` = smallest disc time reachable from `u`'s subtree.
3. Push nodes onto a stack as they're visited.
4. When `disc[u] == low[u]`, pop the stack down to `u` — those nodes form an SCC.

---

#### **Python Implementation**
```python
def tarjan_scc(graph):
    V = len(graph)
    disc = [-1] * V
    low = [0] * V
    on_stack = [False] * V
    stack, sccs = [], []
    timer = [0]

    def dfs(u):
        disc[u] = low[u] = timer[0]
        timer[0] += 1
        stack.append(u)
        on_stack[u] = True

        for v in graph[u]:
            if disc[v] == -1:
                dfs(v)
                low[u] = min(low[u], low[v])
            elif on_stack[v]:
                low[u] = min(low[u], disc[v])

        if low[u] == disc[u]:
            scc = []
            while True:
                w = stack.pop()
                on_stack[w] = False
                scc.append(w)
                if w == u: break
            sccs.append(scc)

    for u in range(V):
        if disc[u] == -1:
            dfs(u)
    return sccs

# Example
graph = {
    0: [1], 1: [2], 2: [0, 3],
    3: [4], 4: [5], 5: [3]
}
print(tarjan_scc(graph))   # [[0,2,1], [3,5,4]]
```

---

#### **When to Use**
✅ Single-pass SCC discovery
✅ Useful in 2-SAT, dependency analysis, condensation graphs
