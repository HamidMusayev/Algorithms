# Gradient Descent (Qradiyent Enişi)

**Fikir:** Gözübağlı təpəli ərazidə ən aşağı vadiyə getmək istəyirsən. Ayağının altında yamacı hiss edirsən. Strategiya: ən sərt aşağı enən istiqamətdə addım at, təkrarla. Gradient Descent itki funksiyası üçün məhz bunu edir: qradiyenti (yamac) hesabla, onun əksinə (aşağıya) addım at, yığışana qədər təkrarla.

## Necə işləyir
**Yeniləmə qaydası:** `θ ← θ − η · ∇L(θ)`
- `θ` = model parametrləri, `η` = öyrənmə sürəti (addım), `∇L(θ)` = itkinin θ-ya görə qradiyenti.

1. Parametrləri başlat.
2. Hər iterasiyada: proqnoz `ŷ = f(X, θ)` → itki `L` → qradiyent `∇L` → yenilə `θ ← θ − η·∇L`.
3. İtki dəyişməsi həddən kiçik olanda dayan.

**Variantlar:** Batch (bütün data), SGD (1 təsadüfi nümunə), Mini-batch (b ölçülü dəstə — müasir standart).

## Nümunə
`y = θx`, nöqtələr `[(1,2),(2,4),(3,6)]`, əsl θ=2, η=0.1
| θ | İtki | Qradiyent |
|---|------|-----------|
| 0.0 | 18.67 | -18.67 |
| 1.87 | 0.24 | -1.73 |
| 2.04 | 0.004 | → 0 |

## Kod
```python
import numpy as np

def gradient_descent(X, y, lr=0.01, epochs=1000):
    """Xətti reqressiya üçün Batch Gradient Descent."""
    n, d = X.shape
    theta = np.zeros(d)
    for _ in range(epochs):
        predictions = X @ theta
        errors = predictions - y
        gradient = (2 / n) * (X.T @ errors)       # ∂L/∂θ
        theta -= lr * gradient                     # qradiyentin əksinə addım
    return theta

# y = 3x + 2 uyğunlaşdır
np.random.seed(42)
X = np.column_stack([np.ones(50), np.linspace(0, 5, 50)])   # bias + xüsusiyyət
y = 3 * X[:, 1] + 2 + np.random.normal(0, 0.1, 50)
theta = gradient_descent(X, y, lr=0.01, epochs=5000)
print(f"bias={theta[0]:.2f}, slope={theta[1]:.2f}")   # ≈2.00, ≈3.00
```

## Mürəkkəblik
| Variant | Hər yeniləmə | İşlənən data |
|---------|--------------|--------------|
| Batch GD | O(n × d) | bütün n nümunə |
| SGD | O(d) | 1 təsadüfi nümunə |
| Mini-batch | O(b × d) | b ölçülü dəstə |

## Nə vaxt
- ✅ Parametrlər üzərində **diferensiallana bilən itkini minimumlaşdırmaq**; istənilən ML modelini öyrətmək.
- ✅ Qabarıq (convex) itki — qlobal minimuma zəmanətli yığılma; böyük data üçün SGD/mini-batch.
- ❌ Diferensiallana bilməyən itki — subqradiyent, evolyusion strategiyalar.
- ❌ Qapalı formalı çox kiçik data — Ən Kiçik Kvadratlar dəqiqdir.

## Əlaqəli
- [Backpropagation](Backpropagation.md) — neyron şəbəkəsi üçün qradiyenti hesablayır; GD onunla yeniləyir.
- [KMeans Clustering](KMeansClustering.md) — oxşar iterativ təkmilləşdirmə (təyin + sentroid yeniləmə).
