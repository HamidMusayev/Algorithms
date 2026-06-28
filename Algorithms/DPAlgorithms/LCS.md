# Longest Common Subsequence (LCS)

**Fikir:** İki sətirdə eyni nisbi sırada görünən simvolların ən uzun ardıcıllığı. **Alt-ardıcıllıq** (subsequence) bitişik olmaya bilər — sadəcə sıra qorunmalıdır. `dp[i][j]` = s1-in ilk i, s2-nin ilk j simvolunun LCS uzunluğu.

## Necə işləyir
**Rekurrens:**
- `s1[i-1] == s2[j-1]` (simvollar uyğun): `dp[i][j] = dp[i-1][j-1] + 1` (diaqonalı uzat).
- Uyğun deyil: `dp[i][j] = max(dp[i-1][j], dp[i][j-1])` (birini atla, ən yaxşını götür).

`dp[m][n]` = LCS uzunluğu. Faktiki sətiri qurmaq üçün cədvəldən geri izlə.

## Nümunə
s1 = "ABC", s2 = "AC"
|   | "" | A | C |
|---|----|---|---|
| "" | 0 | 0 | 0 |
| A | 0 | 1 | 1 |
| B | 0 | 1 | 1 |
| C | 0 | 1 | **2** |

LCS = "AC" (uzunluq 2). "ABCBDAB" və "BDCAB" → "BCAB" (4).

## Kod
```python
def lcs(s1, s2):
    m, n = len(s1), len(s2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s1[i - 1] == s2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    # Geri izləyərək faktiki LCS sətirini qur
    i, j, result = m, n, []
    while i > 0 and j > 0:
        if s1[i - 1] == s2[j - 1]:
            result.append(s1[i - 1]); i -= 1; j -= 1
        elif dp[i - 1][j] >= dp[i][j - 1]:
            i -= 1
        else:
            j -= 1
    return dp[m][n], ''.join(reversed(result))

print(lcs("ABCBDAB", "BDCAB"))   # (4, 'BCAB')
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(m × n) |
| Yaddaş | O(m × n) → O(min(m, n))-ə endirilə bilər |

## Nə vaxt
- ✅ İki ardıcıllığın ən uzun ortaq hissəsini tapmaq (sıra qorunur, bitişik deyil).
- ✅ DNA/protein ardıcıllığı düzləşdirmə; iki sətirin oxşarlığı.
- ✅ Diff alətləri (git diff dəyişən sətirləri tapmaq üçün).
- ❌ Bitişik uyğunluq (alt-sətir) lazımdır — KMP / Z-Alqoritm.
- ❌ Çox uzun ardıcıllıqlar (milyonlarla simvol) — O(mn) yavaşdır.

## Əlaqəli
- [Edit Distance](EditDistance.md) — həm də iki sətir üzərində 2D DP.
- [LIS](LIS.md) — massiv ilə onun sıralanmış versiyasının LCS-i = LIS.
- [Knapsack](Knapsack.md) — eyni 2D DP cədvəl quruluşu.
