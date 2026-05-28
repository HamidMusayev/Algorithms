### **Floyd-Warshall Algorithm 🌐**

Computes **all-pairs shortest paths** in a weighted graph (negative edges allowed, no negative cycles). Dynamic programming approach.

---

#### **Summary**
- **Time Complexity:** O(V³)
- **Space Complexity:** O(V²)
- **Handles negative weights:** ✅ (but no negative cycles)

---

#### **How It Works**
For every intermediate node `k`, check whether going `i → k → j` is shorter than the current `i → j`:
```
dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
```

---

#### **Python Implementation**
```python
def floyd_warshall(graph):
    V = len(graph)
    dist = [row[:] for row in graph]  # copy

    for k in range(V):
        for i in range(V):
            for j in range(V):
                if dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]
    return dist

# Example (INF for no edge)
INF = float('inf')
graph = [
    [0,   3,   INF, 7],
    [8,   0,   2,   INF],
    [5,   INF, 0,   1],
    [2,   INF, INF, 0],
]
for row in floyd_warshall(graph):
    print(row)
```

---

#### **When to Use**
✅ All-pairs shortest paths in small/medium graphs
✅ Transitive closure
🚫 Too slow for very large graphs (O(V³))
