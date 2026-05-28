### **Breadth First Search (BFS) 🌐**

BFS explores a graph **level by level**, visiting all neighbors at the current depth before going deeper. Uses a queue.

---

#### **Summary**
- **Time Complexity:** O(V + E)
- **Space Complexity:** O(V)
- **Use cases:** shortest path in unweighted graphs, level-order traversal, bipartite check.

---

#### **How It Works**
1. Put the start node in a queue and mark as visited.
2. Dequeue a node, visit it, enqueue all unvisited neighbors.
3. Repeat until queue is empty.

---

#### **Python Implementation**
```python
from collections import deque

def bfs(graph, start):
    visited = {start}
    queue = deque([start])
    while queue:
        node = queue.popleft()
        print(node, end=' ')
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    return visited

# Shortest path (unweighted) — returns distance from start
def bfs_shortest(graph, start):
    dist = {start: 0}
    queue = deque([start])
    while queue:
        node = queue.popleft()
        for neighbor in graph[node]:
            if neighbor not in dist:
                dist[neighbor] = dist[node] + 1
                queue.append(neighbor)
    return dist

# Example
graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': [], 'E': ['F'], 'F': []
}
bfs(graph, 'A')                 # A B C D E F
print(bfs_shortest(graph, 'A')) # {'A':0,'B':1,'C':1,'D':2,'E':2,'F':2}
```

---

#### **When to Use**
✅ Shortest path in unweighted graphs
✅ Level-order tree traversal
🚫 Uses more memory than DFS for deep graphs
