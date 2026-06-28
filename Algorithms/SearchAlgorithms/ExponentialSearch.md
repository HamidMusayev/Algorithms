# Exponential Search

**Fikir:** Ölçüsü naməlum, çox böyük sıralı siyahıda axtarış: indeksi 1, 2, 4, 8, 16 ... kimi ikiqat artırırıq, axtarılanı keçəndə tapılan diapazonda Binary Search edirik.

## Necə işləyir
1. İlk element target-dirsə → indeks 0 qaytar.
2. `i = 1`-dən başla, `arr[i] > target` və ya `i >= n` olana qədər `i`-ni ikiqatla.
3. `[i // 2, min(i, n-1)]` diapazonunda **Binary Search** et.

## Nümunə
`[2, 3, 4, 10, 40, 80, 120]`, axtarılan = `10`
- i=1: 3 ≤ 10 → ikiqatla
- i=2: 4 ≤ 10 → ikiqatla
- i=4: 40 > 10 → dayan
- `[2, 4]`-də binary → **indeks 3-də tapıldı** ✅

## Kod
```python
def binary_search(arr, left, right, target):
    while left <= right:
        mid = left + (right - left) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1

def exponential_search(arr, target):
    n = len(arr)
    if n == 0:
        return -1
    if arr[0] == target:
        return 0
    i = 1
    while i < n and arr[i] <= target:
        i *= 2                                 # indeksi ikiqatla
    return binary_search(arr, i // 2, min(i, n - 1), target)

print(exponential_search([2, 3, 4, 10, 40, 80, 120], 10))   # 3
```

## Mürəkkəblik
| Hal | Vaxt | Yaddaş |
|-----|------|--------|
| Ən yaxşı | O(1) | O(1) |
| Orta / Ən pis | O(log n) | O(1) |

> Binary Search ilə eyni asimptotika, amma target massivin **əvvəlinə** yaxın olanda diapazonu daha tez tapır.

## Nə vaxt
- ✅ Sıralı və potensial çox böyük/sonsuz (uzunluğu naməlum) siyahı.
- ✅ Target çox güman ki əvvələ yaxındır; axın/baza kimi sonsuz sıralı siyahı.
- ❌ Kiçik massiv (n < 30) — birbaşa Binary Search daha sadədir.

## Əlaqəli
- [Binary Search](BinarySearch.md) — eksponensial fazadan sonra son addım kimi istifadə olunur.
- [Jump Search](JumpSearch.md) — həm də bloklarla; sabit addım, eksponensial deyil.
- [Interpolation Search](InterpolationSearch.md) — dəyərə görə mövqe təxmin edir.
