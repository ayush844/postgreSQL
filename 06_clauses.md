


# 📘 What Are Clauses in SQL (PostgreSQL)?

In SQL, a **clause** is a **component** of a SQL statement. Think of clauses as the **building blocks** of a query. Each clause performs a specific function — like filtering rows, sorting data, or limiting results.

A typical SQL query is made up of several clauses in this order:

```sql
SELECT column_list
FROM table_name
WHERE condition
ORDER BY column
LIMIT number;
```

---

## 🔍 Common PostgreSQL Clauses Explained

### 1. ✅ `WHERE` Clause

**Used to filter rows based on conditions.**

🔹 **Syntax:**

```sql
SELECT * FROM table_name
WHERE condition;
```

🔹 **Example:**

```sql
SELECT * FROM employees
WHERE dept = 'IT';
```

📝 Returns only the rows where `dept` is `'IT'`.

---

### 2. ✅ `DISTINCT` Clause

**Used to remove duplicate rows from the result.**

🔹 **Syntax:**

```sql
SELECT DISTINCT column_name FROM table_name;
```

🔹 **Example:**

```sql
SELECT DISTINCT dept FROM employees;
```

📝 Returns unique department names.

---

### 3. ✅ `ORDER BY` Clause

**Used to sort the result in ascending (`ASC`) or descending (`DESC`) order.**

🔹 **Syntax:**

```sql
SELECT * FROM table_name
ORDER BY column_name ASC|DESC;
```

🔹 **Example:**

```sql
SELECT * FROM employees
ORDER BY salary DESC;
```

📝 Sorts employees by highest to lowest salary.

---

### 4. ✅ `LIMIT` Clause

**Used to restrict the number of rows returned.**

🔹 **Syntax:**

```sql
SELECT * FROM table_name
LIMIT number;
```

🔹 **Example:**

```sql
SELECT * FROM employees
LIMIT 5;
```

📝 Returns only the first 5 rows.

---

### 5. ✅ `LIKE` Clause

**Used for pattern matching with wildcards.**

* `%` matches **zero or more characters**
* `_` matches **exactly one character**

🔹 **Syntax:**

```sql
SELECT * FROM table_name
WHERE column_name LIKE 'pattern';
```

🔹 **Examples:**

```sql
SELECT * FROM employees
WHERE fname LIKE 'A%';    -- Names starting with 'A'

SELECT * FROM employees
WHERE email LIKE '%@example.com';  -- Emails ending with '@example.com'

SELECT * FROM employees
WHERE fname LIKE '_yush'; -- Names where second letter is 'y'
```

---

## 🔁 Combine Multiple Clauses

Clauses can be combined to form powerful queries:

```sql
SELECT DISTINCT fname, salary
FROM employees
WHERE dept = 'IT'
ORDER BY salary DESC
LIMIT 3;uestions
* Markdown version for your notes

```

📝 Meaning:

* Get unique first names and salaries
* Only for `IT` department
* Sorted by salary (high to low)
* Limit to top 3 records

---
