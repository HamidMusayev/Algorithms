# Random Forest 🌳🌳🌳

---

## 🧠 Intuition

A single decision tree is like asking one expert — it can be confident but wrong. A random forest asks 100 experts, each trained on a slightly different data sample and considering different features. Their collective vote is far more reliable.

The two key ideas:
1. **Bagging:** each tree sees a different random subset (bootstrap sample) of the training data.
2. **Feature randomness:** at each split, only a random subset of features is considered (e.g., √d features).

The random differences between trees ensure they make **different errors**, and their errors cancel out in the aggregate.

**Mental model:** "100 diverse, somewhat-random trees vote. Majority wins. Diversity + number = accuracy."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Training time | O(T × n × d × log n) — T trees |
| Prediction time | O(T × depth) |
| Space | O(T × nodes per tree) |

> T = number of trees (100-500 typical), n = samples, d = features.

---

## ⚙️ How It Works

1. For each of T trees:
   - **Bootstrap sample:** randomly sample n rows WITH replacement from the training data.
   - **Build decision tree:** at each split, consider only a random subset of `m ≈ √d` features.
2. **Predict:**
   - Classification: each tree votes → take the **majority vote**.
   - Regression: each tree predicts a number → take the **mean**.
3. **Feature importance:** how much does each feature reduce impurity across all trees and splits?

---

## 🔢 Step-by-Step Trace

Training data: 6 samples, 2 classes.

**Tree 1:** Bootstrap sample = [samples 1,1,3,4,5,6], features considered at each split = {f1, f2}
**Tree 2:** Bootstrap sample = [samples 2,2,3,4,4,6], features considered = {f1, f3}
**Tree 3:** Bootstrap sample = [samples 1,2,3,5,5,6], features considered = {f2, f3}

**Prediction for new point x:**
- Tree 1 → class 0
- Tree 2 → class 1
- Tree 3 → class 1
- Majority vote: **class 1** ✅

---

## 🐍 Python Implementation

```python
import numpy as np
from collections import Counter

class SimpleDecisionTree:
    """Minimal decision tree for use inside Random Forest."""

    def __init__(self, max_depth=5, max_features=None):
        self.max_depth = max_depth
        self.max_features = max_features
        self.tree = None

    def gini(self, y):
        if len(y) == 0: return 0
        _, counts = np.unique(y, return_counts=True)
        p = counts / len(y)
        return 1 - np.sum(p**2)

    def best_split(self, X, y):
        n, d = X.shape
        # Random feature subset (key to Random Forest!)
        m = self.max_features or int(np.sqrt(d))
        feat_indices = np.random.choice(d, min(m, d), replace=False)

        best, best_f, best_t = float('inf'), None, None
        for f in feat_indices:
            for t in np.unique(X[:, f]):
                left = y[X[:, f] <= t]
                right = y[X[:, f] > t]
                if len(left) == 0 or len(right) == 0: continue
                gini = (len(left)*self.gini(left) + len(right)*self.gini(right)) / n
                if gini < best:
                    best, best_f, best_t = gini, f, t
        return best_f, best_t

    def build(self, X, y, depth=0):
        if depth >= self.max_depth or self.gini(y) == 0 or len(y) < 2:
            return Counter(y).most_common(1)[0][0]
        f, t = self.best_split(X, y)
        if f is None:
            return Counter(y).most_common(1)[0][0]
        mask = X[:, f] <= t
        return {'f': f, 't': t,
                'left': self.build(X[mask], y[mask], depth+1),
                'right': self.build(X[~mask], y[~mask], depth+1)}

    def fit(self, X, y):
        self.tree = self.build(X, y)

    def predict_one(self, x, node):
        if not isinstance(node, dict): return node
        return self.predict_one(x, node['left'] if x[node['f']] <= node['t'] else node['right'])

    def predict(self, X):
        return np.array([self.predict_one(x, self.tree) for x in X])


class RandomForest:
    """Random Forest classifier from scratch."""

    def __init__(self, n_trees=100, max_depth=5, max_features=None):
        self.n_trees = n_trees
        self.max_depth = max_depth
        self.max_features = max_features
        self.trees = []

    def fit(self, X, y):
        n = len(X)
        self.trees = []
        for _ in range(self.n_trees):
            # Bootstrap: sample n rows with replacement
            indices = np.random.choice(n, n, replace=True)
            X_boot, y_boot = X[indices], y[indices]
            tree = SimpleDecisionTree(max_depth=self.max_depth, max_features=self.max_features)
            tree.fit(X_boot, y_boot)
            self.trees.append(tree)

    def predict(self, X):
        # Aggregate predictions: majority vote
        predictions = np.array([tree.predict(X) for tree in self.trees])
        return np.apply_along_axis(lambda col: Counter(col).most_common(1)[0][0], 0, predictions)


# Example
np.random.seed(42)
X = np.random.randn(100, 4)
y = (X[:, 0] + X[:, 1] > 0).astype(int)   # Simple rule: first two features sum > 0

rf = RandomForest(n_trees=50, max_depth=4)
rf.fit(X, y)
print("Accuracy:", np.mean(rf.predict(X) == y))   # Should be high
```

---

## 🎯 Recognize This Problem When...

- You need a **strong baseline** for a tabular data problem.
- You want **feature importances** to understand which variables matter.
- The dataset is **medium-sized** (thousands to millions of rows).
- A single decision tree overfits (Random Forest regularizes it).

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Tabular data, classification or regression | ✅ Often state-of-the-art |
| Need feature importances | ✅ Free with Random Forest |
| Robust baseline before deep learning | ✅ Strong starting point |
| Very high-dimensional sparse data (NLP) | ❌ Linear models or neural nets work better |
| Real-time inference with low latency | ❌ T trees × depth predictions can be slow |
| Need interpretability of each decision | ❌ Individual trees are interpretable; forest is not |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Decision Trees](DecisionTrees.md) | Base learner — each tree in the forest |
| [Gradient Descent](GradientDescent.md) | Gradient Boosted Trees use GD to train ensembles differently |

---

## 📝 Practice / Resources

| Resource | Platform |
|----------|----------|
| [Titanic Survival Prediction](https://www.kaggle.com/c/titanic) | Kaggle |
| [House Price Prediction](https://www.kaggle.com/c/house-prices-advanced-regression-techniques) | Kaggle |
| [Random Forest docs](https://scikit-learn.org/stable/modules/ensemble.html#forest) | scikit-learn |
