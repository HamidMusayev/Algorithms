### **Sieve of Eratosthenes 🔢**

Generates all primes up to `n` efficiently by **iteratively marking** the multiples of each prime as composite.

---

#### **Summary**
- **Time Complexity:** O(n log log n)
- **Space Complexity:** O(n)

A **segmented sieve** can extend this to very large ranges.

---

#### **Python Implementation**
```python
def sieve(n):
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(n ** 0.5) + 1):
        if is_prime[i]:
            for j in range(i * i, n + 1, i):
                is_prime[j] = False
    return [i for i, p in enumerate(is_prime) if p]

# Example
print(sieve(30))   # [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]
```

> **Linear sieve** (O(n)) also computes the smallest prime factor of each number, useful for factorization queries.

---

#### **When to Use**
✅ Generating many primes up to a bound
✅ Pre-computation for competitive programming
🚫 Single primality test on a large number → use Miller-Rabin
