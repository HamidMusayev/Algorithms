# Support Vector Machine (SVM)

**Fikir:** İki sinif nöqtə verildikdə SVM aralarında **mümkün ən geniş boşluğu (margin)** tapır. Qərar sərhədi bu margini maksimumlaşdıran hipermüstəvidir. Sərhədə ən yaxın nöqtələr **dayaq vektorları (support vectors)** adlanır — sərhədi yalnız onlar müəyyən edir. Xətti ayrılmayan data üçün **kernel hiyləsi** nöqtələri xətti ayırıcının mövcud olduğu daha yüksək ölçülü fəzaya keçirir (koordinatları açıq hesablamadan).

**Mental model:** İki sinif arasında ən geniş "küçəni" çək. Yalnız küçə kənarlarına toxunan nöqtələr (dayaq vektorları) küçəni müəyyən edir.

## Necə işləyir
**Xətti SVM (ayrıla bilən):** `w·x + b = 0` hipermüstəvisini tap ki, bütün nöqtələri düzgün təsnif etsin (`yᵢ(w·xᵢ + b) ≥ 1`) və margini (`2/||w||`) maksimumlaşdırsın. Bu, kvadratik proqramlaşdırma məsələsidir.

**Yumşaq margin (ayrılmayan):** bəzi səhvlərə **boşluq dəyişənləri** ξᵢ ilə icazə ver; `||w||² + C·Σξᵢ` minimumlaşdır. Böyük C → kiçik margin, az səhv. Kiçik C → böyük margin, çox səhv.

**Kernel hiyləsi:** `x·z`-i `K(x, z)` ilə əvəz et — yüksək ölçülü fəzada skalyar hasili gizli hesablayır. Tez işlənən kernellər: RBF (Gauss), polinomial, sigmoid.

## Nümunə
Sinif +1: `(2,2),(2,3)`, sinif −1: `(-2,-2),(-2,-3)`
Optimal: `x₁ + x₂ = 0`. Dayaq vektorları: `(2,2)` və `(-2,-2)`. Margin = `2/√2 ≈ 1.41`.

## Kod
```python
import numpy as np

class LinearSVM:
    """Hinge loss üzərində SGD ilə sadə xətti SVM. İstehsalda sklearn.svm.SVC."""
    def __init__(self, C=1.0, lr=0.001, epochs=1000):
        self.C, self.lr, self.epochs = C, lr, epochs
        self.w, self.b = None, 0

    def fit(self, X, y):                          # y ∈ {-1, +1}
        n, d = X.shape
        self.w = np.zeros(d)
        for _ in range(self.epochs):
            for i in np.random.permutation(n):
                if y[i] * (X[i] @ self.w + self.b) < 1:   # margin daxilində/səhv
                    self.w -= self.lr * (self.w - self.C * y[i] * X[i])
                    self.b -= self.lr * (-self.C * y[i])
                else:                                      # düzgün → yalnız nizamlama
                    self.w -= self.lr * self.w

    def predict(self, X):
        return np.sign(X @ self.w + self.b)

# Kernel dəstəyi ilə istehsal üçün:
from sklearn.svm import SVC
clf = SVC(kernel='rbf', C=1.0, gamma='scale')   # SVM üçün xüsusiyyət ölçüləməsi VACİBDİR
```

## Mürəkkəblik
| Əməliyyat | Vaxt |
|-----------|------|
| Öyrətmə | O(n²–n³) (solver-dən asılı) |
| Proqnoz | O(n_sv × d) |
| Yaddaş | O(n_sv) |

## Nə vaxt
- ✅ İkili/çoxsinifli təsnifat; **çox ölçülü** data (mətn, bioinformatika).
- ✅ Siniflər arası aydın margin; kiçik-orta data (n < 10 000) RBF kernel ilə.
- ❌ Böyük data (n > 100 000) — O(n²–n³); LinearSVC/SGD.
- ❌ Ehtimal qiymətləri lazımdır — SVM məsafə verir, ehtimal yox (kalibrləmə lazımdır).

## Əlaqəli
- [Gradient Descent](GradientDescent.md) — SGD ilə öyrədilən SVM hinge loss qradiyentini istifadə edir.
- [Decision Trees](DecisionTrees.md) · [Random Forest](RandomForest.md) — tamamilə fərqli sərhəd yanaşmaları.
