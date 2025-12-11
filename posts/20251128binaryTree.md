---
title: "Binary Tree Elite Squad: The Only 5 Problems You Need for Mastery (Ultimate Edition)"
title_zh: "二叉树特种兵：二刷只需搞定这 5 道母题（终极全解版）"
date: 2025-11-29
author: "Dong"
categories: 
  - "Algorithm"
  - "Binary Tree"
tags:
  - "High Efficiency"
  - "Patterns"
  - "LeetCode"
  - "Java"
summary_en: "The ultimate guide to Binary Trees for the second pass. Covering Traversal, Construction, LCA, BST Validation, and Deletion. Includes Recursive, Iterative, and Unified solutions for every problem."
summary_zh: "二叉树二刷终极指南。覆盖遍历、构造、最近公共祖先、BST 验证与删除。每道题均包含递归、迭代及统一解法，彻底打通底层逻辑。"
---

[EN]
![Binary Tree Strategy|600](https://assets.flowstates.me/2025/20251128binaryTree.jpg)

# The Strategy

We don't have time to re-do all 30 problems. We need to hit the **Vital Points**.
If you master the following 5 problems and their variations (Recursion vs. Iteration), you master the Tree.


## 1. The Foundation: Traversal (Pre/In/Post)
* **Problem**: [LeetCode 94 (In)](https://leetcode.com/problems/binary-tree-inorder-traversal/) / [144 (Pre)](https://leetcode.com/problems/binary-tree-preorder-traversal/) / [145 (Post)](https://leetcode.com/problems/binary-tree-postorder-traversal/)
* **Goal**: Master both Recursion and Iteration. The **Unified Style** is key.

### Solution 1: Recursion (Standard)
```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        dfs(root, res);
        return res;
    }
    
    private void dfs(TreeNode node, List<Integer> res) {
        if (node == null) return;
        // Pre-order position: res.add(node.val);
        dfs(node.left, res);
        res.add(node.val); // In-order position
        dfs(node.right, res);
        // Post-order position: res.add(node.val);
    }
}
```

### Solution 2: Standard Iteration (Stack)

*Using an explicit stack to simulate recursion. This creates a "Left-Road-First" logic.*

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode curr = root;
        
        while (curr != null || !stack.isEmpty()) {
            // 1. Drill down to the leftmost node
            if (curr != null) {
                stack.push(curr);
                curr = curr.left; 
            } 
            // 2. Hit bottom, pop and process
            else {
                curr = stack.pop();
                res.add(curr.val); // In-order processing
                curr = curr.right; // Turn right
            }
        }
        return res;
    }
}
```

### Solution 3: Unified Iteration (Marker Method)

*The most powerful template. Use `null` to mark visited nodes. Swap order to change traversal type.*

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        if (root != null) stack.push(root);
        
        while (!stack.isEmpty()) {
            TreeNode node = stack.peek();
            if (node != null) {
                stack.pop(); // Pop to re-add in correct order
                
                // Order for In-Order: Right -> Center -> Left (Stack is LIFO)
                if (node.right != null) stack.push(node.right);  // Right
                stack.push(node);                                // Center
                stack.push(null);                                // ⚡️ MARKER (Visit later)
                if (node.left != null) stack.push(node.left);    // Left
                
            } else {
                stack.pop();        // Pop null marker
                node = stack.pop(); // Pop real node
                res.add(node.val);  // Process
            }
        }
        return res;
    }
}
```


