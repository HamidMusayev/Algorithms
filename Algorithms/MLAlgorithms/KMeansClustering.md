# K-Means Clustering 🎯

---

## 🧠 Intuition

Imagine you have 100 customer profiles and want to group them into 3 segments. K-Means works iteratively: first place 3 cluster centers randomly, then assign each customer to their nearest center, then move each center to the average of its assigned customers. Repeat until stable.

Like herding sheep: centers move toward the sheep assigned to them, and sheep reassign to whichever shepherd is closest. Eventually, the shepherds settle in stable positions.

**Mental model:** "Assign each point to its nearest centroid → move each centroid to the mean of its points → repeat until centroids stop moving."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time per iteration | O(n × k × d) — n points, k clusters, d dimensions |
| Total time | O(I × n × k × d) — I iterations until convergence |
| Space | O(n + k) |

> Convergence typically in 10-100 iterations. k-means++ initialization reduces iterations needed.

---

## ⚙️ How It Works

1. **Initialize k centroids** (random or k-means++).
2. **Assignment step:** assign each point to its nearest centroid (Euclidean distance).
3. **Update step:** move each centroid to the mean of all points assigned to it.
4. Repeat steps 2-3 until centroids don't change (or change < ε).

**k-means++ initialization (better than random):**
- Choose the first centroid uniformly at random.
- For each subsequent centroid: choose a point with probability proportional to its distance from the nearest existing centroid.
- This gives much better starting positions → fewer iterations → better final clusters.

---

## 🔢 Step-by-Step Trace

6 points: `(1,1),(1,2),(2,1),(8,8),(9,8),(8,9)`, k=2

**Iteration 1:**
- Random centroids: C1=(1,1), C2=(8,8)
- Assign: (1,1),(1,2),(2,1) → C1; (8,8),(9,8),(8,9) → C2
- Update: C1 = mean((1,1),(1,2),(2,1)) = (1.33, 1.33)
         C2 = mean((8,8),(9,8),(8,9)) = (8.33, 8.33)

**Iteration 2:**
- Assign: same as before (centroids moved toward group centers, assignments unchanged)
- Update: C1=(1.33,1.33), C2=(8.33,8.33) → same
- **Converged!**

Final clusters: {(1,1),(1,2),(2,1)} and {(8,8),(9,8),(8,9)} ✅

---

## 🐍 Python Implementation

```python
import numpy as np

def k_means(X, k, max_iter=100, seed=42):
    """
    K-Means clustering from scratch.
    X: (n, d) array of data points
    k: number of clusters
    Returns: (labels, centroids, inertia)
    """
    np.random.seed(seed)
    n, d = X.shape

    # Initialize centroids randomly from data points
    idx = np.random.choice(n, k, replace=False)
    centroids = X[idx].copy()

    labels = np.zeros(n, dtype=int)

    for iteration in range(max_iter):
        # Assignment step: assign each point to nearest centroid
        # dist[i][j] = distance from point i to centroid j
        dist = np.sqrt(((X[:, None, :] - centroids[None, :, :]) ** 2).sum(axis=2))
        new_labels = dist.argmin(axis=1)

        # Check convergence (no label changes)
        if np.all(new_labels == labels):
            print(f"Converged after {iteration + 1} iterations")
            break
        labels = new_labels

        # Update step: move centroids to mean of assigned points
        new_centroids = np.array([
            X[labels == j].mean(axis=0) if np.any(labels == j) else centroids[j]
            for j in range(k)
        ])
        centroids = new_centroids

    # Compute inertia (total within-cluster sum of squares)
    inertia = sum(
        np.sum((X[labels == j] - centroids[j]) ** 2)
        for j in range(k)
        if np.any(labels == j)
    )
    return labels, centroids, inertia


def k_means_plus_plus(X, k):
    """K-Means++ initialization for better starting centroids."""
    n = len(X)
    centroids = [X[np.random.randint(n)]]   # First centroid: random

    for _ in range(k - 1):
        # Distance from each point to its nearest centroid
        dists = np.array([
            min(np.sum((x - c) ** 2) for c in centroids)
            for x in X
        ])
        # Choose next centroid with probability proportional to distance squared
        probs = dists / dists.sum()
        centroids.append(X[np.random.choice(n, p=probs)])

    return np.array(centroids)


# Example
X = np.array([
    [1, 1], [1, 2], [2, 1],     # Cluster 1
    [8, 8], [9, 8], [8, 9]      # Cluster 2
])

labels, centroids, inertia = k_means(X, k=2)
print("Labels:", labels)            # [0 0 0 1 1 1] or [1 1 1 0 0 0]
print("Centroids:", centroids)      # [[1.33,1.33], [8.33,8.33]]
print("Inertia:", inertia)          # Low = tight clusters
```

---

## 🎯 Recognize This Problem When...

- You need to **group unlabeled data** into k groups.
- Keywords: "clustering", "segmentation", "partition into groups".
- You know (or can estimate) the number of clusters k.
- Data is **numeric** and distance-based grouping makes sense.
- Use **elbow method** or **silhouette score** to choose k.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Customer segmentation | ✅ Classic application |
| Image color quantization (reduce colors) | ✅ Cluster pixel colors |
| Spherical, compact clusters | ✅ Euclidean distance works well |
| Non-spherical clusters (crescents, rings) | ❌ Use DBSCAN or GMM |
| Number of clusters unknown | ❌ Requires k as input |
| Clusters of very different sizes/densities | ❌ Use GMM or hierarchical clustering |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Gradient Descent](GradientDescent.md) | K-means is a coordinate descent optimization |
| [Decision Trees](DecisionTrees.md) | Both partition feature space; different approach |

---

## 📝 Practice / Resources

| Resource | Platform |
|----------|----------|
| [Mall Customer Segmentation](https://www.kaggle.com/vjchoudhary7/customer-segmentation-tutorial-in-python) | Kaggle |
| [K-Means Clustering docs](https://scikit-learn.org/stable/modules/clustering.html#k-means) | scikit-learn |
