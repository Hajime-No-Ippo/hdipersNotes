---
title: CS130 SQL 速查表：实验考试 3
date: 2025-12-15
categories: CS130, Database
tags: SQL, JOIN, Aggregate, Transaction, Relational Algebra, CASCADE
summary_zh: CS130 数据库实验考试 3 的核心 SQL 语法、关键易错点、DML/DDL 和级联效应检查清单。
---
[END]
[EN]
---
title: CS130 SQL Cheat Sheet - Lab Exam 3
date: 2025-12-15
categories: CS130, Database
tags: SQL, JOIN, Aggregate, Transaction, Relational Algebra, CASCADE
summary_en: A comprehensive guide covering core SQL syntax, critical tricky points, DML/DDL, and the CASCADE effect checklist for the CS130 Lab Exam 3.
---
[END]

[ZH] # 📚 CS130 SQL 速查表：终极双语版 [END]
[EN] # 📚 CS130 SQL Cheat Sheet: Ultimate Bilingual Version [END]

[ZH]
### [cite_start]考试信息 (Exam Information) [cite: 2]
* [cite_start]**时长 (Duration)**: 90 minutes [cite: 3]
* [cite_start]**问题数量 (Questions)**: 12 个 SQL 查询 (12 SQL queries) [cite: 3]
* [cite_start]**尝试次数 (Attempts)**: 2 次 (第一次尝试后有反馈) [cite: 3]
* [cite_start]**重要提醒 (Important)**: 必须**按顺序**回答问题 (删除/更新操作是顺序执行的) [cite: 3, 171]。
[END]
[EN]
### [cite_start]Exam Information [cite: 2]
* [cite_start]**Duration**: 90 minutes [cite: 3]
* [cite_start]**Questions**: 12 SQL queries [cite: 3]
* [cite_start]**Attempts**: 2 (feedback after first attempt) [cite: 3]
* [cite_start]**Important**: Answer questions IN ORDER (deletes/updates are sequential)[cite: 3, 171].
[END]

---

[cite_start][ZH] ## 1. SELECT 语句 (查询) [cite: 4] [END]
[cite_start][EN] ## 1. SELECT Statements (Queries) [cite: 4] [END]

| [cite_start][ZH] **功能** [cite: 5] | [cite_start][EN] **Function** [cite: 5] | [cite_start][ZH] **SQL 语法** [cite: 6] | [cite_start][EN] **SQL Syntax** [cite: 6] |
| :--- | :--- | :--- | :--- |
| [cite_start][ZH] 基础查询 [cite: 5] | [cite_start][EN] Basic Select [cite: 5] | [cite_start]`SELECT column1, column2 FROM TableName;` [cite: 6] | [cite_start]`SELECT column1, column2 FROM TableName;` [cite: 6] |
| [cite_start][ZH] 唯一值 [cite: 7] | [cite_start][EN] Unique Values [cite: 7] | [cite_start]`SELECT DISTINCT column FROM TableName;` [cite: 7] | [cite_start]`SELECT DISTINCT column FROM TableName;` [cite: 7] |
| [cite_start][ZH] 范围 (包含边界) [cite: 13, 137] | [cite_start][EN] Range (Inclusive) [cite: 13, 137] | [cite_start]`WHERE column BETWEEN 10 AND 50;` [cite: 13] | [cite_start]`WHERE column BETWEEN 10 AND 50;` [cite: 13] |
| [cite_start][ZH] 列表匹配 [cite: 14] | [cite_start][EN] List Match [cite: 14] | [cite_start]`WHERE column IN ('val1', 'val2');` [cite: 14] | [cite_start]`WHERE column IN ('val1', 'val2');` [cite: 14] |
| [cite_start][ZH] 空值 [cite: 15, 135] | [cite_start][EN] IS NULL [cite: 15, 135] | [cite_start]`WHERE column IS NULL` [cite: 15, 136] | [cite_start]`WHERE column IS NULL` [cite: 15, 136] |
| [cite_start][ZH] 排序 (降序) [cite: 23] | [cite_start][EN] Ordering (DESC) [cite: 23] | [cite_start]`ORDER BY column DESC;` [cite: 23] | [cite_start]`ORDER BY column DESC;` [cite: 23] |
| [cite_start][ZH] 限制行数 [cite: 23] | [cite_start][EN] Limit Rows [cite: 23] | [cite_start]`LIMIT 10;` [cite: 23] | [cite_start]`LIMIT 10;` [cite: 23] |

