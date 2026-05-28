### **A* Search Algorithm 🌐**

A* (A-star) is an **informed search** algorithm that finds the shortest path using a heuristic `h(n)` that **estimates** the remaining cost to the goal.

It picks nodes with the smallest **f(n) = g(n) + h(n)** where:
- `g(n)` = actual cost from start to `n`
- `h(n)` = heuristic estimate from `n` to goal

---

#### **Summary**
- **Time Complexity:** O(E) (depends heavily on heuristic)
- **Space Complexity:** O(V)
- **Optimal:** ✅ if `h` is **admissible** (never overestimates)
- **Use cases:** pathfinding in games, maps, robotics.

---

#### **Python Implementation**
```python
import heapq

def a_star(graph, start, goal, h):
    # graph[node] = [(neighbor, cost), ...]
    open_set = [(h(start), 0, start, [start])]
    visited = {}

    while open_set:
        f, g, node, path = heapq.heappop(open_set)
        if node == goal:
            return path, g
        if node in visited and visited[node] <= g:
            continue
        visited[node] = g
        for neighbor, cost in graph[node]:
            ng = g + cost
            heapq.heappush(open_set, (ng + h(neighbor), ng, neighbor, path + [neighbor]))
    return None, float('inf')

# Example with a simple grid-like heuristic
graph = {
    'A': [('B', 1), ('C', 4)],
    'B': [('C', 2), ('D', 5)],
    'C': [('D', 1)],
    'D': []
}
# Trivial heuristic: 0 (turns A* into Dijkstra)
print(a_star(graph, 'A', 'D', lambda _: 0))  # (['A','B','C','D'], 4)
```

---

#### **When to Use**
✅ Pathfinding with a good heuristic (Manhattan/Euclidean distance)
✅ Game AI, GPS, robotics
🚫 Bad heuristic → degrades to Dijkstra (or worse if not admissible)
