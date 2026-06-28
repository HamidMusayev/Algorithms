# Sudoku Solver

**Fikir:** 9×9 lövhəni 1–9 rəqəmləri ilə elə doldurmaq ki, hər sətir, sütun və 3×3 qutuda hər rəqəm bir dəfə olsun. Backtracking: boş xana tap, 1–9 rəqəmlərini **sına**, düzgünlüyünü **yoxla** (sətir/sütun/qutuda toqquşma yox), **rekursiya** et, ziddiyyət olarsa **geri al** və növbəti rəqəmi sına.

## Necə işləyir
1. Növbəti boş xananı (dəyər = 0) **tap**.
2. Hər `d` rəqəmi (1–9) üçün:
   - **Yoxla:** `d` bu sətir, sütun və 3×3 qutuda yoxdur?
   - Düzgündürsə: `d`-ni xanaya **yerləşdir**.
   - Növbəti xana üçün **rekursiya** et.
   - Rekursiya True qaytarsa → həll olundu.
   - False qaytarsa → **geri al** (xananı 0-a qaytar).
3. Heç bir rəqəm işləməsə → False qaytar.
4. Boş xana qalmasa → həll olundu, True.

## Kod
```python
def solve_sudoku(board):
    """9×9 lövhəni yerində həll edir; 0 = boş."""
    empty = find_empty(board)
    if not empty:
        return True                          # boş xana yoxdur → həll olundu
    row, col = empty
    for digit in range(1, 10):
        if is_valid(board, row, col, digit):
            board[row][col] = digit          # sına
            if solve_sudoku(board):
                return True
            board[row][col] = 0              # geri al
    return False

def find_empty(board):
    for r in range(9):
        for c in range(9):
            if board[r][c] == 0:
                return (r, c)
    return None

def is_valid(board, row, col, digit):
    if digit in board[row]:                                  # sətir
        return False
    if digit in [board[r][col] for r in range(9)]:           # sütun
        return False
    br, bc = 3 * (row // 3), 3 * (col // 3)                   # 3×3 qutu
    for r in range(br, br + 3):
        for c in range(bc, bc + 3):
            if board[r][c] == digit:
                return False
    return True
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(9^m) (m = boş xana sayı, ən pis) |
| Yaddaş | O(1) əlavə (lövhə yerində dəyişir) |

> Məhdudiyyət yayılması (məs. ən az seçimli xananı seç — MRV heuristikası) ilə əksər tapmacalar millisaniyələrlə həll olunur.

## Nə vaxt
- ✅ Sətir/sütun/region məhdudiyyəti ilə grid doldurmaq.
- ✅ Bütün düzgün həlləri tapmaq (erkən qayıdışı çıxar, hamısını topla).
- ❌ Milyonlarla xanalı çox mürəkkəb lövhələr — ixtisaslaşmış həlledicilər (Dancing Links).

## Əlaqəli
- [N-Queens](NQueens.md) — eyni "sına → yoxla → geri al" nümunəsi.
- [Hamiltonian Cycle](HamiltonianCycle.md) — həm də məhdudiyyətli tam backtracking.
- [Word Break](WordBreak.md) — həm də "hər variantı sına, alınmasa geri al".
