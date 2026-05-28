### **Gradient Descent 🤖**

An **iterative optimization** algorithm for minimizing a differentiable function `f(θ)`. Repeatedly steps in the direction **opposite** to the gradient:
```
θ ← θ − η · ∇f(θ)
```
where `η` is the **learning rate**.

---

#### **Variants**
| Variant | Update | Notes |
|---------|--------|-------|
| **Batch GD** | uses all data each step | stable, slow on big data |
| **Stochastic GD (SGD)** | uses 1 sample | noisy but fast |
| **Mini-batch GD** | uses a small batch | best of both — most used in practice |

---

#### **Python Implementation (Linear Regression by GD)**
```python
import numpy as np

def gradient_descent(X, y, lr=0.01, epochs=1000):
    n, d = X.shape
    theta = np.zeros(d)
    for _ in range(epochs):
        preds = X @ theta
        grad = (2 / n) * X.T @ (preds - y)
        theta -= lr * grad
    return theta

# Example: fit y = 3x + 2
X = np.array([[1, x] for x in range(10)])   # bias term
y = np.array([3 * x + 2 for x in range(10)])

theta = gradient_descent(X, y, lr=0.02, epochs=2000)
print(theta)   # ~ [2.0, 3.0]
```

---

#### **Tips**
- Too **large** learning rate → diverges. Too **small** → painfully slow.
- Use **momentum / Adam** in practice; plain SGD is rarely best.
- **Normalize features** to keep gradients well-scaled.

---

#### **When to Use**
✅ Training virtually every modern ML model (linear, logistic, NNs)
✅ Convex problems → reaches global minimum
🚫 Non-differentiable losses → try coordinate descent, subgradient, EM
