# 🗄️ Complete SQL Interview Sheet

> **The ultimate SQL guide for coding interviews & tests.**
> Famous LeetCode / HackerRank / interview problems — Joins, Window Functions, CTEs, Subqueries & more.

---

## 📑 Table of Contents

**FOUNDATIONS (1–10)**
1. SQL Quick Reference (Order of Execution, Joins, Aggregates)
2. Select Distinct Values
3. Filter with WHERE / Multiple Conditions
4. Sort Results (ORDER BY)
5. Limit Results (LIMIT / OFFSET / TOP)
6. Group By with Aggregate Functions
7. HAVING vs WHERE
8. Basic JOINs (INNER, LEFT, RIGHT, FULL, CROSS)
9. Self Join Pattern
10. UNION vs UNION ALL vs INTERSECT vs EXCEPT

**EASY — CLASSIC INTERVIEW (11–25)**
11. Second Highest Salary
12. Nth Highest Salary
13. Top N Salaries per Department
14. Employees Earning More Than Their Manager
15. Department Highest Salary
16. Duplicate Emails
17. Customers Who Never Order
18. Combine Two Tables (Left Join)
19. Big Countries
20. Classes With More Than 5 Students
21. Swap Salary (Update with CASE)
22. Delete Duplicate Emails
23. Rising Temperature (Compare Today vs Yesterday)
24. Game Play Analysis — First Login Date
25. Find Customer Referee

**MEDIUM (26–42)**
26. Rank Scores (DENSE_RANK)
27. Consecutive Numbers Appearing 3+ Times
28. Department Top Three Salaries
29. Trips and Users (Cancellation Rate)
30. Human Traffic of Stadium (3+ Consecutive Days)
31. Exchange Seats (Pair Swap)
32. Tree Node Type (Root / Inner / Leaf)
33. Friend Requests — Acceptance Rate
34. Investments in 2016
35. Sales Person — Customers Not Bought
36. Customer Placing Largest Number of Orders
37. Triangle Judgement
38. Reformat Department Table (Pivot)
39. Average Salary — Departments vs Company
40. Game Play Analysis — Players Logged In Next Day
41. Capital Gain / Loss per Stock
42. Active Users (Streak of Consecutive Days)

**HARD (43–55)**
43. Median Employee Salary (No Built-in Median)
44. Find Median in a Number Table (Frequency)
45. Trips Cancellation Rate (Window + Filter)
46. Highest Salary in Each Department (Window)
47. Department Top N Salaries
48. Report Contiguous Dates (Gaps & Islands)
49. Running Total / Cumulative Sum (Window)
50. Year-over-Year / Month-over-Month Growth
51. Pivot Table — Quarterly Sales by Product
52. Find the Quiet Students in All Exams
53. Hopper Company Queries
54. Last Person to Fit in the Bus (Cumulative Weight)
55. Number of Transactions per Visit (LEFT JOIN + COUNT)

---

## ⚡ Quick Reference Cheat Sheet

### Order of Execution (NOT order of writing)

```
FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT
```

### JOIN Types

| JOIN | Returns |
|------|---------|
| `INNER JOIN` | Only matching rows in both tables |
| `LEFT JOIN` | All from left + matched from right (NULLs if no match) |
| `RIGHT JOIN` | All from right + matched from left |
| `FULL OUTER JOIN` | All rows from both (NULLs where no match) |
| `CROSS JOIN` | Cartesian product (every combination) |
| `SELF JOIN` | Table joined with itself (use aliases) |

### Aggregate Functions

```sql
COUNT(*)        -- count all rows including NULLs
COUNT(col)      -- count non-NULL values
COUNT(DISTINCT col)
SUM(col), AVG(col), MIN(col), MAX(col)
STRING_AGG(col, ','), GROUP_CONCAT(col)
```

### Window Functions

```sql
ROW_NUMBER()  OVER (PARTITION BY ... ORDER BY ...)
RANK()        OVER (PARTITION BY ... ORDER BY ...)
DENSE_RANK()  OVER (PARTITION BY ... ORDER BY ...)
LAG(col, n)   OVER (PARTITION BY ... ORDER BY ...)
LEAD(col, n)  OVER (PARTITION BY ... ORDER BY ...)
SUM(col)      OVER (PARTITION BY ... ORDER BY ... ROWS BETWEEN ...)
NTILE(n)      OVER (...)
FIRST_VALUE(col), LAST_VALUE(col), NTH_VALUE(col, n)
```

### Common Functions

