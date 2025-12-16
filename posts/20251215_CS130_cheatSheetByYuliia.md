---
title: "CS130 Lab Exam 3: Official Cheat Sheet (Yuliia's Edition)"
title_en: "CS130 Lab Exam 3: Official Cheat Sheet (Yuliia's Edition)"
title_zh: "CS130 实验考试 3：官方速查表 (Yuliia 版)"
date: 2025-12-15
author: "Yuliia"
categories: 
  - "CS130"
  - "Community Contribution"
tags: 
  - "SQL"
  - "CheatSheet"
  - "PostgreSQL"
summary_en: "A comprehensive cheat sheet contributed by Yuliia, covering all 15 sections including Common Mistakes, Regex, Relational Algebra, and Transaction blocks."
summary_zh: "由 Yuliia 贡献的综合速查表，涵盖常见错误、正则表达式、关系代数和事务块等所有 15 个章节。"
---

[EN]
![CS130_cheatSheet](images/20251215_CS130_cheatSheet.png)

# 📚 Exam Information

| Item             | Details                                                        |
| :--------------- | :------------------------------------------------------------- |
| **Duration**     | 90 minutes                                                     |
| **Questions**    | 12 SQL queries                                                 |
| **Attempts**     | 2 (feedback after first attempt)                               |
| **⚠️ IMPORTANT** | Answer questions **IN ORDER** (deletes/updates are sequential) |

---

# 1. SELECT Statements

### Basic & Filtering

- **Basic:** `SELECT column1, column2 FROM TableName;`
- **Distinct:** `SELECT DISTINCT column FROM TableName;`
- **Where:** `WHERE column > 100`
- **Between:** `WHERE column BETWEEN 10 AND 50`
- **In List:** `WHERE column IN ('val1', 'val2')`
- **Nulls:** `WHERE column IS NULL` or `IS NOT NULL`

### Multiple Conditions

- `WHERE (condition1) AND (condition2);`
- `WHERE (condition1) OR (condition2);`

### Pattern Matching

- **LIKE:** `'pattern%'` (starts with) or `'%pattern%'` (contains)
- **Regex:** `~* '^pattern.*$'`

### Ordering

- `ORDER BY column ASC;` (Ascending)
- `ORDER BY column DESC;` (Descending)

[END]

[ZH]
# 📚 考试信息

| 项目             | 详情                                                           |
| :--------------- | :------------------------------------------------------------- |
| **时长**         | 90 分钟                                                        |
| **问题**         | 12 道 SQL 查询                                                 |
| **尝试次数**     | 2 次（第一次尝试后有反馈）                                     |
| **⚠️ 重要**      | **按顺序**回答问题（删除/更新是连续的）                        |

---

# 1. SELECT 语句

### 基本与过滤

- **基本：** `SELECT column1, column2 FROM TableName;`
- **去重：** `SELECT DISTINCT column FROM TableName;`
- **Where：** `WHERE column > 100`
- **Between：** `WHERE column BETWEEN 10 AND 50`
- **In List：** `WHERE column IN ('val1', 'val2')`
- **Nulls：** `WHERE column IS NULL` 或 `IS NOT NULL`
[END]
