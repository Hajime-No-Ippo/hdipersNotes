---
title: "Java Collections Demystified: Stack, Queue, Deque, and Why 'Stack' Is Dead"
title_zh: "Java 集合框架解惑：Stack、Queue、Deque，以及为什么 Stack 已死"
date: 2025-11-25
author: "Dong"
categories: 
  - "CS"
  - "Java"
tags:
  - "Data Structure"
  - "Best Practice"
  - "Interview Prep"
summary_en: "A professional, precise explanation of why Java's Stack class is obsolete, how Deque replaces it, and how Interface vs. Implementation resolves common beginner confusion."
summary_zh: "一次专业而清晰的讲解：为什么 Java 的 Stack 已经过时？为什么应该使用 Deque？接口与实现的分离如何解决初学者最常遇到的困惑？"
---

[EN]
# Java Collections Demystified  
### Stack, Queue, Deque — and Why `Stack` Is Already Dead

Java beginners (my past self included) often stumble into this puzzling error:

```java
// ❌ Compile Error: incompatible types
Stack<TreeNode> stack = new LinkedList<>();
```

At first glance, both *look* like stacks. LinkedList even supports push/pop.  
So… why does this fail?

## 1. The Core Confusion: Class ≠ Behavior
Here’s the truth:

- `Stack` is a **specific class** (from 1995, JDK 1.0).
- `LinkedList` is a **different class**.
- They share **zero inheritance relationship**.

A LinkedList **can behave like** a stack,  
but it **is not** a Stack in Java’s type system.

It’s like writing:

```
CassettePlayer player = new MP3Player();
```

Both play music, but one is not the other.  
Identity matters.

[END]

[ZH]
# Java 集合框架解惑  
### Stack、Queue、Deque —— 以及为什么 Stack 已经过时

许多 Java 初学者（包括曾经的我）经常遇到这样让人摸不着头脑的错误：

```java
// ❌ 编译错误：类型不兼容
Stack<TreeNode> stack = new LinkedList<>();
```

表面上，两者都“能当栈用”，LinkedList 甚至内置 push/pop。  
那为什么不能赋值？

## 1. 困惑的根源：类 ≠ 行为
原因其实很简单：

- `Stack` 是一个 **具体类**（1995 年产物）。
- `LinkedList` 是另一种 **具体类**。
- 二者 **没有继承关系**。

LinkedList **能像** 栈一样工作，  
但在 Java 类型系统中，它 **不是** 栈。

这就像你试图写：

```
磁带机 player = MP3 播放器；
```

虽然都能放歌，但本质上是两种完全不同的机器。

[END]

[EN]
## 2. The Modern Solution: Interface (Identity) vs Implementation (Body)

To fix the confusion, we separate:

- **Identity** → Interface  
- **Body** → Implementation class  

For stack behavior (LIFO),  
Java’s modern identity is **Deque** (Double-Ended Queue).

### ✔ Correct Ways to Declare a Stack

```java
// Option A — Fastest, recommended
Deque<Integer> stack = new ArrayDeque<>();

// Option B — Linked-list based, if nodes matter
Deque<Integer> stack = new LinkedList<>();
```

Here:

- `Deque` = **contract** (“I behave like a stack/queue”)
- `ArrayDeque` / `LinkedList` = **workers** (actual data structures)

This is the essence of *clean API design*.

[END]

[ZH]
## 2. 现代解决方案：接口（身份） vs 实现（肉体）

要解决困惑，需要分清：

- **身份**：接口  
- **肉体**：实现类  

对于“栈”（后进先出）的需求，  
Java 现代推荐使用的身份是 **Deque**。

### ✔ Java 中声明栈的正确方式

```java
// 方案 A — 基于数组（最快，强烈推荐）
Deque<Integer> stack = new ArrayDeque<>();

// 方案 B — 基于链表（如果你需要节点操作）
Deque<Integer> stack = new LinkedList<>();
```

在这里：

- `Deque` = **合同、角色**
- `ArrayDeque` / `LinkedList` = **真正干活的工人**

这体现了 Java 清晰 API 设计的核心思想。

[END]

[EN]
## 3. Why the `Stack` Class Is Officially Dead

You *can* still write:

```java
Stack<Integer> s = new Stack<>();
```

But you *shouldn’t*.

### Why?

- `Stack` inherits from `Vector`
- `Vector` is fully **synchronized**
- Synchronization = unnecessary overhead  
  (especially for LeetCode or single-thread usage)

Even Oracle's documentation explicitly recommends:

> “Use `Deque` instead of `Stack`.”

### Summary Table

| Goal (Behavior)     | Interface | Implementation            |
|---------------------|-----------|---------------------------|
| Dynamic Array       | List      | ArrayList                 |
| Linked List         | List      | LinkedList                |
| Stack (LIFO)        | Deque     | ArrayDeque (recommended)  |
| Queue (FIFO)        | Queue     | LinkedList / ArrayDeque   |

Stop using `Stack`.  
Use `Deque`.  
Your future self will thank you.

[END]

[ZH]
## 3. 为什么 `Stack` 类已经“死掉”？

你当然可以写：

```java
Stack<Integer> s = new Stack<>();
```

但你不该这样做。

### 原因如下：

- `Stack` 继承自 `Vector`
- `Vector` 的所有方法都是 **同步的**
- 同步意味着 **性能低下**  
  （尤其在单线程算法场景中）

甚至 Oracle 官方文档都明确建议：

> “请使用 Deque 来替代 Stack。”

### 总结表格

| 目标（行为）         | 接口       | 实现类                      |
|----------------------|------------|-----------------------------|
| 动态数组             | List       | ArrayList                   |
| 链表结构             | List       | LinkedList                  |
| 栈（后进先出）       | Deque      | ArrayDeque（推荐）          |
| 队列（先进先出）     | Queue      | LinkedList / ArrayDeque     |

别再用 `Stack` 了。  
拥抱 `Deque` 吧。

[END]
