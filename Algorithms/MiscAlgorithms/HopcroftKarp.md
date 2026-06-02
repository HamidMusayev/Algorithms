# Hopcroft-Karp Algorithm 🔗

---

## 🧠 Intuition

A **matching** in a bipartite graph is a set of edges with no shared endpoints. The **maximum matching** pairs as many left nodes to right nodes as possible (think: job-to-applicant assignment, task-to-worker allocation).

Naive augmenting path approaches are O(V × E). Hopcroft-Karp finds multiple **shortest augmenting paths** in one BFS + DFS round, reducing the total rounds from O(V) to O(√V). Result: O(E × √V).

**Mental model:** "Find all shortest augmenting paths at once (BFS for layers, DFS to augment), repeat until no augmenting path exists."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(E × √V) |
| Space Complexity | O(V + E) |
| Output | Maximum matching + which pairs are matched |

---

## ⚙️ How It Works

**Key concepts:**
- **Free node:** a node with no current match.
- **Augmenting path:** a path from a free left node to a free right node, alternating between unmatched and matched edges.
- Adding an augmenting path increases the matching size by 1 (flip matched/unmatched along the path).

**Algorithm:**
1. **BFS phase:** Build a layered graph of shortest augmenting paths from all free left nodes simultaneously. Stops when any free right node is reached.
2. **DFS phase:** For each free left node, follow the layered graph to find an augmenting path; flip edges along it to increase matching.
3. Repeat until BFS finds no augmenting path.

---

## 🔢 Step-by-Step Trace

Bipartite graph: Left = {a,b,c,d}, Right = {x,y,z}

Edges: a→x, a→y, b→x, c→y, c→z, d→z

**Round 1 BFS:** Find shortest paths (length 1) from free nodes:
- a→x (match!), c→z (match!), b→x... x taken... b→? no augmenting path

**Round 1 DFS results:** Match {a-x, c-z}

**Round 2 BFS:** Find length-3 paths:
- b→x→a→y (path: b unmatched, x matched to a, a unmatched side matches y)
- d→z→c→y... but y is free → augment

**Round 2 DFS results:** Match {b-x, a-y, d-z, c-?} → wait...

Final matching: {a-y, b-x, c-z, d-?} — d has no free match, so matching size = 3. ✅

---

## 🐍 Python Implementation

```python
from collections import deque

def hopcroft_karp(adj, left_nodes, right_nodes):
    """
    Find maximum bipartite matching using Hopcroft-Karp algorithm.
    adj: dict {left_node: [right_nodes]}
    left_nodes, right_nodes: sets/lists of left and right partition nodes
    Returns: (matching_size, match_left dict, match_right dict)
    """
    INF = float('inf')
    match_left = {u: None for u in left_nodes}     # match_left[u] = matched right node
    match_right = {v: None for v in right_nodes}   # match_right[v] = matched left node
    dist = {}

    def bfs():
        """BFS to find layered graph of shortest augmenting paths."""
        queue = deque()
        for u in left_nodes:
            if match_left[u] is None:
                dist[u] = 0
                queue.append(u)
            else:
                dist[u] = INF

        found = False
        while queue:
            u = queue.popleft()
            for v in adj[u]:
                w = match_right[v]   # Who is v currently matched to?
                if w is None:
                    found = True     # Found a free right node → augmenting path exists
                elif dist.get(w, INF) == INF:
                    dist[w] = dist[u] + 1
                    queue.append(w)
        return found

    def dfs(u):
        """DFS to augment along a path from u."""
        for v in adj[u]:
            w = match_right[v]
            if w is None or (dist.get(w, INF) == dist[u] + 1 and dfs(w)):
                match_left[u] = v      # Augment: u is now matched to v
                match_right[v] = u
                return True
        dist[u] = INF   # Mark u as exhausted in this BFS layer
        return False

    matching = 0
    while bfs():
        for u in left_nodes:
            if match_left[u] is None and dfs(u):
                matching += 1

    return matching, match_left, match_right


# Example: Job assignment
adj = {
    'Alice': ['A', 'B'],       # Alice can do jobs A or B
    'Bob': ['A'],              # Bob can only do job A
    'Carol': ['B', 'C'],       # Carol can do jobs B or C
    'Dave': ['C'],             # Dave can only do job C
}
left = ['Alice', 'Bob', 'Carol', 'Dave']
right = ['A', 'B', 'C']

size, match_l, match_r = hopcroft_karp(adj, left, right)
print(f"Maximum matching: {size}")          # 3
print("Assignments:")
for person, job in match_l.items():
    if job:
        print(f"  {person} → {job}")
# Alice → B, Bob → A, Carol → C  (or similar)
```

---

## 🎯 Recognize This Problem When...

- You need to **pair elements** from two groups with constraints (job-to-worker, task-to-server).
- Keywords: "maximum matching", "bipartite matching", "assignment problem".
- The graph has two disjoint sides and edges only between sides.
- You need the **maximum number of compatible pairs**.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Maximum bipartite matching | ✅ Optimal algorithm |
| Large bipartite graphs (E,V in thousands) | ✅ O(E√V) scales well |
| Job/task/student assignment | ✅ Classic application |
| Non-bipartite matching | ❌ Use Edmond's blossom algorithm |
| Weighted matching (maximize total weight) | ❌ Use Hungarian algorithm |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [BFS](../GraphAlgorithms/BFS.md) | BFS phase finds the shortest augmenting paths |
| [DFS](../GraphAlgorithms/DFS.md) | DFS phase follows the layered graph to augment |
| [Kruskal](../GraphAlgorithms/Kruskal.md) | Both use clever data structures for graph problems |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Maximum Bipartite Matching](https://www.hackerrank.com/challenges/red-knights-shortest-path/problem) | HackerRank | 🔴 Hard |
| [Minimum Operations to Make Array Sorted](https://leetcode.com/problems/minimum-operations-to-make-array-sorted/) | LeetCode | 🔴 Hard |