```sql
COALESCE(col, default)     -- returns first non-NULL
IFNULL(col, default)       -- MySQL only (same idea)
ISNULL(col, default)       -- SQL Server
NULLIF(a, b)               -- NULL if a == b, else a
CASE WHEN x THEN y ELSE z END
CAST(col AS INT)           -- standard
CONVERT(INT, col)          -- SQL Server
DATEDIFF(d1, d2)           -- date difference
DATE_ADD(date, INTERVAL 1 DAY)   -- MySQL
DATEADD(day, 1, date)      -- SQL Server
CONCAT(a, b, c) or  a || b -- depending on DB
```

---

# 🟢 FOUNDATIONS

---

## 1. SQL Quick Reference

```sql
-- Sample SELECT skeleton
SELECT col1, col2, AGG(col3) AS alias
FROM table1 t1
JOIN table2 t2 ON t1.id = t2.id
WHERE condition
GROUP BY col1, col2
HAVING AGG(col3) > value
ORDER BY col1 DESC
LIMIT 10 OFFSET 5;
```

---

## 2. Select Distinct Values

```sql
SELECT DISTINCT department
FROM employees;

-- Distinct combinations
SELECT DISTINCT department, role
FROM employees;
```

---

## 3. Filter with WHERE / Multiple Conditions

```sql
SELECT * FROM employees
WHERE department = 'Sales'
  AND salary > 50000
  AND hire_date >= '2020-01-01';

-- IN, BETWEEN, LIKE
SELECT * FROM employees
WHERE department IN ('Sales', 'HR', 'IT')
  AND salary BETWEEN 40000 AND 80000
  AND name LIKE 'A%';            -- starts with A
```

> **`%`** matches any sequence of chars; **`_`** matches a single char.

---

## 4. Sort Results (ORDER BY)

```sql
SELECT name, salary
FROM employees
ORDER BY salary DESC, name ASC;

-- NULLs: NULLS FIRST / NULLS LAST (PostgreSQL); MySQL puts NULLs first on ASC
```

---

## 5. Limit Results (LIMIT / OFFSET / TOP)

```sql
-- MySQL / PostgreSQL
SELECT * FROM employees ORDER BY salary DESC LIMIT 5;
SELECT * FROM employees ORDER BY salary DESC LIMIT 5 OFFSET 10;

-- SQL Server
SELECT TOP 5 * FROM employees ORDER BY salary DESC;
SELECT * FROM employees ORDER BY salary DESC
OFFSET 10 ROWS FETCH NEXT 5 ROWS ONLY;
```

---

## 6. GROUP BY with Aggregate Functions

```sql
SELECT department, COUNT(*) AS num_employees, AVG(salary) AS avg_salary
FROM employees
GROUP BY department;
```

> **Rule:** every column in `SELECT` not inside an aggregate must appear in `GROUP BY`.

---

## 7. HAVING vs WHERE

```sql
-- WHERE: filters rows BEFORE aggregation
-- HAVING: filters groups AFTER aggregation
SELECT department, COUNT(*) AS num_employees
FROM employees
WHERE salary > 30000        -- filter rows first
GROUP BY department
HAVING COUNT(*) > 5;        -- then filter groups
```

---

## 8. Basic JOINs

```sql
-- INNER JOIN: only matching rows
SELECT e.name, d.dept_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.id;

-- LEFT JOIN: all employees, even those without a department
SELECT e.name, d.dept_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.id;

-- FULL OUTER JOIN: every row from both tables
SELECT e.name, d.dept_name
FROM employees e
FULL OUTER JOIN departments d ON e.dept_id = d.id;

-- CROSS JOIN: cartesian product
SELECT e.name, p.project_name
FROM employees e CROSS JOIN projects p;
```

---

## 9. Self Join Pattern

**Find pairs of employees in the same department.**

```sql
SELECT a.name AS emp1, b.name AS emp2, a.department
FROM employees a
JOIN employees b
  ON a.department = b.department AND a.id < b.id;
```

---

## 10. UNION vs UNION ALL vs INTERSECT vs EXCEPT

```sql
-- UNION: combine + remove duplicates
SELECT name FROM employees_2023
UNION
SELECT name FROM employees_2024;

-- UNION ALL: keep duplicates (faster)
SELECT name FROM employees_2023
UNION ALL
SELECT name FROM employees_2024;

-- INTERSECT: rows in BOTH
SELECT name FROM employees_2023
INTERSECT
SELECT name FROM employees_2024;

-- EXCEPT (or MINUS in Oracle): in first, NOT in second
SELECT name FROM employees_2023
EXCEPT
SELECT name FROM employees_2024;
```

---

# 🟢 EASY — CLASSIC INTERVIEW

---

## 11. Second Highest Salary

**LeetCode 176** — return NULL if doesn't exist.

