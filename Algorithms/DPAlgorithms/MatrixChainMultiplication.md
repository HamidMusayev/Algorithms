# Matrix Chain Multiplication (Matris Zəncirinin Vurulması)

**Fikir:** Matris vurması assosiativdir: `(AB)C = A(BC)`, amma mötərizələrin yerinə görə əməliyyat sayı kəskin dəyişir. A=10×30, B=30×5, C=5×60 olsa:
- `(AB)C`: 1500 + 3000 = **4500** əməliyyat
- `A(BC)`: 9000 + 18000 = 27000 əməliyyat (6 dəfə baha!)

`dp[i][j]` = `Aᵢ`-dən `Aⱼ`-yə qədər hasili hesablamaq üçün minimum skalyar vurma.

## Necə işləyir
**Rekurrens (i və j arasında hər k bölünmə nöqtəsini sına):**
- `dp[i][i] = 0` (tək matris).
- `dp[i][j] = min(dp[i][k] + dp[k+1][j] + p[i-1]·p[k]·p[j])`, k ∈ [i..j-1].

`p` — ölçülər massivi: `Aᵢ` matrisi `p[i-1] × p[i]` ölçülüdür.

## Nümunə
A1(10×30), A2(30×5), A3(5×60) → `p = [10, 30, 5, 60]`
→ `dp[1][3] = 4500`, optimal sıra: `(A1 × A2) × A3` ✅

## Kod
```python
def matrix_chain_order(p):
    """p: ölçülər massivi; matris i → p[i-1] × p[i]."""
    n = len(p) - 1                              # matris sayı
    dp = [[0] * (n + 1) for _ in range(n + 1)]
    split = [[0] * (n + 1) for _ in range(n + 1)]
    for length in range(2, n + 1):              # zəncir uzunluğu
        for i in range(1, n - length + 2):
            j = i + length - 1
            dp[i][j] = float('inf')
            for k in range(i, j):               # hər bölünmə nöqtəsi
                cost = dp[i][k] + dp[k + 1][j] + p[i - 1] * p[k] * p[j]
                if cost < dp[i][j]:
                    dp[i][j] = cost
                    split[i][j] = k
    return dp[1][n], split

print(matrix_chain_order([10, 30, 5, 60])[0])   # 4500
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(n³) |
| Yaddaş | O(n²) |

## Nə vaxt
- ✅ Bir ardıcıllığı alt-hissələrə **optimal bölmək**; iki alt-nəticəni birləşdirmə xərci onların ölçüsündən asılı olanda.
- ✅ İnterval DP: optimal BST, çoxbucaqlı bölünməsi, "Burst Balloons".
- ❌ Faktiki matris hasilini tapmaq — bu yalnız optimal **sıranı** tapır, hasili yox.

## Əlaqəli
- [Rod Cutting](RodCutting.md) — həm də "optimal bölmək", amma xətti, interval deyil.
- [LCS](LCS.md) — hər ikisi 2D DP cədvəli istifadə edir.
