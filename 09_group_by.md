
# 📘 What is `GROUP BY` in PostgreSQL?

The `GROUP BY` clause is used to **group rows that have the same values in one or more columns** into summary rows.

It is **mostly used with aggregate functions** like:

* `COUNT()`
* `SUM()`
* `AVG()`
* `MIN()`
* `MAX()`

---

### 🧠 Think of it like this:

If you want answers like:

* How many employees in each department?
* What’s the average salary per department?
* Total sales per city?

You need **`GROUP BY`**!

---

## 🏗️ Syntax of `GROUP BY`

```sql
SELECT column1, AGGREGATE_FUNCTION(column2)
FROM table_name
GROUP BY column1;
```

---

## 🧪 Example Using Your `employees` Table

| emp\_id | fname  | dept      | salary |
| ------- | ------ | --------- | ------ |
| 1       | Raj    | IT        | 50000  |
| 2       | Priya  | HR        | 45000  |
| 3       | Arjun  | IT        | 55000  |
| 4       | Suman  | Finance   | 60000  |
| 5       | Kavita | HR        | 47000  |
| 6       | Amit   | Marketing | 52000  |

---

### ✅ 1. Count of Employees in Each Department

```sql
SELECT dept, COUNT(*) AS total_employees
FROM employees
GROUP BY dept;
```

📌 **Explanation**:

* Groups employees by department.
* Then counts how many are in each department.

🧾 **Output:**

| dept      | total\_employees |
| --------- | ---------------- |
| IT        | 2                |
| HR        | 2                |
| Finance   | 1                |
| Marketing | 1                |

---

### ✅ 2. Average Salary per Department

```sql
SELECT dept, AVG(salary) AS avg_salary
FROM employees
GROUP BY dept;
```

🧾 **Output:**

| dept      | avg\_salary |
| --------- | ----------- |
| IT        | 52500.00    |
| HR        | 46000.00    |
| Finance   | 60000.00    |
| Marketing | 52000.00    |

---

### ✅ 3. Total Salary per Department

```sql
SELECT dept, SUM(salary) AS total_salary
FROM employees
GROUP BY dept;
```

---

## 🔍 `GROUP BY` with `HAVING`

You can use `HAVING` to filter grouped results:

```sql
SELECT dept, AVG(salary) AS avg_salary
FROM employees
GROUP BY dept
HAVING AVG(salary) > 50000;
```

🧾 **Output:** Only departments with avg salary above ₹50,000.

---

## ⚠️ Rules to Remember

| Rule | Description                                                                                     |
| ---- | ----------------------------------------------------------------------------------------------- |
| ✅    | Every column in `SELECT` that is **not inside an aggregate function must appear in `GROUP BY`** |
| ❌    | You cannot use `WHERE` with aggregate functions (use `HAVING` instead)                          |
| ✅    | `GROUP BY` must come **before `HAVING`**                                                        |

---

## 🎯 Summary

| Clause     | Role                             |
| ---------- | -------------------------------- |
| `GROUP BY` | Groups rows with same values     |
| `COUNT()`  | Total rows in each group         |
| `SUM()`    | Total of numeric column          |
| `AVG()`    | Average of numeric column        |
| `HAVING`   | Filters groups after aggregation |

---
