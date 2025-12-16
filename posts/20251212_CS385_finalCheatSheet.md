---
title: "CS385: Reference Architecture & Debugging Protocols"
title_en: "CS385: Reference Architecture & Debugging Protocols"
title_zh: "CS385：参考架构与调试协议"
date: 2025-12-12
author: "Dong Li"
categories: 
  - "CS385"
  - "Reference"
tags: 
  - "JavaScript"
  - "React"
  - "Debugging"
summary_en: "A consolidated reference architecture for CS385. Includes strict equality tables, array mutation matrices, and React rendering lifecycle protocols."
summary_zh: "CS385 的综合参考架构。包含严格相等性表、数组突变矩阵和 React 渲染生命周期协议。"
---

[EN]
# 🚀 Reference Architecture

This document serves as the primary reference for the CS385 Lab Exam 3. It consolidates essential syntax, behavioral patterns, and debugging protocols.

---

## 1. JavaScript Core Mechanics

### 1.1 Equality Matrix

| Feature               | `==` (Loose)        | `===` (Strict)      | `Object.is()`       |
| --------------------- | ------------------- | ------------------- | ------------------- |
| Type Coercion         | ✅ Yes               | ❌ No                | ❌ No                |
| Object Comparison     | Reference           | Reference           | Reference           |
| `NaN` vs `NaN`        | `false`             | `false`             | ✅ `true`            |
| `+0` vs `-0`          | `true`              | `true`              | ✅ `false`           |

### 1.2 Array Mutation Matrix

| Method     | Description                            | Mutates Original? |
| ---------- | -------------------------------------- | ----------------- |
| `splice()` | Adds/removes elements                  | ✅ Yes             |
| `slice()`  | Returns shallow copy                   | ❌ No              |
| `map()`    | Transforms elements                    | ❌ No              |
| `filter()` | Selects elements                       | ❌ No              |

[END]

[ZH]
# 🚀 参考架构

本文档作为 CS385 实验考试 3 的主要参考。它整合了基本语法、行为模式和调试协议。

---

## 1. JavaScript 核心机制

### 1.1 相等性矩阵

| 特性                  | `==` (宽松)         | `===` (严格)        | `Object.is()`       |
| --------------------- | ------------------- | ------------------- | ------------------- |
| 类型强制转换          | ✅ 是                | ❌ 否                | ❌ 否                |
| 对象比较              | 引用                | 引用                | 引用                |
| `NaN` vs `NaN`        | `false`             | `false`             | ✅ `true`            |

### 1.2 数组突变矩阵

| 方法       | 描述                                   | 修改原数组?       |
| ---------- | -------------------------------------- | ----------------- |
| `splice()` | 添加/删除元素                          | ✅ 是              |
| `slice()`  | 返回浅拷贝                             | ❌ 否              |
| `map()`    | 转换元素                               | ❌ 否              |
[END]
