# Binary Search

**Fikir:** Lüğətdə söz axtaran kimi — ortadan açırıq, axtardığımız ondan əvvəldədir ya sonra deyə qərar verir, qalan yarını atırıq. Hər müqayisə axtarış sahəsini yarıya bölür → O(log n). **Massiv sıralı olmalıdır.**

## Necə işləyir
1. İki göstərici qoy: `left = 0`, `right = n - 1`.
2. `left <= right` olduqca:
   - `mid = left + (right - left) // 2` (daşmanın qarşısını alır).
   - `arr[mid] == target` → tapıldı, `mid` qaytar.
   - `arr[mid] < target` → sağ yarıda axtar: `left = mid + 1`.
   - `arr[mid] > target` → sol yarıda axtar: `right = mid - 1`.
3. Dövr bitsə və tapmasan, `-1` qaytar.

## Nümunə
`[3, 9, 15, 22, 34, 47, 58]`, axtarılan = `34`
- mid=22 < 34 → sağa
- mid=47 > 34 → sola
- mid=34 → **indeks 4-də tapıldı** ✅

## Kod
```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = left + (right - left) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1        # sağ yarı
        else:
            right = mid - 1       # sol yarı
    return -1

print(binary_search([3, 9, 15, 22, 34, 47, 58], 34))   # 4
```

## Mürəkkəblik
| Hal | Vaxt | Yaddaş |
|-----|------|--------|
| Ən yaxşı | O(1) | O(1) |
| Orta / Ən pis | O(log n) | O(1) |

> Rekursiv versiya O(log n) stek yaddaşı istifadə edir.

## Nə vaxt
- ✅ Massiv sıralıdır və sürətli axtarış lazımdır (n böyükdür).
- ✅ Sərhəd tapmaq (ilk/son mövqe), və ya **cavab üzərində binary axtarış** (true/false monoton bölünəndə).
- ✅ Fırlanmış sıralı massiv, pik element, kvadrat kök kimi məsələlərdə gizlənir.
- ❌ Massiv sıralanmayıb və sıralamaq baha — Linear Search.
- ❌ Təsadüfi giriş yoxdur (bağlı siyahı) və ya data çox kiçikdir.

## Əlaqəli
- [Linear Search](LinearSearch.md) — sıralanmamış alternativ, O(n).
- [Jump Search](JumpSearch.md) — bloklarla sıçrayır, sonra linear gəzir.
- [Exponential Search](ExponentialSearch.md) — diapazonu tapandan sonra binary çağırır.
- [Interpolation Search](InterpolationSearch.md) — bərabər paylanmış data üçün təkmilləşmiş variant.
