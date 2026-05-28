### **N-Queens Problem 🔄**

Place `N` queens on an `N×N` board so that **no two attack each other** (no shared row, column, or diagonal). Classic backtracking.

---

#### **Summary**
- **Time Complexity:** O(N!) (worst case)
- **Space Complexity:** O(N)
- **Approach:** Place row by row; backtrack on conflict.

---

#### **Python Implementation**
```python
def solve_n_queens(n):
    solutions = []
    cols, diag1, diag2 = set(), set(), set()
    placement = [-1] * n

    def backtrack(row):
        if row == n:
            board = ['.' * c + 'Q' + '.' * (n - c - 1) for c in placement]
            solutions.append(board)
            return
        for col in range(n):
            if col in cols or (row - col) in diag1 or (row + col) in diag2:
                continue
            placement[row] = col
            cols.add(col); diag1.add(row - col); diag2.add(row + col)
            backtrack(row + 1)
            cols.remove(col); diag1.remove(row - col); diag2.remove(row + col)

    backtrack(0)
    return solutions

# Example
sols = solve_n_queens(4)
print(len(sols))             # 2
for row in sols[0]: print(row)
# .Q..
# ...Q
# Q...
# ..Q.
```

---

#### **When to Use**
✅ Classic constraint-satisfaction example
✅ Great teaching tool for pruning / backtracking
🚫 Pure search problems for large `N` need smarter methods (e.g., min-conflicts heuristic)