```sql
-- Approach 1: subquery + LIMIT/OFFSET
SELECT (
  SELECT DISTINCT salary
  FROM employee
  ORDER BY salary DESC
  LIMIT 1 OFFSET 1
) AS SecondHighestSalary;

-- Approach 2: subquery with MAX
SELECT MAX(salary) AS SecondHighestSalary
FROM employee
WHERE salary < (SELECT MAX(salary) FROM employee);

-- Approach 3: DENSE_RANK (handles ties cleanly)
SELECT MAX(salary) AS SecondHighestSalary
FROM (
  SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
  FROM employee
) t
WHERE rnk = 2;
```

---

## 12. Nth Highest Salary

**LeetCode 177**

```sql
-- MySQL function
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  SET N = N - 1;
  RETURN (
    SELECT DISTINCT salary
    FROM employee
    ORDER BY salary DESC
    LIMIT 1 OFFSET N
  );
END;

-- Pure query alternative
SELECT MAX(salary) AS NthHighestSalary
FROM (
  SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
  FROM employee
) t
WHERE rnk = N;       -- replace N with the value
```

---

## 13. Top N Salaries per Department

```sql
SELECT department, name, salary
FROM (
  SELECT name, department, salary,
         DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rnk
  FROM employees
) t
WHERE rnk <= 3;
```

---

## 14. Employees Earning More Than Their Manager

**LeetCode 181**

```sql
SELECT e.name AS Employee
FROM employee e
JOIN employee m ON e.manager_id = m.id
WHERE e.salary > m.salary;
```

---

## 15. Department Highest Salary

**LeetCode 184**

```sql
SELECT d.name AS Department, e.name AS Employee, e.salary AS Salary
FROM employee e
JOIN department d ON e.department_id = d.id
WHERE (e.department_id, e.salary) IN (
  SELECT department_id, MAX(salary)
  FROM employee
  GROUP BY department_id
);

-- Alternate: window function
SELECT Department, Employee, Salary
FROM (
  SELECT d.name AS Department, e.name AS Employee, e.salary AS Salary,
         RANK() OVER (PARTITION BY e.department_id ORDER BY e.salary DESC) AS rnk
  FROM employee e
  JOIN department d ON e.department_id = d.id
) t
WHERE rnk = 1;
```

---

## 16. Duplicate Emails

**LeetCode 182**

```sql
SELECT email AS Email
FROM person
GROUP BY email
HAVING COUNT(*) > 1;
```

---

## 17. Customers Who Never Order

**LeetCode 183**

```sql
SELECT name AS Customers
FROM customers
WHERE id NOT IN (SELECT customer_id FROM orders);

-- Alternative: LEFT JOIN + IS NULL
SELECT c.name AS Customers
FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id
WHERE o.customer_id IS NULL;
```

---

## 18. Combine Two Tables (Left Join)

**LeetCode 175** — show every person, even those without an address.

```sql
SELECT p.firstName, p.lastName, a.city, a.state
FROM person p
LEFT JOIN address a ON p.personId = a.personId;
```

---

## 19. Big Countries

**LeetCode 595** — area ≥ 3,000,000 OR population ≥ 25,000,000.

```sql
SELECT name, population, area
FROM world
WHERE area >= 3000000 OR population >= 25000000;
```

---

## 20. Classes With More Than 5 Students

**LeetCode 596**

```sql
SELECT class
FROM courses
GROUP BY class
HAVING COUNT(DISTINCT student) >= 5;
```

---

## 21. Swap Salary (Update with CASE)

**LeetCode 627** — swap 'm' ↔ 'f' in one update.

```sql
UPDATE salary
SET sex = CASE sex
            WHEN 'm' THEN 'f'
            WHEN 'f' THEN 'm'
          END;
```

---

## 22. Delete Duplicate Emails — Keep Smallest ID

**LeetCode 196**

```sql
DELETE p1
FROM person p1
JOIN person p2
  ON p1.email = p2.email AND p1.id > p2.id;

-- PostgreSQL alternative
DELETE FROM person
WHERE id NOT IN (
  SELECT MIN(id) FROM person GROUP BY email
);
```

---

## 23. Rising Temperature

**LeetCode 197** — find IDs of dates whose temperature is higher than the previous day.

```sql
SELECT w1.id
FROM weather w1
JOIN weather w2
  ON DATEDIFF(w1.recordDate, w2.recordDate) = 1
WHERE w1.temperature > w2.temperature;

-- Window-function alternative (PostgreSQL / SQL Server)
SELECT id FROM (
  SELECT id, temperature, recordDate,
         LAG(temperature) OVER (ORDER BY recordDate) AS prev_temp,
         LAG(recordDate)  OVER (ORDER BY recordDate) AS prev_date
  FROM weather
) t
WHERE temperature > prev_temp
  AND recordDate = prev_date + INTERVAL '1 day';
```

