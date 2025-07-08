

# 📘 What are Aggregate Functions?

Aggregate functions **perform a calculation on a set of rows** and return a **single value**. They are commonly used with the `GROUP BY` clause.

---

## ✅ List of Common Aggregate Functions in PostgreSQL

| Function  | Description                    |
| --------- | ------------------------------ |
| `COUNT()` | Counts the number of rows      |
| `SUM()`   | Adds up all values in a column |
| `AVG()`   | Calculates the average value   |
| `MIN()`   | Returns the smallest value     |
| `MAX()`   | Returns the largest value      |

---

## 🧪 Assume You Have This Table: `employees`

| emp\_id | fname  | dept      | salary |
| ------- | ------ | --------- | ------ |
| 1       | Raj    | IT        | 50000  |
| 2       | Priya  | HR        | 45000  |
| 3       | Arjun  | IT        | 55000  |
| 4       | Suman  | Finance   | 60000  |
| 5       | Kavita | HR        | 47000  |
| 6       | Amit   | Marketing | 52000  |

---

### 🔢 1. `COUNT()` – Count number of rows

```sql
SELECT COUNT(*) FROM employees;
```

📌 **Returns**: Total number of employees → `6`

---

### 💰 2. `SUM()` – Total of numeric column

```sql
SELECT SUM(salary) FROM employees;
```

📌 **Returns**: Total salary of all employees → `309000`

---

### 📊 3. `AVG()` – Average of numeric column

```sql
SELECT AVG(salary) FROM employees;
```

📌 **Returns**: Average salary → `51500.00`

---

### 🔼 4. `MIN()` – Minimum value

```sql
SELECT MIN(salary) FROM employees;
```

📌 **Returns**: Lowest salary → `45000`

---

### 🔽 5. `MAX()` – Maximum value

```sql
SELECT MAX(salary) FROM employees;
```

📌 **Returns**: Highest salary → `60000`

---

## 🧠 Using Aggregate Functions with `GROUP BY`

If you want **totals per department**, use `GROUP BY`:

```sql
SELECT dept, COUNT(*) AS total_employees
FROM employees
GROUP BY dept;
```

📌 **Returns**:

| dept      | total\_employees |
| --------- | ---------------- |
| IT        | 2                |
| HR        | 2                |
| Finance   | 1                |
| Marketing | 1                |

---

### 💡 Another example: Average salary per department

```sql
SELECT dept, AVG(salary) AS avg_salary
FROM employees
GROUP BY dept;
```

📌 **Returns**:

| dept      | avg\_salary |
| --------- | ----------- |
| IT        | 52500.00    |
| HR        | 46000.00    |
| Finance   | 60000.00    |
| Marketing | 52000.00    |

---

## 🚫 Using `WHERE` vs `HAVING`

* `WHERE` → filters **rows before aggregation**
* `HAVING` → filters **groups after aggregation**

```sql
-- Get departments with more than 1 employee
SELECT dept, COUNT(*) 
FROM employees
GROUP BY dept
HAVING COUNT(*) > 1;
```

---

## ✅ Summary

| Function  | Use for…                  |
| --------- | ------------------------- |
| `COUNT()` | Number of rows            |
| `SUM()`   | Total of numeric column   |
| `AVG()`   | Average of numeric column |
| `MIN()`   | Lowest value              |
| `MAX()`   | Highest value             |

---
---
---


## 🧠 Can We Use `WHERE` and `HAVING` with Aggregate Functions?

### ❌ You **CANNOT** use `WHERE` **with aggregate functions directly**.

But…

### ✅ You **CAN** use `HAVING` **with aggregate functions**.

---

## 🔍 Why?

### 🔹 `WHERE` filters **individual rows** **before** grouping or aggregation.

### 🔹 `HAVING` filters **groups** **after** the aggregation has been performed.

---

## ✅ Correct Usage Examples

### 🧱 1. Using `WHERE` (Before Aggregation)

```sql
-- Get average salary of employees only from IT department
SELECT AVG(salary) 
FROM employees 
WHERE dept = 'IT';
```

✔️ This is allowed because `WHERE` is filtering raw rows.

---

### 🧱 2. Using `HAVING` (After Aggregation)

```sql
-- Show departments where average salary > 50000
SELECT dept, AVG(salary) AS avg_salary
FROM employees
GROUP BY dept
HAVING AVG(salary) > 50000;
```

✔️ This is correct because `HAVING` can use aggregate functions.

---

### ❌ Wrong Usage: Aggregate function in `WHERE`

```sql
SELECT dept, AVG(salary)
FROM employees
WHERE AVG(salary) > 50000
GROUP BY dept;
```

🚫 ❌ **This will throw an error**, because `AVG(salary)` cannot be used inside `WHERE`.

---

## ✅ Summary Chart

| Clause   | Filters On        | Can Use Aggregates? |
| -------- | ----------------- | ------------------- |
| `WHERE`  | Individual rows   | ❌ No                |
| `HAVING` | Aggregated groups | ✅ Yes               |

---

## Bonus 🧪: Using Both `WHERE` and `HAVING`

```sql
-- Show average salary of IT and HR departments where average > 50000
SELECT dept, AVG(salary)
FROM employees
WHERE dept IN ('IT', 'HR')       -- Pre-filter raw rows
GROUP BY dept
HAVING AVG(salary) > 50000;      -- Post-filter aggregated result
```

---
---
---


# 🧠 Pro Tip: SQL Clause Order (Always Follow This)

```sql
SELECT        -- what you want to show
FROM          -- which table
WHERE         -- filter rows
GROUP BY      -- group rows
HAVING        -- filter groups
ORDER BY      -- sort result
LIMIT         -- restrict number of rows
```

