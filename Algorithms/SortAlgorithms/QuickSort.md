# Quick Sort

**Fikir:** Bir element "pivot" seçirik, ondan kiçikləri sola, böyükləri sağa yığırıq (pivot öz son yerinə düşür), sonra hər iki tərəfi rekursiv eyni qayda ilə sıralayırıq.

## Necə işləyir
1. Pivot seç (sonuncu element, təsadüfi və ya üçün-medianı).
2. **Bölüşdür (partition):** ≤ pivot olanları sola, > pivot olanları sağa düz; pivot öz son yerinə düşür.
3. Pivotun solundakı alt-massivi rekursiv sırala.
4. Pivotun sağındakı alt-massivi rekursiv sırala.
5. **Baza:** 0 və ya 1 elementli hissə onsuz da sıralıdır — dayan.

## Nümunə
`[10, 80, 30, 90, 40]`, pivot = 40
→ partition: `[10, 30, 40, 90, 80]` (40 indeks 2-də, öz yerində)
→ sol `[10,30]` sıralı, sağ `[90,80]` → `[80,90]`
→ `[10, 30, 40, 80, 90]` ✅

## Kod
```python
import random

def quick_sort(arr, low=0, high=None):
    if high is None:
        high = len(arr) - 1
    if low < high:
        pi = partition(arr, low, high)   # pivotun son indeksi
        quick_sort(arr, low, pi - 1)     # soldakını sırala
        quick_sort(arr, pi + 1, high)    # sağdakını sırala

def partition(arr, low, high):
    rand = random.randint(low, high)     # təsadüfi pivot → pis halın qarşısını alır
    arr[rand], arr[high] = arr[high], arr[rand]
    pivot = arr[high]
    i = low - 1                          # "≤ pivot" zonasının sağ kənarı
    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i + 1], arr[high] = arr[high], arr[i + 1]   # pivotu yerinə qoy
    return i + 1

arr = [10, 80, 30, 90, 40, 50, 70]
quick_sort(arr)
print(arr)   # [10, 30, 40, 50, 70, 80, 90]
```

## Mürəkkəblik
| Hal | Vaxt | Yaddaş |
|-----|------|--------|
| Ən yaxşı / Orta | O(n log n) | O(log n) |
| Ən pis | O(n²) | O(n) |

Stabil deyil ❌ · Yerində ✅ · Pis hal (pivot həmişə min/maks) — **təsadüfi pivot** ilə praktikada demək olar baş vermir.

## Nə vaxt
- ✅ Praktikada ən sürətli yerində sıralama (keş-dostu).
- ✅ k-cı ən kiçik/böyük elementi tapmaq (QuickSelect — orta O(n)).
- ✅ Yaddaş məhduddur, əlavə O(n) yer yoxdur.
- ❌ Zəmanətli O(n log n) və ya stabillik lazımdır — [Merge Sort](MergeSort.md) istifadə et.

## Əlaqəli
- [Merge Sort](MergeSort.md) — stabil, zəmanətli O(n log n), amma O(n) əlavə yaddaş.
- [Heap Sort](HeapSort.md) — yerində O(n log n) pis hal, amma praktikada daha yavaş.
- [Insertion Sort](InsertionSort.md) — hibrid QuickSort-da kiçik hissələr üçün baza.
