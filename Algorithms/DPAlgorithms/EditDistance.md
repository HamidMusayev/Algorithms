# Edit Distance (Levenshtein Məsafəsi)

**Fikir:** Bir sözü digərinə çevirmək üçün lazım olan **minimum əməliyyat** sayı. Hər əməliyyat: simvol əlavə et, sil və ya əvəz et. `dp[i][j]` = `s1[:i]`-i `s2[:j]`-ə çevirmək üçün minimum redaktə.

Məsələn "kitten" → "sitting": k→s, e→i, +g = **3 redaktə**.

## Necə işləyir
**Rekurrens:**
- `s1[i-1] == s2[j-1]` (uyğun): `dp[i][j] = dp[i-1][j-1]` (redaktə lazım deyil).
- Uyğun deyil: `dp[i][j] = 1 + min(dp[i-1][j] sil, dp[i][j-1] əlavə et, dp[i-1][j-1] əvəz et)`.

**Baza:** `dp[i][0] = i` (s1-in i simvolunu sil), `dp[0][j] = j` (j simvol əlavə et).

## Nümunə
s1 = "cat", s2 = "cut"
|   | "" | c | u | t |
|---|----|---|---|---|
| "" | 0 | 1 | 2 | 3 |
| c | 1 | 0 | 1 | 2 |
| a | 2 | 1 | 1 | 2 |
| t | 3 | 2 | 2 | **1** |

`dp[3][3] = 1` → yalnız 'a'-nı 'u' ilə əvəz et ✅

## Kod
```python
def edit_distance(s1, s2):
    m, n = len(s1), len(s2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(m + 1):
        dp[i][0] = i                            # s1-dən i simvol sil
    for j in range(n + 1):
        dp[0][j] = j                            # j simvol əlavə et
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s1[i - 1] == s2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]      # uyğun — redaktə yox
            else:
                dp[i][j] = 1 + min(
                    dp[i - 1][j],                # sil
                    dp[i][j - 1],                # əlavə et
                    dp[i - 1][j - 1])            # əvəz et
    return dp[m][n]

print(edit_distance("kitten", "sitting"))   # 3
print(edit_distance("cat", "cut"))          # 1
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(m × n) |
| Yaddaş | O(m × n) → O(min(m, n))-ə endirilə bilər |

## Nə vaxt
- ✅ İki sətirin oxşarlığını minimum dəyişiklik sayı ilə ölçmək.
- ✅ Orfoqrafiya yoxlayıcı / avtodüzəliş, qeyri-dəqiq uyğunlaşdırma (fuzzy matching).
- ✅ DNA mutasiya məsafəsi; git diff (LCS variantı ilə).
- ❌ Çox uzun sətirlər (milyonlarla simvol) — O(mn) yavaşdır.

## Əlaqəli
- [LCS](LCS.md) — yalnız əlavə/sil halında: edit distance = m + n − 2 × LCS.
- [Knapsack](Knapsack.md) — eyni 2D DP cədvəl quruluşu.
