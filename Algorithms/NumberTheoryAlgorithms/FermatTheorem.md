### **Fermat's Little Theorem 🔢**

If `p` is a prime and `a` is an integer not divisible by `p`, then:
```
a^(p-1) ≡ 1 (mod p)
```
Equivalently: `a^p ≡ a (mod p)` for all integers `a`.

---

#### **Uses**
1. **Modular inverse** when `p` is prime:
   `a⁻¹ ≡ a^(p-2) (mod p)`
2. **Probabilistic primality test** (Fermat test): pick random `a` and check
   `a^(n-1) ≡ 1 (mod n)`. Fails on **Carmichael numbers** → use Miller-Rabin instead.

---

#### **Python Implementation**
```python
def mod_inverse_fermat(a, p):
    # works only when p is prime and gcd(a, p) == 1
    return pow(a, p - 2, p)

def fermat_test(n, k=5):
    """Probabilistic test. Returns False for composites (except Carmichael)."""
    import random
    if n < 2: return False
    if n in (2, 3): return True
    if n % 2 == 0: return False
    for _ in range(k):
        a = random.randrange(2, n - 1)
        if pow(a, n - 1, n) != 1:
            return False
    return True

# Example
print(mod_inverse_fermat(3, 11))   # 4   (3*4 = 12 ≡ 1 mod 11)
print(fermat_test(17))             # True
print(fermat_test(15))             # False
```

> ⚠️ Fermat test fools **Carmichael numbers** (e.g., 561). For real primality testing, prefer **Miller-Rabin**.

---

#### **When to Use**
✅ Quick modular inverse with prime modulus
✅ Background theory for crypto (RSA)
🚫 Reliable primality → use Miller-Rabin
