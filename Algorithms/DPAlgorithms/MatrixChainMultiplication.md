### **Matrix Chain Multiplication 📈**

Given dimensions of `n` matrices, find the **optimal parenthesization** that minimizes the total scalar multiplications.

If matrix `Aᵢ` has dimensions `pᵢ₋₁ × pᵢ`, the cost of multiplying `Aᵢ Aⱼ` (after both are computed) depends on parenthesization.

---

#### **Summary**
- **Time Complexity:** O(n³)
- **Space Complexity:** O(n²)

---

#### **Recurrence**
For chain `i…j`, try every split point `k`:
```
dp[i][j] = min over k of:  dp[i][k] + dp[k+1][j] + p[i-1] * p[k] * p[j]
```

---

#### **Python Implementation**
```python
def matrix_chain_order(p):
    # p[i-1] x p[i] = dims of matrix i  -> n = len(p) - 1 matrices
    n = len(p) - 1
    dp = [[0] * (n + 1) for _ in range(n + 1)]

    for length in range(2, n + 1):                # chain length
        for i in range(1, n - length + 2):
            j = i + length - 1
            dp[i][j] = float('inf')
            for k in range(i, j):
                cost = dp[i][k] + dp[k + 1][j] + p[i - 1] * p[k] * p[j]
                if cost < dp[i][j]:
                    dp[i][j] = cost
    return dp[1][n]

# Example: matrices A1(10x30), A2(30x5), A3(5x60)
print(matrix_chain_order([10, 30, 5, 60]))   # 4500
```

---

#### **When to Use**
✅ Classic interval DP example
✅ Compiler optimization for chained operations
✅ Generalization → optimal BST, polygon triangulation
