### **Topological Sort 🌐**

Linear ordering of vertices in a **Directed Acyclic Graph (DAG)** such that for every edge `u → v`, `u` appears before `v`.

Two common approaches: **DFS-based** and **Kahn's algorithm** (BFS-based, uses in-degree).

---

#### **Summary**
- **Time Complexity:** O(V + E)
- **Space Complexity:** O(V)
- **Constraint:** Graph **must be a DAG** (cycle → no topological order).

---

#### **Use Cases**
- Build/dependency systems (e.g., `make`)
- Course prerequisite ordering
- Task scheduling

---

#### **Python Implementation**

**Kahn's Algorithm (BFS):**
```python
from collections import deque, defaultdict

def topological_sort(V, edges):
    graph = defaultdict(list)
    in_deg = [0] * V
    for u, v in edges:
        graph[u].append(v)
        in_deg[v] += 1

    queue = deque([i for i in range(V) if in_deg[i] == 0])
    order = []
    while queue:
        node = queue.popleft()
        order.append(node)
        for nxt in graph[node]:
            in_deg[nxt] -= 1
            if in_deg[nxt] == 0:
                queue.append(nxt)

    if len(order) != V:
        raise ValueError("Graph has a cycle")
    return order

# Example
edges = [(5, 2), (5, 0), (4, 0), (4, 1), (2, 3), (3, 1)]
print(topological_sort(6, edges))   # e.g., [4, 5, 0, 2, 3, 1]
```

**DFS-based:**
```python
def topo_dfs(V, edges):
    graph = {i: [] for i in range(V)}
    for u, v in edges:
        graph[u].append(v)

    visited, stack = set(), []
    def dfs(u):
        visited.add(u)
        for v in graph[u]:
            if v not in visited:
                dfs(v)
        stack.append(u)

    for i in range(V):
        if i not in visited:
            dfs(i)
    return stack[::-1]
```

---

#### **When to Use**
✅ Any task with dependencies on a DAG
🚫 Cyclic graphs — no valid order exists
