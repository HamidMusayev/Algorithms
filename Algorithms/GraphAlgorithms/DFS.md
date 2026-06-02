# Depth-First Search (DFS) 🔍

---

## 🧠 Intuition

Imagine exploring a maze: you pick a corridor and follow it all the way to a dead end (or the exit) before backtracking and trying the next corridor. That is exactly how DFS works — it dives as deep as possible along one path before exploring alternatives.

**Mental model:** "Go deep first, backtrack when stuck."

---

## 📊 Complexity

| Metric | Value |
|---|---|
| Time Complexity | O(V + E) |
| Space Complexity | O(V) (recursion stack or explicit stack) |
| Finds shortest path | ❌ No — use BFS for unweighted, Dijkstra for weighted |
| Works on directed and undirected graphs | ✅ Yes |
| Detects cycles | ✅ Yes |

> V = number of vertices, E = number of edges. Every node and edge is visited at most once.

---

## ⚙️ How It Works

**Recursive version:**
1. Mark the current node as visited.
2. Process (record) the node.
3. For each unvisited neighbor, recursively call DFS on it.
4. When all neighbors are visited, return (backtrack).

**Iterative version (using an explicit stack):**
1. Push the start node onto a stack.
2. While the stack is not empty:
   - Pop the top node.
   - If not visited, mark it visited and process it.
   - Push all unvisited neighbors onto the stack.

> The key difference from BFS is the data structure: DFS uses a **stack** (LIFO), BFS uses a **queue** (FIFO).

---

## 🔢 Step-by-Step Trace

Graph (directed):
```
A → B → D
↓   ↓
C → E
```
Edges: A→B, A→C, B→D, B→E, C→E. Start: **A**

| Step | Visit | Call Stack (top = current) | Visited |
|---|---|---|---|
| 1 | A | [A] | {A} |
| 2 | B (first neighbor of A) | [A, B] | {A, B} |
| 3 | D (first neighbor of B) | [A, B, D] | {A, B, D} |
| 4 | Backtrack from D | [A, B] | {A, B, D} |
| 5 | E (next neighbor of B) | [A, B, E] | {A, B, D, E} |
| 6 | Backtrack from E, then B | [A] | {A, B, D, E} |
| 7 | C (next neighbor of A) | [A, C] | {A, B, C, D, E} |
| 8 | E already visited, backtrack | [] | {A, B, C, D, E} |

DFS order: A → B → D → E → C

---

## 🐍 Python Implementation

```python
import sys
sys.setrecursionlimit(10000)   # increase limit for large graphs

# --- Recursive DFS ---
def dfs_recursive(graph, start, visited=None):
    """
    Explores the graph depth-first using recursion.
    Returns the set of all visited nodes.
    """
    if visited is None:
        visited = set()      # create fresh set on first call only

    visited.add(start)
    print(start, end=' ')

    for neighbor in graph[start]:
        if neighbor not in visited:
            dfs_recursive(graph, neighbor, visited)   # go deeper before backtracking

    return visited


# --- Iterative DFS ---
def dfs_iterative(graph, start):
    """
    Iterative DFS using an explicit stack — avoids recursion limit issues.
    Note: order may differ from recursive version due to stack reversal.
    """
    visited = set()
    stack = [start]       # stack stores nodes to process

    order = []
    while stack:
        node = stack.pop()    # LIFO: last in, first out
        if node in visited:
            continue          # may push same node multiple times; skip if already done
        visited.add(node)
        order.append(node)

        # Push neighbors in reverse so that left-to-right order is preserved
        for neighbor in reversed(graph[node]):
            if neighbor not in visited:
                stack.append(neighbor)

    return order


# --- DFS for cycle detection in a directed graph ---
def has_cycle(graph):
    """
    Returns True if the directed graph has a cycle.
    Uses three states: unvisited (0), in-progress (1), done (2).
    """
    state = {node: 0 for node in graph}   # 0=unvisited, 1=in stack, 2=done

    def dfs(u):
        state[u] = 1    # mark as currently being explored (in the recursion stack)
        for v in graph[u]:
            if state[v] == 1:   # back-edge found → cycle
                return True
            if state[v] == 0 and dfs(v):
                return True
        state[u] = 2    # fully explored, no cycle through this node
        return False

    return any(dfs(node) for node in graph if state[node] == 0)


# --- Example ---
graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['E'],
    'D': [],
    'E': [],
}

print("Recursive DFS:", end=' ')
dfs_recursive(graph, 'A')
# Expected: A B D E C

print()
print("Iterative DFS:", dfs_iterative(graph, 'A'))
# Expected: ['A', 'B', 'D', 'E', 'C']

print("Has cycle:", has_cycle(graph))
# Expected: False

cyclic_graph = {'A': ['B'], 'B': ['C'], 'C': ['A']}
print("Cyclic graph has cycle:", has_cycle(cyclic_graph))
# Expected: True
```

---

## 🎯 Recognize This Problem When...

- The problem asks to **find all paths** between two nodes
- You need to check if a **path exists** (connectivity)
- The problem involves **cycle detection** in a graph
- You need **topological ordering** of tasks with dependencies
- You need to find **connected components** or **strongly connected components**
- You are working with **recursively structured** data (trees, nested structures)
- Keywords: "all possible paths", "explore", "backtrack", "reachable", "component", "island", "connected"

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|---|---|
| Finding if a path exists | ✅ Simple and efficient |
| Cycle detection | ✅ Natural with the in-stack state tracking |
| Topological sort | ✅ Post-order DFS gives reverse topological order |
| Finding all paths in a graph | ✅ Backtracking variant of DFS |
| Shortest path in unweighted graph | ❌ Use BFS — DFS does not guarantee shortest path |
| Shortest path in weighted graph | ❌ Use Dijkstra or Bellman-Ford |
| Very deep graphs (risk of stack overflow) | ❌ Use iterative DFS or increase recursion limit |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|---|---|
| [BFS](BFS.md) | The sibling traversal; explores breadth-first — gives shortest paths in unweighted graphs |
| [Topological Sort](TopologicalSort.md) | DFS post-order on a DAG yields a topological ordering |
| [Tarjan](Tarjan.md) | Uses DFS with disc/low values to find strongly connected components |
| [Kosaraju](Kosaraju.md) | Uses two DFS passes to find strongly connected components |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---|---|---|
| [Number of Islands](https://leetcode.com/problems/number-of-islands/) | LeetCode | 🟡 Medium |
| [Max Area of Island](https://leetcode.com/problems/max-area-of-island/) | LeetCode | 🟡 Medium |
| [Path Sum (Tree)](https://leetcode.com/problems/path-sum/) | LeetCode | 🟢 Easy |
| [All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/) | LeetCode | 🟡 Medium |
| [Course Schedule (cycle detection)](https://leetcode.com/problems/course-schedule/) | LeetCode | 🟡 Medium |
| [Clone Graph](https://leetcode.com/problems/clone-graph/) | LeetCode | 🟡 Medium |
| [Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/) | LeetCode | 🟡 Medium |
