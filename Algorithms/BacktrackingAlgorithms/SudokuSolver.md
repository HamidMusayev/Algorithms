# Sudoku Solver 🔢

---

## 🧠 Intuition

Sudoku is a constraint satisfaction problem: fill a 9×9 grid with digits 1–9 so every row, column, and 3×3 box contains each digit exactly once. The backtracking approach: find an empty cell, **try** digits 1–9, **check** if the digit is valid (no conflict in row/col/box), **recurse**, and if we reach a contradiction, **undo** and try the next digit.

**Mental model:** "Try a digit → check constraints → recurse deeper → if stuck, erase and try the next digit."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(9^m) worst case, where m = number of empty cells |
| Space Complexity | O(1) extra (board mutated in-place) |

> With constraint propagation (e.g., MRV heuristic — pick the cell with fewest valid choices), most puzzles solve in milliseconds.

---

## ⚙️ How It Works

1. **Find** the next empty cell (value = 0).
2. For each digit `d` from 1 to 9:
   - **Check** validity: is `d` absent from this row, column, and 3×3 box?
   - If valid: **place** `d` in the cell.
   - **Recurse** to the next empty cell.
   - If recursion returns True → puzzle solved, propagate success up.
   - If recursion returns False → **undo** (backtrack): reset cell to 0.
3. If no digit works → return False (trigger backtrack in caller).
4. If no empty cells remain → puzzle is solved, return True.

---

## 🔢 Step-by-Step Trace

Simplified 4×4 Sudoku (2×2 boxes), target: fill the `?` cells:

```
Initial:        After placing 1 at (0,0):   Conflict! Try 2:
? 2 | ? ?        1 2 | ? ?                   2 1 | ? ?
3 ? | ? 1        3 ? | ? 1                   3 ? | ? 1
----+----        ----+----                   ----+----
? ? | 2 ?        ? ? | 2 ?                   ...
? 1 | ? ?        ? 1 | ? ?
```

The algorithm tries each digit, backtracks when stuck, until the grid is complete.

---

## 🐍 Python Implementation

```python
def solve_sudoku(board):
    """
    Solves a 9×9 Sudoku puzzle in-place.
    board: 9×9 list of lists, 0 = empty cell.
    Returns True if solved, False if unsolvable.
    """
    empty = find_empty(board)
    if not empty:
        return True       # No empty cells → solved!
    row, col = empty

    for digit in range(1, 10):
        if is_valid(board, row, col, digit):
            board[row][col] = digit          # Try placing digit

            if solve_sudoku(board):          # Recurse to fill next cell
                return True

            board[row][col] = 0             # Undo (backtrack)

    return False   # No digit worked — trigger backtrack in caller


def find_empty(board):
    """Find the next empty cell (value = 0). Returns (row, col) or None."""
    for r in range(9):
        for c in range(9):
            if board[r][c] == 0:
                return (r, c)
    return None


def is_valid(board, row, col, digit):
    """Check if placing 'digit' at (row, col) violates any Sudoku rule."""
    # Check row
    if digit in board[row]:
        return False
    # Check column
    if digit in [board[r][col] for r in range(9)]:
        return False
    # Check 3×3 box
    box_row, box_col = 3 * (row // 3), 3 * (col // 3)
    for r in range(box_row, box_row + 3):
        for c in range(box_col, box_col + 3):
            if board[r][c] == digit:
                return False
    return True


# Example puzzle (0 = empty)
puzzle = [
    [5, 3, 0,  0, 7, 0,  0, 0, 0],
    [6, 0, 0,  1, 9, 5,  0, 0, 0],
    [0, 9, 8,  0, 0, 0,  0, 6, 0],
    [8, 0, 0,  0, 6, 0,  0, 0, 3],
    [4, 0, 0,  8, 0, 3,  0, 0, 1],
    [7, 0, 0,  0, 2, 0,  0, 0, 6],
    [0, 6, 0,  0, 0, 0,  2, 8, 0],
    [0, 0, 0,  4, 1, 9,  0, 0, 5],
    [0, 0, 0,  0, 8, 0,  0, 7, 9],
]
solve_sudoku(puzzle)
for row in puzzle:
    print(row)
```

---

## 🎯 Recognize This Problem When...

- You need to fill a grid satisfying **row/column/region constraints**.
- The problem involves placing items with **no conflicts** in a structured grid.
- Keywords: "constraint satisfaction", "fill the grid", "valid placement".
- Each decision (digit placement) has immediate constraint implications.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Sudoku and similar constraint grid puzzles | ✅ Classic backtracking |
| Constraint propagation teaching (MRV, AC-3) | ✅ Good base to extend |
| Finding all valid Sudoku solutions | ✅ Remove early return, collect all |
| Very complex boards with millions of cells | ❌ Need specialized solvers (Dancing Links) |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [N-Queens](NQueens.md) | Same "try → check constraints → backtrack" pattern |
| [Hamiltonian Cycle](HamiltonianCycle.md) | Also exhaustive constraint backtracking |
| [Word Break](WordBreak.md) | Also "try each option, backtrack if fails" |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Sudoku Solver](https://leetcode.com/problems/sudoku-solver/) | LeetCode | 🔴 Hard |
| [Valid Sudoku](https://leetcode.com/problems/valid-sudoku/) | LeetCode | 🟡 Medium |
