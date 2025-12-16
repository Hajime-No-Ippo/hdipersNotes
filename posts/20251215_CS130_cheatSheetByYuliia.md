---
title: CS130 Lab Exam 3: SQL Official Cheat Sheet (By Yuliia)
title_en: CS130 Lab Exam 3: SQL Official Cheat Sheet (By Yuliia)
title_zh: CS130 实验考试 3：SQL 官方速查表 (by Yuliia)
date: 2025-12-15
categories: CS130
tags: SQL, CheatSheet, Relational Algebra, PostgreSQL, ExamPrep
summary_en: The comprehensive official cheat sheet covering all 15 sections including Common Mistakes, Regex, Relational Algebra, and Transaction blocks.
summary_zh: 包含常见错误、正则表达式、关系代数和事务块等所有15个章节的完整官方速查表。
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
- `LIMIT 10;`

---

# 2. UPDATE Statements

### Syntax

```sql
UPDATE TableName SET col1 = 'val1', col2 = 'val2' WHERE condition;

```

### Percentage Calculations

- **Decrease by 8%:** `SET price = price * 0.92`

- **Increase by 10%:** `SET price = price * 1.10`

---

# 3. DELETE Statements

- **Standard:** `DELETE FROM TableName WHERE condition;`

- **Null Check:** `DELETE FROM TableName WHERE column IS NULL;`

- **List Check:** `DELETE FROM TableName WHERE column IN ('val1', 'val2');`

---

# 4. JOIN Queries

### Method 1: Comma + WHERE (Used in Labs)

```sql
SELECT T1.col, T2.col
FROM Table1 AS T1, Table2 AS T2
WHERE (T1.key = T2.key) AND (filters);

```

### Method 2: JOIN ... ON

```sql
SELECT T1.col, T2.col
FROM Table1 AS T1
JOIN Table2 AS T2 ON T1.key = T2.key
WHERE filters;

```

> **Example:** 3-Table Join requires linking `S.Student_ID = E.Student_ID` AND `E.ModuleID = M.ModuleID`.

---

# 5. Aggregate Functions

- **Count Rows:** `SELECT COUNT(*) FROM TableName;`

- **Average (Rounded):** `SELECT ROUND(AVG(column), 2) ...`

- **Sum:** `SELECT SUM(column) ...`

- **Group By:** `SELECT column, COUNT(*) FROM TableName GROUP BY column;`

---

# 6. CASCADE Effects (CRITICAL!)

- **Rule:** When deleting/updating, count rows in **BOTH** main table **AND** related tables.

- **Strategy:** Use `UNION ALL` and sum counts together.

```sql
SELECT SUM(cnt) AS total
FROM (
    SELECT COUNT(*) AS cnt FROM MainTable ...
    UNION ALL
    SELECT COUNT(*) AS cnt FROM RelatedTable ...
) s;

```

- **Math:** Add the counts (with `UNION ALL`) = **TOTAL rows affected**.

---

# 7. CREATE TABLE

```sql
CREATE TABLE TableName (
    col1 DATATYPE NOT NULL,
    CONSTRAINT TableName_PKEY PRIMARY KEY (col1)
);

```

**With Foreign Keys:**

```sql
... REFERENCES Parent(key) ON UPDATE CASCADE ON DELETE CASCADE

```

---

# 8. ALTER TABLE

- **Add Column:** `ALTER TABLE TableName ADD COLUMN columnname DATATYPE;`

---

# 9. INSERT Statements

- **Syntax:** `INSERT INTO TableName (col1, col2) VALUES ('val1', 'val2');`

---

# 10. Transaction Blocks

- **Start:** `BEGIN;`

- **Save:** `COMMIT;`

- **Undo:** `ROLLBACK;`

- **Testing Strategy:** Run `BEGIN`, check data, run `DELETE`, check data again, then `ROLLBACK` to undo if testing.

---

# 11. Operators Reference

