# Selection Sort

**Fikir:** Hər dəfə sıralanmamış hissədən ən kiçik elementi tapıb onu sıralanmış hissənin sonuna qoyuruq.

## Necə işləyir
1. Massivi fikrən ikiyə böl: solda sıralı hissə, sağda sıralanmamış hissə (əvvəlcə sol boşdur).
2. Sıralanmamış hissəni tam gəzib ən kiçik elementin indeksini tap.
3. Onu sıralanmamış hissənin ilk elementi ilə yer dəyiş.
4. Sərhədi bir addım sağa sür (sıralı hissə bir element böyüyür).
5. Sıralanmamış hissə bitənə qədər təkrarla.

## Nümunə
`[64, 25, 12, 22, 11]`
- min=11 → `[11, 25, 12, 22, 64]`
- min=12 → `[11, 12, 25, 22, 64]`
- min=22 → `[11, 12, 22, 25, 64]` ✅

## Kod
```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        min_index = i                   # bu mövqe ən kiçikdir deyə fərz et
        for j in range(i + 1, n):       # sıralanmamış hissədə əsl minimumu tap
            if arr[j] < arr[min_index]:
                min_index = j
        arr[i], arr[min_index] = arr[min_index], arr[i]   # minimumu yerinə qoy
    return arr

print(selection_sort([64, 25, 12, 22, 11]))   # [11, 12, 22, 25, 64]
```

## Mürəkkəblik
| Hal | Vaxt | Yaddaş |
|-----|------|--------|
| Hər zaman | O(n²) | O(1) |

Stabil deyil ❌ · Yerində ✅ · Ən çox n−1 yer dəyişmə edir (yazma əməliyyatı bahalı olanda faydalı).

## Nə vaxt
- ✅ Yer dəyişmə (yazma) sayını minimuma endirmək lazım olanda.
- ✅ Kiçik massivlər və ən sadə kod.
- ❌ Böyük və ya sıralıya yaxın data — adaptiv deyil, həmişə O(n²).

## Əlaqəli
- [Bubble Sort](BubbleSort.md) — həm də O(n²), stabil, amma daha çox yer dəyişmə.
- [Insertion Sort](InsertionSort.md) — adaptiv və stabil, sıralıya yaxın datada daha yaxşı.
- [Heap Sort](HeapSort.md) — minimumu O(log n)-də tapmaq üçün topa (heap) istifadə edir.
