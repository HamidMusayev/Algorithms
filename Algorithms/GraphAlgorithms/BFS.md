# Breadth-First Search (BFS) 🌊

---

## 🧠 Intuition

Imagine dropping a stone into a still pond. The ripples spread outward in perfectly circular rings — first the immediate neighbors, then their neighbors, and so on. BFS works exactly like this: starting from a source node, it visits every node at distance 1 before any node at distance 2.

**Mental model:** "Visit all friends before visiting friends-of-friends."

---

## 📊 Complexity

| Metric | Value |
|---|---|
| Time Complexity | O(V + E) |
| Space Complexity | O(V) |
| Works with weighted graphs (shortest path) | ❌ No — use Dijkstra |
| Works with unweighted graphs (shortest path) | ✅ Yes |
| Graph representation | Adjacency list preferred |

> V = number of vertices, E = number of edges. Every node and edge is visited at most once.

---

## ⚙️ How It Works

1. Add the start node to a queue and mark it as visited.
2. While the queue is not empty:
   - Dequeue the front node.
   - Process (or record) it.
   - For each unvisited neighbor, mark it visited and enqueue it.
3. When the queue is empty, all reachable nodes have been visited in level order.

> Key insight: using a queue (FIFO) guarantees that nodes are processed in the order they were discovered, which is level by level.

---

## 🔢 Step-by-Step Trace

Graph (undirected):
```
A -- B -- D
|    |
C -- E
```
Edges: A-B, A-C, B-D, B-E, C-E. Start: **A**

| Step | Dequeue | Queue after | Visited |
|---|---|---|---|
| 0 | — | [A] | {A} |
| 1 | A | [B, C] | {A, B, C} |
| 2 | B | [C, D, E] | {A, B, C, D, E} |
| 3 | C | [D, E] | {A, B, C, D, E} |
| 4 | D | [E] | {A, B, C, D, E} |
| 5 | E | [] | {A, B, C, D, E} |

BFS order: A → B → C → D → E

Shortest distances from A: A=0, B=1, C=1, D=2, E=2

---

## 🐍 Python Implementation

```python
from collections import deque

def bfs(graph, start):
    """Basic BFS traversal — visits all reachable nodes level by level."""
    visited = {start}          # use a set for O(1) membership checks
    queue = deque([start])     # deque gives O(1) popleft (list.pop(0) is O(n))

    order = []
    while queue:
        node = queue.popleft()   # dequeue from the front (FIFO)
        order.append(node)

        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)      # mark before enqueue to avoid duplicates
                queue.append(neighbor)     # enqueue unvisited neighbors

    return order


def bfs_shortest_path(graph, start, goal):
    """Returns the shortest path from start to goal in an unweighted graph."""
    if start == goal:
        return [start]

    visited = {start}
    # Store (current_node, path_so_far) in the queue
    queue = deque([(start, [start])])

    while queue:
        node, path = queue.popleft()

        for neighbor in graph[node]:
            if neighbor not in visited:
                new_path = path + [neighbor]
                if neighbor == goal:
                    return new_path          # found the shortest path
                visited.add(neighbor)
                queue.append((neighbor, new_path))

    return []   # goal not reachable


def bfs_distances(graph, start):
    """Returns distance (hop count) from start to every reachable node."""
    dist = {start: 0}
    queue = deque([start])

    while queue:
        node = queue.popleft()
        for neighbor in graph[node]:
            if neighbor not in dist:
                dist[neighbor] = dist[node] + 1   # one hop further than current node
                queue.append(neighbor)

    return dist


# --- Example ---
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D', 'E'],
    'C': ['A', 'E'],
    'D': ['B'],
    'E': ['B', 'C'],
}

print("BFS order:", bfs(graph, 'A'))
# Expected: ['A', 'B', 'C', 'D', 'E']

print("Shortest path A->D:", bfs_shortest_path(graph, 'A', 'D'))
# Expected: ['A', 'B', 'D']

print("Distances from A:", bfs_distances(graph, 'A'))
# Expected: {'A': 0, 'B': 1, 'C': 1, 'D': 2, 'E': 2}
```

---

## 🎯 Recognize This Problem When...

- The problem asks for the **shortest path** or **minimum number of steps** in a grid or unweighted graph
- The problem says "minimum number of moves / operations / jumps"
- You need to find the **nearest** something (nearest exit, nearest gate, nearest island)
- The problem involves a **level-order** traversal of a tree or graph
- You need to find if a path **exists** between two nodes (though DFS also works)
- Keywords: "minimum distance", "shortest path", "level by level", "spread", "infection"

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|---|---|
| Shortest path in unweighted graph | ✅ BFS guarantees minimum hops |
| Level-order tree traversal | ✅ Natural fit |
| Checking bipartiteness of a graph | ✅ Color nodes layer by layer |
| Detecting cycles in undirected graphs | ✅ Works well |
| Shortest path in weighted graph | ❌ Use Dijkstra instead |
| Memory is very constrained and graph is deep | ❌ DFS uses less stack space for deep graphs |
| You only need to know if a path exists | ❌ DFS is simpler and uses less memory |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|---|---|
| [DFS](DFS.md) | The other fundamental graph traversal; explores depth-first instead of breadth-first |
| [Dijkstra](Dijkstra.md) | Generalizes BFS to weighted graphs using a priority queue |
| [A*](AStar.md) | Further extends Dijkstra with a heuristic to guide the search toward the goal |
| [Topological Sort](TopologicalSort.md) | Kahn's algorithm for topological sort uses BFS on in-degree counts |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---|---|---|
| [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/) | LeetCode | 🟡 Medium |
| [Number of Islands](https://leetcode.com/problems/number-of-islands/) | LeetCode | 🟡 Medium |
| [Word Ladder](https://leetcode.com/problems/word-ladder/) | LeetCode | 🔴 Hard |
| [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/) | LeetCode | 🟡 Medium |
| [01 Matrix](https://leetcode.com/problems/01-matrix/) | LeetCode | 🟡 Medium |
| [Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix/) | LeetCode | 🟡 Medium |
| [Flood Fill](https://leetcode.com/problems/flood-fill/) | LeetCode | 🟢 Easy |