---

## 24. Game Play Analysis — First Login Date

**LeetCode 511**

```sql
SELECT player_id, MIN(event_date) AS first_login
FROM activity
GROUP BY player_id;
```

---

## 25. Find Customer Referee

**LeetCode 584** — return customers NOT referred by id 2 (NULL referees count).

```sql
SELECT name
FROM customer
WHERE referee_id != 2 OR referee_id IS NULL;
```

> **Gotcha:** `referee_id != 2` excludes NULLs because comparison with NULL is unknown. Hence the explicit `IS NULL` check.

---

# 🟡 MEDIUM PROBLEMS

---

## 26. Rank Scores

**LeetCode 178** — DENSE_RANK so equal scores get same rank, no gaps.

```sql
SELECT score,
       DENSE_RANK() OVER (ORDER BY score DESC) AS 'rank'
FROM scores;
```

---

## 27. Consecutive Numbers Appearing 3+ Times

**LeetCode 180**

```sql
SELECT DISTINCT l1.num AS ConsecutiveNums
FROM logs l1
JOIN logs l2 ON l2.id = l1.id + 1 AND l2.num = l1.num
JOIN logs l3 ON l3.id = l1.id + 2 AND l3.num = l1.num;

-- Window-function alternative
WITH groups AS (
  SELECT num,
         id - ROW_NUMBER() OVER (PARTITION BY num ORDER BY id) AS grp
  FROM logs
)
SELECT DISTINCT num AS ConsecutiveNums
FROM groups
GROUP BY num, grp
HAVING COUNT(*) >= 3;
```

> **Trick:** `id - ROW_NUMBER()` is a classic gaps-and-islands technique to identify consecutive runs.

---

## 28. Department Top Three Salaries

**LeetCode 185**

```sql
SELECT d.name AS Department, e.name AS Employee, e.salary AS Salary
FROM (
  SELECT name, salary, department_id,
         DENSE_RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) AS rnk
  FROM employee
) e
JOIN department d ON e.department_id = d.id
WHERE rnk <= 3;
```

---

## 29. Trips and Users — Cancellation Rate

**LeetCode 262**

```sql
SELECT request_at AS Day,
       ROUND(
         SUM(CASE WHEN status != 'completed' THEN 1 ELSE 0 END) * 1.0 / COUNT(*),
         2
       ) AS 'Cancellation Rate'
FROM trips t
WHERE request_at BETWEEN '2013-10-01' AND '2013-10-03'
  AND client_id    NOT IN (SELECT users_id FROM users WHERE banned = 'Yes')
  AND driver_id    NOT IN (SELECT users_id FROM users WHERE banned = 'Yes')
GROUP BY request_at;
```

---

## 30. Human Traffic of Stadium — 3+ Consecutive Days

**LeetCode 601** — find rows where ≥100 visitors AND it's part of a 3+ consecutive day streak.

```sql
WITH high AS (
  SELECT *,
         id - ROW_NUMBER() OVER (ORDER BY id) AS grp
  FROM stadium
  WHERE people >= 100
)
SELECT id, visit_date, people
FROM high
WHERE grp IN (
  SELECT grp FROM high GROUP BY grp HAVING COUNT(*) >= 3
)
ORDER BY visit_date;
```

---

## 31. Exchange Seats

**LeetCode 626** — swap each pair of consecutive seat IDs (1↔2, 3↔4, …). If odd count, last stays.

```sql
SELECT
  CASE
    WHEN id % 2 = 1 AND id = (SELECT MAX(id) FROM seat) THEN id
    WHEN id % 2 = 1 THEN id + 1
    ELSE id - 1
  END AS id,
  student
FROM seat
ORDER BY id;
```

---

## 32. Tree Node Type

**LeetCode 608** — classify each node as Root / Inner / Leaf.

```sql
SELECT id,
       CASE
         WHEN p_id IS NULL THEN 'Root'
         WHEN id IN (SELECT DISTINCT p_id FROM tree WHERE p_id IS NOT NULL) THEN 'Inner'
         ELSE 'Leaf'
       END AS type
FROM tree;
```

---

## 33. Friend Requests — Acceptance Rate

**LeetCode 597**

```sql
SELECT
  ROUND(
    COALESCE(
      (SELECT COUNT(DISTINCT requester_id, accepter_id) FROM request_accepted) * 1.0 /
      NULLIF((SELECT COUNT(DISTINCT sender_id, send_to_id) FROM friend_request), 0),
    0),
    2
  ) AS accept_rate;
```