## 2\. The Construction: Build Tree (Pre + In)

  * **Problem**: [LeetCode 105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
  * **Why**: The ultimate test of **Index Manipulation**.

### Solution 1: Recursion with Map (Optimized)

```java
class Solution {
    // Map to speed up index lookup (O(1))
    Map<Integer, Integer> map = new HashMap<>();

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for (int i = 0; i < inorder.length; i++) {
            map.put(inorder[i], i);
        }
        // Use closed intervals [start, end]
        return helper(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
    }

    private TreeNode helper(int[] preorder, int preStart, int preEnd, 
                            int[] inorder, int inStart, int inEnd) {
        if (preStart > preEnd) return null;

        int rootVal = preorder[preStart];
        TreeNode root = new TreeNode(rootVal);

        int index = map.get(rootVal);
        int leftSize = index - inStart; // ⚡️ Critical: Size of left subtree

        // Left: Pre[start+1, start+size], In[start, index-1]
        root.left = helper(preorder, preStart + 1, preStart + leftSize, inorder, inStart, index - 1);
        // Right: Pre[start+size+1, end], In[index+1, end]
        root.right = helper(preorder, preStart + leftSize + 1, preEnd, inorder, index + 1, inEnd);

        return root;
    }
}
```

### Solution 2: Iterative (Stack)

*Simulating the preorder traversal flow.*

```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if (preorder.length == 0) return null;
        Stack<TreeNode> stack = new Stack<>();
        TreeNode root = new TreeNode(preorder[0]);
        stack.push(root);
        int inorderIndex = 0;
        
        for (int i = 1; i < preorder.length; i++) {
            int preorderVal = preorder[i];
            TreeNode node = stack.peek();
            
            if (node.val != inorder[inorderIndex]) {
                // Still processing left subtree
                node.left = new TreeNode(preorderVal);
                stack.push(node.left);
            } else {
                // Finished left, backtracking to find where to attach right child
                while (!stack.isEmpty() && stack.peek().val == inorder[inorderIndex]) {
                    node = stack.pop();
                    inorderIndex++;
                }
                node.right = new TreeNode(preorderVal);
                stack.push(node.right);
            }
        }
        return root;
    }
}
```


## 3\. The Logic: Lowest Common Ancestor (LCA)

  * **Problem**: [LeetCode 236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)
  * **Why**: Post-order traversal (Bottom-up) logic.

### Solution 1: Recursion (DFS)

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // 1. Base Case: Found target or hit bottom
        if (root == null || root == p || root == q) return root;

        // 2. Recursion
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        // 3. Logic
        if (left != null && right != null) return root; // Found LCA
        if (left != null) return left;
        if (right != null) return right;
        return null;
    }
}
```

### Solution 2: Iterative (Parent Map)

*Treat it like a linked list intersection problem.*

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        Map<TreeNode, TreeNode> parent = new HashMap<>();
        Deque<TreeNode> stack = new ArrayDeque<>();
        parent.put(root, null);
        stack.push(root);

        // 1. Build Parent Pointers until we find both p and q
        while (!parent.containsKey(p) || !parent.containsKey(q)) {
            TreeNode node = stack.pop();
            if (node.left != null) {
                parent.put(node.left, node);
                stack.push(node.left);
            }
            if (node.right != null) {
                parent.put(node.right, node);
                stack.push(node.right);
            }
        }
        
        // 2. Trace back p's ancestors
        Set<TreeNode> ancestors = new HashSet<>();
        while (p != null) {
            ancestors.add(p);
            p = parent.get(p);
        }
        
        // 3. Check q's ancestors. The first match is LCA.
        while (!ancestors.contains(q)) {
            q = parent.get(q);
        }
        return q;
    }
}
```


## 4\. Bonus: Validate Binary Search Tree (BST)

  * **Problem**: [LeetCode 98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)
  * **Why**: The "Hello World" of BST properties.

### Solution 1: Recursion (Range Limit)

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValid(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
    private boolean isValid(TreeNode node, long lower, long upper) {
        if (node == null) return true;
        if (node.val <= lower || node.val >= upper) return false;
        return isValid(node.left, lower, node.val) && isValid(node.right, node.val, upper);
    }
}
```

### Solution 2: Iteration (In-order Traversal)

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        TreeNode curr = root;
        long prev = Long.MIN_VALUE;

        while (curr != null || !stack.isEmpty()) {
            while (curr != null) {
                stack.push(curr);
                curr = curr.left;
            }
            curr = stack.pop();
            // In-order must be strictly increasing
            if (curr.val <= prev) return false;
            prev = curr.val;
            curr = curr.right;
        }
        return true;
    }
}
```


## 5\. The Operation: Delete Node in BST

  * **Problem**: [LeetCode 450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/)
  * **Why**: Complex pointer manipulation.

