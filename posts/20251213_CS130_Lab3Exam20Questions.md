---
title: "CS130: Simulation Drill (20 Questions)"
title_en: "CS130: Simulation Drill (20 Questions)"
title_zh: "CS130：模拟演练 (20 题)"
date: 2025-12-13
author: "Dong Li"
categories: 
  - "CS130"
  - "Simulation"
tags: 
  - "SQL"
  - "Relational Algebra"
  - "Drill"
summary_en: "A 20-question simulation drill designed to test SQL syntax and Relational Algebra comprehension. Includes fixed Unicode symbols for RA notation."
summary_zh: "包含 20 个问题的模拟演练，旨在测试 SQL 语法和关系代数理解。包含用于 RA 符号的修正 Unicode 符号。"
---

[EN]
![CS130_cheatSheet](images/20251213_CS130_Lab3Exam20Questions.png)

# 📚 Essentials Refresher

### 1. Execution Pipeline
`FROM` $\to$ `WHERE` $\to$ `GROUP BY` $\to$ `HAVING` $\to$ `SELECT` $\to$ `ORDER BY`

### 2. Relational Algebra (RA) Notation

*   **$\sigma$ (Sigma) = Selection**: Filters **ROWS**. Maps to SQL `WHERE`.
*   **$\pi$ (Pi) = Projection**: Selects **COLUMNS**. Maps to SQL `SELECT`.
*   **$\bowtie$ (Bowtie) = Natural Join**: Joins tables on common columns.
*   **$\rho$ (Rho) = Rename**: Renames entity. Maps to SQL `AS`.

> **⚠️ Critical Distinction:**
> *   RA Selection ($\sigma$) $\neq$ SQL `SELECT`.
> *   RA Selection filters rows. RA Projection picks columns.

---

# ⚔️ Simulation Drill

## 🟢 Level 1: Syntax Validation

### Q1: Distinct Values
**Task:** Retrieve a list of unique `cities` from the `Students` table.
*   Schema: `Students(id, name, city, gpa)`

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT DISTINCT city FROM Students;
```
</details>

[END]

[ZH]
# 📚 核心复习

### 1. 执行流水线
`FROM` $\to$ `WHERE` $\to$ `GROUP BY` $\to$ `HAVING` $\to$ `SELECT` $\to$ `ORDER BY`

### 2. 关系代数 (RA) 符号

*   **$\sigma$ (Sigma) = 选择**: 过滤 **行**。映射到 SQL `WHERE`。
*   **$\pi$ (Pi) = 投影**: 选择 **列**。映射到 SQL `SELECT`。
*   **$\bowtie$ (Bowtie) = 自然连接**: 在公共列上连接表。
*   **$\rho$ (Rho) = 重命名**: 重命名实体。映射到 SQL `AS`。

> **⚠️ 关键区别：**
> *   RA 选择 ($\sigma$) $\neq$ SQL `SELECT`。
> *   RA 选择过滤行。RA 投影选择列。

---

# ⚔️ 模拟演练

## 🟢 第一级：语法验证

### Q1: 唯一值
**任务：** 从 `Students` 表中检索唯一的 `cities` 列表。
*   架构：`Students(id, name, city, gpa)`

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 显示答案</summary>

```sql
SELECT DISTINCT city FROM Students;
```
</details>
[END]
