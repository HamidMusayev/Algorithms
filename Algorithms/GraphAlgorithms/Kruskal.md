### **Kruskal's Algorithm 🌐**

Builds a **Minimum Spanning Tree (MST)** by sorting all edges and adding the cheapest one that doesn't create a cycle (uses **Union-Find / DSU**).

---

#### **Summary**
- **Time Complexity:** O(E log E)
- **Space Complexity:** O(V)
- **Approach:** Greedy + Disjoint Set Union

---

#### **How It Works**
1. Sort all edges by weight ascending.
2. For each edge `(u, v, w)`: if `u` and `v` are in different components, **union them** and add the edge to MST.
3. Stop when MST has `V−1` edges.

---

#### **Python Implementation**
```python
class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]  # path compression
            x = self.parent[x]
        return x

    def union(self, x, y):
        rx, ry = self.find(x), self.find(y)
        if rx == ry: return False
        if self.rank[rx] < self.rank[ry]: rx, ry = ry, rx
        self.parent[ry] = rx
        if self.rank[rx] == self.rank[ry]: self.rank[rx] += 1
        return True

def kruskal(V, edges):
    edges.sort(key=lambda e: e[2])
    dsu = DSU(V)
    mst, total = [], 0
    for u, v, w in edges:
        if dsu.union(u, v):
            mst.append((u, v, w))
            total += w
    return mst, total

# Example: edges = (u, v, weight)
edges = [(0, 1, 1), (0, 2, 3), (1, 2, 1), (1, 3, 4), (2, 3, 1)]
print(kruskal(4, edges))   # ([(0,1,1),(1,2,1),(2,3,1)], 3)
```

---

#### **When to Use**
✅ Sparse graphs (few edges)
✅ Easy edge-list representation
🚫 Dense graphs — Prim is usually faster
