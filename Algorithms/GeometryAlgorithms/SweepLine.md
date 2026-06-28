# Sweep Line (Süpürən Xətt) Alqoritmi

**Fikir:** Müstəvini soldan-sağa süpürən şaquli xətt təsəvvür et. Hərəkət edərkən **hadisələr** (parçaların uçları, kəsişmələr) baş verir; hər hadisəni emal edib keçdiyi obyektlər haqqında sual cavablandırırsan. Sweep line **ümumi paradiqmadır**, tək alqoritm deyil. Klassik istifadələr: Bentley-Ottmann (kəsişmələr), Fortune (Voronoi), skyline məsələsi, düzbucaqlıların birləşmə sahəsi.

## Necə işləyir (ümumi nümunə)
1. **Hadisələri yarat** — nəyinsə dəyişdiyi bütün diskret anlar (uçlar, kəsişmə nöqtələri).
2. Hadisələri x-ə görə **sırala** (bərabərlikləri y və ya tipə görə).
3. **Status strukturu saxla** — süpürən xətti kəsən aktiv obyektlərin balanslı ağacı (y-ə görə sıralı).
4. **Hər hadisəni emal et:** başlanğıc → obyekti əlavə et, qonşularla kəsişmə yoxla; son → obyekti çıxar; kəsişmə → iki parçanı yer dəyiş.

## Nümunə (Skyline)
Binalar `[(2,9,10),(3,7,15),(5,12,12)]` (sol, sağ, hündürlük)
- x=2: A başlayır → 10
- x=3: B başlayır → 15
- x=7: B bitir → 12
- x=12: C bitir → 0
Açar nöqtələr: `[(2,10),(3,15),(7,12),(12,0)]`

## Kod
```python
import heapq

def skyline(buildings):
    """buildings: [(sol, sağ, hündürlük), ...]; [x, hündürlük] açar nöqtələri qaytarır."""
    events = []
    for L, R, H in buildings:
        events.append((L, -H, R))         # başlanğıc (min-topa hiyləsi üçün -H)
        events.append((R, 0, 0))          # son
    events.sort()
    result = []
    heap = [(0, float('inf'))]            # sentinel: yer səviyyəsi
    for x, neg_h, R in events:
        if neg_h:
            heapq.heappush(heap, (neg_h, R))
        while heap[0][1] <= x:            # bitmiş binaları çıxar (tənbəl silmə)
            heapq.heappop(heap)
        curr = -heap[0][0]
        if not result or result[-1][1] != curr:
            result.append([x, curr])
    return result

print(skyline([(2,9,10),(3,7,15),(5,12,12),(15,20,10)]))
# [[2,10],[3,15],[7,12],[12,0],[15,10],[20,0]]
```

## Mürəkkəblik
| Məsələ | Vaxt |
|--------|------|
| Kəsişmələr (Bentley-Ottmann) | O((n+k) log n) |
| Skyline / düzbucaqlı birləşmə | O(n log n) |
| Fortune Voronoi | O(n log n) |

## Nə vaxt
- ✅ Bir ox boyunca sıralanan interval/parça/hadisə məsələləri; hər x-də nəyin üst-üstə düşdüyünü tapmaq.
- ✅ Skyline, düzbucaqlı birləşmə sahəsi, bütün kəsişmələr.
- ❌ Sadə tək-cüt kəsişmə — birbaşa O(1) oriyentasiya testi.
- ❌ 3D məsələlər — 2D süpürmə asanlıqla ümumiləşmir.

## Əlaqəli
- [Line Intersection](LineIntersection.md) — sweep line daxilindəki O(1) kəsişmə yoxlaması.
- [Voronoi Diagram](VoronoiDiagram.md) — Fortune alqoritmi sweep line-dır.
- [Convex Hull](ConvexHull.md) — Monotone Chain da bir növ "süpürmə"dir.
