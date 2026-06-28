# Heap Sort

**Fikir:** Massivdən **maks-topa (max-heap)** qururuq — kökdə həmişə ən böyük element olur. Sonra dəfələrlə ən böyüyü çıxarıb massivin sonuna qoyuruq.

## Necə işləyir
**Mərhələ 1 — Maks-topa qur:**
1. Son yarpaq olmayan node-dan (`n//2 - 1`) başlayıb 0-a qədər `heapify` çağır.
2. Bundan sonra `arr[0]` ən böyükdür.

**Mərhələ 2 — Bir-bir çıxar:**
3. `arr[0]` (maks) ilə sıralanmamış hissənin sonuncu elementini yer dəyiş — bu element artıq son yerindədir.
4. Topanın ölçüsünü 1 azalt və kökdən `heapify` edib topa xassəsini bərpa et.
5. Topada 1 element qalana qədər təkrarla.

## Nümunə
`[4, 10, 3, 5, 1]` → maks-topa: `[10, 5, 3, 4, 1]`
→ çıxarma: `[..., 10]` → `[..., 5, 10]` → `[..., 4, 5, 10]` → `[1, 3, 4, 5, 10]` ✅

## Kod
```python
def heap_sort(arr):
    n = len(arr)
    # Mərhələ 1: maks-topa qur
    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i)
    # Mərhələ 2: bir-bir çıxar
    for i in range(n - 1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0]   # cari maksimumu sona apar
        heapify(arr, i, 0)                 # azalmış topanı bərpa et

def heapify(arr, n, i):
    largest = i
    left, right = 2 * i + 1, 2 * i + 2
    if left < n and arr[left] > arr[largest]:
        largest = left
    if right < n and arr[right] > arr[largest]:
        largest = right
    if largest != i:                       # kök ən böyük deyil
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify(arr, n, largest)           # təsirlənmiş alt-ağacı düzəlt

arr = [4, 10, 3, 5, 1]
heap_sort(arr)
print(arr)   # [1, 3, 4, 5, 10]
```

## Mürəkkəblik
| Hal | Vaxt | Yaddaş |
|-----|------|--------|
| Hər zaman | O(n log n) | O(1) |

Stabil deyil ❌ · Yerində ✅ · Həm zəmanətli O(n log n), həm də yerində — QuickSort (zəmanət yox) və MergeSort (yerində deyil) arasında qızıl orta.

## Nə vaxt
- ✅ Əlavə yaddaş olmadan zəmanətli O(n log n) lazım olanda.
- ✅ Prioritet növbə (priority queue) və ya k ən böyük/kiçik element.
- ❌ Stabillik lazımdır — [Merge Sort](MergeSort.md).
- ❌ Keş performansı vacibdir — QuickSort praktikada daha sürətli.

## Əlaqəli
- [Quick Sort](QuickSort.md) — yerində; orta halda sürətli, amma pis halda O(n²).
- [Merge Sort](MergeSort.md) — zəmanətli O(n log n) və stabil; O(n) yaddaş.
- [Selection Sort](SelectionSort.md) — eyni "maksimumu tap, sona qoy" fikri; Heap Sort onun O(n log n) versiyasıdır.
