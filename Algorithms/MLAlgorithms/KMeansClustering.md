### **K-Means Clustering 🤖**

An **unsupervised** algorithm that partitions `n` points into `k` clusters by minimizing the within-cluster variance.

---

#### **Algorithm (Lloyd's)**
1. Initialize `k` centroids (random or **k-means++**).
2. **Assign** each point to its nearest centroid.
3. **Update** each centroid = mean of points assigned to it.
4. Repeat 2–3 until centroids stop changing (or max iterations).

---

#### **Summary**
- **Time per iteration:** O(n · k · d)
- **Space Complexity:** O(n + k)
- **Caveats:** sensitive to init (use **k-means++**) and to outliers; needs `k` chosen in advance.

---

#### **Python Implementation (NumPy)**
```python
import numpy as np

def k_means(X, k, max_iter=100, seed=0):
    rng = np.random.default_rng(seed)
    # init: pick k random rows
    idx = rng.choice(len(X), k, replace=False)
    centroids = X[idx].copy()

    for _ in range(max_iter):
        # distances: (n, k)
        dists = ((X[:, None, :] - centroids[None, :, :]) ** 2).sum(axis=2)
        labels = dists.argmin(axis=1)
        new_centroids = np.array([X[labels == j].mean(axis=0) if (labels == j).any() else centroids[j]
                                  for j in range(k)])
        if np.allclose(new_centroids, centroids):
            break
        centroids = new_centroids
    return labels, centroids

# Example
X = np.array([[1,1], [1,2], [2,1], [8,8], [9,8], [8,9]])
labels, centroids = k_means(X, k=2)
print(labels)        # e.g. [1 1 1 0 0 0]
print(centroids)     # [[8.33, 8.33], [1.33, 1.33]]
```

> In practice, use `sklearn.cluster.KMeans` (it uses k-means++ and is well-optimized).

---

#### **When to Use**
✅ Customer segmentation, vector quantization, image color reduction
✅ Need fast, spherical clusters
🚫 Non-spherical or unknown `k` → try **DBSCAN**, **GMM**, or hierarchical clustering
