# Backpropagation 🧠

---

## 🧠 Intuition

Training a neural network means adjusting millions of weights to reduce prediction error. Backpropagation efficiently computes "how much is each weight responsible for the error?" using the **chain rule** from calculus.

Think of it like blame attribution: the output layer gets the error signal first. It distributes blame to the hidden layers proportional to how much each weight contributed. Each layer computes its own gradient and passes a blame signal backward.

**Mental model:** "Forward pass computes predictions. Backward pass propagates error responsibility from output to input, layer by layer, using the chain rule."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time per forward pass | O(W) — W = number of weights |
| Time per backward pass | O(W) — same as forward pass |
| Space | O(W + activations) |

> The beauty of backprop: computing all gradients costs the same as one forward pass.

---

## ⚙️ How It Works

**Forward pass:**
1. For each layer, compute: `z = W × a_prev + b` (weighted sum)
2. Apply activation: `a = σ(z)` (e.g., sigmoid, ReLU)
3. Compute loss at output.

**Backward pass (chain rule):**
4. Compute output gradient: `∂L/∂a_output`
5. For each layer going backward:
   - Gradient of weights: `∂L/∂W = ∂L/∂a × σ'(z) × a_prev^T`
   - Gradient to pass back: `∂L/∂a_prev = W^T × (∂L/∂a × σ'(z))`
6. Update: `W ← W - η × ∂L/∂W`

---

## 🔢 Step-by-Step Trace

Simple 1-hidden-layer network, XOR problem:

**Input:** `[0, 1]`, **Expected output:** `1`

**Forward pass:**
- Layer 1: `z1 = W1 × [0,1] + b1`, `a1 = sigmoid(z1)`
- Layer 2: `z2 = W2 × a1 + b2`, `output = sigmoid(z2)`

**Backward pass:**
- Error: `output - 1 = -0.3` (example)
- `∂L/∂W2 = error × sigmoid'(z2) × a1^T`
- `∂L/∂W1 = (W2^T × (error × sigmoid'(z2))) × sigmoid'(z1) × input^T`

Update weights in both layers. Repeat for all training examples.

---

## 🐍 Python Implementation

```python
import numpy as np

def sigmoid(x):
    return 1 / (1 + np.exp(-np.clip(x, -500, 500)))   # Clipped to avoid overflow

def sigmoid_deriv(s):
    """Derivative of sigmoid given sigmoid output s: s*(1-s)."""
    return s * (1 - s)


class TwoLayerNN:
    """Simple 2-layer neural network trained with backpropagation."""

    def __init__(self, input_size, hidden_size, output_size, lr=0.1):
        np.random.seed(42)
        # Initialize weights randomly (Xavier initialization)
        self.W1 = np.random.randn(hidden_size, input_size) * np.sqrt(2 / input_size)
        self.b1 = np.zeros((hidden_size, 1))
        self.W2 = np.random.randn(output_size, hidden_size) * np.sqrt(2 / hidden_size)
        self.b2 = np.zeros((output_size, 1))
        self.lr = lr

    def forward(self, X):
        """Forward pass: compute predictions."""
        self.X = X
        self.z1 = self.W1 @ X + self.b1      # Hidden layer pre-activation
        self.a1 = sigmoid(self.z1)             # Hidden layer activation
        self.z2 = self.W2 @ self.a1 + self.b2 # Output layer pre-activation
        self.a2 = sigmoid(self.z2)             # Output (prediction)
        return self.a2

    def backward(self, y):
        """Backward pass: compute gradients and update weights."""
        m = y.shape[1]   # Number of samples

        # Output layer gradient
        dL_da2 = self.a2 - y                         # MSE gradient: prediction - target
        dL_dz2 = dL_da2 * sigmoid_deriv(self.a2)     # Chain rule: multiply by σ'
        dL_dW2 = (dL_dz2 @ self.a1.T) / m            # Gradient of W2
        dL_db2 = np.mean(dL_dz2, axis=1, keepdims=True)

        # Hidden layer gradient (chain rule through W2)
        dL_da1 = self.W2.T @ dL_dz2                  # Propagate gradient back through W2
        dL_dz1 = dL_da1 * sigmoid_deriv(self.a1)     # Multiply by σ' of hidden layer
        dL_dW1 = (dL_dz1 @ self.X.T) / m             # Gradient of W1
        dL_db1 = np.mean(dL_dz1, axis=1, keepdims=True)

        # Update weights (gradient descent)
        self.W2 -= self.lr * dL_dW2
        self.b2 -= self.lr * dL_db2
        self.W1 -= self.lr * dL_dW1
        self.b1 -= self.lr * dL_db1

    def train(self, X, y, epochs=10000):
        losses = []
        for epoch in range(epochs):
            output = self.forward(X)
            loss = np.mean((output - y) ** 2)
            losses.append(loss)
            self.backward(y)
        return losses


# Train on XOR problem
X = np.array([[0, 0, 1, 1],
              [0, 1, 0, 1]])   # (2, 4): 4 samples, 2 features each
y = np.array([[0, 1, 1, 0]])   # XOR labels

nn = TwoLayerNN(input_size=2, hidden_size=4, output_size=1, lr=0.5)
losses = nn.train(X, y, epochs=10000)

print("Final loss:", losses[-1])    # Should be very small
print("Predictions:", nn.forward(X).round(2))
# [[0. 1. 1. 0.]] — learned XOR!
```

---

## 🎯 Recognize This Problem When...

- Training any **neural network** of any depth.
- You need to compute gradients through a sequence of differentiable operations.
- Keywords: "train the network", "update weights", "minimize loss", "neural network".
- Any framework (PyTorch, TensorFlow) uses backprop under the hood via **automatic differentiation**.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Training neural networks | ✅ Fundamental — no alternative |
| Differentiable computation graphs | ✅ Autograd (PyTorch/TF) generalizes this |
| Production use | ✅ Use PyTorch/TensorFlow for automatic differentiation |
| Non-differentiable activation/loss | ❌ Use subgradient or evolutionary methods |
| Very shallow models (logistic regression) | ❌ Direct gradient is simpler |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Gradient Descent](GradientDescent.md) | Backprop computes the gradient; GD uses it to update |
| [Decision Trees](DecisionTrees.md) | Non-differentiable model; doesn't use backprop |

---

## 📝 Practice / Resources

| Resource | Platform |
|----------|----------|
| Build a neural network from scratch | Code yourself |
| [Neural Networks and Deep Learning](http://neuralnetworksanddeeplearning.com/) | Free online book |
| [Deep Learning Specialization](https://www.deeplearning.ai/) | Coursera (Andrew Ng) |
