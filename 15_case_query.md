
# 🎯 PostgreSQL `CASE` Expression – Full Explanation

---

## 🔍 What is `CASE`?

The `CASE` expression allows you to **apply if-else logic** inside a SQL statement.

It's like writing:

> **IF condition THEN result ELSE something else**

---

## 📘 Syntax

```sql
CASE 
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ...
    ELSE default_result
END
```

You can use it:

* Inside a `SELECT`
* With `UPDATE`
* In `ORDER BY`, `WHERE`, etc.

---

## 🧪 Examples

### ✅ Example 1: Label salary as "Low", "Mid", or "High"

```sql
SELECT fname, salary,
  CASE 
    WHEN salary < 50000 THEN 'Low'
    WHEN salary BETWEEN 50000 AND 55000 THEN 'Mid'
    ELSE 'High'
  END AS salary_category
FROM employees;
```

🟢 Output:

| fname | salary   | salary\_category |
| ----- | -------- | ---------------- |
| Raj   | 50000.00 | Mid              |
| Arjun | 55000.00 | Mid              |
| Suman | 60000.00 | High             |
| Priya | 45000.00 | Low              |

---

### ✅ Example 2: Replace department names

```sql
SELECT fname, dept,
  CASE dept
    WHEN 'HR' THEN 'Human Resources'
    WHEN 'IT' THEN 'Tech'
    ELSE dept
  END AS dept_full
FROM employees;
```

---

### ✅ Example 3: Use `CASE` in `ORDER BY`

```sql
SELECT * FROM employees
ORDER BY
  CASE 
    WHEN dept = 'IT' THEN 1
    WHEN dept = 'HR' THEN 2
    ELSE 3
  END;
```

💡 This orders rows: IT → HR → Others

---

### ✅ Example 4: Use `CASE` in an `UPDATE`

```sql
UPDATE employees
SET salary = 
  CASE 
    WHEN dept = 'HR' THEN salary + 1000
    WHEN dept = 'IT' THEN salary + 2000
    ELSE salary
  END;
```

🔧 Gives salary raise based on department.

---

### ✅ Example 5: Combine with Aggregate Functions

```sql
SELECT 
  dept,
  COUNT(*) AS total_employees,
  SUM(CASE WHEN salary > 50000 THEN 1 ELSE 0 END) AS high_paid_count
FROM employees
GROUP BY dept;
```

👆 This counts how many employees have salary > 50000 per department.

---

## ✅ Summary

| Use Case                | Example                                    |
| ----------------------- | ------------------------------------------ |
| Label data              | `CASE WHEN salary > 50000 THEN 'High' END` |
| Conditional aggregation | `SUM(CASE WHEN ... THEN 1 ELSE 0 END)`     |
| Conditional sorting     | `ORDER BY CASE WHEN ... THEN 1 ELSE 2 END` |
| Modify during update    | `SET salary = CASE WHEN ... THEN ... END`  |

---

## ⚠️ Tips

* Always include an `ELSE` clause to handle **unexpected values**.
* You can **nest** `CASE` expressions inside one another if needed.
* Prefer `CASE` over complex logic in application code for better performance and maintainability.

---


```sql

(base) ayush@lucas:~$ sudo -iu postgres
postgres@lucas:~$ psql
psql (17.5 (Ubuntu 17.5-1.pgdg22.04+1), server 16.9 (Ubuntu 16.9-1.pgdg22.04+1))
Type "help" for help.

postgres=# select * from my_employess
postgres-# ;
 emp_id | fname  | lname  |          email           |   dept    | salary | hire_date  | umar |    mob     
--------+--------+--------+--------------------------+-----------+--------+------------+------+------------
      1 | Raj    | Sharma | raj.sharma@example.com   | IT        |  50000 | 2020-01-15 |    0 | 1234567890
      2 | Priya  | Singh  | priya.singh@example.com  | HR        |  45000 | 2019-03-22 |    0 | 1234567890
      3 | Arjun  | Verma  | arjun.verma@example.com  | IT        |  55000 | 2021-06-01 |    0 | 1234567890
      4 | Suman  | Patel  | suman.patel@example.com  | Finance   |  60000 | 2018-07-30 |    0 | 1234567890
      5 | Kavita | Rao    | kavita.rao@example.com   | HR        |  47000 | 2020-11-10 |    0 | 1234567890
      6 | Amit   | Gupta  | amit.gupta@example.com   | Marketing |  52000 | 2020-09-25 |    0 | 1234567890
      7 | Neha   | Desai  | neha.desai@example.com   | IT        |  48000 | 2019-05-18 |    0 | 1234567890
      8 | Rahul  | Kumar  | rahul.kumar@example.com  | IT        |  53000 | 2021-02-14 |    0 | 1234567890
      9 | Anjali | Mehta  | anjali.mehta@example.com | Finance   |  61000 | 2018-12-03 |    0 | 1234567890
     10 | Vijay  | Nair   | vijay.nair@example.com   | Marketing |  50000 | 2020-04-19 |    0 | 1234567890
(10 rows)

postgres=# select fname, salary, CASE WHEN salary >= 50000 THEN 'HIGH' ELSE 'LOW' END AS sal_category FROM my_employess;
 fname  | salary | sal_category 
--------+--------+--------------
 Raj    |  50000 | HIGH
 Priya  |  45000 | LOW
 Arjun  |  55000 | HIGH
 Suman  |  60000 | HIGH
 Kavita |  47000 | LOW
 Amit   |  52000 | HIGH
 Neha   |  48000 | LOW
 Rahul  |  53000 | HIGH
 Anjali |  61000 | HIGH
 Vijay  |  50000 | HIGH
(10 rows)

postgres=# select fname, salary, (salary/10) AS bonus FROM my_employess;
 fname  | salary | bonus 
--------+--------+-------
 Raj    |  50000 |  5000
 Priya  |  45000 |  4500
 Arjun  |  55000 |  5500
 Suman  |  60000 |  6000
 Kavita |  47000 |  4700
 Amit   |  52000 |  5200
 Neha   |  48000 |  4800
 Rahul  |  53000 |  5300
 Anjali |  61000 |  6100
 Vijay  |  50000 |  5000
(10 rows)

postgres=# select CASE WHEN salary >= 50000 THEN 'HIGH' ELSE 'LOW' END AS sal_category, COUNT(emp_id) FROM my_employess GROUP BY sal_category;
 sal_category | count 
--------------+-------
 LOW          |     3
 HIGH         |     7
(2 rows)

postgres=# 


```