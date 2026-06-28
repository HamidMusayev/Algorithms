# Random Forest (Təsadüfi Meşə)

**Fikir:** Tək qərar ağacı bir ekspertə müraciət kimidir — əmin, amma səhv ola bilər. Random Forest 100 eksperti soruşur, hər biri bir az fərqli data nümunəsində və fərqli xüsusiyyətlərlə öyrədilib. Kollektiv səs çox daha etibarlıdır.

İki açar fikir:
1. **Bagging:** hər ağac datanın fərqli təsadüfi alt-çoxluğunu (bootstrap nümunə) görür.
2. **Xüsusiyyət təsadüfiliyi:** hər bölünmədə yalnız təsadüfi alt-çoxluq (məs. √d xüsusiyyət) nəzərə alınır.

Ağaclar arası təsadüfi fərqlər **fərqli səhvlər** etmələrini təmin edir, səhvlər toplamda bir-birini ləğv edir.

## Necə işləyir
1. Hər T ağac üçün:
   - **Bootstrap nümunə:** öyrətmə datasından n sətir **təkrarla** təsadüfi seç.
   - **Ağac qur:** hər bölünmədə yalnız təsadüfi `m ≈ √d` xüsusiyyət nəzərə al.
2. **Proqnoz:** təsnifat → **çoxluq səsi**; reqressiya → **orta**.
3. **Xüsusiyyət əhəmiyyəti:** hər xüsusiyyət bütün ağaclarda qarışıqlığı nə qədər azaldır?

## Nümunə
3 ağac yeni `x` nöqtəsi üçün: Ağac1→0, Ağac2→1, Ağac3→1 → çoxluq səsi: **sinif 1** ✅

## Kod
```python
import numpy as np
from collections import Counter

class RandomForest:
    def __init__(self, n_trees=100, max_depth=5, max_features=None):
        self.n_trees = n_trees
        self.max_depth = max_depth
        self.max_features = max_features
        self.trees = []

    def fit(self, X, y):
        n = len(X)
        self.trees = []
        for _ in range(self.n_trees):
            idx = np.random.choice(n, n, replace=True)   # bootstrap
            tree = SimpleDecisionTree(self.max_depth, self.max_features)
            tree.fit(X[idx], y[idx])
            self.trees.append(tree)

    def predict(self, X):
        preds = np.array([t.predict(X) for t in self.trees])
        return np.apply_along_axis(
            lambda col: Counter(col).most_common(1)[0][0], 0, preds)   # çoxluq səsi

# SimpleDecisionTree hər bölünmədə √d təsadüfi xüsusiyyət seçir (Random Forest-in açarı).
np.random.seed(42)
X = np.random.randn(100, 4)
y = (X[:, 0] + X[:, 1] > 0).astype(int)
rf = RandomForest(n_trees=50, max_depth=4)
rf.fit(X, y)
print(np.mean(rf.predict(X) == y))   # yüksək olmalıdır
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Öyrətmə | O(T × n × d × log n) |
| Proqnoz | O(T × dərinlik) |
| Yaddaş | O(T × ağac başına node) |

T = ağac sayı (adətən 100–500).

## Nə vaxt
- ✅ Cədvəl datası üçün güclü baza (təsnifat və ya reqressiya); xüsusiyyət əhəmiyyəti lazımdır.
- ✅ Orta ölçülü data; tək ağac overfit edir (Random Forest onu nizamlayır).
- ❌ Çox ölçülü seyrək data (NLP) — xətti modellər/neyron şəbəkələri.
- ❌ Aşağı gecikməli real-vaxt proqnoz — T ağac × dərinlik yavaş ola bilər.

## Əlaqəli
- [Decision Trees](DecisionTrees.md) — baza öyrənici; meşədəki hər ağac.
- [Gradient Descent](GradientDescent.md) — Gradient Boosted Trees ansamblları fərqli öyrədir.
