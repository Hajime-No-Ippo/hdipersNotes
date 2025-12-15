
---
title: CS130 Lab Exam 3: The 20-Question Drill (Universal Symbols)
title_en: CS130 Lab Exam 3: The 20-Question Drill (Universal Symbols)
title_zh: CS130 终极题库：20 道分级练习 (符号修复版)
date: 2025-12-13
categories: CS130
tags: SQL, Relational Algebra, ExamPrep, PostgreSQL
summary_en: Practice set with 20 questions. Fixed formatting to use universal Unicode symbols for Relational Algebra (σ, π, ⨝, ρ).
summary_zh: 20 道真题模拟。已修复符号显示问题，全部采用通用字符 (σ, π, ⨝, ρ)，确保在任何设备上正确显示。
---

[EN]
![CS130_cheatSheet](images/20251213_CS130_Lab3Exam20Questions.png)
# 📚 Quick Refresher (The Essentials)

### 1. SQL Execution Order
`FROM` -> `WHERE` -> `GROUP BY` -> `HAVING` -> `SELECT` -> `ORDER BY`

### 2. Relational Algebra (RA) Symbols
*(These are the Greek letters you see in the questions)*

*   **σ (Sigma) = Selection:** Filters **ROWS**. Maps to SQL **`WHERE`**.
*   **π (Pi) = Projection:** Selects **COLUMNS**. Maps to SQL **`SELECT`**.
*   **⨝ (Bowtie) = Natural Join:** Joins tables on common columns.
*   **ρ (Rho) = Rename:** Renames a table or column. Maps to SQL `AS`.

> **⚠️ EXAM WARNING:** Do not confuse RA "Selection" with SQL `SELECT`.
> *   RA Selection (**σ**) = Filtering rows (`WHERE`).
> *   RA Projection (**π**) = Picking columns (`SELECT`).

---

# 💀 Top 3 "Instant Fail" Pitfalls

