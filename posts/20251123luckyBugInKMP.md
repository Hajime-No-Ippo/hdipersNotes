---
title: "LeetCode 459: The 'Lucky' Bug in KMP — Why Logic Matters More Than AC"
title_zh: "LeetCode 459: KMP 算法中的“幸运” Bug —— 为什么逻辑比 AC 更重要"
date: 2025-11-23
author: "22abad"
categories: 
  - "CS"
  - "LeetCode"
tags:
  - "Algorithm"
  - "KMP"
  - "Java"
  - "Debugging"
summary_en: "A deep dive into KMP's next array construction. Analyzing a specific bug where 'while(j > 1)' passed test cases despite breaking the algorithm's ability to reset, providing a profound insight into how KMP handles prefix matching."
summary_zh: "深入剖析 KMP 算法 next 数组的构建。分析为什么错误的 'while(j > 1)' 依然能通过测试用例，揭示了该 Bug 破坏了算法的“归零能力”但保留了“增长能力”的本质。"
---

[EN]
# LeetCode 459: The "Lucky" Bug in KMP

## 1. The Problem
The task is to determine if a string `s` can be constructed by taking a substring of it and appending multiple copies of the substring together.
My approach uses the **KMP Algorithm**, specifically analyzing the `next` array (longest equal prefix and suffix).

## 2. The Code & Deep Analysis
During my second pass (Space Repetition), I wrote code that passed LeetCode but contained a logical flaw. I have unified the code and the analysis below.

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        int j = 0; 
        int n = s.length();
        int[] next = new int[n];
        next[0] = 0; // Initialization

        for (int i = 1; i < n; i ++) {
            // 🚨 THE BUG IS HERE: j > 1
            // It should strictly be j > 0
            while (j > 1 && s.charAt(i) != s.charAt(j)) {
                j = next[j - 1];
            }
            
            if (s.charAt(i) == s.charAt(j)) {
                j++;
                next[i] = j; // update next array
            }
        }
        
        // Check if the string is composed of repeating substrings
        // 1. The last prefix length must be > 0
        // 2. The string length must be divisible by the unit length (n - next[n-1])
        if (next[n - 1] > 0 && n % (n - next[n - 1]) == 0) return true;
        else return false;
    }
}

/**
 * ===========================================================================
 * 3. THE DEEP ANALYSIS: Why j > 1 is Wrong but Worked?
 * ===========================================================================
 *
 * The standard KMP backtracking condition is while (j > 0 ...). 
 * This ensures that if a mismatch occurs, we can backtrack all the way 
 * to index 0 (the start of the pattern).
 *
 * ► Why j > 1 is wrong:
 * If j becomes 1, the loop while (j > 1 ...) terminates. We lose the ability 
 * to backtrack from index 1 to index 0. If s[i] doesn't match s[1], we simply 
 * stop, potentially leaving j at 1 incorrectly or failing to find a match 
 * that starts from 0.
 *
 * ► Why it still passed (The Insight):
 * For repetitive strings like "aabaabaab", let's compare the arrays:
 * * Incorrect (j > 1) Next Array: [0, 1, 1, 2, 2, 3, 4, 5, 6]
 * * Correct (j > 0) Next Array:   [0, 1, 0, 1, 2, 3, 4, 5, 6]
 *
 * Notice that for i=3, the incorrect logic kept next[2]=1.
 * However, later in the sequence:
 * * At i=3, j was stuck at 1. s[3] matches s[1], so j becomes 2. next[3]=2.
 * * From this point on, the pattern matches successfully, and j keeps growing.
 *
 * ► The Conclusion:
 * The condition j > 1 destroys the algorithm's "Zeroing Capability" (Reset), 
 * but it does not affect its "Growth Capability".
 * Since the problem checks next[n-1], and repetitive strings usually result 
 * in a large next value towards the end, the error in the middle was "healed" 
 * by the subsequent matches. This is a dangerous coincidence.
 *
 * ► Corrected Code Snippet:
 * while (j > 0 && s.charAt(i) != s.charAt(j)) {
 * j = next[j - 1];
 * }
 */
 ```
[END]

[ZH]

# LeetCode 459: KMP 算法中的“幸运” Bug

## 1. 问题描述
题目要求判断字符串 s 是否可以由其一个子串重复多次构成。 我的解法使用了 KMP 算法，重点在于分析 next 数组（最长相等前后缀）。

## 2. 代码与深度复盘
在二刷过程中，我写下了如下代码。它神奇地通过了测试，但包含逻辑漏洞。为了方便阅读，我将代码与深度分析合并在下方。

```java

class Solution {
    public boolean repeatedSubstringPattern(String s) {
        int j = 0; 
        int n = s.length();
        int[] next = new int[n];
        next[0] = 0; // 初始化

        for (int i = 1; i < n; i ++) {
            // 🚨 Bug 在这里: j > 1
            // 正确的写法应该是 j > 0
            while (j > 1 && s.charAt(i) != s.charAt(j)) {
                j = next[j - 1];
            }
            
            if (s.charAt(i) == s.charAt(j)) {
                j++;
                next[i] = j; // 更新 next 数组
            }
        }
        
        // 核心判断公式
        // 1. 最后一个 next 值必须大于 0
        // 2. 字符串总长度必须能被“最小重复单元长度”整除
        if (next[n - 1] > 0 && n % (n - next[n - 1]) == 0) return true;
        else return false;
    }
}

/**
 * ===========================================================================
 * 3. 深度复盘：为什么 j > 1 是错的却能过？
 * ===========================================================================
 *
 * 标准的 KMP 回退条件是 while (j > 0 ...)。
 * 这保证了如果不匹配，我们可以一路回退到索引 0（模式串的开头）。
 *
 * ► 为什么 j > 1 是错的？
 * 如果 j 退到了 1，循环 while (j > 1 ...) 就会终止。这意味着我们失去了从
 * 索引 1 回退到 0 的能力。如果此时 s[i] 和 s[1] 不匹配，程序就停止回退了，
 * 导致 j 错误地停留在 1，或者错过了从头开始匹配的机会。
 *
 * ► 为什么它还能过？（核心洞察）
 * 以 "aabaabaab" 为例，对比一下生成的数组：
 * * 错误的 (j > 1) Next 数组: [0, 1, 1, 2, 2, 3, 4, 5, 6]
 * * 正确的 (j > 0) Next 数组: [0, 1, 0, 1, 2, 3, 4, 5, 6]
 *
 * 注意在 i=2 时，错误逻辑导致 next[2] 变成了 1（本该回退到 0）。
 * 但是，看后面的发展：
 * * 当 i=3 时，虽然 j 错误地停在 1，但恰好 s[3] 和 s[1] 匹配上了！
 * 于是 j 变成了 2，next[3]=2。
 * * 从这里开始，后面一直匹配成功，j 持续增长。
 *
 * ► 结论：
 * j > 1 这个 Bug 破坏的是算法的“归零能力”（Reset），但对“增长能力”
 * （Growth）没有影响。
 * 因为这道题最终只检查 next[n-1]（最后一个值），而重复子串通常会导致 
 * next 值在后期很大，中间的那个小错误被后续的连续匹配“掩盖”了。
 * 这是一种危险的巧合，而不是正确的逻辑。
 *
 * ► 修正后的正确代码：
 * while (j > 0 && s.charAt(i) != s.charAt(j)) {
 * j = next[j - 1];
 * }
 */
 ```
[END]