---

## 34. Investments in 2016

**LeetCode 585** — sum of `tiv_2016` for rows where `tiv_2015` is unique-shared but `(lat, lon)` is unique.

```sql
SELECT ROUND(SUM(tiv_2016), 2) AS tiv_2016
FROM insurance
WHERE tiv_2015 IN (
  SELECT tiv_2015 FROM insurance GROUP BY tiv_2015 HAVING COUNT(*) > 1
)
AND (lat, lon) IN (
  SELECT lat, lon FROM insurance GROUP BY lat, lon HAVING COUNT(*) = 1
);
```

---

## 35. Sales Person — Customers Not Bought from Red Company

**LeetCode 607**

```sql
SELECT s.name
FROM salesperson s
WHERE s.sales_id NOT IN (
  SELECT o.sales_id
  FROM orders o
  JOIN company c ON o.com_id = c.com_id
  WHERE c.name = 'RED'
);
```

---

## 36. Customer Placing Largest Number of Orders

**LeetCode 586**

```sql
SELECT customer_number
FROM orders
GROUP BY customer_number
ORDER BY COUNT(*) DESC
LIMIT 1;
```

---

## 37. Triangle Judgement

**LeetCode 610** — for each row, is it a triangle (sum of any two sides > third)?

```sql
SELECT x, y, z,
       CASE WHEN x + y > z AND x + z > y AND y + z > x
            THEN 'Yes' ELSE 'No' END AS triangle
FROM triangle;
```

---

## 38. Reformat Department Table — Pivot

**LeetCode 1179** — pivot monthly revenue.

```sql
SELECT id,
  SUM(CASE WHEN month = 'Jan' THEN revenue END) AS Jan_Revenue,
  SUM(CASE WHEN month = 'Feb' THEN revenue END) AS Feb_Revenue,
  SUM(CASE WHEN month = 'Mar' THEN revenue END) AS Mar_Revenue,
  SUM(CASE WHEN month = 'Apr' THEN revenue END) AS Apr_Revenue,
  SUM(CASE WHEN month = 'May' THEN revenue END) AS May_Revenue,
  SUM(CASE WHEN month = 'Jun' THEN revenue END) AS Jun_Revenue,
  SUM(CASE WHEN month = 'Jul' THEN revenue END) AS Jul_Revenue,
  SUM(CASE WHEN month = 'Aug' THEN revenue END) AS Aug_Revenue,
  SUM(CASE WHEN month = 'Sep' THEN revenue END) AS Sep_Revenue,
  SUM(CASE WHEN month = 'Oct' THEN revenue END) AS Oct_Revenue,
  SUM(CASE WHEN month = 'Nov' THEN revenue END) AS Nov_Revenue,
  SUM(CASE WHEN month = 'Dec' THEN revenue END) AS Dec_Revenue
FROM department
GROUP BY id;
```

---

## 39. Average Salary — Departments vs Company

**LeetCode 615** — flag each department-month as "higher" / "lower" / "same".

```sql
WITH dept_avg AS (
  SELECT DATE_FORMAT(s.pay_date, '%Y-%m') AS pay_month,
         e.department_id,
         AVG(s.amount) AS dept_avg
  FROM salary s
  JOIN employee e ON s.employee_id = e.employee_id
  GROUP BY pay_month, e.department_id
),
co_avg AS (
  SELECT DATE_FORMAT(pay_date, '%Y-%m') AS pay_month,
         AVG(amount) AS co_avg
  FROM salary
  GROUP BY pay_month
)
SELECT d.pay_month, d.department_id,
       CASE
         WHEN d.dept_avg > c.co_avg THEN 'higher'
         WHEN d.dept_avg < c.co_avg THEN 'lower'
         ELSE 'same'
       END AS comparison
FROM dept_avg d
JOIN co_avg c USING (pay_month);
```

---

## 40. Game Play Analysis — Players Logged In Next Day Fraction

**LeetCode 550**

```sql
SELECT ROUND(COUNT(DISTINCT a.player_id) * 1.0 /
             (SELECT COUNT(DISTINCT player_id) FROM activity), 2) AS fraction
FROM activity a
WHERE (a.player_id, DATE_SUB(a.event_date, INTERVAL 1 DAY)) IN (
  SELECT player_id, MIN(event_date) FROM activity GROUP BY player_id
);
```

---

## 41. Capital Gain / Loss per Stock

**LeetCode 1393**

```sql
SELECT stock_name,
       SUM(CASE WHEN operation = 'Sell' THEN price ELSE -price END) AS capital_gain_loss
FROM stocks
GROUP BY stock_name;
```

