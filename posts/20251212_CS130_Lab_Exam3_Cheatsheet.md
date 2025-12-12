---
title: CS130 Lab Exam 3 Cheatsheet
title_en: CS130 Lab Exam 3 Cheatsheet
title_zh: CS130 实验考试 3 备考指南 & 小抄
date: 2025-12-12
categories: CS130
tags: SQL, Cheatsheet, Relational Algebra, Exam Prep
summary_en: A comprehensive cheatsheet for the CS130 Lab Exam 3, covering key SQL commands (SELECT, JOIN, UPDATE, DELETE), Relational Algebra, and exam strategy.
summary_zh: CS130 实验考试 3 的终极备考指南与小抄。涵盖核心 SQL 命令 (SELECT, JOIN, UPDATE, DELETE)、关系代数转换，以及详细的应试策略。
---

![CS130 SQL Exam Header Image](images/cs130-sql-exam-header.png)

[EN]

# 1. Exam Strategy & Overview

This is an **OPEN BOOK** exam, but with strict limitations. Focus on using your own notes, past labs, and the official PostgreSQL documentation. **Do not use AI tools or communicate with others.**

- **Duration**: 90 minutes
- **Value**: 25% of final mark
- **Tasks**: 12 SQL queries
- **Key Topics**: `SELECT`, `JOIN`, `UPDATE`, `DELETE`, and Relational Algebra to SQL conversion.

### Recommended Workflow

1.  **Setup**: Before the exam, use the provided `.sql` file to create the tables in your own database schema.
2.  **Execute in Order**: Answer questions sequentially. `UPDATE` and `DELETE` operations will alter the database state for subsequent queries.
3.  **Moodle Quiz**: Run each query, get the result, and enter it into the Moodle quiz.
4.  **Two Attempts**: Use the first attempt to find out which answers are incorrect. The second attempt is for corrections. Don't rush the first one.

---

# 2. Core SQL Cheatsheet

This section covers the essential commands similar to those in labs 6 to 9.

### 2.1 Data Query (SELECT)

The foundation of most of your queries.

```sql
SELECT
    column1,
    COUNT(column2) AS alias_name
FROM
    table1
WHERE
    column3 > 100 AND column4 IS NOT NULL
ORDER BY
    column1 DESC;
```

- **`SELECT`**: Specifies the columns you want to see.
- **`FROM`**: The table you are querying.
- **`WHERE`**: Filters rows based on conditions.
- **`COUNT()`**: An aggregate function to count rows.
- **`ORDER BY`**: Sorts the result. `ASC` (ascending) is default, `DESC` is descending.
- **`NULL`**: Use `IS NULL` or `IS NOT NULL` to check for empty values.

### 2.2 Joining Tables (JOIN)

Used to combine rows from two or more tables based on a related column.

```sql
SELECT
    students.name,
    courses.course_name
FROM
    students
JOIN
    enrollment ON students.student_id = enrollment.student_id
JOIN
    courses ON enrollment.course_id = courses.course_id
WHERE
    courses.department = 'Computer Science';
```

- **`JOIN`** (or `INNER JOIN`): Returns records that have matching values in both tables.
- **`ON`**: Specifies the join condition.

### 2.3 Data Modification (UPDATE & DELETE)

**Warning**: These operations are destructive. The `WHERE` clause is critical to avoid changing the wrong data.

#### UPDATE

Modifies existing records in a table.

```sql
UPDATE
    employees
SET
    salary = salary * 1.10,
    job_title = 'Senior Developer'
WHERE
    employee_id = 12345;
```

- **`UPDATE`**: The table to modify.
- **`SET`**: The column(s) to change and their new values.
- **`WHERE`**: **Crucial**. Specifies which record(s) to update. Without it, you update **all** rows.

#### DELETE

Removes existing records from a table.

```sql
DELETE FROM
    order_items
WHERE
    order_id IN (SELECT order_id FROM orders WHERE status = 'cancelled');
```

- **`DELETE FROM`**: The table to remove rows from.
- **`WHERE`**: **Crucial**. Specifies which record(s) to delete. Without it, you delete **all** rows.

---

# 3. Relational Algebra (RA) to SQL

You will need to convert RA statements to SQL. Review lecture 17 slides.

