# Fractional Knapsack 🏺

---

## 🧠 Intuition

You're at a gold mine with a backpack that holds 50 kg. There are gold nuggets of different sizes and purities. Unlike the 0/1 Knapsack, you can **cut nuggets** — take 1/3 of a nugget if needed. So you always fill the bag with the most valuable material first (highest value-per-kg ratio), cutting the last item to fit perfectly.

**Mental model:** Sort items by value/weight ratio (descending). Greedily fill the bag — take full items while capacity allows, then take a fraction of the next item to fill exactly.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(n log n) — sorting dominates |
| Space Complexity | O(1) extra |
| Greedy choice | Highest value-per-unit-weight first |

---

## ⚙️ How It Works

1. Compute the **value/weight ratio** for each item.
2. **Sort items** by ratio in descending order.
3. For each item (in ratio order):
   - If it fits entirely: take it all, reduce remaining capacity.
   - If it doesn't fit: take a fraction `remaining_capacity / weight`, then stop.
4. Sum up all taken values.

**Why greedy works:** Taking the highest-ratio item first maximizes value gained per unit of capacity. You can always swap any other choice for the highest-ratio item and improve or match the result (exchange argument).

---

## 🔢 Step-by-Step Trace

Items: weights=[10, 20, 30], values=[60, 100, 120], capacity=50

| Item | Weight | Value | Ratio (v/w) |
|------|--------|-------|-------------|
| 1    | 10     | 60    | 6.0         |
| 2    | 20     | 100   | 5.0         |
| 3    | 30     | 120   | 4.0         |

Sorted by ratio: Item1 (6.0), Item2 (5.0), Item3 (4.0)

| Step | Item | Take      | Value Added    | Remaining Capacity |
|------|------|-----------|----------------|--------------------|
| 1    | 1    | All 10 kg | 60.0           | 40 kg              |
| 2    | 2    | All 20 kg | 100.0          | 20 kg              |
| 3    | 3    | 20/30 fraction | 120×(20/30)=80.0 | 0 kg          |

Total: **60 + 100 + 80 = 240.0** ✅

---

## 🐍 Python Implementation

```python
def fractional_knapsack(weights, values, capacity):
    """
    Maximize value with fractional items allowed.
    Returns (max_value, list of (item_index, fraction_taken))
    """
    n = len(weights)
    # Create items sorted by value/weight ratio (descending)
    items = sorted(range(n), key=lambda i: values[i] / weights[i], reverse=True)

    total_value = 0.0
    taken = []           # Track what fraction of each item was taken
    remaining = capacity

    for i in items:
        if remaining <= 0:
            break
        # Take as much of this item as possible
        fraction = min(1.0, remaining / weights[i])
        total_value += fraction * values[i]
        taken.append((i, fraction))
        remaining -= fraction * weights[i]

    return total_value, taken


# Example
weights = [10, 20, 30]
values  = [60, 100, 120]
capacity = 50

max_val, taken = fractional_knapsack(weights, values, capacity)
print(f"Maximum value: {max_val}")   # 240.0
for idx, frac in taken:
    print(f"  Item {idx+1}: took {frac*100:.1f}% ({frac*weights[idx]:.1f} kg, value {frac*values[idx]:.1f})")
```

---

## 🎯 Recognize This Problem When...

- Items are **divisible** (liquids, bulk materials, time allocations).
- You need to maximize value subject to a weight/capacity constraint.
- Fractional amounts are physically meaningful or explicitly allowed.
- Keywords: "take any portion", "divisible resources", "fill the bag optimally".

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Items physically divisible (gold dust, oil, time) | ✅ Greedy gives optimal solution |
| Maximize value in knapsack with divisible items | ✅ Sort by ratio, fill greedily |
| Items are indivisible (must take whole item or none) | ❌ Use 0/1 Knapsack (DP) |
| Discrete units, finite quantity | ❌ Greedy may not be optimal |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Knapsack 0/1](../DPAlgorithms/Knapsack.md) | Indivisible items — greedy fails, use DP |
| [Activity Selection](ActivitySelection.md) | Also greedy; selection without combining |
| [Huffman Coding](HuffmanCoding.md) | Also greedy; minimize total weighted length |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Fractional Knapsack](https://www.hackerrank.com/challenges/fractional-knapsack/problem) | HackerRank | 🟡 Medium |
| [Maximum Units on a Truck](https://leetcode.com/problems/maximum-units-on-a-truck/) | LeetCode | 🟢 Easy |
| [Task Scheduler](https://leetcode.com/problems/task-scheduler/) | LeetCode | 🟡 Medium |
