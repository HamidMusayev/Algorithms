# K-Means Clustering (K-Orta Klasterləşmə)

**Fikir:** 100 müştəri profilini 3 seqmentə bölmək istəyirsən. K-Means iterativ işləyir: əvvəlcə 3 klaster mərkəzini təsadüfi qoy, sonra hər müştərini ən yaxın mərkəzə təyin et, sonra hər mərkəzi öz müştərilərinin ortasına köçür. Sabit olana qədər təkrarla.

**Mental model:** Hər nöqtəni ən yaxın sentroidə təyin et → hər sentroidi öz nöqtələrinin ortasına köçür → sentroidlər dayanana qədər təkrarla.

## Necə işləyir
1. **k sentroid başlat** (təsadüfi və ya k-means++).
2. **Təyin addımı:** hər nöqtəni ən yaxın sentroidə təyin et (Evklid məsafəsi).
3. **Yeniləmə addımı:** hər sentroidi ona təyin olunmuş nöqtələrin ortasına köçür.
4. Sentroidlər dəyişməyənə (və ya dəyişmə < ε) qədər 2–3-ü təkrarla.

> **k-means++** başlatma: ilk sentroidi təsadüfi seç, sonrakıları mövcud sentroidlərdən uzaqlığa mütənasib ehtimalla seç — daha yaxşı başlanğıc → daha az iterasiya.

## Nümunə
6 nöqtə: `(1,1),(1,2),(2,1),(8,8),(9,8),(8,9)`, k=2
- C1=(1.33, 1.33), C2=(8.33, 8.33)
- Klasterlər: {(1,1),(1,2),(2,1)} və {(8,8),(9,8),(8,9)} ✅

## Kod
```python
import numpy as np

def k_means(X, k, max_iter=100, seed=42):
    np.random.seed(seed)
    n = len(X)
    centroids = X[np.random.choice(n, k, replace=False)].copy()
    labels = np.zeros(n, dtype=int)
    for _ in range(max_iter):
        # Təyin: hər nöqtəni ən yaxın sentroidə
        dist = np.sqrt(((X[:, None, :] - centroids[None, :, :]) ** 2).sum(axis=2))
        new_labels = dist.argmin(axis=1)
        if np.all(new_labels == labels):     # yığıldı
            break
        labels = new_labels
        # Yeniləmə: sentroidləri orta nöqtəyə köçür
        centroids = np.array([
            X[labels == j].mean(axis=0) if np.any(labels == j) else centroids[j]
            for j in range(k)
        ])
    return labels, centroids

X = np.array([[1,1],[1,2],[2,1],[8,8],[9,8],[8,9]])
labels, centroids = k_means(X, k=2)
print(labels)      # [0 0 0 1 1 1] (və ya tərsi)
print(centroids)   # [[1.33,1.33],[8.33,8.33]]
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Hər iterasiya | O(n × k × d) |
| Ümumi | O(I × n × k × d) |
| Yaddaş | O(n + k) |

Adətən 10–100 iterasiyada yığılır.

## Nə vaxt
- ✅ Etiketsiz datanı k qrupa bölmək (müştəri seqmentasiyası, şəkil rəng kvantlaşdırması).
- ✅ Sferik, sıx klasterlər; data ədədi və məsafə əsaslı qruplaşma mənalıdır.
- ✅ k-nı seçmək üçün dirsək (elbow) metodu və ya silhouette balı.
- ❌ Qeyri-sferik klasterlər (aypara, halqa) — DBSCAN və ya GMM.
- ❌ k naməlumdur və ya klasterlər çox fərqli ölçülü/sıxlıqlı.

## Əlaqəli
- [Gradient Descent](GradientDescent.md) — K-means koordinat enişi optimizasiyasıdır.
- [Decision Trees](DecisionTrees.md) — hər ikisi xüsusiyyət fəzasını bölür; fərqli yanaşma.