---

## 42. Active Users — 5+ Consecutive Days

```sql
WITH grouped AS (
  SELECT user_id, login_date,
         DATE_SUB(login_date,
                  INTERVAL ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY login_date) DAY)
         AS grp
  FROM (SELECT DISTINCT user_id, login_date FROM logins) d
)
SELECT DISTINCT user_id
FROM grouped
GROUP BY user_id, grp
HAVING COUNT(*) >= 5;
```

---

# 🔴 HARD PROBLEMS

---

## 43. Median Employee Salary (No Built-in Median)

**LeetCode 569** — for each company, find the median salary row(s).

```sql
WITH ranked AS (
  SELECT id, company, salary,
         ROW_NUMBER() OVER (PARTITION BY company ORDER BY salary, id) AS rn,
         COUNT(*)    OVER (PARTITION BY company)                    AS cnt
  FROM employee
)
SELECT id, company, salary
FROM ranked
WHERE rn IN (FLOOR((cnt + 1) / 2), CEIL((cnt + 1) / 2.0));
```

---

## 44. Find Median in a Number Table — Frequency

**LeetCode 571**

```sql
WITH cumulative AS (
  SELECT num, frequency,
         SUM(frequency) OVER (ORDER BY num) AS running,
         SUM(frequency) OVER ()             AS total
  FROM numbers
)
SELECT AVG(num * 1.0) AS median
FROM cumulative
WHERE running        >= total / 2.0
  AND running - frequency <= total / 2.0;
```

---

## 45. Trips Cancellation Rate — Window + Filter

```sql
SELECT request_at AS Day,
       ROUND(
         AVG(CASE WHEN status != 'completed' THEN 1.0 ELSE 0.0 END), 2
       ) AS cancellation_rate
FROM trips t
WHERE NOT EXISTS (
  SELECT 1 FROM users u
  WHERE u.users_id IN (t.client_id, t.driver_id) AND u.banned = 'Yes'
)
GROUP BY request_at;
```

---

## 46. Highest Salary in Each Department — Window

```sql
SELECT department, name, salary
FROM (
  SELECT department, name, salary,
         RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rnk
  FROM employees
) t
WHERE rnk = 1;
```

---

## 47. Department Top N Salaries (Parameterized)

```sql
-- Replace N with the desired value
SELECT department, name, salary
FROM (
  SELECT department, name, salary,
         DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rnk
  FROM employees
) t
WHERE rnk <= N;
```

---

## 48. Report Contiguous Dates — Gaps & Islands

**LeetCode 1285 / Stock periods, etc.**

```sql
WITH ordered AS (
  SELECT status, log_date,
         log_date - INTERVAL ROW_NUMBER() OVER (PARTITION BY status ORDER BY log_date) DAY AS grp
  FROM failed_logs
)
SELECT status,
       MIN(log_date) AS start_date,
       MAX(log_date) AS end_date
FROM ordered
GROUP BY status, grp
ORDER BY start_date;
```

---

## 49. Running Total / Cumulative Sum

```sql
SELECT order_date, amount,
       SUM(amount) OVER (ORDER BY order_date
                         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total
FROM orders;

-- Cumulative per customer
SELECT customer_id, order_date, amount,
       SUM(amount) OVER (PARTITION BY customer_id ORDER BY order_date) AS cust_running_total
FROM orders;
```

---

## 50. Year-over-Year / Month-over-Month Growth

```sql
WITH monthly AS (
  SELECT DATE_FORMAT(order_date, '%Y-%m') AS month, SUM(amount) AS revenue
  FROM orders
  GROUP BY month
)
SELECT month, revenue,
       LAG(revenue) OVER (ORDER BY month)                                 AS prev_revenue,
       ROUND((revenue - LAG(revenue) OVER (ORDER BY month)) * 100.0 /
             NULLIF(LAG(revenue) OVER (ORDER BY month), 0), 2)            AS mom_growth_pct
FROM monthly;
```

---

## 51. Pivot Table — Quarterly Sales by Product

```sql
SELECT product,
       SUM(CASE WHEN QUARTER(sale_date) = 1 THEN amount ELSE 0 END) AS Q1,
       SUM(CASE WHEN QUARTER(sale_date) = 2 THEN amount ELSE 0 END) AS Q2,
       SUM(CASE WHEN QUARTER(sale_date) = 3 THEN amount ELSE 0 END) AS Q3,
       SUM(CASE WHEN QUARTER(sale_date) = 4 THEN amount ELSE 0 END) AS Q4
FROM sales
WHERE YEAR(sale_date) = 2024
GROUP BY product;
```

---

