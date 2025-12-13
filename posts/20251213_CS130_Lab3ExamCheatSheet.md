
---
title: CS130 Lab Exam 3: The 20-Question Drill (Extended)
title_en: CS130 Lab Exam 3: The 20-Question Drill (Extended)
title_zh: CS130 终极题库：20 道分级练习 (含详细解析)
date: 2025-12-13
categories: CS130
tags: SQL, Relational Algebra, ExamPrep, PostgreSQL
summary_en: An expanded practice set with 20 questions ranging from basic syntax to complex relational algebra conversions and subquery logic.
summary_zh: 扩充至 20 道真题模拟。从基础语法到复杂的除法运算、RA转换及高难度子查询，附带详细解题思路。
---

[EN]
# 📚 Quick Refresher (The Essentials)

*   **Order of Execution:** `FROM` -> `WHERE` -> `GROUP BY` -> `HAVING` -> `SELECT` -> `ORDER BY`
*   **Logical Order:** `NOT` -> `AND` -> `OR`
*   **RA Symbols:** $\sigma$ (Select rows), $\pi$ (Select cols), $\bowtie$ (Join), $\rho$ (Rename).

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
**Explanation:** The `DISTINCT` keyword removes duplicate rows from the result set. Without it, you would get 'London' 50 times if 50 students live there.

</details>

### Q2: String Matching (Pattern)
**Task:** Find all students whose name starts with 'J' and ends with 'n' (e.g., John, Jason).

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT *
FROM Students
WHERE name LIKE 'J%n';
```
**Explanation:** `%` is the wildcard for any sequence of characters. `_` is the wildcard for a single character.

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
**Explanation:** You cannot use `= NULL` or `!= NULL` in SQL. You must use `IS NULL` or `IS NOT NULL`.

</details>

### Q4: Simple Calculation
**Task:** Show the `name` and `annual_salary` (monthly_salary * 12) for all staff.
*   Table: `Staff` (name, monthly_salary)

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT name, monthly_salary * 12 AS annual_salary
FROM Staff;
```
**Explanation:** You can perform arithmetic (+, -, *, /) directly in the SELECT clause. `AS` renames the output column.

</details>

### Q5: Basic Sorting
**Task:** List all products sorted by price (high to low), then by name (A-Z).

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT *
FROM Products
ORDER BY price DESC, name ASC;
```
**Explanation:** You can sort by multiple columns. The second column is used only to break ties in the first column.

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
**Explanation:** When using an aggregate function like `COUNT`, any non-aggregated column (like `major`) must be in the `GROUP BY` clause.

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
**Explanation:** `WHERE` filters rows *before* grouping. `HAVING` filters the results *after* grouping (aggregates).

</details>

### Q8: Inner Join (2 Tables)
**Task:** Find the `student_name` and the `course_name` they are enrolled in.
*   Tables: `Students` (id, name), `Enrolled` (sid, cid), `Courses` (id, cname) -> *Wait, let's do 2 tables first.*
*   Tables: `Employees` (name, dept_id), `Departments` (id, dept_name)

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT E.name, D.dept_name
FROM Employees E
JOIN Departments D ON E.dept_id = D.id;
```
**Explanation:** `INNER JOIN` (or just `JOIN`) finds matching records in both tables. We use aliases (E, D) to make the code cleaner.

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
**Explanation:** To link Students to Courses, you must go through the junction table (`Takes`). This requires two JOINs.

</details>

### Q10: RA Selection to SQL
**RA:** $\sigma_{age > 20 \land gender='F'} (Students)$

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT *
FROM Students
WHERE age > 20 AND gender = 'F';
```
**Explanation:** $\sigma$ (Sigma) maps directly to the `WHERE` clause. $\land$ is logical AND.

</details>

### Q11: RA Projection to SQL
**RA:** $\pi_{name, id} (Students)$

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT distinct name, id
FROM Students;
```
**Explanation:** Strictly speaking, Relational Algebra is a set (no duplicates). So $\pi$ usually implies `SELECT DISTINCT`. In CS130, standard `SELECT` is often accepted unless specified otherwise.

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
**Explanation:** `LEFT JOIN` keeps all customers. If there is no matching order, `O.ord_id` becomes NULL. The `WHERE` clause catches these NULLs.

</details>

