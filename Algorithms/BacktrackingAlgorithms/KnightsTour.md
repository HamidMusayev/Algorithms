# Knight's Tour (Atın Gəzintisi)

**Fikir:** Şahmat atı N×N lövhənin hər xanasını dəqiq bir dəfə, öz "L" gedişi ilə ziyarət etməlidir. Backtracking: "L" gedişi sına → hədəf düzgün və ziyarət olunmayıbsa → ora get və rekursiya et → dalana dirənəndə gedişi geri al. Açar optimizasiya — **Warnsdorff qaydası**: həmişə **ən az sonrakı gedişi olan** xanaya keç. Bu heuristika dalanları azaldır və böyük lövhələrdə axtarışı demək olar xəttiyə çevirir.

## Necə işləyir
1. Verilmiş `(r, c)` xanasından başla, addım 0 işarələ.
2. 8 mümkün "L" gedişinin hər biri üçün:
   - Hədəf `(nr, nc)`-ni hesabla.
   - Lövhə daxilində və ziyarət olunmayıbsa:
     - **Yerləşdir:** `board[nr][nc] = addım`.
     - **Rekursiya:** `backtrack(nr, nc, addım + 1)`.
     - Uğurlu olsa → bitdi.
     - **Geri al:** `board[nr][nc] = -1`.
3. Bütün N² xana dolanda → uğur!

## Kod
```python
def knights_tour(n, start=(0, 0)):
    moves = [(2,1),(1,2),(-1,2),(-2,1),(-2,-1),(-1,-2),(1,-2),(2,-1)]   # 8 "L" gedişi
    board = [[-1] * n for _ in range(n)]
    r, c = start
    board[r][c] = 0

    def onward(r, c):                        # (r,c)-dən neçə düzgün gediş var?
        return sum(1 for dr, dc in moves
                   if 0 <= r+dr < n and 0 <= c+dc < n and board[r+dr][c+dc] == -1)

    def backtrack(r, c, step):
        if step == n * n:
            return True                      # bütün xanalar ziyarət olundu
        # Warnsdorff: gedişləri sonrakı seçim sayına görə sırala (ən az əvvəl)
        nexts = sorted((onward(r+dr, c+dc), r+dr, c+dc)
                       for dr, dc in moves
                       if 0 <= r+dr < n and 0 <= c+dc < n and board[r+dr][c+dc] == -1)
        for _, nr, nc in nexts:
            board[nr][nc] = step
            if backtrack(nr, nc, step + 1):
                return True
            board[nr][nc] = -1               # geri al
        return False

    return board if backtrack(r, c, 1) else None
```

## Mürəkkəblik
| Yanaşma | Vaxt | Yaddaş |
|---------|------|--------|
| Sadə backtracking | O(8^(N²)) (ən pis) | O(N²) |
| Warnsdorff heuristikası | ~O(N²) praktikada | O(N²) |

## Nə vaxt
- ✅ Bir fiqur/agent xüsusi gediş qaydaları ilə hər xananı dəqiq bir dəfə ziyarət etməlidir.
- ✅ Hər hansı bir düzgün marşrut (Warnsdorff ilə çox səmərəli).
- ❌ Bütün marşrutları sadalamaq — ağır kəsmə olmadan eksponensial.
- ❌ 8×8-dən böyük lövhə heuristikasız — sadə backtracking yavaşdır.

## Əlaqəli
- [Hamiltonian Cycle](HamiltonianCycle.md) — Knight's Tour "at qrafında" Hamilton yoludur.
- [N-Queens](NQueens.md) — həm də lövhəyə məhdudiyyətlə fiqur yerləşdirmə.
- [DFS](../GraphAlgorithms/DFS.md) — backtracking geri-almalı DFS-dir.
