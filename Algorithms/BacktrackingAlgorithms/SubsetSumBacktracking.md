# Subset Sum (Backtracking)

**Fikir:** DP versiyası yalnız "mümkündürmü?" deyir; backtracking isə hədəfə çatan **bütün alt-çoxluqları tapır və sadalayır**. Hər addımda cari elementi ya **daxil et**, ya **çıxar**, rekursiya et, sonra qərarı **geri al**.

## Necə işləyir
1. Girişi sırala (kəsmə üçün).
2. **Seç:** `nums[i]`-i cari yola daxil etməyi nəzərdən keçir.
3. **Məhdudlaşdır:** `total + nums[i] > target`-dirsə, bu budağı dayandır (kəs).
4. **Məqsəd:** `total == target`-dirsə, cari yolu düzgün alt-çoxluq kimi yaz.
5. **Rekursiya:** `i+1` ilə davam et.
6. **Geri al:** `nums[i]`-i yoldan çıxar.

## Nümunə
nums = `[2, 3, 5, 6]`, hədəf = 8
- [2,6] → 8 ✅
- [3,5] → 8 ✅
- Nəticə: `[[2, 6], [3, 5]]`

## Kod
```python
def subset_sum_all(nums, target):
    """Hədəfə çatan BÜTÜN alt-çoxluqları tapır."""
    results = []
    nums.sort()                              # kəsmə üçün sırala
    def backtrack(start, path, total):
        if total == target:
            results.append(list(path))       # düzgün alt-çoxluq tapıldı
            return
        for i in range(start, len(nums)):
            if total + nums[i] > target:
                break                         # kəs: sıralı olduğu üçün qalanlar da böyük
            path.append(nums[i])              # seç
            backtrack(i + 1, path, total + nums[i])
            path.pop()                        # geri al
    backtrack(0, [], 0)
    return results

print(subset_sum_all([2, 3, 5, 6, 8, 10], 10))   # [[2, 3, 5], [2, 8], [10]]
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(2ⁿ) (ən pis — bütün alt-çoxluqlar) |
| Yaddaş | O(n) (rekursiya dərinliyi) |

Müsbət ədədlərlə kəsmə praktikada xeyli sürətləndirir.

## Nə vaxt
- ✅ Müəyyən xassəli **bütün alt-çoxluqları sadalamaq** (sadəcə mümkünlüyü yox).
- ✅ Kiçik n (≤ 25) və yaxşı kəsmə.
- ❌ Yalnız bəli/xeyr cavabı lazımdır — DP (SubsetSum) çox daha sürətli.
- ❌ Böyük n və böyük hədəf — eksponensial.

## Əlaqəli
- [Subset Sum DP](../DPAlgorithms/SubsetSum.md) — yalnız mümkünlük, O(n×hədəf).
- [N-Queens](NQueens.md) — eyni sına→yoxla→geri al quruluşu.
