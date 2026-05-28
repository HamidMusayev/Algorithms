### **Hamiltonian Cycle 🔄**

A **Hamiltonian Cycle** visits every vertex of a graph **exactly once** and returns to the start. Determining its existence is **NP-complete**; we use backtracking.

---

#### **Summary**
- **Time Complexity:** O(N!) in the worst case
- **Space Complexity:** O(N)

---

#### **Python Implementation**
```python
def hamiltonian_cycle(graph):
    n = len(graph)
    path = [0] + [-1] * (n - 1)

    def is_safe(v, pos):
        if not graph[path[pos - 1]][v]: return False
        if v in path: return False
        return True

    def backtrack(pos):
        if pos == n:
            return graph[path[-1]][path[0]] == 1   # close cycle
        for v in range(1, n):                      # 0 fixed as start
            if is_safe(v, pos):
                path[pos] = v
                if backtrack(pos + 1):
                    return True
                path[pos] = -1
        return False

    return path if backtrack(1) else None

# Example: graph as adjacency matrix
graph = [
    [0, 1, 0, 1, 0],
    [1, 0, 1, 1, 1],
    [0, 1, 0, 0, 1],
    [1, 1, 0, 0, 1],
    [0, 1, 1, 1, 0],
]
print(hamiltonian_cycle(graph))   # [0, 1, 2, 4, 3] (one valid cycle)
```

---

#### **When to Use**
✅ Routing, TSP-like problems (Hamiltonian path is a subproblem)
✅ Constraint search problems
🚫 Large graphs need heuristics / branch-and-bound