### Solution 1: Recursion (Standard)

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) return null;
        if (root.val > key) {
            root.left = deleteNode(root.left, key);
            return root;
        } else if (root.val < key) {
            root.right = deleteNode(root.right, key);
            return root;
        } else {
            if (root.left == null) return root.right;
            if (root.right == null) return root.left;
            
            // Two children: Successor is Min of Right Subtree
            TreeNode minNode = root.right;
            while (minNode.left != null) minNode = minNode.left;
            
            minNode.left = root.left; 
            return root.right; 
        }
    }
}
```

### Solution 2: Iterative (Manual Pointer)

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        TreeNode cur = root, pre = null;
        while (cur != null && cur.val != key) {
            pre = cur;
            if (cur.val > key) cur = cur.left;
            else cur = cur.right;
        }
        if (cur == null) return root;

        if (pre == null) return deleteOneNode(cur);
        if (pre.left == cur) pre.left = deleteOneNode(cur);
        else pre.right = deleteOneNode(cur);
        return root;
    }

    private TreeNode deleteOneNode(TreeNode target) {
        if (target == null) return null;
        if (target.right == null) return target.left;
        TreeNode cur = target.right;
        while (cur.left != null) cur = cur.left;
        cur.left = target.left;
        return target.right;
    }
}
```

[END]

[ZH]

![Binary Tree Strategy|600](https://assets.flowstates.me/2025/20251128binaryTree.jpg)


# 战略：为什么选这几道？

我们没时间把那 30 道题重刷一遍。我们要打**七寸**。
二叉树本质上只考两件事：

1.  **遍历顺序**：你能按你想要的顺序访问节点吗？（递归很简单，迭代才是试金石）。
2.  **结构操作**：你能根据逻辑修改指针或构建节点吗？

搞定下面这 5 道题及其变种，二叉树你就通关了。


## 1\. 地基：遍历（前/中/后序）

  * **对应题目**: [LeetCode 94 (中)](https://leetcode.com/problems/binary-tree-inorder-traversal/) / [144 (前)](https://leetcode.com/problems/binary-tree-preorder-traversal/) / [145 (后)](https://leetcode.com/problems/binary-tree-postorder-traversal/)
  * **目标**: 同时掌握递归和迭代。重点掌握**统一迭代法（标记法）**，它是解决所有遍历问题的万能钥匙。

### 解法 1：递归法（基础）

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        dfs(root, res);
        return res;
    }
    
    private void dfs(TreeNode node, List<Integer> res) {
        if (node == null) return;
        // 前序位置: res.add(node.val);
        dfs(node.left, res);
        res.add(node.val); // 中序位置
        dfs(node.right, res);
        // 后序位置: res.add(node.val);
    }
}
```

### 解法 2：标准迭代法（栈）

*思路：一路向左钻到底，撞墙了就回头处理，然后转向右边。*

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode curr = root;
        
        while (curr != null || !stack.isEmpty()) {
            // 1. 一路向左钻到底
            if (curr != null) {
                stack.push(curr);
                curr = curr.left; 
            } 
            // 2. 到头了，弹出并处理
            else {
                curr = stack.pop();
                res.add(curr.val); // 记录（中序）
                curr = curr.right; // 转向右边
            }
        }
        return res;
    }
}
```

### 解法 3：统一迭代法（标记法）

*万能模板。用 null 标记“来过但未处理”的节点。只需交换 push 顺序即可实现前中后序。*

```java
// 统一迭代法 (标记法)
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        if (root != null) stack.push(root);
        
        while (!stack.isEmpty()) {
            TreeNode node = stack.peek();
            if (node != null) {
                stack.pop(); // 弹出，为了重新按顺序压栈
                
                // 调整这三行的顺序即可改变遍历方式
                // 当前：中序 (左 -> 中 -> 右)
                // 入栈顺序 (反向)：右 -> 中 -> 左
                
                if (node.right != null) stack.push(node.right);  // 右
                stack.push(node);                                // 中
                stack.push(null);                                // ⚡️ 标记 (来过，未处理)
                if (node.left != null) stack.push(node.left);    // 左
                
            } else {
                stack.pop();        // 弹出标记 null
                node = stack.pop(); // 弹出真正的数据节点
                res.add(node.val);  // 处理
            }
        }
        return res;
    }
}
```


