# subqueries

In PostgreSQL, **subqueries** (also called **inner queries** or **nested queries**) are queries nested inside another SQL query. They can be used in `SELECT`, `FROM`, or `WHERE` clauses and are powerful for breaking complex logic into modular parts.

---

### 🔹 Types of Subqueries in PostgreSQL:

1. **Scalar Subquery** – returns a single value.
2. **Row Subquery** – returns a single row.
3. **Column Subquery** – returns a single column.
4. **Table Subquery** – returns a complete result set.
5. **Correlated Subquery** – depends on the outer query.
6. **Subquery in FROM clause** – treated like a derived table.

---

### 🔸 Example Setup: `employees`, `departments`

```sql
CREATE TABLE departments (
    dept_id SERIAL PRIMARY KEY,
    dept_name VARCHAR(50)
);

CREATE TABLE employees (
    emp_id SERIAL PRIMARY KEY,
    name VARCHAR(50),
    salary NUMERIC,
    dept_id INT REFERENCES departments(dept_id)
);
```

---

## ✅ Basic Example

### 1. Get employees with salary above the average salary:

```sql
SELECT name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

📌 *This is a **scalar subquery** in the `WHERE` clause.*

---

## ✅ Advanced Subquery Examples

---

### 2. **Correlated Subquery** – Employees earning more than the average in their own department

```sql
SELECT name, salary, dept_id
FROM employees e
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
    WHERE dept_id = e.dept_id
);
```

📌 *The subquery uses a reference (`e.dept_id`) from the outer query.*

---

### 3. **Subquery in FROM Clause** – Department-wise average salary

```sql
SELECT d.dept_id, d.dept_name, avg_salaries.avg_salary
FROM departments d
JOIN (
    SELECT dept_id, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY dept_id
) AS avg_salaries ON d.dept_id = avg_salaries.dept_id;
```

📌 *The subquery returns a derived table that can be joined.*

---

### 4. **IN with Subquery** – Employees in departments with more than 5 employees

```sql
SELECT name
FROM employees
WHERE dept_id IN (
    SELECT dept_id
    FROM employees
    GROUP BY dept_id
    HAVING COUNT(*) > 5
);
```

📌 *The subquery filters departments, and `IN` matches employees from those.*

---

### 5. **EXISTS with Subquery** – Departments with at least one high-earning employee

```sql
SELECT dept_name
FROM departments d
WHERE EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.dept_id = d.dept_id AND e.salary > 80000
);
```

📌 *`EXISTS` returns true if subquery returns any rows.*

---

### 6. **Using Subqueries for Updates** – Increase salary of lowest paid employee in each department by 10%

```sql
UPDATE employees
SET salary = salary * 1.10
WHERE (dept_id, salary) IN (
    SELECT dept_id, MIN(salary)
    FROM employees
    GROUP BY dept_id
);
```

📌 *This uses a composite subquery for multi-column matching.*

---

### 7. **Subquery Returning Multiple Columns (Row Subquery)** – Find employees with exact same (salary, dept) as a given employee

```sql
SELECT *
FROM employees
WHERE (salary, dept_id) = (
    SELECT salary, dept_id
    FROM employees
    WHERE name = 'Raj'
    LIMIT 1
);
```

---

### 🧠 Tips

* Always alias subqueries in the `FROM` clause.
* Avoid correlated subqueries for large datasets (can be slow); consider `JOIN` if possible.
* PostgreSQL supports **CTEs (Common Table Expressions)** using `WITH` – sometimes better than subqueries.

---
