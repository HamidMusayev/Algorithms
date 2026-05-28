### **Floyd's Cycle Detection (Tortoise & Hare) 🛠**

Detects whether a linked list (or any sequence defined by `next`) **contains a cycle**, using two pointers moving at different speeds — **O(1) extra memory**.

Also finds the **cycle's entry point** in linear time.

---

#### **Summary**
- **Time Complexity:** O(n)
- **Space Complexity:** O(1)

---

#### **How It Works**
1. **slow** moves one step at a time, **fast** moves two.
2. If they ever meet → cycle exists.
3. To find the cycle's start: reset slow to the head; move both one step at a time — they meet at the entry.

---

#### **Python Implementation**
```python
class Node:
    def __init__(self, val, nxt=None):
        self.val = val
        self.next = nxt

def has_cycle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow is fast:
            return True
    return False

def cycle_start(head):
    slow = fast = head
    while fast and fast.next:
        slow, fast = slow.next, fast.next.next
        if slow is fast:
            break
    else:
        return None
    slow = head
    while slow is not fast:
        slow, fast = slow.next, fast.next
    return slow

# Example
a = Node(1); b = Node(2); c = Node(3); d = Node(4)
a.next, b.next, c.next, d.next = b, c, d, b   # cycle starts at b
print(has_cycle(a))           # True
print(cycle_start(a).val)     # 2
```

---

#### **When to Use**
✅ Linked-list cycle problems (very common interview question)
✅ Detecting cycles in iterated functions (`xₙ₊₁ = f(xₙ)`)
✅ Pollard's rho factorization
