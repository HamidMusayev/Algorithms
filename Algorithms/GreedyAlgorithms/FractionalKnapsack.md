### **Fractional Knapsack 💰**

Like 0/1 Knapsack, but you may take **fractions** of items. Solvable optimally with **greedy** by **value-per-weight** ratio (unlike 0/1 which needs DP).

---

#### **Summary**
- **Time Complexity:** O(n log n) (sorting by ratio)
- **Space Complexity:** O(1) extra
- **Greedy choice:** highest value/weight first; take as much as fits.

---

#### **Python Implementation**
```python
def fractional_knapsack(weights, values, capacity):
    items = sorted(zip(weights, values), key=lambda x: x[1] / x[0], reverse=True)
    total_value = 0.0
    for w, v in items:
        if capacity >= w:
            total_value += v
            capacity -= w
        else:
            total_value += v * (capacity / w)
            break
    return total_value

# Example
weights = [10, 20, 30]
values  = [60, 100, 120]
print(fractional_knapsack(weights, values, 50))   # 240.0
```

---

#### **When to Use**
✅ Divisible resources (liquids, time, money)
✅ When fractions are physically meaningful
🚫 Discrete items only → use 0/1 Knapsack (DP)
