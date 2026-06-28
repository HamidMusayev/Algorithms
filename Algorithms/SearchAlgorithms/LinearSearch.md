# Linear Search

**Fikir:** Massivi əvvəldən sona bir-bir gəzirik və hər elementi axtarılan dəyərlə müqayisə edirik. Heç bir fərziyyə yoxdur — sadəcə hamısını yoxla.

## Necə işləyir
1. İndeks 0-dan başla.
2. Cari elementi axtarılan dəyərlə müqayisə et.
3. Uyğun gəlirsə, indeksi qaytar.
4. Gəlmirsə, növbəti elementə keç.
5. Massivin sonuna çatsan və tapmasan, `-1` qaytar.

## Nümunə
`[10, 25, 7, 42, 31]`, axtarılan = `42`
- 10 ≠ 42 → 25 ≠ 42 → 7 ≠ 42 → **42 = 42, indeks 3-də tapıldı** ✅

## Kod
```python
def linear_search(arr, target):
    for i in range(len(arr)):
        if arr[i] == target:       # uyğun gəldi
            return i
    return -1                      # tapılmadı

print(linear_search([10, 25, 7, 42, 31], 42))   # 3
```

## Mürəkkəblik
| Hal | Vaxt | Yaddaş |
|-----|------|--------|
| Ən yaxşı (ilk element) | O(1) | O(1) |
| Orta / Ən pis | O(n) | O(1) |

## Nə vaxt
- ✅ Massiv sıralanmayıb və sıralamaq baha başa gələr.
- ✅ Bağlı siyahıda (linked list) axtarış — təsadüfi giriş yoxdur.
- ✅ Çox kiçik data (n < 20) və ya bütün təkrarları tapmaq lazımdır.
- ❌ Böyük, sıralı massiv — Binary Search dəfələrlə sürətlidir.

## Əlaqəli
- [Binary Search](BinarySearch.md) — sıralı massiv üçün O(log n) variant.
- [Jump Search](JumpSearch.md) — hər blokun içində linear axtarışı istifadə edir.
- [Exponential Search](ExponentialSearch.md) — eksponensial sıçrayışdan sonra binary çağırır.