## 2\. 构造：从前序与中序构造二叉树

  * **对应题目**: [LeetCode 105. 从前序与中序遍历序列构造二叉树](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
  * **理由**: 这是对 **数组下标** 和 **递归拆解** 的终极考验。你必须精准地计算出左子树和右子树在数组中的范围。

### 解法 1：递归 + Map 优化

```java
class Solution {
    // Map 加速查找根节点下标 (O(1))
    Map<Integer, Integer> map = new HashMap<>();

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for (int i = 0; i < inorder.length; i++) {
            map.put(inorder[i], i);
        }
        // 区间：左闭右闭
        return helper(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
    }

    private TreeNode helper(int[] preorder, int preStart, int preEnd, 
                            int[] inorder, int inStart, int inEnd) {
        // Base Case: Empty array range
        if (preStart > preEnd) return null;

        // 1. 找到根节点值 (前序第一个)
        int rootVal = preorder[preStart];
        TreeNode root = new TreeNode(rootVal);

        // 2. 找到根在中序中的位置
        int index = map.get(rootVal);
        
        // 3. 计算左子树大小 (⚡️ 关键点!)
        int leftSize = index - inStart;

        // 4. 递归构造
        // 左子树范围: [preStart+1, preStart+leftSize] | [inStart, index-1]
        root.left = helper(preorder, preStart + 1, preStart + leftSize, inorder, inStart, index - 1);
        
        // 右子树范围: [preStart+leftSize+1, preEnd] | [index+1, inEnd]
        root.right = helper(preorder, preStart + leftSize + 1, preEnd, inorder, index + 1, inEnd);

        return root;
    }
}
```

### 解法 2：迭代法（栈模拟）

*用栈来模拟递归过程。只要一直往左走，就压栈；遇到中序节点，说明左到底了，回溯找右孩子。*

```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if (preorder.length == 0) return null;
        Stack<TreeNode> stack = new Stack<>();
        TreeNode root = new TreeNode(preorder[0]);
        stack.push(root);
        
        int inorderIndex = 0;
        
        for (int i = 1; i < preorder.length; i++) {
            int preorderVal = preorder[i];
            TreeNode node = stack.peek();
            
            if (node.val != inorder[inorderIndex]) {
                // 只要不等于中序当前值，说明还在左子树
                node.left = new TreeNode(preorderVal);
                stack.push(node.left);
            } else {
                // 等于了，说明左边走完了，回溯找右孩子的挂载点
                while (!stack.isEmpty() && stack.peek().val == inorder[inorderIndex]) {
                    node = stack.pop();
                    inorderIndex++;
                }
                node.right = new TreeNode(preorderVal);
                stack.push(node.right);
            }
        }
        return root;
    }
}
```


## 3\. 逻辑：最近公共祖先 (LCA)

  * **对应题目**: [LeetCode 236. 二叉树的最近公共祖先](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)
  * **理由**: 这是 **后序遍历 (Bottom-up)** 的巅峰。你需要收集左右孩子的返回值，然后在父节点做决策。逻辑极其精简。

### 解法 1：递归法 (DFS)

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // 1. 终止条件：到底了，或者找到目标了 -> 返回自己
        if (root == null || root == p || root == q) return root;

        // 2. 递归 (去左右子树找)
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        // 3. 逻辑判断 (回溯/后序处理)
        
        // 左右各找到一个，说明 p 和 q 分居两侧 -> 当前节点就是 LCA
        if (left != null && right != null) return root; 
        
        // 只有一边找到了，说明 LCA 在那一边，往上传
        if (left != null) return left;   
        if (right != null) return right; 
        
        // 啥也没找到
        return null;
    }
}
```

### 解法 2：迭代法（父节点 Map）

*把树问题变成链表问题。先用 Map 记下所有节点的父节点，然后从 p 往上走，再从 q 往上走，相遇点即 LCA。*

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        Map<TreeNode, TreeNode> parent = new HashMap<>();
        Deque<TreeNode> stack = new ArrayDeque<>();
        parent.put(root, null);
        stack.push(root);

        // 1. 遍历整棵树，记录父节点
        while (!parent.containsKey(p) || !parent.containsKey(q)) {
            TreeNode node = stack.pop();
            if (node.left != null) {
                parent.put(node.left, node);
                stack.push(node.left);
            }
            if (node.right != null) {
                parent.put(node.right, node);
                stack.push(node.right);
            }
        }
        
        // 2. 记录 p 的祖先路径
        Set<TreeNode> ancestors = new HashSet<>();
        while (p != null) {
            ancestors.add(p);
            p = parent.get(p);
        }
        
        // 3. q 往上走，遇到的第一个在集合里的就是 LCA
        while (!ancestors.contains(q)) {
            q = parent.get(q);
        }
        return q;
    }
}
```

