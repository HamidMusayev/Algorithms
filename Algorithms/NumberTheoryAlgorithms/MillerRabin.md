### **Miller-Rabin Primality Test 🔢**

A **probabilistic** test for primality, far stronger than Fermat — there are **no Carmichael-like fooling numbers**. With the right witness set, it becomes **deterministic** for all 64-bit integers.

---

#### **Summary**
- **Time Complexity:** O(k · log³ n) for `k` rounds
- **Space Complexity:** O(1)
- **Deterministic witnesses for n < 3,317,044,064,679,887,385,961,981:**
  `[2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37]`

---

#### **How It Works**
Write `n - 1 = 2ʳ · d` (d odd). For a witness `a`, `n` is **composite** unless:
- `a^d ≡ 1 (mod n)`, or
- `a^(2ⁱ·d) ≡ −1 (mod n)` for some `0 ≤ i < r`.

---

#### **Python Implementation**
```python
def miller_rabin(n, witnesses=(2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37)):
    if n < 2: return False
    for p in (2, 3, 5, 7, 11, 13):
        if n == p: return True
        if n % p == 0: return False

    # write n-1 = 2^r * d
    d, r = n - 1, 0
    while d % 2 == 0:
        d //= 2
        r += 1

    for a in witnesses:
        if a % n == 0: continue
        x = pow(a, d, n)
        if x == 1 or x == n - 1:
            continue
        for _ in range(r - 1):
            x = (x * x) % n
            if x == n - 1:
                break
        else:
            return False
    return True

# Example
print(miller_rabin(561))                  # False (Carmichael — Fermat would fail)
print(miller_rabin(1_000_000_007))        # True
```

---

#### **When to Use**
✅ Primality of large numbers (crypto)
✅ Anywhere Fermat fails
✅ Deterministic for 64-bit ints with the witness set above
