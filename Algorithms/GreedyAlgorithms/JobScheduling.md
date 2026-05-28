### **Job Scheduling (with Deadlines) 💰**

Given `n` jobs with **profit** and **deadline** (each job takes 1 unit of time), schedule a subset to **maximize total profit**, where each job must finish before its deadline.

---

#### **Summary**
- **Time Complexity:** O(n log n) — sort jobs, then place each
- **Space Complexity:** O(d) for the time slots
- **Greedy choice:** consider jobs by **highest profit first**; assign each to its latest free slot ≤ deadline.

---

#### **Python Implementation**
```python
def job_scheduling(jobs):
    # jobs = [(id, deadline, profit), ...]
    jobs.sort(key=lambda x: x[2], reverse=True)   # by profit desc
    max_deadline = max(j[1] for j in jobs)
    slots = [None] * (max_deadline + 1)
    total_profit = 0

    for jid, dl, profit in jobs:
        # find latest free slot <= deadline
        for t in range(min(dl, max_deadline), 0, -1):
            if slots[t] is None:
                slots[t] = jid
                total_profit += profit
                break
    return [j for j in slots if j is not None], total_profit

# Example
jobs = [('a', 2, 100), ('b', 1, 19), ('c', 2, 27), ('d', 1, 25), ('e', 3, 15)]
print(job_scheduling(jobs))   # (['c', 'a', 'e'], 142)
```

> Using a DSU for "find latest free slot" reduces inner loop cost to nearly O(α(n)).

---

#### **When to Use**
✅ CPU / task scheduling with deadlines
✅ Greedy interval-style problems
🚫 If durations vary, you need a different formulation (often DP)