### Q13: INSERT Data
**Task:** Add a new student 'Alice' with ID 101 to the 'CS' department.

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
INSERT INTO Students (id, name, department)
VALUES (101, 'Alice', 'CS');
```
**Explanation:** Always specify column names before `VALUES` to be safe, especially if the table structure changes later.

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
**Explanation:** You cannot put `AVG(salary)` directly in a WHERE clause. You must calculate it in a subquery first.

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
**Explanation:** A correlated subquery runs once for *each row* of the outer query. It checks if the relationship holds true.

</details>

### Q16: Set Operations (Difference)
**Task:** Find student names who are in the 'Math' club but **NOT** in the 'Science' club.
*   Assumes two queries or tables.

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
-- Option 1: EXCEPT (Standard SQL)
SELECT name FROM MathClub
EXCEPT
SELECT name FROM ScienceClub;

-- Option 2: NOT IN
SELECT name FROM MathClub
WHERE name NOT IN (SELECT name FROM ScienceClub);
```
**Explanation:** `EXCEPT` (or `MINUS` in Oracle) performs set difference ($\text{Math} - \text{Science}$).

</details>

### Q17: Relational Algebra (Cartesian Product)
**RA:** $R \times S$ (or $R \times S$ where R has N rows, S has M rows).
**Question:** How many rows are in the result?

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

**Answer:** $N \times M$ rows.

**SQL:** `SELECT * FROM R, S;` or `SELECT * FROM R CROSS JOIN S;`

**Explanation:** A Cartesian product pairs every single row of R with every single row of S. If R has 10 rows and S has 5, the result is 50 rows.

</details>

### Q18: Self Join (Hierarchy)
**Task:** Find pairs of employees `(A, B)` where A and B work in the same department, but A is not B.

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT A.name, B.name
FROM Employees A
JOIN Employees B ON A.dept_id = B.dept_id
WHERE A.id != B.id; -- or A.id < B.id to avoid duplicates
```
**Explanation:** You join the table to itself using two aliases (A and B). The `WHERE` clause prevents matching an employee to themselves.

</details>

### Q19: The "Division" Problem (Universal Quantifier)
**Task:** Find students who have taken **ALL** courses available in the catalog.
*   (This is the hardest type of question).

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

**Approach 1: Counting**
```sql
SELECT sid
FROM Takes
GROUP BY sid
HAVING COUNT(distinct cid) = (SELECT COUNT(*) FROM Courses);
```
**Explanation:** If a student has taken 5 unique courses, and the total number of courses in the catalog is 5, then they have taken everything.

</details>

### Q20: Complex RA to SQL
**RA:** $\pi_{sname} ( (\sigma_{dept='CS'}(Student)) \bowtie (\rho_{takes}(Enrolled)) )$

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ Show Answer</summary>

```sql
SELECT S.sname
FROM Student S
JOIN Enrolled takes ON S.id = takes.sid
WHERE S.dept = 'CS';
```
**Explanation:**
1.  $\rho_{takes}$ renames the Enrolled table to `takes`.
2.  $\bowtie$ is the Natural Join (implied join on matching IDs).
3.  $\sigma$ filters for CS department.

</details>

[END]

[ZH]
# 📚 考前速览 (核心要点)

*   **执行顺序:** `FROM` -> `WHERE` -> `GROUP BY` -> `HAVING` -> `SELECT` -> `ORDER BY`
*   **逻辑优先级:** `NOT` -> `AND` -> `OR`
*   **RA 符号:** $\sigma$ (选行), $\pi$ (选列), $\bowtie$ (连接), $\rho$ (重命名).

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
**解析:** `DISTINCT` 关键字用于去除重复行。如果不加，如果有 50 个学生住在伦敦，你会看到 50 次 'London'。

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
**解析:** 在 SQL 中不能用 `= NULL` 或 `!= NULL`。必须使用 `IS NULL` 或 `IS NOT NULL`。

</details>

### Q4: 简单计算
**任务:** 显示所有员工的姓名和**年薪** (`annual_salary` = monthly_salary * 12)。
*   表: `Staff` (name, monthly_salary)

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT name, monthly_salary * 12 AS annual_salary
FROM Staff;
```
**解析:** 你可以在 SELECT 子句中直接进行四则运算。`AS` 用于给结果列起别名。

</details>

### Q5: 基础排序
**任务:** 列出所有产品，先按价格从高到低排序，价格相同的按名称 A-Z 排序。

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT *
FROM Products
ORDER BY price DESC, name ASC;
```
**解析:** `ORDER BY` 可以接受多个列。第二个列仅在第一个列数值相同时用于打破平局。

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
**解析:** 当使用聚合函数 (如 COUNT) 时，任何未被聚合的普通列 (如 major) 都必须出现在 `GROUP BY` 子句中。

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
**解析:** `WHERE` 在分组**前**过滤行。`HAVING` 在分组**后**过滤结果（聚合值）。

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
**解析:** `INNER JOIN` (或简写为 JOIN) 只找出两张表中都有匹配的记录。使用别名 (E, D) 可以让代码更清晰。

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
**解析:** 多对多关系通常通过中间表 (`Takes`) 连接。需要做两次 JOIN 才能把学生和课程关联起来。

</details>

### Q10: RA 选择转 SQL
**RA:** $\sigma_{age > 20 \land gender='F'} (Students)$

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT *
FROM Students
WHERE age > 20 AND gender = 'F';
```
**解析:** $\sigma$ (Sigma) 直接对应 `WHERE` 子句。$\land$ 对应逻辑与 `AND`。

