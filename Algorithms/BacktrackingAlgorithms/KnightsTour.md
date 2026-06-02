# Knight's Tour Problem ♞

---

## 🧠 Intuition

A chess knight must visit every square of an N×N board exactly once, moving in its characteristic L-shape. The challenge: find a sequence of moves that covers all N² squares without revisiting any.

**Mental model:** "Try an L-shaped move → if the target is valid and unvisited → go there and recurse → if we hit a dead end → undo the move and try the next direction."

The key optimization — **Warnsdorff's rule** — always move to the square with the **fewest onward moves**. This heuristic avoids dead ends dramatically and turns an exponential search into a near-linear one for large boards.

---

## 📊 Complexity

| Approach | Time | Space |
|----------|------|-------|
| Plain backtracking | O(8^(N²)) worst case | O(N²) |
| With Warnsdorff's heuristic | ~O(N²) in practice | O(N²) |

---

## ⚙️ How It Works

**Backtracking:**
1. Start at a given cell `(r, c)`, mark it as step 0.
2. For each of the 8 possible L-shaped moves:
   - Compute the target `(nr, nc)`.
   - If `(nr, nc)` is within bounds and unvisited:
     - **Place:** mark `board[nr][nc] = step`.
     - **Recurse:** call `backtrack(nr, nc, step + 1)`.
     - If recursion succeeds → done!
     - **Undo:** reset `board[nr][nc] = -1`.
3. If all N² squares are filled → success!

**Warnsdorff's heuristic:** Sort the 8 moves by how many moves are accessible from the destination (fewest first). This dramatically reduces backtracking.

---

## 🔢 Step-by-Step Trace

5×5 board, starting at (0,0):

The knight visits all 25 squares. One valid sequence (numbered by visit order):
```
 0  11  22   3  16
21   4  17  12  23
10   1  24  15   2
 5  20  13  18   9
14   7   8  19   6
```

Each number = the step at which that square was visited. No square is visited twice. ✅

---

## 🐍 Python Implementation

```python
def knights_tour(n, start=(0, 0)):
    """
    Find a Knight's Tour on an n×n board starting at 'start'.
    Returns the board with step numbers, or None if no tour exists.
    """
    # All 8 possible L-shaped knight moves
    moves = [(2, 1), (1, 2), (-1, 2), (-2, 1),
             (-2, -1), (-1, -2), (1, -2), (2, -1)]

    board = [[-1] * n for _ in range(n)]
    r, c = start
    board[r][c] = 0   # Mark starting square as step 0

    def count_onward_moves(r, c):
        """Count how many valid moves exist from (r, c) — used by Warnsdorff's."""
        return sum(
            1 for dr, dc in moves
            if 0 <= r + dr < n and 0 <= c + dc < n and board[r + dr][c + dc] == -1
        )

    def backtrack(r, c, step):
        if step == n * n:
            return True   # All squares visited!

        # Warnsdorff's heuristic: sort moves by number of onward options (fewest first)
        next_moves = []
        for dr, dc in moves:
            nr, nc = r + dr, c + dc
            if 0 <= nr < n and 0 <= nc < n and board[nr][nc] == -1:
                next_moves.append((count_onward_moves(nr, nc), nr, nc))
        next_moves.sort()   # Sort by fewest onward options

        for _, nr, nc in next_moves:
            board[nr][nc] = step          # Try this move
            if backtrack(nr, nc, step + 1):
                return True
            board[nr][nc] = -1            # Undo (backtrack)

        return False

    if backtrack(r, c, 1):
        return board
    return None


# Example
tour = knights_tour(5)
if tour:
    print("Knight's Tour (5×5):")
    for row in tour:
        print([f"{x:2d}" for x in row])
```

---

## 🎯 Recognize This Problem When...

- A piece/agent must **visit every cell/node exactly once** with specific movement rules.
- Keywords: "Hamiltonian path on a grid", "visit all cells", "chess knight".
- The problem has a structured graph (grid) with a complex movement pattern.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Find any valid tour (one solution) | ✅ With Warnsdorff's, very efficient |
| Enumerate all tours | ❌ Exponential without heavy pruning |
| Board size > 8×8 without heuristic | ❌ Plain backtracking too slow |
| Undirected graph Hamiltonian path | ✅ Same idea applies |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [N-Queens](NQueens.md) | Also place pieces on a chessboard with constraints |
| [Hamiltonian Cycle](HamiltonianCycle.md) | Knight's tour = Hamiltonian path on the "knight graph" |
| [DFS](../GraphAlgorithms/DFS.md) | Backtracking IS DFS with undo |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| Knight's Tour | [HackerEarth](https://www.hackerearth.com/practice/algorithms/graphs/hamiltonian-path/tutorial/) | 🔴 Hard |
| [Minimum Knight Moves](https://leetcode.com/problems/minimum-knight-moves/) | LeetCode | 🟡 Medium |
