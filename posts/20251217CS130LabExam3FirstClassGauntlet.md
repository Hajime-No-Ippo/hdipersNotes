---
title: CS130 Lab Exam 3: The 1.1 First Class Gauntlet
title_en: CS130 Lab Exam 3: The Full 20-Question Gauntlet
title_zh: CS130 终极冲刺：电影系统 20 题全解析版 (拿下 1.1)
date: 2025-12-16
categories: CS130
tags: SQL, Relational Algebra, ExamPrep, PostgreSQL
summary_en: The complete 20-question drill. No placeholders. Full DML, Joins, and Relational Algebra logic for Lab Exam 3.
summary_zh: 20 道模拟题完整补全。包含 DML 更新、多表连接、关系代数转换，无任何缺失。
---

# 🍿 Scenario: Cinema Management System
**Schema Definition:**
1.  **`lab3_movie`**: `movie_id`, `title`, `genre`, `duration`, `rating`
2.  **`lab3_viewer`**: `viewer_id`, `name`, `age`, `gender`, `membership`, `total_visits`
3.  **`lab3_watched`**: `viewer_id`, `movie_id`, `watch_date`, `score`, `cinema_location`

---

## [EN] English Version

### 🟢 Level 1: RA & Warm-up (RA to SQL)

### Q1: Selection & Projection
**Task:** `π title, duration ( σ genre='Action' AND duration > 120 (lab3_movie) )`

<details>
<summary>▼ Show Answer</summary>

```sql
SELECT title, duration 
FROM lab3_movie 
WHERE genre = 'Action' AND duration > 120;
```
</details>

### Q2: Triple Bowtie Join
**Task:** `π name ( lab3_viewer ⨝ lab3_watched ⨝ lab3_movie )`

<details>
<summary>▼ Show Answer</summary>

```sql
SELECT v.name 
FROM lab3_viewer v 
JOIN lab3_watched w ON v.viewer_id = w.viewer_id 
JOIN lab3_movie m ON w.movie_id = m.movie_id;
```
</details>

### Q3: Complex RA Filter
**Task:** `π viewer_id ( σ score=10 AND genre='Horror' (watched ⨝ movie) )`

<details>
<summary>▼ Show Answer</summary>

```sql
SELECT w.viewer_id 
FROM lab3_watched w 
JOIN lab3_movie m ON w.movie_id = m.movie_id 
WHERE w.score = 10 AND m.genre = 'Horror';
```
</details>

---

## 🟡 Level 2: DML & Logic Precision (The 1.1 Zone)

### Q4: Rating Cleanup
**Task:** Update `genre` to 'B-Movie' for all movies with a `rating` below 5.0.

<details>
<summary>▼ Show Answer</summary>

```sql
UPDATE lab3_movie 
SET genre = 'B-Movie' 
WHERE rating < 5.0;
```
</details>

### Q5: Post-Exam GDPR Delete
**Task:** Delete movie 'Inception'. How to calculate total rows affected (assuming `ON DELETE CASCADE`)?

<details>
<summary>▼ Show Answer</summary>

```sql
-- Step 1: Count records in child table (watched) that will be automatically deleted
SELECT COUNT(*) FROM lab3_watched 
WHERE movie_id IN (SELECT movie_id FROM lab3_movie WHERE title = 'Inception');

-- Step 2: Count the movie itself
SELECT COUNT(*) FROM lab3_movie WHERE title = 'Inception';

-- Step 3: Execute Delete
DELETE FROM lab3_movie WHERE title = 'Inception';
```
</details>

### Q6: Incremental Duration Update
**Task:** For all 'Horror' movies, increase `duration` by 10 minutes.

<details>
<summary>▼ Show Answer</summary>

```sql
UPDATE lab3_movie 
SET duration = duration + 10 
WHERE genre = 'Horror';
```
</details>

### Q7: String Modification (The ID Prefix)
**Task:** Prefix 'GOLD-' to `viewer_id` for all members with status 'Gold'.

<details>
<summary>▼ Show Answer</summary>

```sql
UPDATE lab3_viewer 
SET viewer_id = 'GOLD-' || viewer_id 
WHERE membership = 'Gold';
```
**Pitfall:** Use `||` for concatenation in PostgreSQL, not `+`.
</details>

### Q8: Handle Missing Data (NULL)
**Task:** Delete viewer records where the `gender` column is empty (Null).

<details>
<summary>▼ Show Answer</summary>

```sql
DELETE FROM lab3_viewer 
WHERE gender IS NULL;
```
**Pitfall:** Never use `= NULL`. Always use `IS NULL`.
</details>