## 52. Find the Quiet Students in All Exams

**LeetCode 1412** — students who never scored the lowest or highest in any exam.

```sql
WITH exam_stats AS (
  SELECT exam_id,
         MIN(score) AS lo,
         MAX(score) AS hi
  FROM exam
  GROUP BY exam_id
),
loud AS (
  SELECT DISTINCT e.student_id
  FROM exam e
  JOIN exam_stats s ON e.exam_id = s.exam_id
  WHERE e.score = s.lo OR e.score = s.hi
)
SELECT s.student_id, s.student_name
FROM student s
JOIN exam e ON s.student_id = e.student_id
WHERE s.student_id NOT IN (SELECT student_id FROM loud)
GROUP BY s.student_id, s.student_name
ORDER BY s.student_id;
```

---

## 53. Hopper Company Queries — Active Drivers per Month

**LeetCode 1645 sketch** — hires-by-month with cumulative count.

```sql
WITH months AS (
  SELECT 1 AS m UNION SELECT 2 UNION SELECT 3 UNION SELECT 4
  UNION SELECT 5 UNION SELECT 6 UNION SELECT 7 UNION SELECT 8
  UNION SELECT 9 UNION SELECT 10 UNION SELECT 11 UNION SELECT 12
)
SELECT m.m AS month,
       (SELECT COUNT(*) FROM drivers d
        WHERE YEAR(d.join_date) < 2020
           OR (YEAR(d.join_date) = 2020 AND MONTH(d.join_date) <= m.m)
       ) AS active_drivers
FROM months m;
```

---

## 54. Last Person to Fit in the Bus — Cumulative Weight

**LeetCode 1204** — running sum until weight ≤ 1000.

```sql
SELECT person_name
FROM (
  SELECT person_name, weight, turn,
         SUM(weight) OVER (ORDER BY turn) AS cum_weight
  FROM queue
) t
WHERE cum_weight <= 1000
ORDER BY turn DESC
LIMIT 1;
```

---

## 55. Number of Transactions per Visit

**LeetCode 1async / 1336-style** — for each (user, visit), how many transactions?

```sql
SELECT v.user_id, v.visit_date, COUNT(t.transaction_id) AS num_transactions
FROM visits v
LEFT JOIN transactions t
  ON v.user_id = t.user_id AND v.visit_date = t.transaction_date
GROUP BY v.user_id, v.visit_date;
```

---

# 🎯 Bonus — Quick Reference

## ⭐ Pattern Recognition

| Problem Signal | Technique |
|----------------|-----------|
| "Nth highest / lowest" | `LIMIT/OFFSET` or `DENSE_RANK()` |
| "Top N per group" | Window function + filter rank |
| "Running / cumulative total" | `SUM() OVER (ORDER BY ...)` |
| "Compare with previous row" | `LAG()` / self-join on offset id |
| "Compare today vs yesterday (date)" | `DATEDIFF` self-join or `LAG` over ordered date |
| "Consecutive runs / streaks" | `id - ROW_NUMBER()` gaps & islands trick |
| "Pivot wide format" | `SUM(CASE WHEN col=val THEN ... END)` |
| "Filter then aggregate" | `WHERE` |
| "Aggregate then filter" | `HAVING` |
| "Find duplicates" | `GROUP BY + HAVING COUNT(*) > 1` |
| "Customers with no orders" | `LEFT JOIN ... WHERE x IS NULL` or `NOT IN` |
| "Median" | Window + count-based filtering |
| "Acceptance / cancellation rate" | `AVG(CASE WHEN ... THEN 1.0 ELSE 0.0 END)` |
| "Earliest / latest event" | `MIN/MAX` per group, or `ROW_NUMBER()` |

---

## ⭐ Top 10 Must-Know Concepts

1. **JOINs** — INNER, LEFT, SELF (cover 70% of interview questions)
2. **GROUP BY + HAVING** — filtered aggregates
3. **Window Functions** — `ROW_NUMBER`, `RANK`, `DENSE_RANK`, `LAG`, `LEAD`
4. **CTEs (`WITH`)** — readable multi-step queries
5. **Subqueries** — correlated and uncorrelated
6. **CASE WHEN** — conditional logic, pivots, classifications
7. **`DENSE_RANK` for Nth-highest** — handles ties cleanly
8. **Gaps & Islands** — `id − ROW_NUMBER()` for consecutive groups
9. **Date Functions** — `DATEDIFF`, `DATE_ADD`, `DATE_FORMAT`
10. **NULL handling** — `IS NULL`, `COALESCE`, `NULLIF`, `!=` ignores NULLs

---

## ⭐ Common Pitfalls

