### **Activity Selection Problem 💰**

Given a set of activities with **start** and **finish** times, select the **maximum number** of non-overlapping activities.

Classic greedy: **always pick the activity that finishes earliest** and is compatible with the last chosen one.

---

#### **Summary**
- **Time Complexity:** O(n log n) (dominated by sorting)
- **Space Complexity:** O(1) extra
- **Greedy choice:** earliest finish time → leaves the most room for others.

---

#### **Python Implementation**
```python
def activity_selection(activities):
    # activities = [(start, finish), ...]
    activities.sort(key=lambda x: x[1])     # sort by finish time
    selected = []
    last_finish = -1
    for start, finish in activities:
        if start >= last_finish:
            selected.append((start, finish))
            last_finish = finish
    return selected

# Example
activities = [(1, 4), (3, 5), (0, 6), (5, 7), (3, 9), (5, 9), (6, 10), (8, 11)]
print(activity_selection(activities))
# [(1, 4), (5, 7), (8, 11)]
```

---

#### **When to Use**
✅ Meeting / classroom scheduling
✅ Interval scheduling (maximum jobs)
🚫 If you also care about *value* of each activity → use weighted interval scheduling (DP)