[ZH]
* [cite_start]**模式匹配 (Pattern Matching)** [cite: 19][cite_start]: `WHERE column LIKE 'pattern%';` (以 `pattern` 开头) [cite: 20]
* [cite_start]**正则表达式 (Regex)**: `WHERE column ~* '^pattern.*\$';` [cite: 21]
[END]
[EN]
* [cite_start]**Pattern Matching**: `WHERE column LIKE 'pattern%';` (Starts with `pattern`) [cite: 20]
* [cite_start]**Regex**: `WHERE column ~* '^pattern.*\$';` [cite: 21]
[END]

---

[ZH] ## 2. DML 语句 (修改数据) [END]
[EN] ## 2. DML Statements (Data Modification) [END]

### [cite_start][ZH] 2.1 UPDATE (更新) [cite: 24] [END]
### [cite_start][EN] 2.1 UPDATE [cite: 24] [END]
* [cite_start][ZH] **基本语法**: `UPDATE TableName SET column = 'new_value' WHERE condition:` [cite: 27, 28]
* [cite_start][EN] **Basic Syntax**: `UPDATE TableName SET column = 'new_value' WHERE condition:` [cite: 27, 28]

* [cite_start][ZH] **🚨 关键：百分比计算 (Percentage Calculations)**[cite: 30]: [END]
* [cite_start][EN] **🚨 CRITICAL: Percentage Calculations**[cite: 30]: [END]
    * [cite_start][ZH] 减少 8% (保留 92%): `UPDATE Table SET price = price * 0.92 WHERE cond;` [cite: 30, 31, 32, 142, 143, 167]
    * [cite_start][EN] Decrease by 8% (keep 92%): `UPDATE Table SET price = price * 0.92 WHERE cond;` [cite: 30, 31, 32, 142, 143, 167]
    * [cite_start][ZH] 增加 10%: `UPDATE Table SET price = price * 1.10 WHERE cond;` [cite: 30, 31, 32, 147, 148, 149]
    * [cite_start][EN] Increase by 10%: `UPDATE Table SET price = price * 1.10 WHERE cond;` [cite: 30, 31, 32, 147, 148, 149]

### [cite_start][ZH] 2.2 DELETE (删除) [cite: 33] [END]
### [cite_start][EN] 2.2 DELETE [cite: 33] [END]
* [cite_start][ZH] **基本语法**: `DELETE FROM TableName WHERE condition:` [cite: 34]
* [cite_start][EN] **Basic Syntax**: `DELETE FROM TableName WHERE condition:` [cite: 34]
* [cite_start][ZH] **空值删除**: `DELETE FROM TableName WHERE column IS NULL:` [cite: 36]
* [cite_start][EN] **Delete NULL Rows**: `DELETE FROM TableName WHERE column IS NULL:` [cite: 36]

---

[cite_start][ZH] ## 3. JOIN 查询 (连接) [cite: 38] [END]
[cite_start][EN] ## 3. JOIN Queries [cite: 38] [END]

* [cite_start][ZH] **黄金法则**: $n$ 个表需要 $n-1$ 个连接条件 [cite: 127, 163, 177]。 [END]
* [cite_start][EN] **Golden Rule**: $n$ tables require $n-1$ join conditions[cite: 127, 163, 177]. [END]

### [cite_start][ZH] 3.1 逗号 + WHERE (Lab 标准) [cite: 39] [END]
### [cite_start][EN] 3.1 Comma + WHERE (Lab Standard) [cite: 39] [END]

