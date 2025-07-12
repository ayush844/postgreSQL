
# 🧱 PostgreSQL `ALTER` Command

---

## 📘 What is `ALTER`?

The `ALTER TABLE` statement is used to **modify the structure** of an existing table — **without deleting any data**.

You can use it to:

| Purpose                   | Example                             |
| ------------------------- | ----------------------------------- |
| Add a column              | `ADD COLUMN`                        |
| Remove a column           | `DROP COLUMN`                       |
| Rename a column           | `RENAME COLUMN`                     |
| Change column data type   | `ALTER COLUMN TYPE`                 |
| Set or drop default value | `SET DEFAULT`, `DROP DEFAULT`       |
| Rename table              | `RENAME TO`                         |
| Add or drop a constraint  | `ADD CONSTRAINT`, `DROP CONSTRAINT` |

---

## 🧪 1. Add a New Column

```sql
ALTER TABLE employees 
ADD COLUMN gender VARCHAR(10);
```

✅ Adds a new column `gender` to the `employees` table.

---

## 🧪 2. Drop (Delete) a Column

```sql
ALTER TABLE employees 
DROP COLUMN gender;
```

⚠️ Removes the `gender` column and **its data permanently**.

---

## 🧪 3. Rename a Column

```sql
ALTER TABLE employees 
RENAME COLUMN fname TO first_name;
```

✅ Changes the column name from `fname` to `first_name`.

---

## 🧪 4. Change Data Type of a Column

```sql
ALTER TABLE employees 
ALTER COLUMN salary TYPE INTEGER;
```

⚠️ Only works if **existing data can be safely cast** to the new type.

---

## 🧪 5. Set a Default Value for a Column

```sql
ALTER TABLE employees 
ALTER COLUMN dept SET DEFAULT 'General';
```

📌 All new rows inserted **without a value for `dept`** will now get `'General'`.

---

## 🧪 6. Drop Default Value

```sql
ALTER TABLE employees 
ALTER COLUMN dept DROP DEFAULT;
```

---

## 🧪 7. Rename a Table

```sql
ALTER TABLE employees 
RENAME TO employee_data;
```

✅ Renames `employees` table to `employee_data`.

---

## 🧪 8. Add a Constraint (e.g., UNIQUE)

```sql
ALTER TABLE employees 
ADD CONSTRAINT unique_email UNIQUE (email);
```

🔒 Ensures that no two employees have the same email.

---

## 🧪 9. Drop a Constraint

To drop a constraint, you need its name (use `\d employees` to check).

```sql
ALTER TABLE employees 
DROP CONSTRAINT unique_email;
```

---

## 🔍 Summary of Common `ALTER` Operations

| Operation          | Syntax Example                                          |
| ------------------ | ------------------------------------------------------- |
| Add column         | `ALTER TABLE table ADD COLUMN col datatype;`            |
| Drop column        | `ALTER TABLE table DROP COLUMN col;`                    |
| Rename column      | `ALTER TABLE table RENAME COLUMN old TO new;`           |
| Change column type | `ALTER TABLE table ALTER COLUMN col TYPE new_type;`     |
| Set default        | `ALTER TABLE table ALTER COLUMN col SET DEFAULT value;` |
| Drop default       | `ALTER TABLE table ALTER COLUMN col DROP DEFAULT;`      |
| Rename table       | `ALTER TABLE oldname RENAME TO newname;`                |
| Add constraint     | `ALTER TABLE table ADD CONSTRAINT name type (column);`  |
| Drop constraint    | `ALTER TABLE table DROP CONSTRAINT constraint_name;`    |

---

## 🚨 Tips to Avoid Errors

* Always **back up your table** before major schema changes.
* Use `\d table_name` in psql to view structure & constraint names.
* Don’t change data types if incompatible values already exist (e.g., `VARCHAR` to `INT`).

---