| Operator     | Meaning                  | Example                         |
| ------------ | ------------------------ | ------------------------------- |
| `=`          | Equals                   | `WHERE col = 'value'`           |
| `<>` or `!=` | Not equals               | `WHERE col <> 'value'`          |
| `BETWEEN`    | Range (inclusive!)       | `WHERE price BETWEEN 10 AND 50` |
| `IN`         | In list                  | `WHERE col IN ('A', 'B')`       |
| `IS NULL`    | Is null                  | `WHERE col IS NULL`             |
| `LIKE`       | Pattern match            | `WHERE col LIKE '%text%'`       |
| `~*`         | Regex (case-insensitive) | `WHERE col ~* 'pattern'`        |

---

# 12. Regex Patterns (PostgreSQL)

| Pattern           | Meaning                                    | Example                  |
| ----------------- | ------------------------------------------ | ------------------------ |
| `^`               | Start of string                            | `^A` (Starts with A)     |
| `$`               | End of string                              | `Road$` (Ends with Road) |
| `*`               | 0 or more chars                            | `.*` (Anything)          |
| `+`               | 1 or more chars                            | `.+` (Something)         |
| `\d`              | Any digit (PCRE-style; not PostgreSQL ARE) | `\d`                     |
| `\d{4,}`          | 4 or more digits (PCRE-style)              | `\d{4,}`                 |
| `[0-9]`           | Any digit (PostgreSQL-friendly)            | `[0-9]`                  |
| `[0-9]{4,}`       | 4 or more digits (PostgreSQL-friendly)     | `[0-9]{4,}`              |
| `[[:digit:]]`     | Any digit (PostgreSQL ARE class)           | `[[:digit:]]`            |
| `[[:digit:]]{4,}` | 4 or more digits (ARE class)               | `[[:digit:]]{4,}`        |
| `(A&#124;B)`      | A or B                                     | `(A&#124;B)`             |

---

# 13. Relational Algebra

| Symbol        | Name              | SQL Equivalent                    |
| ------------- | ----------------- | --------------------------------- |
| **σ** (sigma) | Selection         | `WHERE`                           |
| **π** (pi)    | Projection        | `SELECT columns`                  |
| **X**         | Cartesian Product | `FROM T1, T2` (no join condition) |
| **⨝**         | Natural Join      | `WHERE T1.key = T2.key`           |
| **U**         | Union             | `UNION`                           |
| **^**         | AND               | `AND`                             |
| **V**         | OR                | `OR`                              |

---

# 14. Tricky Queries - Common Mistakes

1. **Forgetting Join Conditions:** N tables need **N-1** joins.

2. **NULL Comparisons:** Never use `= NULL`. Must use `IS NULL`.

3. **BETWEEN:** It is **inclusive** (includes both 10 and 50).

4. **Percentage Math:** Decrease by 8% is `* 0.92`. `* 0.08` is WRONG.

5. **AND vs OR Logic:** Use parentheses. `OR` often deletes too much.

6. **GROUP BY:** `COUNT(*)` counts rows. `SUM()` adds values. Don't mix them up.

7. **UNION vs UNION ALL for counts:** Do not use `UNION` to “add” counts; it deduplicates rows. Use `UNION ALL` + `SUM`, or add scalar subqueries, or use a `CROSS JOIN` to add totals.

---

# 15. Pre-Submission Checklist

- [ ] All tables listed in `FROM` clause?

- [ ] All join conditions present (n-1)?

- [ ] Using `IS NULL`, not `= NULL`?

- [ ] `BETWEEN` is inclusive - is that what you want?

- [ ] AND/OR logic is correct?

- [ ] Percentage math correct? (decrease 8% = \* 0.92)

- [ ] Counted CASCADE effects for total rows?

[END]

[ZH]
![CS130_cheatSheet](images/20251215_CS130_cheatSheet.png)

# 📚 考试信息

| 项目         | 详情                                   |
| ------------ | -------------------------------------- |
| **时长**     | 90 分钟                                |
| **题量**     | 12 道 SQL 查询题                       |
| **尝试次数** | 2 次 (第一次尝试后会有反馈)            |
| **⚠️ 重要**  | **按顺序**答题 (删除/更新操作是连贯的) |

