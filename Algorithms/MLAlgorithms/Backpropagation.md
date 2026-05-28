### **Backpropagation 🤖**

The standard algorithm for training **neural networks**. It applies the **chain rule** to compute gradients of the loss with respect to every weight, going **backward** layer by layer.

Conceptually two passes:
1. **Forward pass:** compute the output of each layer given the input.
2. **Backward pass:** propagate the loss gradient from the output back to the inputs, updating weights with [gradient descent](GradientDescent.md).

---

#### **Summary**
- **Time per step:** O(W) — proportional to # weights
- **Combined with:** SGD / Adam optimizer
- **Key requirement:** every operation must be differentiable

---

#### **Python Implementation (1-Hidden-Layer NN from Scratch)**
```python
import numpy as np

def sigmoid(x):  return 1 / (1 + np.exp(-x))
def dsigmoid(y): return y * (1 - y)

def train_mlp(X, y, hidden=4, lr=0.1, epochs=10000):
    np.random.seed(0)
    n_in = X.shape[1]
    W1 = np.random.randn(n_in, hidden)
    b1 = np.zeros(hidden)
    W2 = np.random.randn(hidden, 1)
    b2 = np.zeros(1)

    for _ in range(epochs):
        # forward
        h = sigmoid(X @ W1 + b1)
        o = sigmoid(h @ W2 + b2)

        # backward (MSE loss)
        d_o = (o - y) * dsigmoid(o)
        d_h = (d_o @ W2.T) * dsigmoid(h)

        # update
        W2 -= lr * h.T @ d_o
        b2 -= lr * d_o.sum(axis=0)
        W1 -= lr * X.T @ d_h
        b1 -= lr * d_h.sum(axis=0)

    return W1, b1, W2, b2

# Example: learn XOR
X = np.array([[0,0], [0,1], [1,0], [1,1]])
y = np.array([[0], [1], [1], [0]])
W1, b1, W2, b2 = train_mlp(X, y)

h = sigmoid(X @ W1 + b1)
print(sigmoid(h @ W2 + b2).round(2))
# ~ [[0.] [1.] [1.] [0.]]
```

---

#### **When to Use**
✅ Training **any** neural network (image, text, audio, …)
✅ Building block underlying PyTorch / TensorFlow autograd
🚫 In production, use frameworks — they handle autodiff for you
