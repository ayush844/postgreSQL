

## 🧠 What is `SERIAL`?

In PostgreSQL:

```sql
emp_id SERIAL PRIMARY KEY
```

Is a shortcut for:

```sql
emp_id INTEGER NOT NULL DEFAULT nextval('employees_emp_id_seq')
```

This means:

* PostgreSQL creates a **sequence** called `employees_emp_id_seq`
* Every time you insert a row **without specifying `emp_id`**, it automatically assigns the next number from this sequence.

---

## 💥 Problem: Manually Inserting ID Breaks the Sequence

### ⚠️ Example:

```sql
-- Manually inserted ID = 1
INSERT INTO employees (emp_id, fname, ...) VALUES (1, 'Raj', ...);
```

This works fine, **but PostgreSQL's sequence doesn't know** that `1` was just used.

Now if you do:

```sql
-- Let PostgreSQL assign emp_id
INSERT INTO employees (fname, ...) VALUES ('Ayush', ...);
```

You may get:

```
ERROR: duplicate key value violates unique constraint "employees_pkey"
DETAIL: Key (emp_id)=(1) already exists.
```

That's because `employees_emp_id_seq` still thinks the **current value is 0**, so it tries to assign `1`.

---

## ✅ Fixing the Sequence Using `setval()`

### 🔍 Step 1: Check current sequence value

```sql
SELECT currval('employees_emp_id_seq');
-- Will throw error if nextval() hasn't been called yet in session
```

### 🔍 Step 2: Set the correct sequence value

If you've already inserted rows with manual IDs, fix the sequence like this:

```sql
-- Set the sequence to the maximum ID already used
SELECT setval('employees_emp_id_seq', 1);
```

> 🔁 Now when you insert without providing `emp_id`, PostgreSQL will assign **2**, then **3**, etc.

You can also auto-detect and set it safely like this:

```sql
-- Automatically adjust to the max existing ID
SELECT setval('employees_emp_id_seq', (SELECT MAX(emp_id) FROM employees));
```

---

## ✅ Best Practice

If you're using `SERIAL`, **never manually insert `emp_id`** unless you also update the sequence accordingly.

---

## 🧪 Example Summary

### 🏗️ Table

```sql
CREATE TABLE employees (
  emp_id SERIAL PRIMARY KEY,
  fname VARCHAR(50)
);
```

### 🧍 Manually inserted row

```sql
INSERT INTO employees (emp_id, fname) VALUES (1, 'Raj');
```

### 🚨 Error on next auto insert

```sql
INSERT INTO employees (fname) VALUES ('Ayush');
-- ERROR: duplicate key value violates unique constraint
```

### 🛠 Fix

```sql
SELECT setval('employees_emp_id_seq', 1);
-- Now next INSERT will use emp_id = 2
```

---
