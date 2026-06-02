# Decision Trees 🌲

---

## 🧠 Intuition

A decision tree makes predictions by asking a series of yes/no questions about the features. Like a flowchart: "Is the temperature > 30°C? → Yes → Is it raining? → No → Go to the beach!"

Each internal node asks about one feature, each branch is a decision, and each leaf gives a prediction. The tree is built by finding the question that **best separates** the data at each step.

**Mental model:** "Ask the most informative question, split the data, repeat on each group until data is pure or stopping criteria met."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Training time | O(n × d × log n) — n samples, d features |
| Prediction time | O(depth) |
| Space | O(nodes) = O(2^depth) worst case |

---

## ⚙️ How It Works

1. **Choose the best split:** for each feature, find the threshold that best separates classes.
   - **Classification:** use Gini impurity or information gain (entropy).
   - **Regression:** use MSE reduction.
2. **Split the data** on the chosen feature and threshold.
3. **Recurse** on each sub-group until:
   - All samples in the group have the same label (pure), or
   - Max depth is reached, or
   - Too few samples to split.
4. **Leaf node:** predict the majority class (classification) or mean (regression).

**Gini impurity:** `G = 1 - Σ pᵢ²` (lower = purer)

**Information gain:** `IG = H(parent) - Σ (|child|/|parent|) × H(child)` where H is entropy.

---

## 🔢 Step-by-Step Trace

Data: 4 samples, feature = `[1, 2, 3, 4]`, labels = `[0, 0, 1, 1]`

**Try split at x=2.5:**
- Left group: `[1,2]` → labels `[0,0]` → Gini = 0 (pure!)
- Right group: `[3,4]` → labels `[1,1]` → Gini = 0 (pure!)
- Weighted Gini = 0 → perfect split!

Tree:
```
x ≤ 2.5?
   YES → predict 0
   NO  → predict 1
```

---

## 🐍 Python Implementation

```python
import numpy as np

def gini_impurity(y):
    """Compute Gini impurity of a label array."""
    if len(y) == 0:
        return 0
    classes, counts = np.unique(y, return_counts=True)
    probs = counts / len(y)
    return 1 - np.sum(probs ** 2)


def best_split(X, y):
    """Find the best feature and threshold to split on."""
    n, d = X.shape
    best_gain, best_feat, best_thresh = -1, None, None
    parent_gini = gini_impurity(y)

    for feature in range(d):
        thresholds = np.unique(X[:, feature])
        for thresh in thresholds:
            left_mask = X[:, feature] <= thresh
            right_mask = ~left_mask
            if not any(left_mask) or not any(right_mask):
                continue

            # Weighted gini of the split
            n_left, n_right = sum(left_mask), sum(right_mask)
            weighted_gini = (n_left * gini_impurity(y[left_mask]) +
                            n_right * gini_impurity(y[right_mask])) / n
            gain = parent_gini - weighted_gini

            if gain > best_gain:
                best_gain = gain
                best_feat = feature
                best_thresh = thresh

    return best_feat, best_thresh


class DecisionTreeClassifier:
    """Simple decision tree classifier built from scratch."""

    def __init__(self, max_depth=5, min_samples_split=2):
        self.max_depth = max_depth
        self.min_samples_split = min_samples_split
        self.tree = None

    def fit(self, X, y):
        self.tree = self._build_tree(X, y, depth=0)

    def _build_tree(self, X, y, depth):
        # Stopping conditions
        if (depth >= self.max_depth or
            len(y) < self.min_samples_split or
            gini_impurity(y) == 0):
            return {"leaf": True, "prediction": np.bincount(y).argmax()}

        feat, thresh = best_split(X, y)
        if feat is None:
            return {"leaf": True, "prediction": np.bincount(y).argmax()}

        left_mask = X[:, feat] <= thresh
        return {
            "leaf": False,
            "feature": feat,
            "threshold": thresh,
            "left": self._build_tree(X[left_mask], y[left_mask], depth + 1),
            "right": self._build_tree(X[~left_mask], y[~left_mask], depth + 1),
        }

    def predict_one(self, x, node):
        if node["leaf"]:
            return node["prediction"]
        if x[node["feature"]] <= node["threshold"]:
            return self.predict_one(x, node["left"])
        return self.predict_one(x, node["right"])

    def predict(self, X):
        return np.array([self.predict_one(x, self.tree) for x in X])


# Example: classify points
X = np.array([[1, 2], [2, 3], [3, 1], [4, 4], [5, 2], [6, 5]])
y = np.array([0, 0, 0, 1, 1, 1])   # Binary classes

tree = DecisionTreeClassifier(max_depth=3)
tree.fit(X, y)
print("Predictions:", tree.predict(X))   # [0 0 0 1 1 1]
print("Accuracy:", np.mean(tree.predict(X) == y))   # 1.0
```

---

## 🎯 Recognize This Problem When...

- You want an **interpretable** model — you can explain every decision.
- The data is **tabular** (rows and columns, mixed types).
- You need a quick **baseline** before trying complex models.
- Keywords: "decision rule", "if-then conditions", "interpretable model".

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Interpretability is critical | ✅ Each path is a human-readable rule |
| Mixed numerical/categorical features | ✅ Handles both naturally |
| Quick baseline model | ✅ Easy to train and interpret |
| Noisy data (risk of overfitting) | ❌ Prune or use Random Forest |
| Complex relationships (images, text) | ❌ Use neural networks |
| Need probabilistic outputs | ❌ Use logistic regression or calibration |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Random Forest](RandomForest.md) | Ensemble of many decision trees — much more robust |
| [Gradient Descent](GradientDescent.md) | Decision trees don't use gradient descent |

---

## 📝 Practice / Resources

| Resource | Platform |
|----------|----------|
| [Decision Tree](https://scikit-learn.org/stable/modules/tree.html) | scikit-learn docs |
| Iris Classification | Classic beginner dataset |
| [titanic survival prediction](https://www.kaggle.com/c/titanic) | Kaggle competition |
