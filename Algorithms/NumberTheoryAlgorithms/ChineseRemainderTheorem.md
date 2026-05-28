### **Chinese Remainder Theorem (CRT) 🔢**

Solves a system of simultaneous congruences with **pairwise coprime moduli**:
```
x ≡ a₁ (mod n₁)
x ≡ a₂ (mod n₂)
...
x ≡ aₖ (mod nₖ)
```
There is a unique solution modulo `N = n₁·n₂·…·nₖ`.

---

#### **Summary**
- **Time Complexity:** O(k log N) (k congruences)
- **Space Complexity:** O(1)
- **Requires:** moduli pairwise coprime.

---

#### **Formula**
```
x = Σ aᵢ · Mᵢ · yᵢ (mod N)
where Mᵢ = N / nᵢ  and  yᵢ = Mᵢ⁻¹ (mod nᵢ)
```

---

#### **Python Implementation**
```python
from math import gcd

def extended_gcd(a, b):
    if b == 0: return a, 1, 0
    g, x1, y1 = extended_gcd(b, a % b)
    return g, y1, x1 - (a // b) * y1

def mod_inverse(a, m):
    g, x, _ = extended_gcd(a % m, m)
    if g != 1:
        raise ValueError("inverse doesn't exist")
    return x % m

def crt(remainders, moduli):
    N = 1
    for n in moduli: N *= n
    x = 0
    for a, n in zip(remainders, moduli):
        M = N // n
        y = mod_inverse(M, n)
        x = (x + a * M * y) % N
    return x

# Example: x ≡ 2 (mod 3), x ≡ 3 (mod 5), x ≡ 2 (mod 7) → 23
print(crt([2, 3, 2], [3, 5, 7]))   # 23
```

---

#### **When to Use**
✅ Cryptography (RSA optimization)
✅ Reconstruction from residues
✅ Competitive programming with large modular problems
