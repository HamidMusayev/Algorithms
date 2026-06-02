# Fibonacci Sequence 🌀

---

## 🧠 Intuition

Imagine you are counting the number of ways to climb a staircase where you can take 1 or 2 steps at a time. To figure out how many ways you can reach step `n`, you just need to know how many ways reach step `n-1` and step `n-2` — because those are the only two places you could have stepped from. These two smaller answers are the **overlapping subproblems**.

In naive recursion, `fib(5)` calls `fib(4)` and `fib(3)`, but `fib(4)` also calls `fib(3)` — so `fib(3)` is computed twice. As `n` grows, the redundancy explodes to O(2ⁿ). Dynamic programming caches each result so every subproblem is solved exactly once.

**Mental model:** "If I already know the answer for smaller inputs, I can build the answer for larger inputs for free."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time — Naive recursion | O(2ⁿ) |
| Time — Memoization / Tabulation | O(n) |
| Space — Memoization | O(n) (cache + call stack) |
| Space — Tabulation | O(n) (dp array) |
| Space — Optimized (2 variables) | **O(1)** |

---

## ⚙️ How It Works

1. **Base cases:** `F(0) = 0`, `F(1) = 1`
2. **Recurrence relation:**
   ```
   F(n) = F(n-1) + F(n-2)   for n >= 2
   ```
3. **Top-down (memoization):** Recurse as normal, but store each computed value in a dictionary before returning. On a repeat call, return the cached value immediately.
4. **Bottom-up (tabulation):** Fill a dp array left to right. `dp[i] = dp[i-1] + dp[i-2]`. No recursion at all.
5. **Space optimization:** Because `dp[i]` only depends on the previous two values, you only need two variables — discard the rest of the array.

---

## 🔢 Step-by-Step Trace

Computing `F(7)` with tabulation:

| Index | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|-------|---|---|---|---|---|---|---|---|
| dp    | 0 | 1 | 1 | 2 | 3 | 5 | 8 | **13** |

- `dp[2] = dp[1] + dp[0] = 1 + 0 = 1`
- `dp[3] = dp[2] + dp[1] = 1 + 1 = 2`
- `dp[4] = dp[3] + dp[2] = 2 + 1 = 3`
- `dp[5] = dp[4] + dp[3] = 3 + 2 = 5`
- `dp[6] = dp[5] + dp[4] = 5 + 3 = 8`
- `dp[7] = dp[6] + dp[5] = 8 + 5 = 13` ✓

---

## 🐍 Python Implementation

```python
# ── Approach 1: Memoization (Top-Down) ──────────────────────────────────────

def fib_memo(n, memo=None):
    if memo is None:
        memo = {}               # shared cache across recursive calls
    if n < 2:
        return n                # base cases: F(0)=0, F(1)=1
    if n in memo:
        return memo[n]          # already computed — O(1) lookup
    # Recurrence: F(n) = F(n-1) + F(n-2)
    memo[n] = fib_memo(n - 1, memo) + fib_memo(n - 2, memo)
    return memo[n]


# ── Approach 2: Tabulation (Bottom-Up) ──────────────────────────────────────

def fib_tab(n):
    if n < 2:
        return n
    dp = [0] * (n + 1)         # dp[i] will hold F(i)
    dp[1] = 1                   # seed the base case
    for i in range(2, n + 1):
        dp[i] = dp[i - 1] + dp[i - 2]   # recurrence: F(n) = F(n-1) + F(n-2)
    return dp[n]


# ── Approach 3: Space-Optimized (O(1) space) ────────────────────────────────

def fib(n):
    a, b = 0, 1                 # a = F(i-2), b = F(i-1)
    for _ in range(n):
        a, b = b, a + b         # shift window forward by one step
    return a


# ── Quick test ───────────────────────────────────────────────────────────────

for i in range(8):
    print(f"F({i}) = {fib(i)}")
# F(0)=0  F(1)=1  F(2)=1  F(3)=2  F(4)=3  F(5)=5  F(6)=8  F(7)=13

print(fib(10))   # 55
```

---

## 🎯 Recognize This Problem When...

- The problem asks for the **n-th term** of a sequence where each term depends on previous terms.
- A naive recursive solution makes the **same function call multiple times** with the same argument.
- The call tree forms a **binary tree with heavily overlapping branches** (classic overlapping subproblems signal).
- The problem has a clean **base case** (small input with a known answer) and a clear **transition rule**.
- You see phrases like "number of ways", "how many paths", "how many combinations" where the count for `n` breaks into counts for smaller values.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Classic Fibonacci / staircase counting | ✅ Textbook DP — use tabulation or memoization |
| Sequence where each term depends on the last 2 | ✅ Space-optimized (2 variables) is ideal |
| Sequence where each term depends on ALL prior terms | ✅ Tabulation with full dp array |
| Only need F(n) once for very small n | ❌ Simple loop or direct formula is cleaner |
| Closed-form exists (e.g., Binet's formula) | ❌ Math formula is O(1) but loses integer precision at large n |
| Need all Fibonacci numbers up to n | ✅ Tabulation naturally gives you every value |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Coin Change](./CoinChange.md) | Same "number of ways" DP pattern; Fibonacci is a special case with coins [1,2] |
| [Climbing Stairs (LeetCode 70)](./CoinChange.md) | Literally Fibonacci in disguise |
| [Knapsack](./Knapsack.md) | Both use a 1D dp array built bottom-up from base cases |
| [LIS](./LIS.md) | Also builds dp[i] from all previous dp[j] values |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Fibonacci Number](https://leetcode.com/problems/fibonacci-number/) | LeetCode | 🟢 Easy |
| [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) | LeetCode | 🟢 Easy |
| [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/) | LeetCode | 🟢 Easy |
| [Tribonacci Number](https://leetcode.com/problems/n-th-tribonacci-number/) | LeetCode | 🟢 Easy |
| [House Robber](https://leetcode.com/problems/house-robber/) | LeetCode | 🟡 Medium |