### Q9: Multi-Date Filter
**Task:** Find `viewer_id` who watched movies on '2025-12-24' or '2025-12-25'.

<details>
<summary>▼ Show Answer</summary>

```sql
SELECT DISTINCT viewer_id 
FROM lab3_watched 
WHERE watch_date IN ('2025-12-24', '2025-12-25');
```
</details>

---

## 🔴 Level 3: Advanced Joins & Subqueries

### Q10: Complex Sorting
**Task:** Order all watched records by `score` (Desc), then `watch_date` (Asc), then `name` (Desc/Alpha Last).

<details>
<summary>▼ Show Answer</summary>

```sql
SELECT * 
FROM lab3_watched w 
JOIN lab3_viewer v ON w.viewer_id = v.viewer_id 
ORDER BY w.score DESC, w.watch_date ASC, v.name DESC;
```
</details>

### Q11: Subquery Filtering
**Task:** Find names of viewers who watched the movie with the absolute minimum `rating`.

<details>
<summary>▼ Show Answer</summary>

```sql
SELECT v.name 
FROM lab3_viewer v 
JOIN lab3_watched w ON v.viewer_id = w.viewer_id 
JOIN lab3_movie m ON w.movie_id = m.movie_id 
WHERE m.rating = (SELECT MIN(rating) FROM lab3_movie);
```
</details>

### Q12: Regex Pattern
**Task:** Find names of all viewers whose name starts with 'A' and ends with 'n'.

<details>
<summary>▼ Show Answer</summary>

```sql
SELECT name 
FROM lab3_viewer 
WHERE name ~ '^A.*n$';
-- Or: WHERE name LIKE 'A%n';
```
</details>

### Q13: Join with Numerical Filter
**Task:** List titles of movies watched by viewers aged 18 or under.

<details>
<summary>▼ Show Answer</summary>

```sql
SELECT DISTINCT m.title 
FROM lab3_movie m 
JOIN lab3_watched w ON m.movie_id = w.movie_id 
JOIN lab3_viewer v ON w.viewer_id = v.viewer_id 
WHERE v.age <= 18;
```
</details>

### Q14: Membership Status Update
**Task:** Update `membership` to 'Veteran' for anyone who has more than 50 `total_visits`.

<details>
<summary>▼ Show Answer</summary>

```sql
UPDATE lab3_viewer 
SET membership = 'Veteran' 
WHERE total_visits > 50;
```
</details>

### Q15: Age Calculation Deletion
**Task:** Delete records where the duration between `watch_date` and today (e.g., 2025) is > 10 years.

<details>
<summary>▼ Show Answer</summary>

```sql
DELETE FROM lab3_watched 
WHERE (2025 - EXTRACT(YEAR FROM watch_date)) > 10;
```
</details>

### Q16: Find Duplicate Watchers
**Task:** List viewer names who have watched the same movie more than once.

<details>
<summary>▼ Show Answer</summary>

```sql
SELECT v.name 
FROM lab3_viewer v 
JOIN lab3_watched w ON v.viewer_id = w.viewer_id 
GROUP BY v.name, w.movie_id 
HAVING COUNT(*) > 1;
```
</details>

### Q17: Join with Location Filter
**Task:** List names of viewers who watched movies in 'Dublin' (using `cinema_location`).

<details>
<summary>▼ Show Answer</summary>

```sql
SELECT v.name 
FROM lab3_viewer v 
JOIN lab3_watched w ON v.viewer_id = w.viewer_id 
WHERE w.cinema_location = 'Dublin';
```
</details>

### Q18: Average Score per Genre
**Task:** Find the average `score` given to movies in each `genre`.

<details>
<summary>▼ Show Answer</summary>

```sql
SELECT m.genre, AVG(w.score) 
FROM lab3_movie m 
JOIN lab3_watched w ON m.movie_id = w.movie_id 
GROUP BY m.genre;
```
</details>

### Q19: Update Score by Movie Title
**Task:** Set `score` to 10 for all watches of the movie 'The Godfather'.

<details>
<summary>▼ Show Answer</summary>

```sql
UPDATE lab3_watched 
SET score = 10 
WHERE movie_id IN (SELECT movie_id FROM lab3_movie WHERE title = 'The Godfather');
```
</details>

### Q20: Final RA to SQL Challenge
**Task:** `π title ( σ age < 20 (viewer ⨝ watched ⨝ movie) )`

<details>
<summary>▼ Show Answer</summary>

