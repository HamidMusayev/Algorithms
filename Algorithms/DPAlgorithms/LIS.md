# Longest Increasing Subsequence (LIS)

**Fikir:** Massivdə soldan-sağa, geri qayıtmadan seçilən, ciddi artan ən uzun "pilləkən". `dp[i]` = məhz `i` indeksində bitən ən uzun artan alt-ardıcıllığın uzunluğu. (Alt-ardıcıllıq bitişik olmaya bilər.)

## Necə işləyir
**O(n²) DP:** `dp[i] = max(dp[j] + 1)`, bütün `j < i` üçün `arr[j] < arr[i]` olduqda. Baza: `dp[i] = 1`.

**O(n log n) (səbir sıralaması):**
1. `tails` massivi saxla: `tails[i]` = `i+1` uzunluqlu bütün artan ardıcıllıqların ən kiçik son elementi.
2. Hər element üçün `tails`-də binary axtarış et:
   - Hamısından böyükdürsə → əlavə et (yeni daha uzun LIS).
   - Yoxsa → tapılan mövqeyi əvəz et (ən kiçik mümkün son).

## Nümunə
`[10, 9, 2, 5, 3, 7, 101, 18]`
- ən uzun: [2, 3, 7, 101] və ya [2, 3, 7, 18] → uzunluq **4** ✅

## Kod
```python
def lis_n2(arr):
    """O(n²) — başa düşülən, bərpa edilə bilən."""
    if not arr:
        return 0
    dp = [1] * len(arr)
    for i in range(1, len(arr)):
        for j in range(i):
            if arr[j] < arr[i]:               # i-də bitən ardıcıllığı uzada bilər
                dp[i] = max(dp[i], dp[j] + 1)
    return max(dp)

from bisect import bisect_left

def lis_nlogn(arr):
    """O(n log n) — binary axtarış ilə."""
    tails = []
    for x in arr:
        pos = bisect_left(tails, x)
        if pos == len(tails):
            tails.append(x)                   # ən uzun LIS-i uzadır
        else:
            tails[pos] = x                    # daha kiçik son ilə əvəz et
    return len(tails)                          # qədəh sayı = LIS uzunluğu

arr = [10, 9, 2, 5, 3, 7, 101, 18]
print(lis_n2(arr), lis_nlogn(arr))   # 4 4
```

## Mürəkkəblik
| Yanaşma | Vaxt | Yaddaş |
|---------|------|--------|
| DP | O(n²) | O(n) |
| Binary axtarış | O(n log n) | O(n) |

## Nə vaxt
- ✅ Massivin ən uzun **ciddi artan** alt-ardıcıllığı (alt-sətir yox).
- ✅ Qutu yığma (hər ölçü növbətindən kiçik), ən uzun söz-cüt zənciri.
- ✅ Faktiki ardıcıllıq lazımdırsa — O(n²) DP + sələf izləmə.
- ❌ Bitişik artan alt-massiv lazımdır — sadə xətti gəzinti.

## Əlaqəli
- [LCS](LCS.md) — arr ilə sorted(arr)-ın LCS-i = LIS uzunluğu.
- [Knapsack](Knapsack.md) — oxşar "uzat və ya atla" DP nümunəsi.
