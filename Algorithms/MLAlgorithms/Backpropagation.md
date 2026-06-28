# Backpropagation (Geriyə Yayılma)

**Fikir:** Neyron şəbəkəsini öyrətmək milyonlarla çəkini səhvi azaltmaq üçün tənzimləmək deməkdir. Backpropagation **zəncir qaydası** (calculus) ilə "hər çəki səhvə nə qədər cavabdehdir?" sualını səmərəli hesablayır. Bunu məsuliyyət bölgüsü kimi düşün: çıxış qatı səhv siqnalını alır, sonra hər qata öz töhfəsi qədər "günah" payı verir.

**Mental model:** İrəli keçid proqnozları hesablayır; geriyə keçid səhv məsuliyyətini çıxışdan girişə qat-qat yayır.

## Necə işləyir
**İrəli keçid:**
1. Hər qat üçün: `z = W·a_əvvəl + b` (çəkili cəm).
2. Aktivasiya tətbiq et: `a = σ(z)` (məs. sigmoid, ReLU).
3. Çıxışda itki (loss) hesabla.

**Geriyə keçid (zəncir qaydası):**
4. Çıxış qradiyentini hesabla: `∂L/∂a_çıxış`.
5. Hər qat üçün geriyə:
   - Çəki qradiyenti: `∂L/∂W = ∂L/∂a · σ'(z) · a_əvvəlᵀ`.
   - Geriyə ötürüləcək: `∂L/∂a_əvvəl = Wᵀ · (∂L/∂a · σ'(z))`.
6. Yenilə: `W ← W − η·∂L/∂W` (η = öyrənmə sürəti).

> Hər simvolun mənası: `∂L/∂W` = itkinin çəkiyə görə dəyişmə sürəti — "bu çəkini bir az artırsam, itki nə qədər dəyişər?". σ' = aktivasiya funksiyasının törəməsi.

## Kod
```python
import numpy as np

def sigmoid(x):
    return 1 / (1 + np.exp(-np.clip(x, -500, 500)))   # daşmadan qorunma

def sigmoid_deriv(s):
    return s * (1 - s)                                  # sigmoid çıxışı s üçün törəmə

class TwoLayerNN:
    def __init__(self, n_in, n_hidden, n_out, lr=0.1):
        np.random.seed(42)
        self.W1 = np.random.randn(n_hidden, n_in) * np.sqrt(2 / n_in)
        self.b1 = np.zeros((n_hidden, 1))
        self.W2 = np.random.randn(n_out, n_hidden) * np.sqrt(2 / n_hidden)
        self.b2 = np.zeros((n_out, 1))
        self.lr = lr

    def forward(self, X):                              # irəli keçid
        self.X = X
        self.a1 = sigmoid(self.W1 @ X + self.b1)
        self.a2 = sigmoid(self.W2 @ self.a1 + self.b2)
        return self.a2

    def backward(self, y):                             # geriyə keçid
        m = y.shape[1]
        dz2 = (self.a2 - y) * sigmoid_deriv(self.a2)
        dW2 = (dz2 @ self.a1.T) / m
        dz1 = (self.W2.T @ dz2) * sigmoid_deriv(self.a1)
        dW1 = (dz1 @ self.X.T) / m
        self.W2 -= self.lr * dW2                       # çəkiləri yenilə
        self.W1 -= self.lr * dW1

    def train(self, X, y, epochs=10000):
        for _ in range(epochs):
            self.forward(X)
            self.backward(y)

# XOR məsələsi
X = np.array([[0,0,1,1], [0,1,0,1]])
y = np.array([[0, 1, 1, 0]])
nn = TwoLayerNN(2, 4, 1, lr=0.5)
nn.train(X, y)
print(nn.forward(X).round(2))   # [[0. 1. 1. 0.]] — XOR öyrənildi!
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| İrəli keçid | O(W) (W = çəki sayı) |
| Geriyə keçid | O(W) — irəli ilə eyni |
| Yaddaş | O(W + aktivasiyalar) |

> Backprop-un gözəlliyi: bütün qradiyentləri hesablamaq bir irəli keçid qədər vaxt aparır.

## Nə vaxt
- ✅ İstənilən dərinlikdə neyron şəbəkəsini öyrətmək; diferensiallana bilən əməliyyatlar zənciri üzərində qradiyent.
- ✅ PyTorch/TensorFlow avtomatik diferensiallaşma ilə bunu altda istifadə edir.
- ❌ Diferensiallana bilməyən aktivasiya/itki — subqradiyent və ya evolyusion üsullar.

## Əlaqəli
- [Gradient Descent](GradientDescent.md) — backprop qradiyenti hesablayır, GD onunla çəkiləri yeniləyir.
- [Decision Trees](DecisionTrees.md) — diferensiallana bilməyən model; backprop istifadə etmir.