```sql
SELECT DISTINCT m.title 
FROM lab3_movie m 
JOIN lab3_watched w ON m.movie_id = w.movie_id 
JOIN lab3_viewer v ON w.viewer_id = v.viewer_id 
WHERE v.age < 20;
```
</details>

[END]

---

## [ZH] 中文版 (Chinese Version)

### 🟢 第一关：关系代数热身 (RA to SQL)

### Q1: 选择与投影 (Selection & Projection)
**任务:** `π title, duration ( σ genre='Action' AND duration > 120 (lab3_movie) )`
*(找出所有时长超过 120 分钟的动作片，只显示标题和时长)*

<details>
<summary>▼ 显示答案</summary>

```sql
SELECT title, duration 
FROM lab3_movie 
WHERE genre = 'Action' AND duration > 120;
```
</details>

### Q2: 三表连接 (Triple Bowtie Join)
**任务:** `π name ( lab3_viewer ⨝ lab3_watched ⨝ lab3_movie )`
*(列出所有看过电影的观众姓名)*

<details>
<summary>▼ 显示答案</summary>

```sql
SELECT v.name 
FROM lab3_viewer v 
JOIN lab3_watched w ON v.viewer_id = w.viewer_id 
JOIN lab3_movie m ON w.movie_id = m.movie_id;
```
</details>

### Q3: 复杂 RA 过滤
**任务:** `π viewer_id ( σ score=10 AND genre='Horror' (watched ⨝ movie) )`
*(找出给恐怖片打满分 10 分的观众 ID)*

<details>
<summary>▼ 显示答案</summary>

```sql
SELECT w.viewer_id 
FROM lab3_watched w 
JOIN lab3_movie m ON w.movie_id = m.movie_id 
WHERE w.score = 10 AND m.genre = 'Horror';
```
</details>

---

## 🟡 第二关：DML 与逻辑精度 (1.1 关键区)

### Q4: 评分清理
**任务:** 将所有评分 (`rating`) 低于 5.0 的电影的类型 (`genre`) 更新为 'B-Movie'。

<details>
<summary>▼ 显示答案</summary>

```sql
UPDATE lab3_movie 
SET genre = 'B-Movie' 
WHERE rating < 5.0;
```
</details>

### Q5: 考后 GDPR 删除
**任务:** 删除电影 'Inception'。如何计算受影响的总行数（假设有 `ON DELETE CASCADE`）？

<details>
<summary>▼ 显示答案</summary>

```sql
-- 第一步：计算子表 (watched) 中将被自动删除的记录
SELECT COUNT(*) FROM lab3_watched 
WHERE movie_id IN (SELECT movie_id FROM lab3_movie WHERE title = 'Inception');

-- 第二步：计算电影表本身的记录
SELECT COUNT(*) FROM lab3_movie WHERE title = 'Inception';

-- 第三步：执行删除
DELETE FROM lab3_movie WHERE title = 'Inception';
```
</details>

### Q6: 时长增量更新
**任务:** 将所有 'Horror'（恐怖）电影的时长增加 10 分钟。

<details>
<summary>▼ 显示答案</summary>

```sql
UPDATE lab3_movie 
SET duration = duration + 10 
WHERE genre = 'Horror';
```
</details>

### Q7: 字符串修改 (ID 前缀)
**任务:** 为所有状态为 'Gold' 的会员的 `viewer_id` 加上前缀 'GOLD-'。

<details>
<summary>▼ 显示答案</summary>

```sql
UPDATE lab3_viewer 
SET viewer_id = 'GOLD-' || viewer_id 
WHERE membership = 'Gold';
```
**易错点:** PostgreSQL 中字符串拼接使用 `||`，不要用 `+`。
</details>

### Q8: 处理缺失数据 (NULL)
**任务:** 删除 `gender`（性别）列为空 (Null) 的观众记录。

<details>
<summary>▼ 显示答案</summary>

```sql
DELETE FROM lab3_viewer 
WHERE gender IS NULL;
```
**易错点:** 永远不要使用 `= NULL`，必须使用 `IS NULL`。
</details>

### Q9: 多日期过滤
**任务:** 找出在 '2025-12-24' 或 '2025-12-25' 看过电影的 `viewer_id`。

<details>
<summary>▼ 显示答案</summary>

```sql
SELECT DISTINCT viewer_id 
FROM lab3_watched 
WHERE watch_date IN ('2025-12-24', '2025-12-25');
```
</details>

---

## 🔴 第三关：高级连接与子查询

