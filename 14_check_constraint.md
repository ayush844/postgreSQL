

# ✅ PostgreSQL `CHECK` Constraint – Full Explanation

---

## 🔍 What is a `CHECK` Constraint?

A `CHECK` constraint ensures that **a column or combination of columns** meets a specific condition **before a row is inserted or updated**.

If the condition is **not true**, PostgreSQL will **reject the operation**.

---

```sql



postgres=# ALTER table my_employess ADD COLUMN mob VARCHAR(15) DEFAULT '1234567890' CHECK(LENGTH(mob) >= 10);
ALTER TABLE
postgres=# \d my_employess
                                      Table "public.my_employess"
  Column   |          Type          | Collation | Nullable |                  Default                  
-----------+------------------------+-----------+----------+-------------------------------------------
 emp_id    | integer                |           | not null | nextval('employees_emp_id_seq'::regclass)
 fname     | character varying(50)  |           | not null | 
 lname     | character varying(50)  |           | not null | 
 email     | character varying(150) |           | not null | 
 dept      | character varying(50)  |           |          | 
 salary    | integer                |           |          | 35000
 hire_date | date                   |           | not null | CURRENT_DATE
 umar      | integer                |           |          | 0
 mob       | character varying(15)  |           |          | '1234567890'::character varying
Indexes:
    "employees_pkey" PRIMARY KEY, btree (emp_id)
    "employees_email_key" UNIQUE CONSTRAINT, btree (email)
Check constraints:
    "my_employess_mob_check" CHECK (length(mob::text) >= 10)

postgres=# ALTER table my_employess DROP CONSTRAINT my_employess_mob_check;
ALTER TABLE
postgres=# \d my_employess
                                      Table "public.my_employess"
  Column   |          Type          | Collation | Nullable |                  Default                  
-----------+------------------------+-----------+----------+-------------------------------------------
 emp_id    | integer                |           | not null | nextval('employees_emp_id_seq'::regclass)
 fname     | character varying(50)  |           | not null | 
 lname     | character varying(50)  |           | not null | 
 email     | character varying(150) |           | not null | 
 dept      | character varying(50)  |           |          | 
 salary    | integer                |           |          | 35000
 hire_date | date                   |           | not null | CURRENT_DATE
 umar      | integer                |           |          | 0
 mob       | character varying(15)  |           |          | '1234567890'::character varying
Indexes:
    "employees_pkey" PRIMARY KEY, btree (emp_id)
    "employees_email_key" UNIQUE CONSTRAINT, btree (email)

postgres=# ALTER table my_employess ADD CONSTRAINT my_employess_mob_check CHECK(LENGTH(mob) >= 10);
ALTER TABLE
postgres=# \d my_employess
                                      Table "public.my_employess"
  Column   |          Type          | Collation | Nullable |                  Default                  
-----------+------------------------+-----------+----------+-------------------------------------------
 emp_id    | integer                |           | not null | nextval('employees_emp_id_seq'::regclass)
 fname     | character varying(50)  |           | not null | 
 lname     | character varying(50)  |           | not null | 
 email     | character varying(150) |           | not null | 
 dept      | character varying(50)  |           |          | 
 salary    | integer                |           |          | 35000
 hire_date | date                   |           | not null | CURRENT_DATE
 umar      | integer                |           |          | 0
 mob       | character varying(15)  |           |          | '1234567890'::character varying
Indexes:
    "employees_pkey" PRIMARY KEY, btree (emp_id)
    "employees_email_key" UNIQUE CONSTRAINT, btree (email)
Check constraints:
    "my_employess_mob_check" CHECK (length(mob::text) >= 10)




postgres=# CREATE TABLE my_contacts(name VARCHAR(50), mob VARCHAR(15) UNIQUE, CONSTRAINT mob_no_less_than_10digits CHECK (LENGTH(mob) >= 10));
CREATE TABLE
postgres=# \d my_contacts
                   Table "public.my_contacts"
 Column |         Type          | Collation | Nullable | Default 
--------+-----------------------+-----------+----------+---------
 name   | character varying(50) |           |          | 
 mob    | character varying(15) |           |          | 
Indexes:
    "my_contacts_mob_key" UNIQUE CONSTRAINT, btree (mob)
Check constraints:
    "mob_no_less_than_10digits" CHECK (length(mob::text) >= 10)

postgres=# 

```


---

## 🧱 Basic Syntax

You can define a `CHECK`:

### ➤ While creating a table:

```sql
CREATE TABLE employees (
  emp_id SERIAL PRIMARY KEY,
  fname VARCHAR(50),
  salary NUMERIC CHECK (salary > 0)
);
```

### ➤ Or after table creation:

```sql
ALTER TABLE employees
ADD CONSTRAINT check_salary_positive CHECK (salary > 0);
```

---

## 🧠 How It Works:

* If someone tries to insert a `salary` of `-10000`, PostgreSQL will **raise an error**:

  ```
  ERROR:  new row for relation "employees" violates check constraint "check_salary_positive"
  ```

---

## 🧪 More Examples

---

### ✅ 1. Check Minimum Age

```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(50),
  age INTEGER CHECK (age >= 18)
);
```

🔐 This ensures no one under 18 can be added.

---

### ✅ 2. Check Department is only from a Fixed Set

```sql
CREATE TABLE employees (
  emp_id SERIAL,
  dept VARCHAR(20),
  CHECK (dept IN ('IT', 'HR', 'Finance', 'Marketing'))
);
```

🛑 An insert with `dept = 'Sales'` will be rejected.

---

### ✅ 3. Check Date Range (Joining Date must not be in future)

```sql
CREATE TABLE joining_info (
  emp_id INT,
  joining_date DATE CHECK (joining_date <= CURRENT_DATE)
);
```

---

### ✅ 4. Composite CHECK on Multiple Columns

```sql
CREATE TABLE bank_accounts (
  id SERIAL PRIMARY KEY,
  balance NUMERIC,
  account_status VARCHAR(10),
  CHECK (
    (account_status = 'active' AND balance >= 0) OR
    (account_status = 'closed')
  )
);
```

🧠 Logic here:

* If account is **active**, balance must be ≥ 0.
* If account is **closed**, balance can be anything.

---

## 🧑‍🔧 View Existing `CHECK` Constraints

Use the `\d tablename` command in `psql`, or:

```sql
SELECT conname, consrc
FROM pg_constraint
WHERE conrelid = 'employees'::regclass AND contype = 'c';
```

---

## 🧹 Drop a CHECK Constraint

To drop a check, you need the constraint name:

```sql
ALTER TABLE employees
DROP CONSTRAINT check_salary_positive;
```

Use `\d employees` to find constraint names.

---

## ⚠️ Important Notes

| Tip                           | Detail                                                             |
| ----------------------------- | ------------------------------------------------------------------ |
| Cannot Reference Other Tables | CHECK only validates **within the row** being inserted or updated. |
| Custom Logic                  | Perfect for enforcing business rules at the database level.        |
| Use Descriptive Names         | Helpful for debugging when a CHECK fails.                          |

---

## ✅ Summary

| Use Case           | Example                                     |
| ------------------ | ------------------------------------------- |
| Salary must be > 0 | `CHECK (salary > 0)`                        |
| Age at least 18    | `CHECK (age >= 18)`                         |
| Dept in fixed set  | `CHECK (dept IN ('IT','HR'))`               |
| Date not in future | `CHECK (date <= CURRENT_DATE)`              |
| Multi-column logic | `CHECK (status = 'active' AND balance > 0)` |

---

