### **Reservoir Sampling 🛠**

Randomly pick `k` items from a **stream of unknown length** so that every item is equally likely to be chosen.

Algorithm **R** (Vitter): keep the first `k` items; for each later item at position `i ≥ k`, replace a random reservoir slot with probability `k/i`.

---

#### **Summary**
- **Time Complexity:** O(n)
- **Space Complexity:** O(k)
- **Property:** uniform random sample without knowing `n` in advance.

---

#### **Python Implementation**
```python
import random

def reservoir_sample(stream, k):
    reservoir = []
    for i, item in enumerate(stream):
        if i < k:
            reservoir.append(item)
        else:
            j = random.randrange(i + 1)   # 0..i
            if j < k:
                reservoir[j] = item
    return reservoir

# Example: pick 3 random items from a "stream"
print(reservoir_sample(range(1, 1_000_001), 3))   # e.g. [482991, 17, 750214]
```

---

#### **When to Use**
✅ Sampling from huge or unbounded streams (logs, social-media feeds)
✅ When the total size is unknown or memory is tiny
✅ Distributed sampling