| Relational Algebra Operator | SQL Equivalent                        | Example (RA)                              |
| :-------------------------- | :------------------------------------ | :---------------------------------------- |
| **σ (Selection)**           | `WHERE` clause                        | `σ_{salary > 50000}(Employee)`            |
| **π (Projection)**          | `SELECT` clause                       | `π_{name, department}(Employee)`          |
| **⋈ (Natural Join)**        | `JOIN` with matching column names     | `Employee ⋈ Department`                   |
| **× (Cartesian Product)**   | `FROM table1, table2` or `CROSS JOIN` | `Employee × Dependent`                    |
| **∪ (Union)**               | `UNION`                               | `π_{name}(Faculty) ∪ π_{name}(Staff)`     |
| **- (Difference)**          | `EXCEPT` (in PostgreSQL)              | `π_{id}(AllStudents) - π_{id}(Graduated)` |
| **ρ (Rename)**              | `AS` keyword for columns or tables    | `ρ_{E}(Employee)`                         |

### Example Conversion:

**Relational Algebra**:
`π_{name, course_name}(σ_{department='CS'}(Student ⋈ Enrolled ⋈ Course))`

**Equivalent SQL**:

```sql
SELECT
    S.name,
    C.course_name
FROM
    Student AS S
JOIN
    Enrolled AS E ON S.student_id = E.student_id
JOIN
    Course AS C ON E.course_id = C.course_id
WHERE
    C.department = 'CS';
```

### Advanced RA Conversions:

**Example 2 - Difference (EXCEPT)**:
**RA**: `π_{student_id}(Student) - π_{student_id}(Enrolled)`
**SQL**:

```sql
SELECT student_id FROM Student
EXCEPT
SELECT student_id FROM Enrolled;
```

**Example 3 - Complex with Rename**:
**RA**: `π_{E1.name, E2.name}(ρ_{E1}(Employee) ⋈_{E1.manager_id = E2.employee_id} ρ_{E2}(Employee))`
**SQL**:

```sql
SELECT E1.name, E2.name
FROM Employee AS E1
JOIN Employee AS E2 ON E1.manager_id = E2.employee_id;
```

---

# 4. SQL Core Commands: Examples with Common Pitfalls

This section provides three levels of examples for each core SQL command to help you prepare for the exam.

### 4.1 SELECT Queries

#### Simple

Basic data retrieval:

```sql
SELECT name, age
FROM students
WHERE age > 20;
```

#### Medium

Grouping and counting:

```sql
SELECT department, COUNT(*) as student_count
FROM students
WHERE age > 18
GROUP BY department
ORDER BY student_count DESC;
```

#### Hard/Pitfall

Using HAVING with GROUP BY (common exam trap):

```sql
SELECT department, AVG(gpa) as avg_gpa
FROM students
GROUP BY department
HAVING COUNT(*) > 5 AND AVG(gpa) > 3.0
ORDER BY avg_gpa DESC;
```

**Pitfall**: Remember HAVING filters groups, WHERE filters rows before grouping.

### 4.2 JOIN Operations

#### Simple

Basic INNER JOIN:

```sql
SELECT s.name, c.course_name
FROM students s
JOIN enrollments e ON s.student_id = e.student_id
JOIN courses c ON e.course_id = c.course_id;
```

#### Medium

LEFT JOIN to find unmatched records:

```sql
SELECT s.name
FROM students s
LEFT JOIN enrollments e ON s.student_id = e.student_id
WHERE e.student_id IS NULL;
```

**Purpose**: Find students who haven't enrolled in any courses.

#### Hard/Pitfall

Multiple JOINs with aggregation:

```sql
SELECT d.department_name, AVG(s.gpa) as avg_gpa
FROM departments d
JOIN students s ON d.department_id = s.department_id
JOIN enrollments e ON s.student_id = e.student_id
GROUP BY d.department_id, d.department_name
HAVING COUNT(DISTINCT e.course_id) >= 3;
```

**Pitfall**: Without DISTINCT, you might count duplicate course enrollments.

### 4.3 UPDATE Operations

#### Simple

Update single record:

```sql
UPDATE employees
SET salary = 55000
WHERE employee_id = 1001;
```

#### Medium

Conditional update with JOIN:

```sql
UPDATE employees e
SET salary = salary * 1.10
FROM departments d
WHERE e.department_id = d.department_id
  AND d.department_name = 'Engineering';
```

#### Hard/Pitfall

Update using subquery:

```sql
UPDATE products
SET status = 'Returned'
WHERE product_id IN (
    SELECT oi.product_id
    FROM order_items oi
    JOIN orders o ON oi.order_id = o.order_id
    WHERE o.status = 'Cancelled'
);
```

**Pitfall**: Always test subqueries separately first to avoid mass updates.

### 4.4 DELETE Operations

#### Simple

Delete single record:

```sql
DELETE FROM students
WHERE student_id = 12345;
```

#### Medium

Delete with JOIN condition:

```sql
DELETE o FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
WHERE c.status = 'Inactive' AND o.order_date < '2023-01-01';
```

#### Hard/Pitfall

Delete using subquery (be very careful!):

