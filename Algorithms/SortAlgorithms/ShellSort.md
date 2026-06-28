# Shell Sort

**Fikir:** Insertion Sort kiçik elementlər önə yalnız bir-bir gəldiyi üçün yavaşdır. Shell Sort əvvəlcə **böyük addımla (gap)** Insertion Sort edir — elementlər uzaq məsafəyə sıçraya bilir — sonra addımı tədricən 1-ə endirir.

## Necə işləyir
1. Başlanğıc `gap` seç (adətən `n // 2`).
2. **Addımlı Insertion Sort:** hər elementi `gap` qədər əvvəlindəki elementlə müqayisə et, səhvdirsə sürüşdür.
3. Addımı yarıya endir və təkrarla.
4. `gap = 1` olanda bu adi Insertion Sort-dur — amma massiv artıq demək olar sıralıdır, ona görə tez bitir.

## Nümunə
`[12, 34, 54, 2, 3]`
- gap=2 → `[3, 2, 12, 34, 54]`
- gap=1 → `[2, 3, 12, 34, 54]` ✅

## Kod
```python
def shell_sort(arr):
    n = len(arr)
    gap = n // 2
    while gap > 0:
        for i in range(gap, n):
            temp = arr[i]
            j = i
            # temp-dən böyük elementləri gap qədər sürüşdür
            while j >= gap and arr[j - gap] > temp:
                arr[j] = arr[j - gap]
                j -= gap
            arr[j] = temp
        gap //= 2          # addımı azalt → sonda 1 olur
    return arr

print(shell_sort([12, 34, 54, 2, 3]))   # [2, 3, 12, 34, 54]
```

> Daha sürətli üçün Ciura ardıcıllığı: `gaps = [1, 4, 10, 23, 57, 132, 301, 701]` (böyükdən kiçiyə keç).

## Mürəkkəblik
| Hal | Vaxt | Yaddaş |
|-----|------|--------|
| Ən yaxşı | O(n log n) | O(1) |
| Orta | ~O(n^1.5) | O(1) |
| Ən pis | O(n²) | O(1) |

Stabil deyil ❌ · Yerində ✅ · Mürəkkəblik **addım ardıcıllığından** çox asılıdır.

## Nə vaxt
- ✅ Orta ölçülü massivlər (n ≈ 100–10 000) və sadə, yerində kod.
- ✅ Yaddaş məhduddur, amma sadə Insertion Sort-dan yaxşı performans istəyirsən.
- ❌ Çox böyük data — QuickSort/MergeSort.
- ❌ Stabillik və ya nəzəri zəmanət lazımdır.

## Əlaqəli
- [Insertion Sort](InsertionSort.md) — Shell Sort dəyişən addımlı Insertion Sort-dur.
- [Quick Sort](QuickSort.md) · [Merge Sort](MergeSort.md) — böyük data üçün daha yaxşı.