---

# 1. SELECT 语句 (查询)

### 基础与筛选

- **基础:** `SELECT column1, column2 FROM TableName;`

- **去重:** `SELECT DISTINCT column FROM TableName;`

- **Where:** `WHERE column > 100`

- **Between:** `WHERE column BETWEEN 10 AND 50` (范围)

- **In List:** `WHERE column IN ('val1', 'val2')` (在列表中)

- **Nulls:** `WHERE column IS NULL` 或 `IS NOT NULL`

### 多重条件

- `WHERE (条件1) AND (条件2);`

- `WHERE (条件1) OR (条件2);`

### 模式匹配

- **LIKE:** `'pattern%'` (以...开头) 或 `'%pattern%'` (包含)

- **正则:** `~* '^pattern.*$'` (不区分大小写)

### 排序

- `ORDER BY column ASC;` (升序)

- `ORDER BY column DESC;` (降序)

- `LIMIT 10;` (限制行数)

---

# 2. UPDATE 语句 (更新)

### 语法

```sql
UPDATE TableName SET col1 = 'val1', col2 = 'val2' WHERE condition;

```

### 百分比计算

- **减少 8%:** `SET price = price * 0.92`

- **增加 10%:** `SET price = price * 1.10`

---

# 3. DELETE 语句 (删除)

- **标准:** `DELETE FROM TableName WHERE condition;`

- **空值检查:** `DELETE FROM TableName WHERE column IS NULL;`

- **列表检查:** `DELETE FROM TableName WHERE column IN ('val1', 'val2');`

---

# 4. JOIN Queries (连接查询)

### 方法 1: 逗号 + WHERE (实验室常用)

```sql
SELECT T1.col, T2.col
FROM Table1 AS T1, Table2 AS T2
WHERE (T1.key = T2.key) AND (过滤条件);

```

### 方法 2: JOIN ... ON

```sql
SELECT T1.col, T2.col
FROM Table1 AS T1
JOIN Table2 AS T2 ON T1.key = T2.key
WHERE 过滤条件;

```

> **例子:** 3 表连接需要 `S.Student_ID = E.Student_ID` AND `E.ModuleID = M.ModuleID`。

---

# 5. 聚合函数

- **计数:** `SELECT COUNT(*) FROM TableName;`

- **平均值 (四舍五入):** `SELECT ROUND(AVG(column), 2) ...`

- **求和:** `SELECT SUM(column) ...`

- **分组:** `SELECT column, COUNT(*) FROM TableName GROUP BY column;`

---

# 6. 级联效应 (CASCADE) - 关键！

- **规则:** 当删除/更新时，计算 **主表** 和 **关联表** 中的行数。

- **策略:** 使用 `UNION ALL` 并对计数求和。

```sql
SELECT SUM(cnt) AS total
FROM (
    SELECT COUNT(*) AS cnt FROM MainTable ...
    UNION ALL
    SELECT COUNT(*) AS cnt FROM RelatedTable ...
) s;

```

- **数学:** 通过 `UNION ALL` 将多次 `COUNT(*)` 相加，得到 **受影响的总行数**。

---

# 7. CREATE TABLE (建表)

```sql
CREATE TABLE TableName (
    col1 DATATYPE NOT NULL,
    CONSTRAINT TableName_PKEY PRIMARY KEY (col1)
);

```

**带外键:**

```sql
... REFERENCES Parent(key) ON UPDATE CASCADE ON DELETE CASCADE

```

---

# 8. ALTER TABLE (修改表)

- **添加列:** `ALTER TABLE TableName ADD COLUMN columnname DATATYPE;`

---

# 9. INSERT Statements (插入)

- **语法:** `INSERT INTO TableName (col1, col2) VALUES ('val1', 'val2');`

---

# 10. 事务块 (Transaction Blocks)

- **开始:** `BEGIN;`

- **保存:** `COMMIT;`

- **撤销:** `ROLLBACK;`

- **测试策略:** 运行 `BEGIN`，查看数据，运行 `DELETE`，再次查看数据，最后 `ROLLBACK` 撤销测试。

