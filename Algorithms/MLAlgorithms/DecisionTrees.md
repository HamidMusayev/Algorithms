# Decision Trees (Qərar Ağacları)

**Fikir:** Qərar ağacı xüsusiyyətlər haqqında bəli/xeyr suallar verərək proqnoz qurur. Axın diaqramı kimi: "Temperatur > 30°C? → Bəli → Yağış var? → Yox → Çimərliyə!" Hər daxili node bir xüsusiyyət soruşur, hər yarpaq proqnoz verir. Ağac hər addımda datanı **ən yaxşı ayıran** sualı taparaq qurulur.

## Necə işləyir
1. **Ən yaxşı bölünməni seç:** hər xüsusiyyət üçün sinifləri ən yaxşı ayıran həddi tap.
   - **Təsnifat:** Gini qarışıqlığı və ya informasiya qazancı (entropiya).
   - **Reqressiya:** MSE azalması.
2. Seçilmiş xüsusiyyət və hədd üzrə datanı **böl**.
3. Hər alt-qrupda **rekursiya** et: sinif təmiz olana, maksimum dərinliyə çatana və ya nümunə azalana qədər.
4. **Yarpaq:** çoxluq sinfini (təsnifat) və ya ortanı (reqressiya) proqnozlaşdır.

> **Gini qarışıqlığı:** `G = 1 − Σ pᵢ²` (aşağı = təmiz). **İnformasiya qazancı:** `IG = H(valideyn) − Σ (|uşaq|/|valideyn|)·H(uşaq)`.

## Nümunə
Xüsusiyyət `[1,2,3,4]`, etiketlər `[0,0,1,1]`; x=2.5-də böl:
- Sol `[1,2]` → `[0,0]` (təmiz), sağ `[3,4]` → `[1,1]` (təmiz) → mükəmməl bölünmə!
```
x ≤ 2.5? → BƏLİ: 0, YOX: 1
```

## Kod
```python
import numpy as np

def gini(y):
    if len(y) == 0:
        return 0
    _, counts = np.unique(y, return_counts=True)
    probs = counts / len(y)
    return 1 - np.sum(probs ** 2)

def best_split(X, y):
    n, d = X.shape
    best = (-1, None, None)                       # (qazanc, xüsusiyyət, hədd)
    parent = gini(y)
    for feature in range(d):
        for thresh in np.unique(X[:, feature]):
            left = X[:, feature] <= thresh
            if not left.any() or left.all():
                continue
            wg = (left.sum() * gini(y[left]) + (~left).sum() * gini(y[~left])) / n
            if parent - wg > best[0]:
                best = (parent - wg, feature, thresh)
    return best[1], best[2]

def build_tree(X, y, depth=0, max_depth=5):
    if depth >= max_depth or gini(y) == 0 or len(y) < 2:
        return {"leaf": True, "pred": np.bincount(y).argmax()}
    feat, thresh = best_split(X, y)
    if feat is None:
        return {"leaf": True, "pred": np.bincount(y).argmax()}
    left = X[:, feat] <= thresh
    return {"leaf": False, "feat": feat, "thresh": thresh,
            "left": build_tree(X[left], y[left], depth+1, max_depth),
            "right": build_tree(X[~left], y[~left], depth+1, max_depth)}

X = np.array([[1,2],[2,3],[3,1],[4,4],[5,2],[6,5]])
y = np.array([0,0,0,1,1,1])
tree = build_tree(X, y)
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Öyrətmə | O(n × d × log n) |
| Proqnoz | O(dərinlik) |
| Yaddaş | O(node sayı) |

## Nə vaxt
- ✅ **İzah edilə bilən** model lazımdır — hər qərarı açıqlaya bilirsən.
- ✅ Cədvəl datası (qarışıq ədədi/kateqorial); sürətli baza model.
- ❌ Səs-küylü data (overfitting riski) — budama və ya Random Forest.
- ❌ Mürəkkəb əlaqələr (şəkil, mətn) — neyron şəbəkələri.

## Əlaqəli
- [Random Forest](RandomForest.md) — çoxlu qərar ağacının ansamblı, çox daha sağlam.
- [Gradient Descent](GradientDescent.md) — qərar ağacları qradiyent enişi istifadə etmir.
