---
title: "CS130 Lab Exam 3: Operational Manual"
title_en: "CS130 Lab Exam 3: Operational Manual"
title_zh: "CS130 实验考试 3：操作手册"
date: 2025-12-12
author: "Dong Li"
categories: 
  - "CS130"
  - "SQL"
tags: 
  - "PostgreSQL"
  - "Relational Algebra"
  - "Exam Strategy"
summary_en: "A comprehensive operational manual for CS130 Lab Exam 3. Covers SQL command syntax (SELECT, JOIN, UPDATE), Relational Algebra to SQL mapping, and execution protocols."
summary_zh: "CS130 实验考试 3 的综合操作手册。涵盖 SQL 命令语法 (SELECT, JOIN, UPDATE)、关系代数到 SQL 的映射以及执行协议。"
---

[EN]
![CS130 SQL Exam Header Image](images/20251212_CS130_Lab_Exam3_Cheatsheet.png)

# 1. Operational Protocol

**Constraints:** OPEN BOOK, but strictly limited to personal notes and official documentation. No AI assistance.
**Parameters:**
- **Duration**: 90 minutes
- **Weight**: 25% of final grade
- **Scope**: 12 SQL queries

### Execution Workflow

1.  **Initialization**: Execute the provided `.sql` script to instantiate the schema.
2.  **Sequential Execution**: Queries must be executed in order. `UPDATE` and `DELETE` operations are state-mutating; skipping order will corrupt the dataset for subsequent questions.
3.  **Validation Loop**:
    *   Attempt 1: Execute query. Submit. Analyze feedback.
    *   Attempt 2: Refine logic based on feedback. Submit final.

---

# 2. SQL Command Reference

### 2.1 Data Retrieval (SELECT)

The primary mechanism for data extraction.

```sql
SELECT
    column1,
    COUNT(column2) AS alias_name
FROM
    table1
WHERE
    column3 > 100 AND column4 IS NOT NULL
ORDER BY
    column1 ASC;
```

### 2.2 Relational Algebra Mapping

*   **Selection ($\sigma$)**: Maps to `WHERE`. Filters rows.
*   **Projection ($\pi$)**: Maps to `SELECT`. Filters columns.
*   **Natural Join ($\bowtie$)**: Maps to `NATURAL JOIN` or `JOIN ON`.
*   **Rename ($\rho$)**: Maps to `AS`.

[END]

[ZH]
# 1. 操作协议

**约束：** 开卷，但严格限制为个人笔记和官方文档。禁止 AI 辅助。
**参数：**
- **时长**：90 分钟
- **权重**：占总成绩的 25%
- **范围**：12 道 SQL 查询

### 执行工作流

1.  **初始化**：执行提供的 `.sql` 脚本以实例化架构。
2.  **顺序执行**：查询必须按顺序执行。`UPDATE` 和 `DELETE` 操作是状态突变的；跳过顺序将破坏后续问题的数据集。
3.  **验证循环**：
    *   尝试 1：执行查询。提交。分析反馈。
    *   尝试 2：根据反馈优化逻辑。提交最终结果。

---

# 2. SQL 命令参考

### 2.1 数据检索 (SELECT)

数据提取的主要机制。

```sql
SELECT
    column1,
    COUNT(column2) AS alias_name
FROM
    table1
WHERE
    column3 > 100 AND column4 IS NOT NULL
ORDER BY
    column1 ASC;
```
[END]
