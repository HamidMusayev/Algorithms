# Jump Search

**Fikir:** Sıralı massivdə hər dəfə sabit ölçülü blok (√n element) irəli "sıçrayırıq". Axtarılanı keçəndə (cari blokun sonu ≥ target), o blokun içində geri linear axtarış edirik.

## Necə işləyir
1. Addımı hesabla: `step = ⌊√n⌋`.
2. İndeks 0-dan başla. `arr[blok_sonu] >= target` olana və ya massivin sonuna çatana qədər `step` qədər irəli sıçra.
3. Uyğun bloku tapanda blokun əvvəlindən **linear axtarış** et.
4. Tapsan indeksi qaytar, yoxsa `-1`.

## Nümunə
`[2, 5, 8, 12, 16, 23, 38, 45, 56, 72]` (n=10), axtarılan = `23`, addım = 3
- arr[2]=8 < 23 → sıçra
- arr[5]=23 ≥ 23 → dayan
- blokda linear: 12 → 16 → **23, indeks 5-də tapıldı** ✅

## Kod
```python
import math

def jump_search(arr, target):
    n = len(arr)
    step = int(math.sqrt(n))   # optimal blok ölçüsü √n
    prev = 0
    # Faza 1: uyğun bloku tap
    while prev < n and arr[min(step, n) - 1] < target:
        prev = step
        step += int(math.sqrt(n))
        if prev >= n:
            return -1
    # Faza 2: blokun içində linear axtar
    for i in range(prev, min(step, n)):
        if arr[i] == target:
            return i
    return -1

print(jump_search([2, 5, 8, 12, 16, 23, 38, 45, 56, 72], 23))   # 5
```

## Mürəkkəblik
| Hal | Vaxt | Yaddaş |
|-----|------|--------|
| Ən yaxşı | O(1) | O(1) |
| Orta / Ən pis | O(√n) | O(1) |

> Optimal blok ölçüsü √n-dir. Linear-dən sürətli, binary-dən yavaş.

## Nə vaxt
- ✅ Sıralı massiv; linear-dən sürətli, binary-dən sadə bir şey lazımdır.
- ✅ Geriyə hərəkət bahalıdır (məs. maqnit lent) — yalnız bir blok geri qayıdır.
- ❌ Massiv sıralanmayıb və ya maksimum sürət lazımdır — Binary Search O(log n).

## Əlaqəli
- [Linear Search](LinearSearch.md) — hər blokun içində istifadə olunur.
- [Binary Search](BinarySearch.md) — daha sürətli alternativ (O(log n)).
- [Exponential Search](ExponentialSearch.md) — sabit addım yox, ikiqatlama ilə sıçrayır.
