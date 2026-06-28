# N-Queens (N Vəzir Məsələsi)

**Fikir:** Vəzirləri şahmat lövhəsinə sətir-sətir qoyuruq. Hər sətirdə bir sütun **sınayırıq**, əvvəlki vəzirlərlə toqquşma (eyni sütun və ya diaqonal) **yoxlayırıq**; toqquşursa qoyuluşu **geri alıb** növbəti sütuna keçirik. Sətirdə bütün sütunlar bitsə, əvvəlki sətirə qayıdırıq. Klassik backtracking: "sına → yoxla → geri al".

## Necə işləyir
1. **Seç:** cari sətir üçün bir sütun seç.
2. **Yoxla:** heç bir əvvəlki vəzir eyni sütunu, `row - col` diaqonalını və ya `row + col` diaqonalını paylaşmamalıdır.
3. **Yerləşdir:** təhlükəsizdirsə, sütun/diaqonalları izləmə dəstlərinə əlavə et.
4. **Rekursiya:** növbəti sətirə keç.
5. **Geri al:** sətirdə heç bir sütun işləmirsə, vəziri çıxar və əvvəlki sətirdə növbəti sütunu sına.
6. **Baza:** bütün N sətir dolanda lövhəni həll kimi yaz.

## Nümunə (N=4)
İki həll var. Birincisi:
```
.Q..
...Q
Q...
..Q.
```

## Kod
```python
def solve_n_queens(n):
    solutions = []
    cols, diag1, diag2 = set(), set(), set()   # sütun, ↘ diaqonal, ↗ diaqonal
    placement = [-1] * n

    def backtrack(row):
        if row == n:                            # baza: bütün sətirlər dolu
            board = ['.' * c + 'Q' + '.' * (n - c - 1) for c in placement]
            solutions.append(board)
            return
        for col in range(n):
            if col in cols or (row - col) in diag1 or (row + col) in diag2:
                continue                         # təhlükəsiz deyil
            placement[row] = col                 # yerləşdir
            cols.add(col); diag1.add(row - col); diag2.add(row + col)
            backtrack(row + 1)
            cols.remove(col); diag1.remove(row - col); diag2.remove(row + col)  # geri al

    backtrack(0)
    return solutions

print(len(solve_n_queens(4)))   # 2
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(N!) (ən pis) |
| Yaddaş | O(N) |

> Toqquşma yoxlamaları əksər budaqları erkən kəsir — praktikada O(N!)-dən çox yaxşıdır.

## Nə vaxt
- ✅ Bir-birinə **toqquşma məhdudiyyəti** olan elementləri grid/ardıcıllığa yerləşdirmək.
- ✅ Bütün düzgün düzülüşləri tapmaq (sadəcə birini yox); kiçik N (≤ ~15).
- ❌ Böyük N (≥ 20) və bütün həllər — eksponensial; heuristika (min-conflicts) lazımdır.

## Əlaqəli
- [Sudoku Solver](SudokuSolver.md) — eyni sətir/sütun/qutu məhdudiyyəti.
- [Hamiltonian Cycle](HamiltonianCycle.md) — həm də məhdudiyyətli düzgün yolları sadalayır.
- [Subset Sum (Backtracking)](SubsetSumBacktracking.md) · [Knight's Tour](KnightsTour.md) — eyni "sına → yoxla → geri al" quruluşu.
