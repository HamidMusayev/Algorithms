### **Fibonacci (DP Approach) 📈**

The Fibonacci sequence: `F(0)=0, F(1)=1, F(n)=F(n-1)+F(n-2)`.

The naive recursive solution is **O(2ⁿ)**; DP brings it down to **O(n)** (memoization or tabulation).

---

#### **Summary**
| Approach | Time | Space |
|----------|------|-------|
| Naive recursion | O(2ⁿ) | O(n) stack |
| Memoization | O(n) | O(n) |
| Tabulation | O(n) | O(n) |
| Tabulation + 2 vars | O(n) | **O(1)** |

---

#### **Python Implementations**

**1. Memoization (Top-Down)**
```python
def fib_memo(n, memo=None):
    if memo is None: memo = {}
    if n < 2: return n
    if n in memo: return memo[n]
    memo[n] = fib_memo(n - 1, memo) + fib_memo(n - 2, memo)
    return memo[n]
```

**2. Tabulation (Bottom-Up)**
```python
def fib_tab(n):
    if n < 2: return n
    dp = [0] * (n + 1)
    dp[1] = 1
    for i in range(2, n + 1):
        dp[i] = dp[i - 1] + dp[i - 2]
    return dp[n]
```

**3. Space-Optimized**
```python
def fib(n):
    a, b = 0, 1
    for _ in range(n):
        a, b = b, a + b
    return a

print(fib(10))   # 55
```

---

#### **Key Idea**
DP avoids recomputing the same subproblems. Fibonacci is the textbook intro because subproblems overlap heavily.
