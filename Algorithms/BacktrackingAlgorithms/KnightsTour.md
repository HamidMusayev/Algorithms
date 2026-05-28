### **Knight's Tour Problem 🔄**

A knight on an `N×N` chessboard must visit **every square exactly once**, moving in L-shapes. Backtracking with optional Warnsdorff's heuristic for speed.

---

#### **Summary**
- **Time Complexity:** exponential without heuristic; near-linear with Warnsdorff's
- **Space Complexity:** O(N²)

---

#### **Python Implementation (Plain Backtracking)**
```python
def knights_tour(n, start=(0, 0)):
    moves = [(2,1), (1,2), (-1,2), (-2,1),
             (-2,-1), (-1,-2), (1,-2), (2,-1)]
    board = [[-1] * n for _ in range(n)]
    r, c = start
    board[r][c] = 0

    def backtrack(r, c, step):
        if step == n * n:
            return True
        for dr, dc in moves:
            nr, nc = r + dr, c + dc
            if 0 <= nr < n and 0 <= nc < n and board[nr][nc] == -1:
                board[nr][nc] = step
                if backtrack(nr, nc, step + 1):
                    return True
                board[nr][nc] = -1
        return False

    if backtrack(r, c, 1):
        return board
    return None

# Example (small board for speed)
tour = knights_tour(5)
for row in tour: print(row)
```

> **Warnsdorff's rule** (heuristic): always move to the square with the fewest onward moves. Reduces dead-ends massively for larger N.

---

#### **When to Use**
✅ Constraint-search teaching example
✅ Heuristic backtracking (Warnsdorff's)
🚫 Plain backtracking is too slow for N ≥ 8 without pruning