1.  **Updating without WHERE:** `UPDATE Students SET gpa = 4.0;` (Resets EVERYONE. Don't do it.)
2.  **Select non-agg columns:** `SELECT name, COUNT(*) FROM Students;` (Error! `name` is not grouped.)
3.  **Null Comparisons:** `WHERE grade = NULL` (Wrong!) -> `WHERE grade IS NULL` (Correct!).

---

# ⚔️ The 20-Question Gauntlet

## 🟢 Level 1: Warm-up (Basic Syntax)

### Q1: Distinct Values
**Task:** Retrieve a list of all unique `cities` where students live.
*   Table: `Students` (id, name, city, gpa)

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT DISTINCT city
FROM Students;
```
**Explanation:** The `DISTINCT` keyword removes duplicate rows from the result set.

</details>

### Q2: String Matching (Pattern)
**Task:** Find all students whose name starts with 'J' and ends with 'n' (e.g., John, Jason).

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT *
FROM Students
WHERE name LIKE 'J%n';
```
**Explanation:** `%` is the wildcard for any sequence of characters. `_` is for a single character.

</details>

### Q3: Null Check
**Task:** Find employees who do **not** have a manager assigned.
*   Table: `Employees` (id, name, manager_id)

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT name
FROM Employees
WHERE manager_id IS NULL;
```
**Explanation:** Never use `= NULL`. Always use `IS NULL`.

</details>

### Q4: Simple Calculation
**Task:** Show the `name` and `annual_salary` (monthly_salary * 12) for all staff.
*   Table: `Staff` (name, monthly_salary)

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT name, monthly_salary * 12 AS annual_salary
FROM Staff;
```
**Explanation:** Arithmetic operates on the data in the columns. `AS` creates an alias for the output header.

</details>

### Q5: Basic Sorting
**Task:** List all products sorted by price (high to low), then by name (A-Z).

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT *
FROM Products
ORDER BY price DESC, name ASC;
```

</details>

---

## 🟡 Level 2: Core Competency (Joins & Aggregates)

### Q6: Group By Counting
**Task:** Count how many students are in each major.
*   Table: `Students` (id, major, name)

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT major, COUNT(*)
FROM Students
GROUP BY major;
```
**Explanation:** Any column in `SELECT` that isn't inside an aggregate function (like `COUNT`) must be in `GROUP BY`.

</details>

### Q7: Filtering Groups (HAVING)
**Task:** Find departments that have an average salary greater than 5000.
*   Table: `Employees` (dept_id, salary)

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT dept_id, AVG(salary)
FROM Employees
GROUP BY dept_id
HAVING AVG(salary) > 5000;
```
**Explanation:** `WHERE` filters rows. `HAVING` filters aggregated groups.

</details>

### Q8: Inner Join (2 Tables)
**Task:** Find the `employee_name` and their `dept_name`.
*   Tables: `Employees` (name, dept_id), `Departments` (id, dept_name)

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT E.name, D.dept_name
FROM Employees E
JOIN Departments D ON E.dept_id = D.id;
```

</details>

### Q9: Three-Table Join
**Task:** List student names and the courses they took.
*   `Student` (id, name), `Course` (cid, cname), `Takes` (sid, cid)

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT S.name, C.cname
FROM Student S
JOIN Takes T ON S.id = T.sid
JOIN Course C ON T.cid = C.cid;
```

</details>

### Q10: RA Selection to SQL
**RA:** σ (age > 20 AND gender = 'F') on **Students**

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT *
FROM Students
WHERE age > 20 AND gender = 'F';
```
**Explanation:** **σ** (Selection) means "filter rows", which corresponds to `WHERE`.

</details>

### Q11: RA Projection to SQL
**RA:** π (name, id) on **Students**

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT DISTINCT name, id
FROM Students;
```
**Explanation:** **π** (Projection) means "keep specific columns", which corresponds to SQL `SELECT`. (DISTINCT is implied in strict RA).

</details>

### Q12: Left Join (Finding Unmatched)
**Task:** Find customers who have registered but **never** placed an order.
*   `Customers` (id, name), `Orders` (ord_id, cust_id)

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT C.name
FROM Customers C
LEFT JOIN Orders O ON C.id = O.cust_id
WHERE O.ord_id IS NULL;
```

</details>

### Q13: INSERT Data
**Task:** Add a new student 'Alice' with ID 101 to the 'CS' department.

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
INSERT INTO Students (id, name, department)
VALUES (101, 'Alice', 'CS');
```

</details>

---

## 🔴 Level 3: Hard (Subqueries & RA Logic)

### Q14: Subquery (Scalar)
**Task:** Find employees who earn more than the **average salary of the entire company**.

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT name, salary
FROM Employees
WHERE salary > (SELECT AVG(salary) FROM Employees);
```

</details>

### Q15: Correlated Subquery (EXISTS)
**Task:** Find courses that have at least one student enrolled (Using `EXISTS`).

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT cname
FROM Courses C
WHERE EXISTS (
    SELECT 1 FROM Enrolled E
    WHERE E.cid = C.cid
);
```

</details>

### Q16: Set Operations (Difference)
**Task:** Find students in 'Math' club but **NOT** in 'Science' club.

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT name FROM MathClub
EXCEPT
SELECT name FROM ScienceClub;
```
**RA Equivalent:** Math − Science

</details>

### Q17: Relational Algebra (Cartesian Product)
**RA:** R × S (R has N rows, S has M rows).
**Question:** How many rows in result?

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

**Answer:** N × M rows.
**SQL:** `CROSS JOIN`

</details>

### Q18: Self Join (Hierarchy)
**Task:** Find pairs of employees `(A, B)` where A and B work in the same department, but A is not B.

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT A.name, B.name
FROM Employees A
JOIN Employees B ON A.dept_id = B.dept_id
WHERE A.id != B.id;
```

</details>

### Q19: The "Division" Problem (Universal Quantifier)
**Task:** Find students who have taken **ALL** courses available in the catalog.

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT sid
FROM Takes
GROUP BY sid
HAVING COUNT(distinct cid) = (SELECT COUNT(*) FROM Courses);
```

</details>

### Q20: Complex RA to SQL
**RA:** π_sname ( σ_dept='CS'(Student) ⨝ ρ_takes(Enrolled) )

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT S.sname
FROM Student S
JOIN Enrolled takes ON S.id = takes.sid
WHERE S.dept = 'CS';
```
**Explanation:**
1.  **ρ** (Rename): `Enrolled` -> `takes`.
2.  **⨝** (Join): Matches IDs.
3.  **σ** (Selection): Filters `WHERE dept='CS'`.
4.  **π** (Projection): Selects `sname`.

</details>

[END]

[ZH]
![CS130_cheatSheet](images/20251213_CS130_Lab3Exam20Questions.png)
# 📚 考前速览 (核心要点)

### 1. SQL 执行顺序
`FROM` -> `WHERE` -> `GROUP BY` -> `HAVING` -> `SELECT` -> `ORDER BY`

### 2. 关系代数 (RA) 符号
*(以下符号已改为通用字符，非代码)*

*   **σ (Sigma / 希腊字母s) = 选择 (Selection):** 筛选符合条件的**行**。对应 SQL 的 **`WHERE`**。
*   **π (Pi / 希腊字母p) = 投影 (Projection):** 提取指定的**列**。对应 SQL 的 **`SELECT`**。
*   **⨝ (Bowtie / 领结) = 自然连接 (Natural Join):** 基于同名列合并表。
*   **ρ (Rho / 希腊字母r) = 重命名 (Rename):** 重命名表或列。对应 SQL 的 `AS`。

> **⚠️ 考试深坑警告:**
> *   RA 的 **σ** (Selection) 是在**挑行** (SQL `WHERE`)。
> *   RA 的 **π** (Projection) 是在**挑列** (SQL `SELECT`)。

---

# 💀 三大“挂科”深坑

1.  **无条件更新:** `UPDATE Students SET gpa = 4.0;` (千万别这么写，会把全校GPA都改了!)
2.  **未分组列:** `SELECT name, COUNT(*) FROM Students;` (报错! `name` 没有被 Group By.)
3.  **空值比较:** `WHERE grade = NULL` (错!) -> `WHERE grade IS NULL` (对!).

---

# ⚔️ 终极 20 题特训

## 🟢 等级 1: 热身 (基础语法)

### Q1: 去重查询 (Distinct)
**任务:** 列出学生居住的所有不同的城市 (`cities`)。
*   表: `Students` (id, name, city, gpa)

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT DISTINCT city
FROM Students;
```
**解析:** `DISTINCT` 关键字用于去除重复行。

</details>

### Q2: 字符串模式匹配 (Pattern)
**任务:** 找出名字以 'J' 开头且以 'n' 结尾的所有学生 (例如 John, Jason)。

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT *
FROM Students
WHERE name LIKE 'J%n';
```
**解析:** `%` 是匹配任意多个字符的通配符。`_` 是匹配单个字符的通配符。

</details>

### Q3: 空值检查 (Null Check)
**任务:** 找出**没有**分配经理的员工。
*   表: `Employees` (id, name, manager_id)

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT name
FROM Employees
WHERE manager_id IS NULL;
```
**解析:** 在 SQL 中不能用 `= NULL` 或 `!= NULL`。必须使用 `IS NULL`。

</details>

### Q4: 简单计算
**任务:** 显示所有员工的姓名和**年薪** (`annual_salary` = monthly_salary * 12)。
*   表: `Staff` (name, monthly_salary)

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT name, monthly_salary * 12 AS annual_salary
FROM Staff;
```
**解析:** `AS` 用于给计算出的结果列起别名。

</details>

### Q5: 基础排序
**任务:** 列出所有产品，先按价格从高到低排序，价格相同的按名称 A-Z 排序。

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT *
FROM Products
ORDER BY price DESC, name ASC;
```

</details>

---

## 🟡 等级 2: 核心能力 (连接与聚合)

### Q6: 分组计数 (Group By)
**任务:** 统计每个专业 (`major`) 有多少名学生。

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT major, COUNT(*)
FROM Students
GROUP BY major;
```
**解析:** 任何出现在 SELECT 中但没有被聚合函数包裹的列，必须出现在 `GROUP BY` 中。

</details>

### Q7: 过滤分组 (HAVING)
**任务:** 找出平均工资高于 5000 的部门。

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT dept_id, AVG(salary)
FROM Employees
GROUP BY dept_id
HAVING AVG(salary) > 5000;
```
**解析:** `WHERE` 过滤原始行，`HAVING` 过滤聚合后的组数据。

</details>

### Q8: 内连接 (Inner Join)
**任务:** 找出员工姓名及其所属部门的名称。
*   表: `Employees` (name, dept_id), `Departments` (id, dept_name)

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT E.name, D.dept_name
FROM Employees E
JOIN Departments D ON E.dept_id = D.id;
```

</details>

### Q9: 三表连接
**任务:** 列出学生姓名和他们所选修的课程名称。
*   表: `Student` (id, name), `Course` (cid, cname), `Takes` (sid, cid)

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT S.name, C.cname
FROM Student S
JOIN Takes T ON S.id = T.sid
JOIN Course C ON T.cid = C.cid;
```

</details>

### Q10: RA 选择转 SQL
**RA:** σ (age > 20 AND gender = 'F') 作用于 **Students**

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT *
FROM Students
WHERE age > 20 AND gender = 'F';
```
**解析:** **σ** (Selection) 意思是“选择行”，对应 SQL 的 `WHERE` 子句。

</details>

### Q11: RA 投影转 SQL
**RA:** π (name, id) 作用于 **Students**

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT DISTINCT name, id
FROM Students;
```
**解析:** **π** (Projection) 意思是“投影列”，对应 SQL 的 `SELECT`。

</details>

### Q12: 左连接 (找未匹配项)
**任务:** 找出注册了但**从未**下过单的客户。
*   `Customers` (id, name), `Orders` (ord_id, cust_id)

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT C.name
FROM Customers C
LEFT JOIN Orders O ON C.id = O.cust_id
WHERE O.ord_id IS NULL;
```

</details>

### Q13: 插入数据
**任务:** 向 'CS' 系插入一个 ID 为 101，名字叫 'Alice' 的新学生。

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
INSERT INTO Students (id, name, department)
VALUES (101, 'Alice', 'CS');
```

</details>

---

## 🔴 等级 3: 困难 (子查询与逻辑陷阱)

### Q14: 标量子查询
**任务:** 找出薪水高于**全公司平均薪水**的员工。

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT name, salary
FROM Employees
WHERE salary > (SELECT AVG(salary) FROM Employees);
```

</details>

### Q15: 相关子查询 (EXISTS)
**任务:** 找出至少有一名学生选修的课程 (使用 `EXISTS`)。

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT cname
FROM Courses C
WHERE EXISTS (
    SELECT 1 FROM Enrolled E
    WHERE E.cid = C.cid
);
```

</details>

### Q16: 集合操作 (差集)
**任务:** 找出在“数学俱乐部”但**不在**“科学俱乐部”的学生名单。

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT name FROM MathClub
EXCEPT
SELECT name FROM ScienceClub;
```
**对应 RA:** Math − Science

</details>

### Q17: 关系代数 (笛卡尔积)
**RA:** R × S (假设 R 有 N 行，S 有 M 行)。
**问题:** 结果有多少行？

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

**答案:** N × M 行。
**SQL:** `CROSS JOIN`

</details>

### Q18: 自连接 (Self Join)
**任务:** 找出同一部门的员工对 `(A, B)`，要求 A 不是 B (避免自己匹配自己)。

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT A.name, B.name
FROM Employees A
JOIN Employees B ON A.dept_id = B.dept_id
WHERE A.id != B.id;
```

</details>

### Q19: "除法" 问题 (全称量词)
**任务:** 找出选修了目录中**所有**课程的学生。

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT sid
FROM Takes
GROUP BY sid
HAVING COUNT(distinct cid) = (SELECT COUNT(*) FROM Courses);
```

</details>

### Q20: 复杂 RA 转 SQL
**RA:** π_sname ( σ_dept='CS'(Student) ⨝ ρ_takes(Enrolled) )

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT S.sname
FROM Student S
JOIN Enrolled takes ON S.id = takes.sid
WHERE S.dept = 'CS';
```
**解析:**
1.  **ρ** (重命名): 将表重命名为 `takes`。
2.  **⨝** (连接): 匹配 ID。
3.  **σ** (选择): 对应 `WHERE dept='CS'`。
4.  **π** (投影): 对应 `SELECT sname`。

</details>

[END]