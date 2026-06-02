# Support Vector Machine (SVM) 🎯

---

## 🧠 Intuition

Given two classes of points, SVM finds the **widest possible gap (margin)** between them. The decision boundary is the hyperplane that maximizes this margin. The points closest to the boundary are called **support vectors** — they're the only training examples that matter for defining the boundary.

For non-linearly separable data, the **kernel trick** maps points to a higher-dimensional space where a linear separator exists — without ever explicitly computing the high-dimensional coordinates.

**Mental model:** "Draw the widest possible street between two classes. Only the points touching the street edges (support vectors) define the street."

---

## 📊 Complexity

| Operation | Time |
|-----------|------|
| Training (standard SVM) | O(n² to n³) depending on solver |
| Prediction | O(n_sv × d) — n_sv = support vectors |
| Space | O(n_sv) — only stores support vectors |

> In practice, use the `LibSVM` solver (scipy/sklearn) which handles this efficiently.

---

## ⚙️ How It Works

**Linear SVM (separable case):**
1. Find the hyperplane `w·x + b = 0` that:
   - Correctly classifies all points: `y_i(w·x_i + b) ≥ 1`
   - **Maximizes the margin**: `2/||w||`
2. This is a **quadratic programming** problem: minimize `||w||²` subject to the constraints.

**Soft-margin SVM (non-separable):**
- Allow some misclassifications with **slack variables** ξᵢ.
- Minimize `||w||² + C × Σξᵢ` — C controls trade-off between margin size and violations.
- Larger C → smaller margin, fewer violations. Smaller C → larger margin, more violations.

**Kernel trick:**
- Replace `x·z` with `K(x, z)` (kernel function).
- The kernel implicitly computes the dot product in a high-dimensional space.
- Common kernels: RBF (Gaussian), polynomial, sigmoid.

---

## 🔢 Step-by-Step Trace

2D points: Class +1: `(2,2),(2,3)`, Class -1: `(-2,-2),(-2,-3)`

Optimal hyperplane: `x₁ + x₂ = 0` (i.e., w=(1,1), b=0)

Support vectors: `(2,2)` and `(-2,-2)` — on the boundary `w·x + b = ±2`

Margin width = `2/||w|| = 2/√2 = √2 ≈ 1.41`

Any other hyperplane through the origin would have a smaller margin. ✅

---

## 🐍 Python Implementation

```python
import numpy as np

class LinearSVM:
    """
    Simple linear SVM using gradient descent on the hinge loss.
    For production use, always prefer sklearn.svm.SVC or LibSVM.
    """

    def __init__(self, C=1.0, lr=0.001, epochs=1000):
        self.C = C         # Regularization parameter
        self.lr = lr       # Learning rate
        self.epochs = epochs
        self.w = None
        self.b = 0

    def fit(self, X, y):
        """
        Train SVM with SGD on hinge loss.
        y must be in {-1, +1}.
        """
        n, d = X.shape
        self.w = np.zeros(d)

        for epoch in range(self.epochs):
            # Shuffle data each epoch
            idx = np.random.permutation(n)
            for i in idx:
                # Hinge loss gradient
                margin = y[i] * (X[i] @ self.w + self.b)
                if margin < 1:
                    # Misclassified or within margin → update with full gradient
                    self.w -= self.lr * (self.w - self.C * y[i] * X[i])
                    self.b -= self.lr * (-self.C * y[i])
                else:
                    # Correctly classified with margin → only regularization
                    self.w -= self.lr * self.w

    def predict(self, X):
        """Predict class labels (-1 or +1)."""
        return np.sign(X @ self.w + self.b)

    def score(self, X, y):
        return np.mean(self.predict(X) == y)


# Example: linearly separable data
np.random.seed(42)
X_pos = np.random.randn(50, 2) + [2, 2]    # Class +1 centered at (2,2)
X_neg = np.random.randn(50, 2) + [-2, -2]  # Class -1 centered at (-2,-2)
X = np.vstack([X_pos, X_neg])
y = np.array([1] * 50 + [-1] * 50)

svm = LinearSVM(C=1.0, lr=0.001, epochs=500)
svm.fit(X, y)
print(f"Training accuracy: {svm.score(X, y):.2%}")    # Should be ~100%
print(f"Weight vector: {svm.w.round(3)}")
print(f"Bias: {svm.b:.3f}")


# Using sklearn for production (with kernel support)
from sklearn.svm import SVC
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler

X, y = make_classification(n_samples=500, n_features=10, random_state=42)
X = StandardScaler().fit_transform(X)    # Feature scaling is CRUCIAL for SVM!
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=42)

clf = SVC(kernel='rbf', C=1.0, gamma='scale')
clf.fit(X_train, y_train)
print(f"\nsklearn SVM test accuracy: {clf.score(X_test, y_test):.2%}")
print(f"Number of support vectors: {clf.n_support_}")
```

---

## 🎯 Recognize This Problem When...

- Binary or multi-class **classification** problem.
- Data is **high-dimensional** (text/images) — SVM handles this well.
- The **margin** concept is useful (clear separation between classes).
- Small-to-medium dataset where training time is acceptable.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| High-dimensional data (text, bioinformatics) | ✅ Excellent — many features, fewer samples |
| Clear margin between classes | ✅ Maximum-margin guarantee |
| Small dataset (n < 10,000) | ✅ Efficient with RBF kernel |
| Large dataset (n > 100,000) | ❌ Training is O(n²-n³); use LinearSVC or SGD |
| Multi-label classification | ❌ Native SVM is binary; need one-vs-rest |
| Need probability estimates | ❌ SVM gives distances, not probabilities (use calibration) |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Gradient Descent](GradientDescent.md) | SGD-trained SVM uses gradient of hinge loss |
| [Decision Trees](DecisionTrees.md) | Entirely different decision boundary approach |
| [Random Forest](RandomForest.md) | Ensemble tree alternative — often comparable accuracy |

---

## 📝 Practice / Resources

| Resource | Platform |
|----------|----------|
| [SVM tutorial](https://scikit-learn.org/stable/modules/svm.html) | scikit-learn docs |
| [Digit Recognition](https://www.kaggle.com/c/digit-recognizer) | Kaggle |
| [Text Classification with SVM](https://scikit-learn.org/stable/tutorial/text_analytics/) | scikit-learn tutorial |
