# Merge Sort

**Fikir:** Massivi yarıya bölürük, hər yarını rekursiv sıralayırıq, sonra iki sıralı yarını birləşdiririk (merge). Əsl iş birləşdirmə mərhələsində baş verir.

## Necə işləyir
1. **Baza:** 0 və ya 1 elementli massiv onsuz da sıralıdır — qaytar.
2. **Böl:** Ortanı tap, `left` və `right` yarılara ayır.
3. **Sırala:** Hər iki yarını rekursiv sırala.
4. **Birləşdir:** İki sıralı yarının önündəki elementləri müqayisə et, kiçiyini nəticəyə əlavə et; biri bitəndə qalanını sona köçür.

## Nümunə
`[38, 27, 43, 3]` → `[38,27]` və `[43,3]` → `[27,38]` və `[3,43]`

Birləşdirmə: 3 → 27 → 38 → 43 = `[3, 27, 38, 43]` ✅

## Kod
```python
def merge_sort(arr):
    if len(arr) <= 1:               # baza: onsuz da sıralı
        return arr
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])    # sol yarını sırala
    right = merge_sort(arr[mid:])   # sağ yarını sırala
    return merge(left, right)

def merge(left, right):
    result, i, j = [], 0, 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:     # <= stabilliyi qoruyur
            result.append(left[i]); i += 1
        else:
            result.append(right[j]); j += 1
    result.extend(left[i:])         # qalanları əlavə et
    result.extend(right[j:])
    return result

print(merge_sort([38, 27, 43, 3, 9, 82, 10]))   # [3, 9, 10, 27, 38, 43, 82]
```

## Mürəkkəblik
| Hal | Vaxt | Yaddaş |
|-----|------|--------|
| Hər zaman | O(n log n) | O(n) |

Stabil ✅ · Yerində deyil ❌ (əlavə O(n) yaddaş — QuickSort ilə əsas fərq).

## Nə vaxt
- ✅ Hər halda zəmanətli O(n log n) lazım olanda (QuickSort-un O(n²) pis halı yoxdur).
- ✅ Stabil sıralama; bağlı siyahıların (linked list) sıralanması.
- ✅ İnversiyaların sayını saymaq; data RAM-a sığmayanda (xarici sıralama).
- ❌ Yaddaş çox məhduddursa — əlavə O(n) yer lazımdır.

## Əlaqəli
- [Quick Sort](QuickSort.md) — həm də böl-və-idarə et; orta halda daha sürətli, amma pis halda O(n²).
- [Insertion Sort](InsertionSort.md) — Timsort-da kiçik hissələr üçün baza kimi istifadə olunur.
- [Heap Sort](HeapSort.md) — həm də O(n log n); yerində, amma stabil deyil.
