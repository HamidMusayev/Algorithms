### **Monte Carlo & Las Vegas Algorithms 🛠**

Two flavors of **randomized algorithms**:

| | Monte Carlo | Las Vegas |
|---|-------------|-----------|
| **Output correctness** | May be wrong (bounded error) | Always correct |
| **Running time** | Deterministic / fixed | Random (expected) |
| **Example** | Miller-Rabin primality | Randomized QuickSort |
| **Use when** | OK with rare errors for speed | Must be correct, OK with variable time |

---

#### **Python — Monte Carlo (Estimate π)**
```python
import random

def estimate_pi(samples=1_000_000):
    inside = 0
    for _ in range(samples):
        x, y = random.random(), random.random()
        if x*x + y*y <= 1:
            inside += 1
    return 4 * inside / samples

print(estimate_pi())   # ~3.1415
```

#### **Python — Las Vegas (Randomized QuickSort)**
```python
import random

def lv_quicksort(arr):
    if len(arr) <= 1:
        return arr
    pivot = random.choice(arr)
    return (lv_quicksort([x for x in arr if x < pivot])
            + [x for x in arr if x == pivot]
            + lv_quicksort([x for x in arr if x > pivot]))

print(lv_quicksort([5, 2, 8, 1, 9, 3]))   # [1, 2, 3, 5, 8, 9]
```

Random pivots make worst-case O(n²) **extremely unlikely**, while the result is always correct.

---

#### **When to Use**
- **Monte Carlo:** approximations, primality, ML sampling
- **Las Vegas:** quicksort, randomized hashing, geometric searches
