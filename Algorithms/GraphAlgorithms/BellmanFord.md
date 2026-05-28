### **Bellman-Ford Algorithm 🌐**

Computes shortest paths from a source to all nodes, **supporting negative edge weights**, and can **detect negative cycles**.

---

#### **Summary**
- **Time Complexity:** O(V × E)
- **Space Complexity:** O(V)
- **Handles negative weights:** ✅
- **Detects negative cycles:** ✅

---

#### **How It Works**
1. Initialize distances: 0 for source, ∞ for others.
2. **Relax all edges `V−1` times**: for each edge `(u, v, w)`, if `dist[u] + w < dist[v]`, update `dist[v]`.
3. One more pass: if any edge still relaxes → **negative cycle**.

---

#### **Python Implementation**
```python
def bellman_ford(edges, V, start):
    dist = [float('inf')] * V
    dist[start] = 0

    for _ in range(V - 1):
        for u, v, w in edges:
            if dist[u] + w < dist[v]:
                dist[v] = dist[u] + w

    # Detect negative cycle
    for u, v, w in edges:
        if dist[u] + w < dist[v]:
            raise ValueError("Graph contains a negative cycle")
    return dist

# Example: edges = (u, v, weight)
edges = [(0, 1, 4), (0, 2, 5), (1, 2, -3), (2, 3, 4)]
print(bellman_ford(edges, 4, 0))   # [0, 4, 1, 5]
```

---

#### **When to Use**
✅ Graphs with negative edge weights
✅ Negative cycle detection (currency arbitrage, etc.)
🚫 Slower than Dijkstra for non-negative weights