## 4\. 补充：验证二叉搜索树 (BST)

  * **对应题目**: [LeetCode 98. 验证二叉搜索树](https://leetcode.com/problems/validate-binary-search-tree/)
  * **理由**: BST 的入门必修课。考察对 **有效取值范围** 和 **中序遍历有序性** 的理解。

### 解法 1：递归法（区间界定）

*直觉：每个节点必须在父辈给定的 (min, max) 范围内。*

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValid(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    private boolean isValid(TreeNode node, long lower, long upper) {
        if (node == null) return true;
        if (node.val <= lower || node.val >= upper) return false;
        return isValid(node.left, lower, node.val) && isValid(node.right, node.val, upper);
    }
}
```

### 解法 2：迭代法（中序栈）

*BST 的中序遍历必须是严格递增的。*

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        TreeNode curr = root;
        long prev = Long.MIN_VALUE;

        while (curr != null || !stack.isEmpty()) {
            while (curr != null) {
                stack.push(curr);
                curr = curr.left;
            }
            curr = stack.pop();
            // 检查递增性
            if (curr.val <= prev) return false;
            prev = curr.val;
            curr = curr.right;
        }
        return true;
    }
}
```


## 5\. 操作：删除 BST 中的节点

  * **对应题目**: [LeetCode 450. 删除二叉搜索树中的节点](https://leetcode.com/problems/delete-node-in-a-bst/)
  * **理由**: BST 操作的综合大题。结合了 **搜索** 和 **指针操作**。最难的是处理“双子节点”的情况——需要找到右子树的最小值来“继位”。

### 解法 1: 递归法（推荐）

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) return null;

        // 1. 搜索
        if (root.val > key) {
            root.left = deleteNode(root.left, key);
            return root;
        } else if (root.val < key) {
            root.right = deleteNode(root.right, key);
            return root;
        } else {
            // 2. 找到了：删除逻辑
            
            // 情况 A: 单孩子/无孩子
            if (root.left == null) return root.right;
            if (root.right == null) return root.left;

            // 情况 B: 双孩子
            // 策略: 找到右子树中的最小值 (继承人)
            TreeNode minNode = root.right;
            while (minNode.left != null) minNode = minNode.left;
            
            // 技巧: 把root的左子树，嫁接到继承人的左边
            minNode.left = root.left; 
            return root.right; // 右孩子上位
        }
    }
}
```

### 解法 2: 迭代法（硬核指针）

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        TreeNode cur = root, pre = null;
        while (cur != null && cur.val != key) {
            pre = cur;
            if (cur.val > key) cur = cur.left;
            else cur = cur.right;
        }
        if (cur == null) return root;

        if (pre == null) return deleteOneNode(cur);
        if (pre.left == cur) pre.left = deleteOneNode(cur);
        else pre.right = deleteOneNode(cur);
        return root;
    }

    private TreeNode deleteOneNode(TreeNode target) {
        if (target == null) return null;
        if (target.right == null) return target.left;
        TreeNode cur = target.right;
        while (cur.left != null) cur = cur.left;
        cur.left = target.left;
        return target.right;
    }
}
```

[END]