✅ **`!=` excludes NULL** — use `OR col IS NULL` to include them.
✅ **`COUNT(col)`** ignores NULLs; **`COUNT(*)`** counts all rows.
✅ **GROUP BY** — every non-aggregated SELECT column must be in GROUP BY (strict mode).
✅ **`WHERE` cannot reference column aliases** in most dialects (use the full expression or wrap in subquery).
✅ **`ORDER BY` without `LIMIT`** is fine but expensive on huge tables.
✅ **Floor division (integer arithmetic)** — multiply by `1.0` or cast to `DECIMAL` for percentages.
✅ **`DISTINCT` is global** to the SELECT list, not per-column.
✅ **Self join** — always use aliases (`a`, `b`) and a join condition that excludes the same row (`a.id < b.id`).
✅ **`UNION` removes duplicates** (slower); `UNION ALL` keeps them (faster).
✅ **`HAVING COUNT(*) > 0`** is redundant — `INNER JOIN` already filters.
✅ **Window function alias unusable in same SELECT** — wrap in subquery or CTE to filter.
✅ **`LIMIT` with `OFFSET`** is unstable without `ORDER BY`.

---

## ⭐ Useful Snippets

```sql
-- Latest record per group
SELECT *
FROM (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY event_time DESC) AS rn
  FROM events
) t
WHERE rn = 1;

-- Percentage of total
SELECT category,
       SUM(amount) AS total,
       SUM(amount) * 100.0 / SUM(SUM(amount)) OVER () AS pct_of_total
FROM sales
GROUP BY category;

-- Count distinct conditional
SELECT COUNT(DISTINCT CASE WHEN status = 'paid' THEN user_id END) AS paid_users
FROM orders;

-- Anti-join with EXISTS / NOT EXISTS
SELECT *
FROM customers c
WHERE NOT EXISTS (
  SELECT 1 FROM orders o WHERE o.customer_id = c.id
);

-- Date bucketing
SELECT DATE_TRUNC('month', order_date) AS month, SUM(amount)
FROM orders
GROUP BY month;
-- MySQL alternative:
-- DATE_FORMAT(order_date, '%Y-%m')

-- IN with subquery (correlated)
SELECT * FROM employees e
WHERE salary > (
  SELECT AVG(salary) FROM employees WHERE department_id = e.department_id
);
```

---

## ⭐ Tricks Worth Remembering

| Trick | Use Case |
|-------|----------|
| `id − ROW_NUMBER() OVER (...)` | Identify consecutive runs (gaps & islands) |
| `DENSE_RANK()` | Nth-highest with ties handled |
| `ROW_NUMBER() = 1` after PARTITION | Latest / top per group |
| `LAG()` / `LEAD()` | Compare with prev / next row |
| `(col1, col2) IN (SELECT col1, col2 FROM ...)` | Tuple membership |
| `COUNT(CASE WHEN ... THEN 1 END)` | Conditional count |
| `SUM(CASE WHEN month=... THEN amt END)` | Pivot wide |
| `LEFT JOIN ... IS NULL` | Anti-join (rows in A not in B) |
| `NULLIF(x, 0)` | Avoid divide-by-zero |
| `COALESCE(a, b, c)` | First non-NULL fallback |

---

## ⭐ Optimization Tips (Briefly)

✅ **Indexes** speed up WHERE, JOIN, ORDER BY columns.
✅ **Avoid `SELECT *`** — be explicit; helps the planner.
✅ **`EXISTS`** is often faster than `IN` for subqueries with large sets.
✅ **Push filters into subqueries** so less data flows up.
✅ **CTE vs subquery** — most modern engines optimize CTEs well; use what's clearest.
✅ **Avoid functions on indexed columns in WHERE** (`WHERE YEAR(date) = 2024` may skip the index — use `WHERE date BETWEEN '2024-01-01' AND '2024-12-31'`).
✅ **Use `LIMIT`** when you don't need every row.

---

# 💪 GO ACE THAT TEST!

> **Strategy on the test:**
> 1. **Read the schema first** — know each column's type & relationships.
> 2. **Check edge cases**: NULLs, empty groups, duplicates, ties.
> 3. **Default to `LEFT JOIN` over subqueries** for "missing match" problems.
> 4. **Use a CTE** the moment your query gets nested — much more readable.
> 5. **Test on small data first** — write the query, mentally trace through 2–3 rows.
> 6. **Match the dialect:** `LIMIT` (MySQL/PG) vs `TOP` (SQL Server); `||` (PG/Oracle) vs `CONCAT` (MySQL).
> 7. Watch for **`!=` and NULL** — the silent killer of correctness.

**You've got this! 🚀**
