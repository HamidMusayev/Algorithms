### **Depth First Search (DFS) 🌐**

DFS explores a graph by going **as deep as possible** along each branch before backtracking. Implemented with recursion or an explicit stack.

---

#### **Summary**
- **Time Complexity:** O(V + E)
- **Space Complexity:** O(V) (recursion stack / visited set)
- **Use cases:** topological sort, cycle detection, connected components, maze/path search.

---

#### **How It Works**
1. Mark the start node as visited.
2. For each unvisited neighbor, recurse into it.
3. Backtrack when no unvisited neighbors remain.

---

#### **Python Implementation**
```python
def dfs(graph, start, visited=None):
    if visited is None:
        visited = set()
    visited.add(start)
    print(start, end=' ')
    for neighbor in graph[start]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)
    return visited

# Iterative version
def dfs_iterative(graph, start):
    visited, stack = set(), [start]
    while stack:
        node = stack.pop()
        if node not in visited:
            visited.add(node)
            print(node, end=' ')
            stack.extend(graph[node])
    return visited

# Example
graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': [], 'E': ['F'], 'F': []
}
dfs(graph, 'A')   # A B D E F C
```

---

#### **When to Use**
✅ Path finding when path existence (not shortest) matters
✅ Topological sort, SCC, cycle detection
🚫 Doesn't give shortest path in unweighted graphs (use BFS)