```sql
[ZH]
SELECT T1.col, T2.col
FROM Table1 AS T1, Table2 AS T2
WHERE (T1.key = T2.key) AND (额外过滤条件);
[END]
[EN]
SELECT T1.col, T2.col
FROM Table1 AS T1, Table2 AS T2
WHERE (T1.key = T2.key) AND (additional_filters);
[END]
[ZH] ## 4. 聚合函数与分组 1 [END][EN] ## 4. Aggregate Functions and GROUP BY 2 [END][ZH] 函数 [EN] Function [ZH] SQL 语法 [EN] SQL Syntax [ZH] 计数 7[EN] Count 8SELECT COUNT(*) FROM TableName; 9SELECT COUNT(*) FROM TableName; 10[ZH] 平均值 11[EN] Average 12SELECT AVG(column) FROM TableName; 13SELECT AVG(column) FROM TableName; 14[ZH] 四舍五入 15[EN] Round 16SELECT ROUND(AVG(column), 2) FROM TableName; 17SELECT ROUND(AVG(column), 2) FROM TableName; 18[ZH] 4.1 GROUP BY 分组 19 [END][EN] 4.1 GROUP BY 20 [END][ZH] 基本分组: SELECT column, COUNT(*) FROM TableName GROUP BY column; 21[EN] Basic Grouping: SELECT column, COUNT(*) FROM TableName GROUP BY column; 22[ZH] 🚨 关键：COUNT vs SUM23: [END][EN] 🚨 CRITICAL: COUNT vs SUM24: [END][ZH] 正确: COUNT(*) 统计行数 25。 [END][EN] Correct: COUNT(*) counts rows26. [END][ZH] 错误: SUM() 会将列值相加 27。 [END][EN] WRONG: SUM() adds column values28. [END][ZH] ## 5. DDL/事务与级联 [END][EN] ## 5. DDL / Transactions and CASCADE [END][ZH] 5.1 CREATE TABLE (创建表) 29 [END][EN] 5.1 CREATE TABLE 30 [END][ZH] 带级联外键 (With CASCADE Foreign Keys): 31[EN] With CASCADE Foreign Keys: 32SQL[ZH]
CREATE TABLE RelationshipTable (
    col1 TEXT NOT NULL REFERENCES Parent1(key) ON UPDATE CASCADE ON DELETE CASCADE,
    col2 INTEGER NOT NULL REFERENCES Parent2(key) ON UPDATE CASCADE ON DELETE CASCADE,
    CONSTRAINT RelTable_PKEY PRIMARY KEY (col1, col2)
);
[END]
[EN]
CREATE TABLE RelationshipTable (
    col1 TEXT NOT NULL REFERENCES Parent1(key) ON UPDATE CASCADE ON DELETE CASCADE,
    col2 INTEGER NOT NULL REFERENCES Parent2(key) ON UPDATE CASCADE ON DELETE CASCADE,
    CONSTRAINT RelTable_PKEY PRIMARY KEY (col1, col2)
);
[END]
[ZH] 5.2 事务块 (Transaction Blocks) 33 [END][EN] 5.2 Transaction Blocks 34 [END][ZH] 用途: 用于安全测试 DML (DELETE/UPDATE) 的影响 35。 [END][EN] Purpose: Used for safely testing DML (DELETE/UPDATE) effects36. [END]SQL[ZH]
BEGIN; -- 开始事务 [cite: 93, 97]
-- DML 语句在这里 [cite: 94]
SELECT COUNT(*) FROM AffectedTable;
ROLLBACK; -- 撤销所有更改 (Undo all changes) [cite: 96, 99, 105]
-- COMMIT; -- 永久保存更改 (Save changes permanently) [cite: 95, 98]
[END]
[EN]
BEGIN; -- Start Transaction [cite: 93, 97]
-- DML statements here [cite: 94]
SELECT COUNT(*) FROM AffectedTable;
ROLLBACK; -- Undo all changes [cite: 96, 99, 105]
-- COMMIT; -- Save changes permanently [cite: 95, 98]
[END]
[ZH] ## 6. CRITICAL: 级联效应与检查清单 [END][EN] ## 6. CRITICAL: CASCADE & Checklist [END][ZH] 6.1 CRITICAL: 级联效应 (CASCADE) 影响统计 37 [END][EN] 6.1 CRITICAL: CASCADE Effect Row Count 38 [END][ZH] 计算方法: 必须统计主表和所有关联表受影响的行数总和 393939393939393939393939。 [END][EN] Calculation: MUST count the total number of affected rows in the main table AND all related tables404040404040404040404040. [END]SQL[ZH]
-- 统计受影响的总行数
SELECT COUNT(*) FROM MainTable WHERE condition -- (主表行数) [cite: 68]
UNION
SELECT COUNT(*) FROM RelatedTable WHERE condition; -- (关联表行数) [cite: 69]
-- 将 UNION 的结果加总得到最终答案！ [cite: 70]
[END]
[EN]
-- Count Total Affected Rows
SELECT COUNT(*) FROM MainTable WHERE condition -- (Main Table Rows) [cite: 68]
UNION
SELECT COUNT(*) FROM RelatedTable WHERE condition; -- (Related Table Rows) [cite: 69]
-- Sum the results of the UNION to get the final answer! [cite: 70]
[END]
[ZH] 6.2 易错点总结 (Tricky Points) 41 [END][EN] 6.2 Common Mistakes 42 [END][ZH] 易错点 [EN] Mistake [ZH] 正确做法 [EN] Correction [ZH] NULL 比较 47[EN] NULL Comparison 48WHERE col IS NULL 49WHERE col IS NULL 50[ZH] 缺少 JOIN 条件 51[EN] Missing Join Cond. 52$n$ tables $= n-1$ joins 53$n$ tables $= n-1$ joins 54[ZH] 百分比错误 55[EN] Percentage Math 56$\times 0.92$ (减 8%) 57$\times 0.92$ (Decrease 8%) 58[ZH] AND/OR 逻辑 59[EN] AND/OR Logic 60使用 () 明确分组 61Use () for grouping 62[ZH] 6.3 最终检查清单 (Final Checklist) 63 [END][EN] 6.3 Final Checklist 64 [END][ZH] 是否使用了 IS NULL 而不是 = NULL? 65656565 [END][EN] Used IS NULL not = NULL? 66666666 [END][ZH] 是否所有 JOIN 条件都存在？ 67 [END][EN] All join conditions present? 68 [END][ZH] 是否计算了 CASCADE 效应，以统计受影响的总行数？ 69696969 [END][EN] Counted CASCADE effects for total rows affected? 70707070 [END][ZH] 百分比计算正确吗 (例如，减少 $8\% = \times 0.92$)? 71 [END][EN] Percentage math correct (e.g., decrease $8\% = \times 0.92$)? 72 [END][ZH] BETWEEN 包含边界，这是你想要的吗？ 73737373 [END][EN] BETWEEN includes boundaries - is that what you want? 74747474 [END][ZH] ## 7. 关系代数 (Relational Algebra, RA) 75 [END][EN] ## 7. Relational Algebra (RA) 76 [END][ZH] 符号 [EN] Symbol [ZH] 名称 [EN] Name [ZH] SQL 等价物 [EN] SQL Equivalent $\sigma$ (sigma)$\sigma$ (sigma)[ZH] 选择 [END][EN] Selection [END]WHERE 83WHERE 84$\pi$ (pi)$\pi$ (pi)[ZH] 投影 [END][EN] Projection [END]SELECT columns 85SELECT columns 86$\times$$\times$[ZH] 笛卡尔积 [END][EN] Cartesian Product [END]FROM T1, T2 (无 WHERE 连接) 87FROM T1, T2 (no WHERE join) 88$\bowtie$$\bowtie$[ZH] 自然连接 [END][EN] Natural Join [END]FROM T1, T2 WHERE T1.key = T2.key 89FROM T1, T2 WHERE T1.key = T2.key 90$\cup$$\cup$[ZH] 联合 [END][EN] Union [END]