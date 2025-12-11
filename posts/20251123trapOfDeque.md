---
title: "Monotonic Queue: Why We Store Indices and The Trap of Deque APIs"
title_zh: "单调队列：为什么存下标？以及 Deque API 的方向陷阱"
date: 2025-11-24
author: "Dong"
categories: 
  - "CS"
  - "LeetCode"
tags:
  - "Algorithm"
  - "Monotonic Queue"
  - "Java"
  - "Data Structure"
summary_en: "Reflecting on the Sliding Window Maximum problem. Realizing that storing indices (not values) in the Deque is the key to handling window boundaries. Also, a crucial reminder about Java's Deque API directions."
summary_zh: "复盘“滑动窗口最大值”问题。深刻领悟单调队列中存储“下标”而非“数值”的必要性，以及对 Java Deque API 操作方向（队头 vs 队尾）的精准把控。"
---

[EN]
# Monotonic Queue: Why We Store Indices and The Trap of Deque APIs

## 1. The "Index vs. Value" Epiphany
In the **Sliding Window Maximum** problem, beginners often instinctively store the **values** (`nums[i]`) in the queue.
However, I realized today that storing **indices** (`i`) is the only way to solve the "expiration" problem.

* **If we store values:** We know *what* the max is, but we don't know *where* it is. When the window slides, we can't tell if the max value has fallen out of the left boundary (`i - k`).
* **If we store indices:** We have everything.
    * **Value?** Just call `nums[index]`.
    * **Position?** Just check `index`.
    * **Expiration?** Check if `index < i - k + 1`.

**Key Takeaway:** In sliding window problems, **Index > Value**. The index carries position information, which is the lifeline of the window.

## 2. Mastering Deque APIs: Head vs. Tail
Java's `Deque` (Double Ended Queue) is powerful but confusing. Today I solidified the default behaviors:

* **Default = Head:** Methods like `peek()`, `poll()`, `remove()` all operate on the **HEAD** (the oldest element, the maximum candidate).
* **Explicit = Tail:** Only methods with "Last" (e.g., `peekLast()`, `pollLast()`) operate on the **TAIL** (the newest element, the one being compared).

**The Monotonic Queue Logic:**
1.  **Head (Oldest):** Responsible for rules. If it's expired (out of window), kick it out (`poll()`).
2.  **Tail (Newest):** Responsible for competition. If the new guy is stronger (`nums[i]`), kick the weak ones out (`pollLast()`).

[END]

[ZH]
# 单调队列：为什么存下标？以及 Deque API 的方向陷阱

## 1. “存下标”的顿悟
在解决**滑动窗口最大值**问题时，直觉往往让我们在队列里存**数值**（`nums[i]`）。
但我今天彻底明白了，只有存**下标**（`i`），才能完美解决“元素过期”的问题。

* **如果存数值：** 我们知道最大值是“谁”，但不知道它在“哪”。当窗口滑动时，我们无法判断这个最大值是不是已经滑出了左边界（`i - k`）。
* **如果存下标：** 我们全都有了。
    * **要数值？** `nums[index]` 一查就有。
    * **要位置？** `index` 就在手边。
    * **判断过期？** 直接看 `index < i - k + 1`。

**核心心法：** 在滑动窗口类题目中，**下标 > 数值**。下标携带了位置信息，那是窗口的生命线。

## 2. 拿捏 Deque API：队头还是队尾？
Java 的 `Deque`（双端队列）很强大，但也容易晕。今天我把它的默认行为彻底钉死了：

* **默认 = 队头 (Head)：** 像 `peek()`, `poll()`, `remove()` 这些不带后缀的方法，默认都是操作**队头**（也就是最老的元素，当前窗口的最大值）。
* **显式 = 队尾 (Tail)：** 只有带 "Last" 的方法（如 `peekLast()`, `pollLast()`），才是操作**队尾**（也就是最新进来的元素，正在进行比较的元素）。

**单调队列的铁律：**
1.  **队头（老大哥）**：负责守规矩。如果位置过期了（滑出窗口），直接走人 (`poll()`)。
2.  **队尾（新挑战者）**：负责拼实力。如果新来的 (`nums[i]`) 比队尾强，队尾的弱鸡就得滚蛋 (`pollLast()`)。

```java
// 1. 检查队头霸主是否已经“过期”（滑出窗口左边界）
// 注意：这里存的是 index！
while (!deque.isEmpty() && deque.peek() < i - k + 1) {
    deque.poll(); // 默认操作队头
}

// 2. 维护单调递减：如果新来的比队尾的大，清洗无用数据
while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
    deque.pollLast(); // 显式操作队尾
}
```
[END]