# N-Queens Problem 👑

---

## 🧠 Intuition

Imagine placing queens one by one on a chessboard, row by row. At each row you **try** a column, **check** whether the queen conflicts with any previously placed queen (same column or diagonal), and if it does you **undo** the placement and move to the next column. If you exhaust all columns in a row, you backtrack to the previous row and shift that queen.

**Mental model:** "Place queen in this column → safe? recurse down → conflict found? remove and try next column."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time   | O(N!) worst case |
| Space  | O(N) for the call stack and tracking sets |

> Backtracking complexity is worst-case because every branch of the decision tree is explored before pruning kicks in. In practice, conflict checks eliminate most branches early, making average performance much better than O(N!).

---

## ⚙️ How It Works

1. **Choose:** For the current row, pick a column to place a queen.
2. **Constrain:** Check that no previously placed queen shares the same column, the same top-left-to-bottom-right diagonal (`row - col`), or the same top-right-to-bottom-left diagonal (`row + col`).
3. **Place:** If the position is safe, record the placement and add the column/diagonals to tracking sets.
4. **Recurse:** Move to the next row.
5. **Backtrack:** If no column works in the current row (or recursion returns without a solution), remove the queen's column/diagonals from tracking sets and try the next column in the previous row.
6. **Base case:** When all N rows have a queen placed, record the board as a valid solution.

---

## 🔢 Step-by-Step Trace

Using N=4 as example. Each step shows `(row, col)` being tried.

```
Row 0: try col 0 → place Q at (0,0)
  Row 1: try col 0 → conflict (same col) ✗
         try col 1 → conflict (diagonal) ✗
         try col 2 → place Q at (1,2)
    Row 2: try col 0 → conflict (diagonal) ✗
           try col 1 → conflict (diagonal) ✗
           try col 2 → conflict (same col) ✗
           try col 3 → conflict (diagonal) ✗
           ← BACKTRACK to row 1, remove (1,2)
         try col 3 → place Q at (1,3)
    Row 2: try col 0 → conflict (diagonal) ✗
           try col 1 → place Q at (2,1)
      Row 3: try col 0 → conflict (diagonal) ✗
             try col 1 → conflict (same col) ✗
             try col 2 → conflict (diagonal) ✗
             try col 3 → conflict (same col) ✗
             ← BACKTRACK to row 2, remove (2,1)
           try col 2 → conflict (same col) ✗
           try col 3 → conflict (diagonal) ✗
           ← BACKTRACK to row 1, remove (1,3)
         no more columns → BACKTRACK to row 0, remove (0,0)

Row 0: try col 1 → place Q at (0,1)
  Row 1: try col 3 → place Q at (1,3)
    Row 2: try col 0 → place Q at (2,0)
      Row 3: try col 2 → place Q at (3,2) ✓
             All 4 rows filled → SOLUTION FOUND!

Board 1:
.Q..
...Q
Q...
..Q.
```

---

## 🐍 Python Implementation

```python
def solve_n_queens(n):
    solutions = []
    cols  = set()   # columns already occupied
    diag1 = set()   # (row - col) diagonals occupied  ↘
    diag2 = set()   # (row + col) diagonals occupied  ↗
    placement = [-1] * n  # placement[row] = col for queen in that row

    def backtrack(row):
        # Base case: all rows have a queen — record solution
        if row == n:
            board = ['.' * c + 'Q' + '.' * (n - c - 1) for c in placement]
            solutions.append(board)
            return

        for col in range(n):
            # Check: would placing a queen here cause a conflict?
            if col in cols or (row - col) in diag1 or (row + col) in diag2:
                continue   # skip — not safe

            # Try: place the queen
            placement[row] = col
            cols.add(col)
            diag1.add(row - col)
            diag2.add(row + col)

            backtrack(row + 1)   # recurse to the next row

            # Undo: remove the queen (backtrack)
            cols.remove(col)
            diag1.remove(row - col)
            diag2.remove(row + col)

    backtrack(0)
    return solutions


# --- Runnable example ---
solutions = solve_n_queens(4)
print(f"Number of solutions for N=4: {len(solutions)}")  # 2
print("\nFirst solution:")
for row in solutions[0]:
    print(row)
# .Q..
# ...Q
# Q...
# ..Q.
```

---

## 🎯 Recognize This Problem When...

- The task is to **place items** on a grid or sequence with mutual **conflict constraints**
- You need to **find all valid arrangements**, not just one
- The constraint check for each position depends on **previously placed elements**
- Brute force would mean trying all N^N combinations — you need **pruning**
- Keywords: "no two share a row/column/diagonal", "arrangement", "constraint satisfaction"

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Finding all solutions for small N (≤ ~15) | ✅ Backtracking is efficient with pruning |
| Teaching constraint satisfaction | ✅ Classic, well-understood example |
| Counting solutions only | ✅ Works, but DP approaches exist for counting |
| Large N (≥ 20) needing all solutions | ❌ Exponential blowup — use heuristics (min-conflicts) |
| Just checking existence for large N | ❌ Use constructive algorithms that run in O(N) |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Sudoku Solver](./SudokuSolver.md) | Same row/column/box constraint pattern |
| [Hamiltonian Cycle](./HamiltonianCycle.md) | Also enumerates valid paths under conflict constraints |
| [Subset Sum (Backtracking)](./SubsetSumBacktracking.md) | Same "try → check → undo" structure applied to numbers |
| [Knight's Tour](./KnightsTour.md) | Backtracking on a chessboard with movement constraints |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [N-Queens](https://leetcode.com/problems/n-queens/) | LeetCode | 🔴 Hard |
| [N-Queens II](https://leetcode.com/problems/n-queens-ii/) | LeetCode | 🔴 Hard |
| [Queens That Can Attack the King](https://leetcode.com/problems/queens-that-can-attack-the-king/) | LeetCode | 🟡 Medium |
