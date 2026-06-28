# Bubble Sort

**Fikir:** Massivi dəfələrlə gəzib qonşu, yeri səhv olan cütləri yer dəyişdiririk. Hər keçiddə ən böyük element sona "qabarcıq kimi" qalxır.

## Necə işləyir
1. Massivin əvvəlindən başla.
2. Hər qonşu cütü `arr[j]` və `arr[j+1]` müqayisə et; səhv sıradadırsa, yerini dəyiş.
3. Bir tam keçiddən sonra ən böyük element öz son yerindədir — qalan hissəni təkrarla.
4. Bir keçiddə heç bir dəyişmə olmadısa, massiv artıq sıralıdır → dayan.

## Nümunə
`[5, 3, 1, 4, 2]`
- Keçid 1 → `[3, 1, 4, 2, 5]` (5 sona qalxdı)
- Keçid 2 → `[1, 3, 2, 4, 5]`
- Keçid 3 → `[1, 2, 3, 4, 5]`
- Keçid 4 → dəyişmə yoxdur, dayan ✅

## Kod
```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        swapped = False
        # Sonuncu i element artıq yerindədir
        for j in range(n - i - 1):
            if arr[j] > arr[j + 1]:        # səhv sıra → yer dəyiş
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
        if not swapped:                    # dəyişmə yoxdursa, sıralıdır
            break
    return arr

print(bubble_sort([5, 3, 1, 4, 2]))   # [1, 2, 3, 4, 5]
```

## Mürəkkəblik
| Hal | Vaxt | Yaddaş |
|-----|------|--------|
| Ən yaxşı (sıralı) | O(n) | O(1) |
| Orta / Ən pis | O(n²) | O(1) |

Stabil ✅ · Yerində (in-place) ✅ · `swapped` optimallaşdırması sıralı massivi bir keçiddə tutur.

## Nə vaxt
- ✅ Kiçik (n < 20) və ya demək olar sıralı massivlər.
- ✅ Sadə, öyrətmə məqsədli kod lazım olanda.
- ❌ Böyük data — çox yavaşdır.

## Əlaqəli
- [Selection Sort](SelectionSort.md) — həm də O(n²), amma stabil deyil, daha az yer dəyişmə edir.
- [Insertion Sort](InsertionSort.md) — O(n²), amma kiçik/sıralıya yaxın datada praktikada daha sürətli.
- [Merge Sort](MergeSort.md) · [Quick Sort](QuickSort.md) — böyük data üçün O(n log n).