```sql
DELETE FROM customers
WHERE customer_id NOT IN (
    SELECT DISTINCT customer_id
    FROM orders
    WHERE customer_id IS NOT NULL
);
```

**Pitfall**: The `IS NOT NULL` is crucial! Without it, if any order has NULL customer_id, the entire NOT IN returns NULL and deletes nothing.

[END]

[ZH]

# 1. 考试策略与概览

这是一场**开卷**考试，但有严格的限制。请专注于使用你自己的笔记、过去的实验和 PostgreSQL 官方文档。**禁止使用 AI 工具或与他人交流。**

- **时长**: 90 分钟
- **分值**: 占期末总成绩的 25%
- **任务**: 12 个 SQL 查询
- **核心主题**: `SELECT`, `JOIN`, `UPDATE`, `DELETE` 以及关系代数到 SQL 的转换。

### 推荐工作流程

1.  **考前准备**: 考试开始前，使用提供的 `.sql` 文件在你自己的数据库模式中创建表格。
2.  **按序执行**: 按顺序回答问题。`UPDATE` 和 `DELETE` 操作会改变数据库状态，影响后续查询。
3.  **Moodle 测验**: 运行每个查询，获取结果，并将其输入到 Moodle 测验中。
4.  **两次尝试机会**: 利用第一次尝试找出哪些答案是错误的。第二次尝试用于修正。不要急于完成第一次。

---

# 2. 核心 SQL 小抄

本节涵盖了与实验 6 到 9 类似的基本命令。

### 2.1 数据查询 (SELECT)

这是你大多数查询的基础。

```sql
SELECT
    column1,
    COUNT(column2) AS alias_name
FROM
    table1
WHERE
    column3 > 100 AND column4 IS NOT NULL
ORDER BY
    column1 DESC;
```

- **`SELECT`**: 指定你希望看到的列。
- **`FROM`**: 你正在查询的表。
- **`WHERE`**: 根据条件筛选行。
- **`COUNT()`**: 用于计数的聚合函数。
- **`ORDER BY`**: 对结果进行排序。`ASC` (升序) 是默认值，`DESC` 是降序。
- **`NULL`**: 使用 `IS NULL` 或 `IS NOT NULL` 来检查空值。

### 2.2 连接表 (JOIN)

用于根据相关列合并来自两个或多个表的行。

```sql
SELECT
    students.name,
    courses.course_name
FROM
    students
JOIN
    enrollment ON students.student_id = enrollment.student_id
JOIN
    courses ON enrollment.course_id = courses.course_id
WHERE
    courses.department = 'Computer Science';
```

- **`JOIN`** (或 `INNER JOIN`): 返回在两个表中都有匹配值的记录。
- **`ON`**: 指定连接条件。

### 2.3 数据修改 (UPDATE & DELETE)

**警告**: 这些操作是破坏性的。`WHERE` 子句至关重要，以避免修改错误的数据。

#### UPDATE

修改表中的现有记录。

```sql
UPDATE
    employees
SET
    salary = salary * 1.10,
    job_title = 'Senior Developer'
WHERE
    employee_id = 12345;
```

- **`UPDATE`**: 要修改的表。
- **`SET`**: 要更改的列及其新值。
- **`WHERE`**: **至关重要**。指定要更新的记录。如果没有它，你将更新**所有**行。

#### DELETE

从表中删除现有记录。

```sql
DELETE FROM
    order_items
WHERE
    order_id IN (SELECT order_id FROM orders WHERE status = 'cancelled');
```

- **`DELETE FROM`**: 要从中删除行的表。
- **`WHERE`**: **至关重要**。指定要删除的记录。如果没有它，你将删除**所有**行。

---

# 3. 关系代数 (RA) 到 SQL

你需要将关系代数语句转换为 SQL。请复习第 17 讲的幻灯片。

| 关系代数运算符   | SQL 等价物                            | 示例 (RA)                                 |
| :--------------- | :------------------------------------ | :---------------------------------------- |
| **σ (选择)**     | `WHERE` 子句                          | `σ_{salary > 50000}(Employee)`            |
| **π (投影)**     | `SELECT` 子句                         | `π_{name, department}(Employee)`          |
| **⋈ (自然连接)** | `JOIN` (使用匹配的列名)               | `Employee ⋈ Department`                   |
| **× (笛卡尔积)** | `FROM table1, table2` 或 `CROSS JOIN` | `Employee × Dependent`                    |
| **∪ (并集)**     | `UNION`                               | `π_{name}(Faculty) ∪ π_{name}(Staff)`     |
| **- (差集)**     | `EXCEPT` (在 PostgreSQL 中)           | `π_{id}(AllStudents) - π_{id}(Graduated)` |
| **ρ (重命名)**   | `AS` 关键字 (用于列或表)              | `ρ_{E}(Employee)`                         |

