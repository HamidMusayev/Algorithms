### **Sudoku Solver 🔄**

Fill a 9×9 Sudoku board so that each row, column, and 3×3 sub-box contains digits 1–9 exactly once. Classic backtracking.

---

#### **Summary**
- **Time Complexity:** worst case exponential; typically fast with pruning
- **Space Complexity:** O(1) extra (board mutated in place)

---

#### **How It Works**
1. Find the next empty cell.
2. Try digits 1–9; check if valid for row, col, and box.
3. If valid → place and recurse.
4. If recursion fails → undo and try next digit.
5. If no digit works → return False (trigger backtrack).

---

#### **Python Implementation**
```python
def solve_sudoku(board):
    def is_valid(r, c, d):
        for i in range(9):
            if board[r][i] == d or board[i][c] == d: return False
        br, bc = 3 * (r // 3), 3 * (c // 3)
        for i in range(br, br + 3):
            for j in range(bc, bc + 3):
                if board[i][j] == d: return False
        return True

    def backtrack():
        for r in range(9):
            for c in range(9):
                if board[r][c] == 0:
                    for d in range(1, 10):
                        if is_valid(r, c, d):
                            board[r][c] = d
                            if backtrack(): return True
                            board[r][c] = 0
                    return False
        return True

    backtrack()
    return board

# Example (0 = empty)
puzzle = [
    [5,3,0, 0,7,0, 0,0,0],
    [6,0,0, 1,9,5, 0,0,0],
    [0,9,8, 0,0,0, 0,6,0],
    [8,0,0, 0,6,0, 0,0,3],
    [4,0,0, 8,0,3, 0,0,1],
    [7,0,0, 0,2,0, 0,0,6],
    [0,6,0, 0,0,0, 2,8,0],
    [0,0,0, 4,1,9, 0,0,5],
    [0,0,0, 0,8,0, 0,7,9],
]
for row in solve_sudoku(puzzle): print(row)
```

---

#### **When to Use**
✅ Constraint propagation + backtracking demonstration
✅ Pruning techniques (e.g., MRV heuristic) speed it up dramatically
