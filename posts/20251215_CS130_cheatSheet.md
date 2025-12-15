---
title: CS130 Lab Exam 3: SQL Official Cheat Sheet (Full Version)
title_en: CS130 Lab Exam 3: SQL Official Cheat Sheet (Full Version)
title_zh: CS130 实验考试 3：SQL 官方速查表 (完整版)
date: 2025-12-13
categories: CS130
tags: SQL, CheatSheet, Relational Algebra, PostgreSQL, ExamPrep
summary_en: The complete, official cheat sheet including Transactions, Cascade counting, Regex, and DDL.
summary_zh: 包含事务处理、级联计数、正则和 DDL 的完整官方速查表。
---

[EN]
![CS130_cheatSheet](images/20251215_CS130_cheatSheet.png)
# 📚 Exam Information & Rules

| Item | Details |
| :--- | :--- |
| **Duration** | 90 minutes |
| **Questions** | 12 SQL queries |
| **Attempts** | 2 (feedback available after first attempt) |
| **⚠️ CRITICAL** | [cite_start]Answer questions **IN ORDER** (deletes/updates are sequential) [cite: 3] |

---

# 1. SELECT Statements

### Basic & Filtering
* [cite_start]**Select All:** `SELECT * FROM TableName;` [cite: 6]
* [cite_start]**Distinct:** `SELECT DISTINCT column FROM TableName;` [cite: 7]
* [cite_start]**Comparison:** `WHERE column > 100` [cite: 12]
* [cite_start]**Range (Inclusive):** `WHERE column BETWEEN 10 AND 50` (Includes 10 & 50!) [cite: 13]
* [cite_start]**List Check:** `WHERE column IN ('val1', 'val2')` [cite: 14]

> **💀 NULL TRAP:**
> Never use `= NULL`. [cite_start]Always use `IS NULL` or `IS NOT NULL`. [cite: 15, 16]

### [cite_start]Pattern Matching (LIKE & Regex) [cite: 19, 21]
* **Contains:** `LIKE '%pattern%'`
* **Starts with:** `LIKE 'pattern%'`
* **Regex (Case-Insensitive):** `WHERE column ~* '^pattern.*$'`

### [cite_start]Ordering & Limits [cite: 23]
* **Ascending:** `ORDER BY column ASC`
* **Descending:** `ORDER BY column DESC`
* **Limit:** `LIMIT 10`

---

# 2. UPDATE Statements

### Syntax
```sql
UPDATE TableName SET col1 = 'val1' WHERE condition;

```

