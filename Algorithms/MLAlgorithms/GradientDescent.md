# Gradient Descent 📉

---

## 🧠 Intuition

Imagine you're blindfolded on a hilly landscape, trying to reach the lowest valley. You can feel the slope under your feet. Strategy: take a step in the direction where the ground slopes downward the most. Repeat until you can't go lower.

Gradient Descent does exactly this for a loss function: compute the gradient (slope), step opposite to it (downhill), and repeat until convergence.

**Mental model:** "Always step downhill by a small amount (learning rate × gradient). Repeat until the slope is flat."

---

## 📊 Complexity

| Variant | Time per Update | Data Used |
|---------|----------------|-----------|
| Batch GD | O(n × d) | All n samples |
| Stochastic GD (SGD) | O(d) | 1 random sample |
| Mini-batch GD | O(b × d) | Batch of size b |

> n = samples, d = features/parameters, b = batch size. Mini-batch is the practical standard.

---

## ⚙️ How It Works

**Update rule:**
```
θ ← θ - η × ∇L(θ)
```
- `θ` = model parameters
- `η` = learning rate (step size)
- `∇L(θ)` = gradient of loss with respect to θ

**Algorithm:**
1. Initialize parameters θ (randomly or zeros).
2. For each iteration:
   - Compute predictions: `ŷ = f(X, θ)`
   - Compute loss: `L = loss(y, ŷ)`
   - Compute gradient: `∇L = ∂L/∂θ`
   - Update: `θ ← θ - η × ∇L`
3. Stop when loss change < threshold or after max iterations.

---

## 🔢 Step-by-Step Trace

**Linear Regression:** fit `y = θx` to points `[(1,2), (2,4), (3,6)]`. True `θ=2`.

| Iter | θ    | Loss (MSE) | Gradient | Update |
|------|------|-----------|----------|--------|
| 0    | 0.0  | 18.67     | -18.67   | +1.87 (η=0.1) |
| 1    | 1.87 | 0.24      | -1.73    | +0.17 |
| 2    | 2.04 | 0.004     | 0.24     | -0.024|
| ...  | →2.0 | →0        | →0       | convergence |

---

## 🐍 Python Implementation

```python
import numpy as np

def gradient_descent_linear(X, y, lr=0.01, epochs=1000):
    """
    Batch Gradient Descent for linear regression.
    X: (n, d) feature matrix
    y: (n,) targets
    lr: learning rate
    Returns: (theta, loss_history)
    """
    n, d = X.shape
    theta = np.zeros(d)     # Initialize parameters to zero
    losses = []

    for epoch in range(epochs):
        # Forward pass: compute predictions
        predictions = X @ theta                      # (n,)

        # Compute MSE loss
        errors = predictions - y                     # (n,)
        loss = np.mean(errors ** 2)
        losses.append(loss)

        # Compute gradient: dL/dtheta = (2/n) * X^T * errors
        gradient = (2 / n) * (X.T @ errors)         # (d,)

        # Update parameters (step in opposite direction of gradient)
        theta -= lr * gradient

    return theta, losses


def sgd(X, y, lr=0.01, epochs=100):
    """
    Stochastic Gradient Descent: one random sample per update.
    Noisier but faster per epoch and helps escape local minima.
    """
    n, d = X.shape
    theta = np.zeros(d)

    for epoch in range(epochs):
        # Shuffle data each epoch
        indices = np.random.permutation(n)
        for i in indices:
            xi = X[i:i+1]          # Single sample (1, d)
            yi = y[i:i+1]          # Single target (1,)
            pred = xi @ theta
            gradient = 2 * xi.T @ (pred - yi)
            theta -= lr * gradient

    return theta


def mini_batch_gd(X, y, batch_size=32, lr=0.01, epochs=100):
    """
    Mini-batch Gradient Descent: best of batch and stochastic.
    Used in virtually all modern deep learning.
    """
    n, d = X.shape
    theta = np.zeros(d)

    for epoch in range(epochs):
        indices = np.random.permutation(n)
        for start in range(0, n, batch_size):
            batch = indices[start:start + batch_size]
            Xb, yb = X[batch], y[batch]
            preds = Xb @ theta
            gradient = (2 / len(batch)) * (Xb.T @ (preds - yb))
            theta -= lr * gradient

    return theta


# Example: Fit y = 3x + 2
np.random.seed(42)
X = np.column_stack([np.ones(50), np.linspace(0, 5, 50)])   # Bias + feature
y = 3 * X[:, 1] + 2 + np.random.normal(0, 0.1, 50)         # y = 3x + 2 + noise

theta, losses = gradient_descent_linear(X, y, lr=0.01, epochs=5000)
print(f"Fitted: bias={theta[0]:.2f}, slope={theta[1]:.2f}")
# bias≈2.00, slope≈3.00
print(f"Final loss: {losses[-1]:.4f}")
```

---

## 🎯 Recognize This Problem When...

- You need to **minimize a differentiable loss function** over parameters.
- Training any machine learning model (linear regression, logistic regression, neural networks).
- The parameter space is too large to solve analytically (closed form).
- Keywords: "optimize", "train model", "minimize loss", "update weights".

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Training neural networks | ✅ Essential — no alternative |
| Convex loss functions | ✅ Guaranteed convergence to global min |
| Large datasets | ✅ Use SGD/mini-batch for efficiency |
| Non-differentiable loss | ❌ Use subgradient, evolution strategies |
| Very small dataset with closed form | ❌ Ordinary Least Squares is exact |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Backpropagation](Backpropagation.md) | Computes the gradient for neural networks; GD uses it to update |
| [KMeans Clustering](KMeansClustering.md) | K-means alternates between assignment and centroid update — similar iterative refinement |

---

## 📝 Practice / Resources

| Resource | Platform |
|----------|----------|
| [Machine Learning Course](https://www.coursera.org/learn/machine-learning) | Coursera (Andrew Ng) |
| Optimize a quadratic function | Code yourself |
| [Gradient Descent visualization](https://ml-cheatsheet.readthedocs.io/en/latest/gradient_descent.html) | ML Cheatsheet |
