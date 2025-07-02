

# 🧠 Section 1: `employees` Table – Real Example Recap

Here’s the actual command you used to create the `employees` table:

```sql
CREATE TABLE employees (
  emp_id SERIAL PRIMARY KEY,
  fname VARCHAR(50) NOT NULL,
  lname VARCHAR(50) NOT NULL,
  email VARCHAR(150) UNIQUE NOT NULL,
  dept VARCHAR(50),
  salary DECIMAL(10, 2) DEFAULT 30000.00,
  hire_date DATE NOT NULL DEFAULT CURRENT_DATE
);
```

---

### 🔍 Let's Understand Each Column (Datatypes + Constraints):

| Column      | Data Type       | Constraints                     | Description                                     |
| ----------- | --------------- | ------------------------------- | ----------------------------------------------- |
| `emp_id`    | `SERIAL`        | `PRIMARY KEY`                   | Auto-incremented unique ID for each employee    |
| `fname`     | `VARCHAR(50)`   | `NOT NULL`                      | First name (must not be empty)                  |
| `lname`     | `VARCHAR(50)`   | `NOT NULL`                      | Last name (must not be empty)                   |
| `email`     | `VARCHAR(150)`  | `UNIQUE`, `NOT NULL`            | Email (must be unique and not null)             |
| `dept`      | `VARCHAR(50)`   | (nullable)                      | Department name                                 |
| `salary`    | `DECIMAL(10,2)` | `DEFAULT 30000.00`              | Salary with exact precision (default ₹30000.00) |
| `hire_date` | `DATE`          | `NOT NULL DEFAULT CURRENT_DATE` | Date of hiring (auto-filled with today’s date)  |

---

# 🔄 Section 2: CRUD Operations on the `employees` Table

### ✅ **CREATE** (Insert rows)

```sql
-- You inserted 10 employees
INSERT INTO employees (...) VALUES (...);
```

### 📖 **READ** (Retrieve data)

```sql
-- View all employees
SELECT * FROM employees;
```

### ✏️ **UPDATE** (Change data)

```sql
-- Example: Increase salary of emp_id = 1
UPDATE employees
SET salary = 55000.00
WHERE emp_id = 1;
```

### ❌ **DELETE** (Remove a row)

```sql
-- Example: Remove employee with emp_id = 10
DELETE FROM employees
WHERE emp_id = 10;
```

---

## 📊 `\d employees` Output Explained

After running `\d employees`, PostgreSQL shows this:

```plaintext
 Column   |          Type          | Nullable | Default                  
----------+------------------------+----------+---------------------------
 emp_id    | integer                | not null | nextval('employees_emp_id_seq')
 fname     | character varying(50)  | not null | 
 lname     | character varying(50)  | not null | 
 email     | character varying(150) | not null | 
 dept      | character varying(50)  |          | 
 salary    | numeric(10,2)          |          | 30000.00
 hire_date | date                   | not null | CURRENT_DATE
Indexes:
    "employees_pkey" PRIMARY KEY, btree (emp_id)
    "employees_email_key" UNIQUE CONSTRAINT, btree (email)
```

👉 **Takeaways**:

* `SERIAL` is implemented using a sequence: `nextval('employees_emp_id_seq')`
* `UNIQUE` creates its own index: `"employees_email_key"`
* `PRIMARY KEY` is enforced through: `"employees_pkey"`

---
