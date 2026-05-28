### **Support Vector Machine (SVM) 🤖**

A supervised classifier that finds the **maximum-margin hyperplane** separating two classes. Points closest to the boundary are **support vectors** — they alone define the decision boundary.

For non-linearly separable data, SVM uses the **kernel trick** to map inputs into a higher-dimensional space without computing it explicitly.

---

#### **Common Kernels**
| Kernel | Form |
|--------|------|
| **Linear** | `k(x, y) = x·y` |
| **Polynomial** | `(γ x·y + r)^d` |
| **RBF (Gaussian)** | `exp(-γ‖x − y‖²)` |
| **Sigmoid** | `tanh(γ x·y + r)` |

---

#### **Summary**
- **Train time:** between O(n²) and O(n³) depending on solver
- **Strengths:** effective in high dimensions, robust to overfitting with proper `C`
- **Weaknesses:** doesn't scale well to huge `n`; choice of kernel + hyperparams critical

---

#### **Python Implementation (scikit-learn)**
```python
from sklearn.svm import SVC
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler

X, y = make_classification(n_samples=300, n_features=4, random_state=0)
X = StandardScaler().fit_transform(X)
X_tr, X_te, y_tr, y_te = train_test_split(X, y, random_state=0)

svm = SVC(kernel="rbf", C=1.0, gamma="scale")
svm.fit(X_tr, y_tr)
print("Accuracy:", svm.score(X_te, y_te))
```

---

#### **Key Hyperparameters**
- `C` — regularization. Small → wider margin, more misclassification tolerance.
- `kernel` — `linear`, `rbf`, `poly`, ...
- `gamma` — kernel width for RBF/poly.

---

#### **When to Use**
✅ Small-to-medium datasets, high-dimensional data (text, bioinformatics)
✅ Clear margin between classes
🚫 Huge datasets → prefer linear models (logistic regression, linear SVM with SGD) or neural nets