Percentage Math (Don't fail this!) 

* **Decrease by 8%:** Multiply by **0.92** (1.00 - 0.08).
* `UPDATE Table SET price = price * 0.92 WHERE cond;`


* **Increase by 10%:** Multiply by **1.10**.
* `UPDATE Table SET price = price * 1.10 WHERE cond;`



---

3. DELETE Statements 

* **Standard:** `DELETE FROM TableName WHERE condition;`
* **Safety:** Always check what you are deleting first (See Section 10: Transactions).

---

#4. JOIN QueriesMethod 1: Comma + WHERE (Used in Labs) 

*This is the style often used in CS130 labs.*

```sql
SELECT T1.col, T2.col
FROM Table1 AS T1, Table2 AS T2
WHERE (T1.key = T2.key) AND (filters);

```

Method 2: JOIN ... ON 

```sql
SELECT T1.col, T2.col
FROM Table1 AS T1
JOIN Table2 AS T2 ON T1.key = T2.key
WHERE filters;

```

> 
> **⚠️ N Tables Rule:** If you join **N** tables, you need **N-1** join conditions. 
> 
> 

---

5. Aggregate Functions 

| Function | Usage | Note |
| --- | --- | --- |
| **COUNT(*)** | `SELECT COUNT(*) ...` | Counts **rows**. |
| **SUM(col)** | `SELECT SUM(cost) ...` | Adds up **values**. |
| **AVG(col)** | `SELECT ROUND(AVG(cost), 2) ...` | Averages values. `ROUND(x, 2)` keeps 2 decimals. 

 |

GROUP BY Rule 

If a column is in `SELECT` but NOT in an aggregate function, it **MUST** be in `GROUP BY`.

---

6. CASCADE Effects (CRITICAL!) 

When deleting/updating a row, if `ON DELETE CASCADE` is set, related rows in other tables are also deleted.
**Exam Task:** Calculate TOTAL rows affected.

**The Strategy (Union):** 

```sql
SELECT COUNT(*) FROM MainTable WHERE key = 'value'
UNION
SELECT COUNT(*) FROM RelatedTable WHERE key = 'value';

```

> **Math:** Result 1 + Result 2 = **Total Rows Affected**.

---

7. CREATE TABLE 

```sql
CREATE TABLE TableName (
    col1 DATATYPE NOT NULL,
    col2 DATATYPE NOT NULL,
    CONSTRAINT TableName_PKEY PRIMARY KEY (col1)
);

```

**With Foreign Keys & Cascade:** 

```sql
... REFERENCES Parent(key) ON UPDATE CASCADE ON DELETE CASCADE

```

---

8. ALTER TABLE 

* **Add Column:** `ALTER TABLE TableName ADD COLUMN columnname DATATYPE;`

---

9. Transaction Blocks (Safe Testing) 

Use this to test a `DELETE` or `UPDATE` without permanently ruining the data.

```sql
BEGIN;          -- 1. Start transaction
SELECT ...;     -- 2. Check data before
DELETE ...;     -- 3. Perform risky operation
SELECT ...;     -- 4. Check if it looks right
ROLLBACK;       -- 5. Undo everything!

```

*(Only use `COMMIT;` if you are 100% sure and the question asks for it)*

---

#10. Regex & Operators ReferenceOperators 

| Op | Meaning | Op | Meaning |
| --- | --- | --- | --- |
| `=` | Equals | `IS NULL` | Is Empty |
| `<>` | Not Equals | `IN (...)` | In a list |
| `~*` | Regex (Case-insensitive) | `BETWEEN` | Inclusive Range |

Regex Patterns 

| Symbol | Meaning | Example |
| --- | --- | --- |
| `^` | Start of string | `^CS` (Starts with CS) |
| `$` | End of string | `Road$` (Ends with Road) |
| `.` | Any char | `A.B` (A, anything, B) |
| `*` | 0 or more | `.*` (Anything) |
| `+` | 1 or more | `.+` (At least one char) |
| `\d` | Digit | `\d{4,}` (4+ digits) |
| `|` | OR | `(A|B)` (A or B) |

---

11. Relational Algebra (RA) Symbols 

| Symbol | Name | SQL Mapping |
| --- | --- | --- |
| **σ** (Sigma) | Selection | `WHERE` (Filters Rows) |
| **π** (Pi) | Projection | `SELECT` (Selects Columns) |
| **×** | Cartesian | `FROM T1, T2` (No Join condition) |
| **⨝** | Natural Join | `WHERE T1.key = T2.key` |
| **∪** | Union | `UNION` |
| **∧** | AND | `AND` |
| **∨** | OR | `OR` |

[END]

[ZH]
![CS130_cheatSheet](images/20251215_CS130_cheatSheet.png)

#📚 考试信息与规则 (官方版)| 项目 | 详情 |
| --- | --- |
| **时长** | 90 分钟 |
| **题量** | 12 道 SQL 查询题 |
| **尝试次数** | 2 次 (第一次尝试后会有反馈) |
| **⚠️ 关键点** | 必须**按顺序**答题 (因为删除/更新操作是连贯的，跳题会导致数据对不上) 

 |

---

#1. SELECT 语句 (查询)###基础与筛选* 
**查所有:** `SELECT * FROM TableName;` 


* 
**去重:** `SELECT DISTINCT column FROM TableName;` 


* 
**范围 (包含边界):** `WHERE column BETWEEN 10 AND 50` (包含 10 和 50!) 


* 
**列表检查:** `WHERE column IN ('val1', 'val2')` 



> **💀 空值陷阱 (NULL TRAP):**
> 永远不要写 `= NULL`。必须使用 `IS NULL` 或 `IS NOT NULL`。 
> 
> 

模式匹配 (LIKE 与 正则) 

* **模糊匹配:** `LIKE '%pattern%'` (包含)
* **正则 (忽略大小写):** `WHERE column ~* '^pattern.*$'`

排序与限制 

* **升序:** `ORDER BY column ASC`
* **降序:** `ORDER BY column DESC`
* **限制行数:** `LIMIT 10`

---

#2. UPDATE 语句 (更新)###语法```sql
UPDATE TableName SET col1 = 'val1' WHERE condition;

```

百分比数学题 (千万别算错!) 

* **减少 8% (打92折):** 乘以 **0.92** (1.00 - 0.08)。
* `UPDATE Table SET price = price * 0.92 WHERE cond;`


* **增加 10% (涨价):** 乘以 **1.10**。
* `UPDATE Table SET price = price * 1.10 WHERE cond;`



---

3. DELETE 语句 (删除) 

* **标准写法:** `DELETE FROM TableName WHERE condition;`
* **安全建议:** 在删除前，先用 SELECT 确认你要删的是什么 (参见第 9 节: 事务)。

---

#4. JOIN 查询 (连接表)方法 1: 逗号隔开 (实验室常用) 

*这是 CS130 Lab 中最常见的写法。*

```sql
SELECT T1.col, T2.col
FROM Table1 AS T1, Table2 AS T2
WHERE (T1.key = T2.key) AND (其他过滤条件);

```

方法 2: 标准写法 (JOIN ON) 

```sql
SELECT T1.col, T2.col
FROM Table1 AS T1
JOIN Table2 AS T2 ON T1.key = T2.key
WHERE 其他过滤条件;

```

> 
> **⚠️ N 表定律:** 如果你要连接 **N** 张表，你必须写 **N-1** 个连接条件 (Join Conditions)。 
> 
> 

---

5. 聚合函数与分组 

| 函数 | 用法 | 注意 |
| --- | --- | --- |
| **COUNT(*)** | `SELECT COUNT(*) ...` | 统计**行数**。 |
| **SUM(col)** | `SELECT SUM(cost) ...` | 统计数值总和 (别用来数行数!)。 |
| **AVG(col)** | `SELECT ROUND(AVG(cost), 2) ...` | 平均值。`ROUND(x, 2)` 保留两位小数。 

 |

GROUP BY 铁律 

如果在 SELECT 中出现了一个列，且它**没有**被包含在聚合函数(如 COUNT, SUM)中，那么它**必须**出现在 `GROUP BY` 子句里。

---

6. 级联效应 (CASCADE) - 必考点! 

当删除/更新某行时，如果表定义了 `ON DELETE CASCADE`，关联表的数据也会被自动删除。
**考试任务:** 计算受影响的**总行数**。

**解题策略 (使用 UNION):** 

```sql
SELECT COUNT(*) FROM MainTable WHERE key = 'value'
UNION
SELECT COUNT(*) FROM RelatedTable WHERE key = 'value';

```

> **计算:** 结果 1 (主表删除数) + 结果 2 (关联表级联删除数) = **受影响总行数**。

---

7. CREATE TABLE (建表) 

```sql
CREATE TABLE TableName (
    col1 DATATYPE NOT NULL,
    col2 DATATYPE NOT NULL,
    CONSTRAINT TableName_PKEY PRIMARY KEY (col1)
);

```

**外键与级联设置:** 

```sql
... REFERENCES Parent(key) ON UPDATE CASCADE ON DELETE CASCADE

```

---

8. ALTER TABLE (修改表) 

* **添加列:** `ALTER TABLE TableName ADD COLUMN columnname DATATYPE;`

---

9. 事务块 (安全测试大法) 

考试神器！用于测试 `DELETE` 或 `UPDATE` 是否正确，而不会真的毁掉数据。

```sql
BEGIN;          -- 1. 开启事务
SELECT ...;     -- 2. 操作前先看一眼数据
DELETE ...;     -- 3. 执行危险操作
SELECT ...;     -- 4. 再次查询，确认删对了吗？
ROLLBACK;       -- 5. 回滚！撤销所有操作，就像什么都没发生过。

```

*(除非题目明确要求永久保存，否则尽量使用 ROLLBACK 恢复现场)*

---

#10. 正则表达式与符号参考常用操作符 

| 符号 | 含义 | 符号 | 含义 |
| --- | --- | --- | --- |
| `=` | 等于 | `IS NULL` | 为空 |
| `<>` | 不等于 | `IN (...)` | 在列表中 |
| `~*` | 正则 (忽略大小写) | `BETWEEN` | 范围 (包含边界) |

正则符号 (PostgreSQL) 

| 符号 | 含义 | 例子 |
| --- | --- | --- |
| `^` | 字符串开头 | `^CS` (以 CS 开头) |
| `$` | 字符串结尾 | `Road$` (以 Road 结尾) |
| `.` | 任意字符 | `A.B` (A, 任意, B) |
| `*` | 0次或多次 | `.*` (任意内容) |
| `+` | 1次或多次 | `.+` (至少有一个字符) |
| `\d` | 数字 | `\d{4,}` (4个以上数字) |
| `|` | 或 (OR) | `(A|B)` (A 或者 B) |

---

11. 关系代数 (RA) 符号对照 

| 符号 | 名称 | SQL 对应 |
| --- | --- | --- |
| **σ** (Sigma) | 选择 (Selection) | `WHERE` (筛选行) |
| **π** (Pi) | 投影 (Projection) | `SELECT` (筛选列) |
| **×** | 笛卡尔积 | `FROM T1, T2` (无连接条件) |
| **⨝** | 自然连接 | `WHERE T1.key = T2.key` |
| **∪** | 并集 | `UNION` |
| **∧** | AND | `AND` |
| **∨** | OR | `OR` |

[END]