</details>

### Q11: RA 投影转 SQL
**RA:** $\pi_{name, id} (Students)$

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT distinct name, id
FROM Students;
```
**解析:** 严格来说，关系代数是集合（不允许重复），所以 $\pi$ 通常暗示 `SELECT DISTINCT`。但在 CS130 中，除非特别说明，写普通 `SELECT` 也可以。

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
**解析:** `LEFT JOIN` 保留所有客户。如果没有匹配的订单，`O.ord_id` 会变成 NULL。`WHERE` 子句专门抓出这些 NULL 行。

</details>

### Q13: 插入数据
**任务:** 向 'CS' 系插入一个 ID 为 101，名字叫 'Alice' 的新学生。

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
INSERT INTO Students (id, name, department)
VALUES (101, 'Alice', 'CS');
```
**解析:** 养成好习惯：在 `VALUES` 前明确列出列名，防止表结构变化导致插入错误。

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
**解析:** 你不能在 WHERE 子句里直接写 `AVG(salary)`。必须先在一个子查询里把它算出来。

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
**解析:** 相关子查询会为外层查询的**每一行**执行一次。它检查 `E.cid = C.cid` 这个关系是否存在。

</details>

### Q16: 集合操作 (差集)
**任务:** 找出在“数学俱乐部”但**不在**“科学俱乐部”的学生名单。

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
-- 写法 1: EXCEPT (标准 SQL)
SELECT name FROM MathClub
EXCEPT
SELECT name FROM ScienceClub;

-- 写法 2: NOT IN
SELECT name FROM MathClub
WHERE name NOT IN (SELECT name FROM ScienceClub);
```
**解析:** `EXCEPT` (在 Oracle 里叫 MINUS) 执行集合差运算 ($\text{Math} - \text{Science}$)。

</details>

### Q17: 关系代数 (笛卡尔积)
**RA:** $R \times S$ (假设 R 有 N 行，S 有 M 行)。
**问题:** 结果有多少行？

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

**答案:** $N \times M$ 行。

**SQL:** `SELECT * FROM R, S;` 或 `SELECT * FROM R CROSS JOIN S;`

**解析:** 笛卡尔积会将 R 的每一行与 S 的每一行配对。如果 R 有 10 行，S 有 5 行，结果就是 50 行。

</details>

### Q18: 自连接 (Self Join)
**任务:** 找出同一部门的员工对 `(A, B)`，要求 A 不是 B (避免自己匹配自己)。

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT A.name, B.name
FROM Employees A
JOIN Employees B ON A.dept_id = B.dept_id
WHERE A.id != B.id; -- 或者 A.id < B.id 来避免 (Bob, Tom) 和 (Tom, Bob) 重复
```
**解析:** 将表与自身连接需要两个别名 (A 和 B)。`WHERE` 子句用于排除自己匹配自己的情况。

</details>

### Q19: "除法" 问题 (全称量词)
**任务:** 找出选修了目录中**所有**课程的学生。
*   (这是最难的题型之一)。

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

**解法: 计数比较法**
```sql
SELECT sid
FROM Takes
GROUP BY sid
HAVING COUNT(distinct cid) = (SELECT COUNT(*) FROM Courses);
```
**解析:** 如果一个学生选修的不重复课程数量等于课程表里的总课程数，那他肯定全选了。

</details>

### Q20: 复杂 RA 转 SQL
**RA:** $\pi_{sname} ( (\sigma_{dept='CS'}(Student)) \bowtie (\rho_{takes}(Enrolled)) )$

<details> <summary style="cursor: pointer; color: #facc15; font-weight: bold;">▼ 点击揭晓答案</summary>

```sql
SELECT S.sname
FROM Student S
JOIN Enrolled takes ON S.id = takes.sid
WHERE S.dept = 'CS';
```
**解析:**
1.  $\rho_{takes}$ 将 Enrolled 表重命名为 `takes`。
2.  $\bowtie$ 是自然连接 (隐含了 ID 相等的条件)。
3.  $\sigma$ 筛选 CS 系的学生。

</details>

[END]
