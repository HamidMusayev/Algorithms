### **Hopcroft-Karp Algorithm 🛠**

Finds the **maximum matching in a bipartite graph** in **O(E · √V)** time — much faster than the naive O(V·E) augmenting-path approach.

A *matching* is a set of edges with no shared endpoints. The **maximum** matching uses as many edges as possible.

---

#### **Summary**
- **Time Complexity:** O(E · √V)
- **Space Complexity:** O(V + E)
- **Idea:** BFS to find **layered** alternating paths, then DFS to augment **multiple disjoint** paths per phase.

---

#### **Python Implementation**
```python
from collections import deque

def hopcroft_karp(adj, U, V):
    """
    adj: dict mapping each u in U to list of v in V
    U, V: sets/lists of vertices in left and right partition
    """
    INF = float('inf')
    pair_u = {u: None for u in U}
    pair_v = {v: None for v in V}
    dist = {}

    def bfs():
        queue = deque()
        for u in U:
            if pair_u[u] is None:
                dist[u] = 0
                queue.append(u)
            else:
                dist[u] = INF
        found = False
        while queue:
            u = queue.popleft()
            for v in adj[u]:
                pu = pair_v[v]
                if pu is None:
                    found = True
                elif dist.get(pu, INF) == INF:
                    dist[pu] = dist[u] + 1
                    queue.append(pu)
        return found

    def dfs(u):
        for v in adj[u]:
            pu = pair_v[v]
            if pu is None or (dist.get(pu, INF) == dist[u] + 1 and dfs(pu)):
                pair_u[u] = v
                pair_v[v] = u
                return True
        dist[u] = INF
        return False

    matching = 0
    while bfs():
        for u in U:
            if pair_u[u] is None and dfs(u):
                matching += 1
    return matching, pair_u

# Example
adj = {
    'a': ['x', 'y'],
    'b': ['x'],
    'c': ['y', 'z'],
    'd': ['z'],
}
print(hopcroft_karp(adj, ['a','b','c','d'], ['x','y','z']))
# (3, {'a':'y', 'b':'x', 'c':'z', 'd':None})
```

---

#### **When to Use**
✅ Job → worker assignments
✅ Bipartite matching at large scale (much faster than naive)
✅ Building block for many problems reducible to bipartite matching