### 转换示例:

**关系代数**:
`π_{name, course_name}(σ_{department='CS'}(Student ⋈ Enrolled ⋈ Course))`

**等效 SQL**:

```sql
SELECT
    S.name,
    C.course_name
FROM
    Student AS S
JOIN
    Enrolled AS E ON S.student_id = E.student_id
JOIN
    Course AS C ON E.course_id = C.course_id
WHERE
    C.department = 'CS';
```

### 高级关系代数转换:

**示例 2 - 差集 (EXCEPT)**:
**关系代数**: `π_{student_id}(Student) - π_{student_id}(Enrolled)`
**SQL**:

```sql
SELECT student_id FROM Student
EXCEPT
SELECT student_id FROM Enrolled;
```

**示例 3 - 复杂重命名连接**:
**关系代数**: `π_{E1.name, E2.name}(ρ_{E1}(Employee) ⋈_{E1.manager_id = E2.employee_id} ρ_{E2}(Employee))`
**SQL**:

```sql
SELECT E1.name, E2.name
FROM Employee AS E1
JOIN Employee AS E2 ON E1.manager_id = E2.employee_id;
```

---

# 4. SQL 核心命令示例详解 (附常见陷阱)

本节为每个核心 SQL 命令提供三个难度层次的示例，帮助你备考。

### 4.1 SELECT 查询

#### 简单

基础数据检索:

```sql
SELECT name, age
FROM students
WHERE age > 20;
```

#### 中等

分组和计数:

```sql
SELECT department, COUNT(*) as student_count
FROM students
WHERE age > 18
GROUP BY department
ORDER BY student_count DESC;
```

#### 困难/陷阱

使用 HAVING 与 GROUP BY (常见考试陷阱):

```sql
SELECT department, AVG(gpa) as avg_gpa
FROM students
GROUP BY department
HAVING COUNT(*) > 5 AND AVG(gpa) > 3.0
ORDER BY avg_gpa DESC;
```

**陷阱**: 记住 HAVING 过滤分组，WHERE 在分组前过滤行。

### 4.2 JOIN 连接操作

#### 简单

基础 INNER JOIN:

```sql
SELECT s.name, c.course_name
FROM students s
JOIN enrollments e ON s.student_id = e.student_id
JOIN courses c ON e.course_id = c.course_id;
```

#### 中等

LEFT JOIN 查找不匹配记录:

```sql
SELECT s.name
FROM students s
LEFT JOIN enrollments e ON s.student_id = e.student_id
WHERE e.student_id IS NULL;
```

**目的**: 查找没有选课的学生。

#### 困难/陷阱

多表 JOIN 与聚合:

```sql
SELECT d.department_name, AVG(s.gpa) as avg_gpa
FROM departments d
JOIN students s ON d.department_id = s.department_id
JOIN enrollments e ON s.student_id = e.student_id
GROUP BY d.department_id, d.department_name
HAVING COUNT(DISTINCT e.course_id) >= 3;
```

**陷阱**: 没有 DISTINCT 可能会重复计算课程选课。

### 4.3 UPDATE 更新操作

#### 简单

更新单条记录:

```sql
UPDATE employees
SET salary = 55000
WHERE employee_id = 1001;
```

#### 中等

基于 JOIN 的条件更新:

```sql
UPDATE employees e
SET salary = salary * 1.10
FROM departments d
WHERE e.department_id = d.department_id
  AND d.department_name = 'Engineering';
```

#### 困难/陷阱

使用子查询更新:

```sql
UPDATE products
SET status = 'Returned'
WHERE product_id IN (
    SELECT oi.product_id
    FROM order_items oi
    JOIN orders o ON oi.order_id = o.order_id
    WHERE o.status = 'Cancelled'
);
```

**陷阱**: 总是先单独测试子查询，避免大规模误更新。

### 4.4 DELETE 删除操作

#### 简单

删除单条记录:

```sql
DELETE FROM students
WHERE student_id = 12345;
```

#### 中等

基于 JOIN 条件删除:

```sql
DELETE o FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
WHERE c.status = 'Inactive' AND o.order_date < '2023-01-01';
```

#### 困难/陷阱

使用子查询删除 (非常小心!):

```sql
DELETE FROM customers
WHERE customer_id NOT IN (
    SELECT DISTINCT customer_id
    FROM orders
    WHERE customer_id IS NOT NULL
);
```

**陷阱**: `IS NOT NULL` 至关重要！没有它，如果任何订单有 NULL customer_id，整个 NOT IN 返回 NULL 且不删除任何内容。

[END]
