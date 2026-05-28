### **Modular Exponentiation 🔢**

Computes `(base^exp) mod m` efficiently in **O(log exp)** using **binary exponentiation**. Essential for cryptography, hashing, and competitive programming.

Naive computation of `base^exp` overflows; we keep numbers small by taking `mod m` at every step.

---

#### **Summary**
- **Time Complexity:** O(log exp)
- **Space Complexity:** O(1) iterative

---

#### **How It Works**
Repeated squaring: `base^exp` can be built from the binary representation of `exp`.
```
exp = 13 = 1101₂
base^13 = base^8 · base^4 · base^1
```

---

#### **Python Implementation**
```python
def mod_pow(base, exp, m):
    result = 1
    base %= m
    while exp > 0:
        if exp & 1:
            result = (result * base) % m
        base = (base * base) % m
        exp >>= 1
    return result

# Example
print(mod_pow(2, 10, 1000))     # 24
print(mod_pow(7, 256, 13))      # 9
```

> Python's built-in `pow(base, exp, m)` does the same thing — and is implemented in C. Prefer it in real code.

---

#### **When to Use**
✅ RSA, Diffie-Hellman, and crypto
✅ Computing modular inverses via Fermat: `a^(-1) ≡ a^(p-2) mod p` (when `p` prime)
✅ Hashing (Rabin-Karp), combinatorics with large primes