---

# 11. 运算符参考

| 运算符       | 含义                | 例子                            |
| ------------ | ------------------- | ------------------------------- |
| `=`          | 等于                | `WHERE col = 'value'`           |
| `<>` or `!=` | 不等于              | `WHERE col <> 'value'`          |
| `BETWEEN`    | 范围 (包含!)        | `WHERE price BETWEEN 10 AND 50` |
| `IN`         | 在列表中            | `WHERE col IN ('A', 'B')`       |
| `IS NULL`    | 为空                | `WHERE col IS NULL`             |
| `LIKE`       | 模式匹配            | `WHERE col LIKE '%text%'`       |
| `~*`         | 正则 (不区分大小写) | `WHERE col ~* 'pattern'`        |

---

# 12. 正则表达式模式 (PostgreSQL)

| 模式              | 含义                                         | 例子                   |
| ----------------- | -------------------------------------------- | ---------------------- |
| `^`               | 字符串开头                                   | `^A` (以 A 开头)       |
| `$`               | 字符串结尾                                   | `Road$` (以 Road 结尾) |
| `*`               | 0 或更多字符                                 | `.*` (任意)            |
| `+`               | 1 或更多字符                                 | `.+` (至少一个)        |
| `\d`              | 任意数字（PCRE 风格，PostgreSQL ARE 不推荐） | `\d`                   |
| `\d{4,}`          | 4 个或更多数字（PCRE 风格）                  | `\d{4,}`               |
| `[0-9]`           | 任意数字（PostgreSQL 兼容）                  | `[0-9]`                |
| `[0-9]{4,}`       | 4 个或更多数字（PostgreSQL 兼容）            | `[0-9]{4,}`            |
| `[[:digit:]]`     | 任意数字（ARE 字符类，PostgreSQL 原生）      | `[[:digit:]]`          |
| `[[:digit:]]{4,}` | 4 个或更多数字（ARE 字符类）                 | `[[:digit:]]{4,}`      |
| `(A&#124;B)`      | A 或 B                                       | `(A&#124;B)`           |

---

# 13. 关系代数 (Relational Algebra)

| 符号          | 名称     | SQL 等价                   |
| ------------- | -------- | -------------------------- |
| **σ** (sigma) | 选择     | `WHERE`                    |
| **π** (pi)    | 投影     | `SELECT columns`           |
| **X**         | 笛卡尔积 | `FROM T1, T2` (无连接条件) |
| **⨝**         | 自然连接 | `WHERE T1.key = T2.key`    |
| **U**         | 并集     | `UNION`                    |
| **^**         | AND      | `AND`                      |
| **V**         | OR       | `OR`                       |

---

# 14. 棘手的查询 - 常见错误

1. **忘记连接条件:** N 个表需要 **N-1** 个连接。
2. **NULL 比较:** 永远不要用 `= NULL`。必须用 `IS NULL`。
3. **BETWEEN:** 它是 **包含** 边界的 (包括 10 和 50)。
4. **百分比数学:** 减少 8% 是 `* 0.92`。`* 0.08` 是错的。
5. **AND vs OR 逻辑:** 使用括号。`OR` 经常会删除太多内容。
6. **GROUP BY:** `COUNT(*)` 统计行数。`SUM()` 数值求和。不要混淆。

7. **UNION 与 UNION ALL 计数:** 不要用 `UNION` 去“相加”计数；它会去重。请用 `UNION ALL` + `SUM`，或用标量子查询相加，或用 `CROSS JOIN` 相加总数。

---

# 15. 提交前检查清单

- [ ] `FROM` 子句中列出了所有表吗？

- [ ] 所有的连接条件都存在吗 (n-1)？

- [ ] 使用的是 `IS NULL` 而不是 `= NULL` 吗？

- [ ] `BETWEEN` 是包含边界的 - 这是你想要的吗？

- [ ] AND/OR 逻辑正确吗？

- [ ] 百分比数学正确吗？(减少 8% = \* 0.92)

- [ ] 计算 CASCADE 效应的总行数了吗？

[END]
