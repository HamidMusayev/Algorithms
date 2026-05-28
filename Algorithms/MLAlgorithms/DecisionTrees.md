### **Decision Trees 🤖**

A supervised learning model that splits data by features in a **tree structure**. Each internal node tests a feature; leaves predict a class or value.

Common split criteria:
- **Gini impurity** (CART, classification)
- **Information gain / Entropy** (ID3, C4.5)
- **MSE reduction** (regression trees)

---

#### **Summary**
- **Train time:** O(n · d · log n) on average
- **Predict time:** O(depth)
- **Strengths:** interpretable, no scaling needed
- **Weaknesses:** prone to overfitting → prune, or use [Random Forest](RandomForest.md)

---

#### **Python Implementation (with scikit-learn)**
```python
from sklearn.tree import DecisionTreeClassifier
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split

X, y = load_iris(return_X_y=True)
X_tr, X_te, y_tr, y_te = train_test_split(X, y, random_state=42)

clf = DecisionTreeClassifier(criterion="gini", max_depth=3)
clf.fit(X_tr, y_tr)
print("Accuracy:", clf.score(X_te, y_te))   # ~0.97
```

---

#### **From-Scratch Sketch (Gini Split, Binary Classification)**
```python
import numpy as np

def gini(y):
    _, counts = np.unique(y, return_counts=True)
    p = counts / counts.sum()
    return 1 - (p * p).sum()

def best_split(X, y):
    n, d = X.shape
    best = (None, None, float('inf'))
    for f in range(d):
        for t in np.unique(X[:, f]):
            left = y[X[:, f] <= t]
            right = y[X[:, f] > t]
            if len(left) == 0 or len(right) == 0: continue
            g = (len(left) * gini(left) + len(right) * gini(right)) / n
            if g < best[2]:
                best = (f, t, g)
    return best
```

---

#### **When to Use**
✅ Interpretable models, tabular data
✅ Mixed numerical / categorical features
🚫 High-variance on noisy data → use Random Forest / Gradient Boosting
