---
title: "Cracking the Stack: The Philosophy Behind Standard vs. Unified Iteration"
title_zh: "破解栈的哲学：标准迭代 vs 统一迭代的底层逻辑差异"
date: 2025-11-30
author: "Dong"
categories: 
  - "Algorithm"
  - "Deep Dive"
tags:
  - "Binary Tree"
  - "Stack"
  - "Thinking Model"
summary_en: "A deep dive into the two schools of iterative tree traversal. Analyzing why Standard Iteration relies on 'Path Timing' (hard to unify) while Unified Iteration relies on 'State Marking' (easy to unify). Uncovering the core logic of when to collect results."
summary_zh: "深入剖析二叉树迭代遍历的两个流派。分析为什么“标准迭代”依赖“路径时机”（难以统一），而“统一迭代”依赖“状态标记”（轻松统一）。揭示“何时收集结果”的核心逻辑。"
---



[EN]
![Stack Philosophy|600](https://assets.flowstates.me/2025/20251130stackPhilosophy.jpg)
# The Confusion of Iteration

When learning Binary Tree iteration, most people get confused.
* **Preorder** uses a simple stack.
* **Inorder** requires a pointer to drill left.
* **Postorder** needs a double stack or reverse logic.
Three different logics for one tree? That's messy.

Then comes the **Unified Iteration (Marker Method)**. Suddenly, one logic rules them all.
But why? What is the fundamental difference?
Today, I figured out the "Secret of Collection."
[END]

[ZH]
![Stack Philosophy|600](https://assets.flowstates.me/2025/20251130stackPhilosophy.jpg)
# 迭代的困惑

在学二叉树迭代时，大多数人都会晕。
* **前序** 用一个简单的栈。
* **中序** 需要一个指针一路向左钻。
* **后序** 需要双栈或者反转逻辑。
一棵树，三种逻辑？太乱了。

然后，**统一迭代法（标记法）** 出现了。突然间，一套逻辑通吃所有。
但为什么？本质区别在哪里？
今天，我悟出了那个**“收割秘密”**。
[END]

---

[EN]
## 1. Standard Iteration: Harvest by "Timing"
In Standard Iteration (especially Inorder), the Stack is used to **record the path**.
We push nodes as we walk.
* **The Rule:** We only collect the value (add to `res`) when we are **popping** it off the stack *after* hitting a dead end on the left.
* **The Problem:** We are relying on the **Timing** of the pop.
    * For Preorder, we collect *before* pushing.
    * For Inorder, we collect *after* popping.
    * For Postorder, it's even more complex.
Because the "Time to Collect" is different for each order, the code structure *must* be different.

```java
// Standard Inorder: Logic depends on pointer movement
while (curr != null || !stack.isEmpty()) {
    if (curr != null) {
        stack.push(curr);
        curr = curr.left; // Move Left
    } else {
        curr = stack.pop();
        res.add(curr.val); // <--- Harvest Timing: After left is done
        curr = curr.right;
    }
}
```
[END]

[ZH]

1. 标准迭代：靠“时机”收割
在标准迭代（特别是中序）中，栈是用来记录路径的。 我们边走边压栈。

规则：我们只有在左路走到头，弹栈（Pop） 的时候，才去收集它的值（加入 res）。

问题：我们依赖的是动作发生的时机。

前序：压栈前收集（进门就拿）。

中序：弹栈后收集（回来再拿）。

后序：更复杂。 因为每种遍历“收割的时机”不同，所以代码结构必须不同。这就导致了难以记忆。

```Java

// 标准中序迭代：逻辑依赖指针移动
while (curr != null || !stack.isEmpty()) {
    if (curr != null) {
        stack.push(curr);
        curr = curr.left; // 向左走
    } else {
        curr = stack.pop();
        res.add(curr.val); // <--- 收割时机：左边走完回来时
        curr = curr.right;
    }
}
```
[END]

[EN]

2. Unified Iteration: Harvest by "State"
The Unified Method (Marker Method) changes the game. The Stack is no longer just a path recorder; it becomes a Task List. We explicitly mark the state of a node:

Node without NULL: "I need to visit your children first." (Processing state)

Node followed by NULL: "I have visited your children. You are ready." (Harvest state)

The Golden Rule of Collection: We never worry about timing. We only look for the Marker (NULL).

Pop a node. Is it NULL?

Yes: The next node in the stack is ready to be harvested. Collect it.

No: It's not ready. Push it back with a marker, and push its children according to the order (Right-Center-Left).

This decouples "Traversal" from "Collection". That is why it is unified.

```Java

// Unified: Logic depends on the MARKER
while (!stack.isEmpty()) {
    TreeNode node = stack.peek();
    if (node != null) {
        stack.pop(); 
        // Push Order determines Traversal Order (Right-Center-Left -> Inorder)
        if (node.right != null) stack.push(node.right);
        
        stack.push(node); 
        stack.push(null); // <--- The State Marker
        
        if (node.left != null) stack.push(node.left);
        
    } else {
        stack.pop(); // Remove Marker
        node = stack.pop(); // The node is ready
        res.add(node.val); // <--- Harvest Condition: Always here
    }
}
```
[END]

[ZH]

2. 统一迭代：靠“状态”收割
统一迭代法（标记法）改变了游戏规则。 栈不再只是路径记录器，它变成了一个任务列表。 我们显式地标记节点的状态：

没有 NULL 标记的节点：“我还需要先去看看你的孩子。”（处理中状态）

紧跟 NULL 标记的节点：“我已经看过你的孩子了，你是成品了。”（收割状态）

收割黄金法则： 我们从不关心时机。我们只找 标记 (NULL)。

弹出一个元素。是 NULL 吗？

是：说明栈里紧接着的那个节点是可以收割的。拿走它（res.add）。

否：说明它还生着呢。把它放回去，贴个 NULL 标签，然后把它的孩子按规矩（右-中-左）塞进去。

这彻底解耦了“遍历”和“收割”。 这就是为什么它能统一。

```Java

// 统一迭代：逻辑依赖 标记 (MARKER)
while (!stack.isEmpty()) {
    TreeNode node = stack.peek();
    if (node != null) {
        stack.pop(); 
        // 入栈顺序决定遍历顺序 (右-中-左 -> 中序)
        if (node.right != null) stack.push(node.right);
        
        stack.push(node); 
        stack.push(null); // <--- 状态标记
        
        if (node.left != null) stack.push(node.left);
        
    } else {
        stack.pop(); // 弹出标记
        node = stack.pop(); // 这个节点是熟的
        res.add(node.val); // <--- 收割条件：永远在这里
    }
}
```
[END]