### Q10: 复杂排序
**任务:** 将所有观影记录按 `score` 降序排列，然后按 `watch_date` 升序排列，最后按 `name` 降序（字母表倒序）排列。

<details>
<summary>▼ 显示答案</summary>

```sql
SELECT * 
FROM lab3_watched w 
JOIN lab3_viewer v ON w.viewer_id = v.viewer_id 
ORDER BY w.score DESC, w.watch_date ASC, v.name DESC;
```
</details>

### Q11: 子查询过滤
**任务:** 找出看过评分绝对最低 (`MIN(rating)`) 的电影的观众姓名。

<details>
<summary>▼ 显示答案</summary>

```sql
SELECT v.name 
FROM lab3_viewer v 
JOIN lab3_watched w ON v.viewer_id = w.viewer_id 
JOIN lab3_movie m ON w.movie_id = m.movie_id 
WHERE m.rating = (SELECT MIN(rating) FROM lab3_movie);
```
</details>

### Q12: 正则表达式模式
**任务:** 找出名字以 'A' 开头且以 'n' 结尾的所有观众姓名。

<details>
<summary>▼ 显示答案</summary>

```sql
SELECT name 
FROM lab3_viewer 
WHERE name ~ '^A.*n$';
-- 或者: WHERE name LIKE 'A%n';
```
</details>

### Q13: 带数值过滤的连接
**任务:** 列出 18 岁或以下观众看过的电影标题。

<details>
<summary>▼ 显示答案</summary>

```sql
SELECT DISTINCT m.title 
FROM lab3_movie m 
JOIN lab3_watched w ON m.movie_id = w.movie_id 
JOIN lab3_viewer v ON w.viewer_id = v.viewer_id 
WHERE v.age <= 18;
```
</details>

### Q14: 会员状态更新
**任务:** 将所有 `total_visits`（总访问次数）超过 50 的观众的 `membership` 更新为 'Veteran'。

<details>
<summary>▼ 显示答案</summary>

```sql
UPDATE lab3_viewer 
SET membership = 'Veteran' 
WHERE total_visits > 50;
```
</details>

### Q15: 年龄计算删除
**任务:** 删除 `watch_date` 距今（例如 2025 年）超过 10 年的记录。

<details>
<summary>▼ 显示答案</summary>

```sql
DELETE FROM lab3_watched 
WHERE (2025 - EXTRACT(YEAR FROM watch_date)) > 10;
```
</details>

### Q16: 查找重复观影者
**任务:** 列出看过同一部电影超过一次的观众姓名。

<details>
<summary>▼ 显示答案</summary>

```sql
SELECT v.name 
FROM lab3_viewer v 
JOIN lab3_watched w ON v.viewer_id = w.viewer_id 
GROUP BY v.name, w.movie_id 
HAVING COUNT(*) > 1;
```
</details>

### Q17: 带地点过滤的连接
**任务:** 列出在 'Dublin'（都柏林）看电影的观众姓名（使用 `cinema_location` 字段）。

<details>
<summary>▼ 显示答案</summary>

```sql
SELECT v.name 
FROM lab3_viewer v 
JOIN lab3_watched w ON v.viewer_id = w.viewer_id 
WHERE w.cinema_location = 'Dublin';
```
</details>

### Q18: 各类型平均分
**任务:** 找出每种电影类型 (`genre`) 的平均评分 (`score`)。

<details>
<summary>▼ 显示答案</summary>

```sql
SELECT m.genre, AVG(w.score) 
FROM lab3_movie m 
JOIN lab3_watched w ON m.movie_id = w.movie_id 
GROUP BY m.genre;
```
</details>

### Q19: 按电影标题更新分数
**任务:** 将所有观看 'The Godfather' 的记录的评分 (`score`) 设为 10。

<details>
<summary>▼ 显示答案</summary>

```sql
UPDATE lab3_watched 
SET score = 10 
WHERE movie_id IN (SELECT movie_id FROM lab3_movie WHERE title = 'The Godfather');
```
</details>

### Q20: 终极 RA 转 SQL 挑战
**任务:** `π title ( σ age < 20 (viewer ⨝ watched ⨝ movie) )`
*(找出 20 岁以下观众看过的电影标题)*

<details>
<summary>▼ 显示答案</summary>

```sql
SELECT DISTINCT m.title 
FROM lab3_movie m 
JOIN lab3_watched w ON m.movie_id = w.movie_id 
JOIN lab3_viewer v ON w.viewer_id = v.viewer_id 
WHERE v.age < 20;
```
</details>

[END]