```sql

(base) ayush@lucas:~$ sudo -iu postgres
[sudo] password for ayush: 
postgres@lucas:~$ psql
psql (17.5 (Ubuntu 17.5-1.pgdg22.04+1), server 16.9 (Ubuntu 16.9-1.pgdg22.04+1))
Type "help" for help.

postgres=# select * from employees;
 emp_id | fname  | lname  |          email           |   dept    |  salary  | hire_date  
--------+--------+--------+--------------------------+-----------+----------+------------
      1 | Raj    | Sharma | raj.sharma@example.com   | IT        | 50000.00 | 2020-01-15
      2 | Priya  | Singh  | priya.singh@example.com  | HR        | 45000.00 | 2019-03-22
      3 | Arjun  | Verma  | arjun.verma@example.com  | IT        | 55000.00 | 2021-06-01
      4 | Suman  | Patel  | suman.patel@example.com  | Finance   | 60000.00 | 2018-07-30
      5 | Kavita | Rao    | kavita.rao@example.com   | HR        | 47000.00 | 2020-11-10
      6 | Amit   | Gupta  | amit.gupta@example.com   | Marketing | 52000.00 | 2020-09-25
      7 | Neha   | Desai  | neha.desai@example.com   | IT        | 48000.00 | 2019-05-18
      8 | Rahul  | Kumar  | rahul.kumar@example.com  | IT        | 53000.00 | 2021-02-14
      9 | Anjali | Mehta  | anjali.mehta@example.com | Finance   | 61000.00 | 2018-12-03
     10 | Vijay  | Nair   | vijay.nair@example.com   | Marketing | 50000.00 | 2020-04-19
(10 rows)

postgres=# ALTER table employees ADD COLUMN gender VARCHAR(10);
ALTER TABLE
postgres=# select * from employees;
 emp_id | fname  | lname  |          email           |   dept    |  salary  | hire_date  | gender 
--------+--------+--------+--------------------------+-----------+----------+------------+--------
      1 | Raj    | Sharma | raj.sharma@example.com   | IT        | 50000.00 | 2020-01-15 | 
      2 | Priya  | Singh  | priya.singh@example.com  | HR        | 45000.00 | 2019-03-22 | 
      3 | Arjun  | Verma  | arjun.verma@example.com  | IT        | 55000.00 | 2021-06-01 | 
      4 | Suman  | Patel  | suman.patel@example.com  | Finance   | 60000.00 | 2018-07-30 | 
      5 | Kavita | Rao    | kavita.rao@example.com   | HR        | 47000.00 | 2020-11-10 | 
      6 | Amit   | Gupta  | amit.gupta@example.com   | Marketing | 52000.00 | 2020-09-25 | 
      7 | Neha   | Desai  | neha.desai@example.com   | IT        | 48000.00 | 2019-05-18 | 
      8 | Rahul  | Kumar  | rahul.kumar@example.com  | IT        | 53000.00 | 2021-02-14 | 
      9 | Anjali | Mehta  | anjali.mehta@example.com | Finance   | 61000.00 | 2018-12-03 | 
     10 | Vijay  | Nair   | vijay.nair@example.com   | Marketing | 50000.00 | 2020-04-19 | 
(10 rows)

postgres=# ALTER table employees DROP COLUMN gender;
ALTER TABLE
postgres=# select * from employees;
 emp_id | fname  | lname  |          email           |   dept    |  salary  | hire_date  
--------+--------+--------+--------------------------+-----------+----------+------------
      1 | Raj    | Sharma | raj.sharma@example.com   | IT        | 50000.00 | 2020-01-15
      2 | Priya  | Singh  | priya.singh@example.com  | HR        | 45000.00 | 2019-03-22
      3 | Arjun  | Verma  | arjun.verma@example.com  | IT        | 55000.00 | 2021-06-01
      4 | Suman  | Patel  | suman.patel@example.com  | Finance   | 60000.00 | 2018-07-30
      5 | Kavita | Rao    | kavita.rao@example.com   | HR        | 47000.00 | 2020-11-10
      6 | Amit   | Gupta  | amit.gupta@example.com   | Marketing | 52000.00 | 2020-09-25
      7 | Neha   | Desai  | neha.desai@example.com   | IT        | 48000.00 | 2019-05-18
      8 | Rahul  | Kumar  | rahul.kumar@example.com  | IT        | 53000.00 | 2021-02-14
      9 | Anjali | Mehta  | anjali.mehta@example.com | Finance   | 61000.00 | 2018-12-03
     10 | Vijay  | Nair   | vijay.nair@example.com   | Marketing | 50000.00 | 2020-04-19
(10 rows)

postgres=# ALTER table employees ADD COLUMN age INT DEFAULT 0;
ALTER TABLE
postgres=# select * from employees;
 emp_id | fname  | lname  |          email           |   dept    |  salary  | hire_date  | age 
--------+--------+--------+--------------------------+-----------+----------+------------+-----
      1 | Raj    | Sharma | raj.sharma@example.com   | IT        | 50000.00 | 2020-01-15 |   0
      2 | Priya  | Singh  | priya.singh@example.com  | HR        | 45000.00 | 2019-03-22 |   0
      3 | Arjun  | Verma  | arjun.verma@example.com  | IT        | 55000.00 | 2021-06-01 |   0
      4 | Suman  | Patel  | suman.patel@example.com  | Finance   | 60000.00 | 2018-07-30 |   0
      5 | Kavita | Rao    | kavita.rao@example.com   | HR        | 47000.00 | 2020-11-10 |   0
      6 | Amit   | Gupta  | amit.gupta@example.com   | Marketing | 52000.00 | 2020-09-25 |   0
      7 | Neha   | Desai  | neha.desai@example.com   | IT        | 48000.00 | 2019-05-18 |   0
      8 | Rahul  | Kumar  | rahul.kumar@example.com  | IT        | 53000.00 | 2021-02-14 |   0
      9 | Anjali | Mehta  | anjali.mehta@example.com | Finance   | 61000.00 | 2018-12-03 |   0
     10 | Vijay  | Nair   | vijay.nair@example.com   | Marketing | 50000.00 | 2020-04-19 |   0
(10 rows)

postgres=# ALTER table employees RENAME COLUMN age TO umar;
ALTER TABLE
postgres=# select * from employees;
 emp_id | fname  | lname  |          email           |   dept    |  salary  | hire_date  | umar 
--------+--------+--------+--------------------------+-----------+----------+------------+------
      1 | Raj    | Sharma | raj.sharma@example.com   | IT        | 50000.00 | 2020-01-15 |    0
      2 | Priya  | Singh  | priya.singh@example.com  | HR        | 45000.00 | 2019-03-22 |    0
      3 | Arjun  | Verma  | arjun.verma@example.com  | IT        | 55000.00 | 2021-06-01 |    0
      4 | Suman  | Patel  | suman.patel@example.com  | Finance   | 60000.00 | 2018-07-30 |    0
      5 | Kavita | Rao    | kavita.rao@example.com   | HR        | 47000.00 | 2020-11-10 |    0
      6 | Amit   | Gupta  | amit.gupta@example.com   | Marketing | 52000.00 | 2020-09-25 |    0
      7 | Neha   | Desai  | neha.desai@example.com   | IT        | 48000.00 | 2019-05-18 |    0
      8 | Rahul  | Kumar  | rahul.kumar@example.com  | IT        | 53000.00 | 2021-02-14 |    0
      9 | Anjali | Mehta  | anjali.mehta@example.com | Finance   | 61000.00 | 2018-12-03 |    0
     10 | Vijay  | Nair   | vijay.nair@example.com   | Marketing | 50000.00 | 2020-04-19 |    0
(10 rows)

postgres=# ALTER table employees RENAME TO my_employess;
ALTER TABLE
postgres=# \l
                                                       List of databases
          Name           |  Owner   | Encoding | Locale Provider | Collate | Ctype | Locale | ICU Rules |   Access privileges   
-------------------------+----------+----------+-----------------+---------+-------+--------+-----------+-----------------------
 TodoApplicationDatabase | postgres | UTF8     | libc            | en_IN   | en_IN |        |           | 
 bankdb                  | postgres | UTF8     | libc            | en_IN   | en_IN |        |           | 
 instaDB                 | postgres | UTF8     | libc            | en_IN   | en_IN |        |           | 
 my_pgdb                 | postgres | UTF8     | libc            | en_IN   | en_IN |        |           | 
 mynewdb                 | postgres | UTF8     | libc            | en_IN   | en_IN |        |           | 
 persondb                | postgres | UTF8     | libc            | en_IN   | en_IN |        |           | 
 postgres                | postgres | UTF8     | libc            | en_IN   | en_IN |        |           | 
 template0               | postgres | UTF8     | libc            | en_IN   | en_IN |        |           | =c/postgres          +
                         |          |          |                 |         |       |        |           | postgres=CTc/postgres
 template1               | postgres | UTF8     | libc            | en_IN   | en_IN |        |           | =c/postgres          +
                         |          |          |                 |         |       |        |           | postgres=CTc/postgres
(9 rows)

postgres=# \dt
            List of relations
 Schema |     Name     | Type  |  Owner   
--------+--------------+-------+----------
 public | my_employess | table | postgres
(1 row)

postgres=# \d my _employess
Did not find any relation named "my".
postgres=# \d my_employess
                                      Table "public.my_employess"
  Column   |          Type          | Collation | Nullable |                  Default                  
-----------+------------------------+-----------+----------+-------------------------------------------
 emp_id    | integer                |           | not null | nextval('employees_emp_id_seq'::regclass)
 fname     | character varying(50)  |           | not null | 
 lname     | character varying(50)  |           | not null | 
 email     | character varying(150) |           | not null | 
 dept      | character varying(50)  |           |          | 
 salary    | numeric(10,2)          |           |          | 30000.00
 hire_date | date                   |           | not null | CURRENT_DATE
 umar      | integer                |           |          | 0
Indexes:
    "employees_pkey" PRIMARY KEY, btree (emp_id)
    "employees_email_key" UNIQUE CONSTRAINT, btree (email)

postgres=# ALTER table my_employess ALTER COLUMN salary TYPE INTEGER;
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
 salary    | integer                |           |          | 30000.00
 hire_date | date                   |           | not null | CURRENT_DATE
 umar      | integer                |           |          | 0
Indexes:
    "employees_pkey" PRIMARY KEY, btree (emp_id)
    "employees_email_key" UNIQUE CONSTRAINT, btree (email)

postgres=# ALTER table my_employess ALTER COLUMN salary set default 35000;
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
Indexes:
    "employees_pkey" PRIMARY KEY, btree (emp_id)
    "employees_email_key" UNIQUE CONSTRAINT, btree (email)

postgres=# 


```