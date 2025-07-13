
---

# 🔗 JOINS in PostgreSQL – Full Detailed Guide


## 🔍 What is a JOIN?

A **JOIN** is used to **combine rows** from two or more tables based on a **related column**, typically a **foreign key** relationship.

> Think of JOINs as stitching tables together to form a complete view.

---

## ✅ Common Use Case

You have:

* A `departments` table (with dept info)
* An `employees` table (each with a dept\_id)

You want to view employee names **with their department names**. That’s where `JOIN` comes in.

---

## 📦 Types of JOINS

| Join Type    | Description                                                            |
| ------------ | ---------------------------------------------------------------------- |
| `INNER JOIN` | Returns rows that have **matching values in both tables**              |
| `LEFT JOIN`  | Returns **all rows from the left** table + matched rows from the right |
| `RIGHT JOIN` | Returns **all rows from the right** table + matched rows from the left |
| `FULL JOIN`  | Returns **all rows from both** tables, matched where possible          |
| `CROSS JOIN` | Returns the **Cartesian product** (all combinations of rows)           |
| `SELF JOIN`  | Joins a table to **itself** (used in hierarchical data)                |

---

## 🧱 Sample Tables

### `departments`

| dept\_id | dept\_name |
| -------- | ---------- |
| 1        | IT         |
| 2        | HR         |
| 3        | Marketing  |

### `employees`

| emp\_id | name  | dept\_id |
| ------- | ----- | -------- |
| 101     | Raj   | 1        |
| 102     | Suman | 2        |
| 103     | Neha  | 3        |
| 104     | Riya  | NULL     |

---

## 🔹 1. INNER JOIN

> Returns rows **only when both tables have matching values**

```sql
SELECT e.name, d.dept_name
FROM employees e
INNER JOIN departments d
ON e.dept_id = d.dept_id;
```

✔️ Output:

* Raj → IT
* Suman → HR
* Neha → Marketing

(Excludes Riya because her dept\_id is NULL)

---

## 🔹 2. LEFT JOIN

> Returns **all employees**, and department info if available

```sql
SELECT e.name, d.dept_name
FROM employees e
LEFT JOIN departments d
ON e.dept_id = d.dept_id;
```

✔️ Output:

* Raj → IT
* Suman → HR
* Neha → Marketing
* Riya → NULL (no department)

---

## 🔹 3. RIGHT JOIN

> Returns **all departments**, with employee info if available

```sql
SELECT e.name, d.dept_name
FROM employees e
RIGHT JOIN departments d
ON e.dept_id = d.dept_id;
```

✔️ Output:

* Raj → IT
* Suman → HR
* Neha → Marketing
  (If there’s a department with no employee, it'll still appear)

---

## 🔹 4. FULL JOIN

> Returns all matching and non-matching rows from **both tables**

```sql
SELECT e.name, d.dept_name
FROM employees e
FULL JOIN departments d
ON e.dept_id = d.dept_id;
```

✔️ You get all employees and all departments, with `NULL`s where no match.

---

## 🔹 5. CROSS JOIN

> Returns the **Cartesian product** of both tables (every row in one matched with every row in the other)

```sql
SELECT e.name, d.dept_name
FROM employees e
CROSS JOIN departments d;
```

If 4 employees and 3 departments → 4 × 3 = 12 rows

---

## 🔹 6. SELF JOIN

> Joining a table **to itself** to compare rows

Example: Find employees in the **same department**

```sql
SELECT e1.name AS employee1, e2.name AS employee2, e1.dept_id
FROM employees e1
JOIN employees e2
ON e1.dept_id = e2.dept_id AND e1.emp_id <> e2.emp_id;
```

---

## 🧪 Tips for Writing JOINs

✅ Use **table aliases** like `e` and `d` for readability
✅ Always specify the **join condition** using `ON`
✅ Use `JOIN` with `GROUP BY`, `ORDER BY`, or `WHERE` as needed
✅ Know your data — understand if NULLs can appear

---

## 🔍 Check Relationships with `\d` in `psql`

```bash
\d employees
```

Look for foreign keys like:

```text
Foreign-key constraints:
    "employees_dept_id_fkey" FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
```

---

## ✅ Summary Table

| Join Type    | Use Case Example                              |
| ------------ | --------------------------------------------- |
| `INNER JOIN` | Employees with valid departments              |
| `LEFT JOIN`  | All employees, even those without depts       |
| `RIGHT JOIN` | All departments, even those without employees |
| `FULL JOIN`  | All employees + all departments               |
| `CROSS JOIN` | All combinations (rare)                       |
| `SELF JOIN`  | Compare within same table                     |

---



```sql

relationshipdb=# \d orders
                               Table "public.orders"
  Column  |  Type   | Collation | Nullable |                Default                 
----------+---------+-----------+----------+----------------------------------------
 ord_id   | integer |           | not null | nextval('orders_ord_id_seq'::regclass)
 ord_date | date    |           | not null | 
 price    | numeric |           | not null | 
 cust_id  | integer |           | not null | 
Indexes:
    "orders_pkey" PRIMARY KEY, btree (ord_id)
Foreign-key constraints:
    "orders_cust_id_fkey" FOREIGN KEY (cust_id) REFERENCES customers(cust_id)

relationshipdb=# \d customers
                                        Table "public.customers"
  Column   |          Type          | Collation | Nullable |                  Default                   
-----------+------------------------+-----------+----------+--------------------------------------------
 cust_id   | integer                |           | not null | nextval('customers_cust_id_seq'::regclass)
 cust_name | character varying(100) |           | not null | 
Indexes:
    "customers_pkey" PRIMARY KEY, btree (cust_id)
Referenced by:
    TABLE "orders" CONSTRAINT "orders_cust_id_fkey" FOREIGN KEY (cust_id) REFERENCES customers(cust_id)

relationshipdb=# INSERT INTO customers (cust_name)

VALUES 

    ('Raju'), ('Sham'), ('Paul'), ('Alex');
INSERT 0 4
relationshipdb=# select * from customers;
 cust_id | cust_name 
---------+-----------
       1 | Raju
       2 | Sham
       3 | Paul
       4 | Alex
(4 rows)

relationshipdb=# INSERT INTO orders (ord_date, cust_id, price)

VALUES 

    ('2024-01-01', 1, 250.00),  

    ('2024-01-15', 1, 300.00),  

    ('2024-02-01', 2, 150.00),

    ('2024-03-01', 3, 450.00),

    ('2024-04-04', 2, 550.00);  
INSERT 0 5
relationshipdb=# select * from orders;
 ord_id |  ord_date  | price  | cust_id 
--------+------------+--------+---------
      1 | 2024-01-01 | 250.00 |       1
      2 | 2024-01-15 | 300.00 |       1
      3 | 2024-02-01 | 150.00 |       2
      4 | 2024-03-01 | 450.00 |       3
      5 | 2024-04-04 | 550.00 |       2
(5 rows)

relationshipdb=# \d orders
                               Table "public.orders"
  Column  |  Type   | Collation | Nullable |                Default                 
----------+---------+-----------+----------+----------------------------------------
 ord_id   | integer |           | not null | nextval('orders_ord_id_seq'::regclass)
 ord_date | date    |           | not null | 
 price    | numeric |           | not null | 
 cust_id  | integer |           | not null | 
Indexes:
    "orders_pkey" PRIMARY KEY, btree (ord_id)
Foreign-key constraints:
    "orders_cust_id_fkey" FOREIGN KEY (cust_id) REFERENCES customers(cust_id)

relationshipdb=# \d customers
                                        Table "public.customers"
  Column   |          Type          | Collation | Nullable |                  Default                   
-----------+------------------------+-----------+----------+--------------------------------------------
 cust_id   | integer                |           | not null | nextval('customers_cust_id_seq'::regclass)
 cust_name | character varying(100) |           | not null | 
Indexes:
    "customers_pkey" PRIMARY KEY, btree (cust_id)
Referenced by:
    TABLE "orders" CONSTRAINT "orders_cust_id_fkey" FOREIGN KEY (cust_id) REFERENCES customers(cust_id)

relationshipdb=# SELECT * FROM customers;
 cust_id | cust_name 
---------+-----------
       1 | Raju
       2 | Sham
       3 | Paul
       4 | Alex
(4 rows)

relationshipdb=# SELECT * FROM orders;
 ord_id |  ord_date  | price  | cust_id 
--------+------------+--------+---------
      1 | 2024-01-01 | 250.00 |       1
      2 | 2024-01-15 | 300.00 |       1
      3 | 2024-02-01 | 150.00 |       2
      4 | 2024-03-01 | 450.00 |       3
      5 | 2024-04-04 | 550.00 |       2
(5 rows)

relationshipdb=# SELECT * FROM customers CROSS JOIN orders;
 cust_id | cust_name | ord_id |  ord_date  | price  | cust_id 
---------+-----------+--------+------------+--------+---------
       1 | Raju      |      1 | 2024-01-01 | 250.00 |       1
       2 | Sham      |      1 | 2024-01-01 | 250.00 |       1
       3 | Paul      |      1 | 2024-01-01 | 250.00 |       1
       4 | Alex      |      1 | 2024-01-01 | 250.00 |       1
       1 | Raju      |      2 | 2024-01-15 | 300.00 |       1
       2 | Sham      |      2 | 2024-01-15 | 300.00 |       1
       3 | Paul      |      2 | 2024-01-15 | 300.00 |       1
       4 | Alex      |      2 | 2024-01-15 | 300.00 |       1
       1 | Raju      |      3 | 2024-02-01 | 150.00 |       2
       2 | Sham      |      3 | 2024-02-01 | 150.00 |       2
       3 | Paul      |      3 | 2024-02-01 | 150.00 |       2
       4 | Alex      |      3 | 2024-02-01 | 150.00 |       2
       1 | Raju      |      4 | 2024-03-01 | 450.00 |       3
       2 | Sham      |      4 | 2024-03-01 | 450.00 |       3
       3 | Paul      |      4 | 2024-03-01 | 450.00 |       3
       4 | Alex      |      4 | 2024-03-01 | 450.00 |       3
       1 | Raju      |      5 | 2024-04-04 | 550.00 |       2
       2 | Sham      |      5 | 2024-04-04 | 550.00 |       2
       3 | Paul      |      5 | 2024-04-04 | 550.00 |       2
       4 | Alex      |      5 | 2024-04-04 | 550.00 |       2
(20 rows)

relationshipdb=# SELECT * FROM customers c INNER JOIN orders o ON c.cust_id = o.cust_id;
 cust_id | cust_name | ord_id |  ord_date  | price  | cust_id 
---------+-----------+--------+------------+--------+---------
       1 | Raju      |      1 | 2024-01-01 | 250.00 |       1
       1 | Raju      |      2 | 2024-01-15 | 300.00 |       1
       2 | Sham      |      3 | 2024-02-01 | 150.00 |       2
       3 | Paul      |      4 | 2024-03-01 | 450.00 |       3
       2 | Sham      |      5 | 2024-04-04 | 550.00 |       2
(5 rows)

relationshipdb=# SELECT c.cust_id, cust_name, COUNT(ord_id), sum(price) FROM customers c INNER JOIN orders o ON c.cust_id = o.cust_id GROUP BY c.cust_id, c.cust_name;
 cust_id | cust_name | count |  sum   
---------+-----------+-------+--------
       2 | Sham      |     2 | 700.00
       1 | Raju      |     2 | 550.00
       3 | Paul      |     1 | 450.00
(3 rows)

relationshipdb=# SELECT * FROM customers c LEFT JOIN orders o ON c.cust_id = o.cust_id;
 cust_id | cust_name | ord_id |  ord_date  | price  | cust_id 
---------+-----------+--------+------------+--------+---------
       1 | Raju      |      1 | 2024-01-01 | 250.00 |       1
       1 | Raju      |      2 | 2024-01-15 | 300.00 |       1
       2 | Sham      |      3 | 2024-02-01 | 150.00 |       2
       3 | Paul      |      4 | 2024-03-01 | 450.00 |       3
       2 | Sham      |      5 | 2024-04-04 | 550.00 |       2
       4 | Alex      |        |            |        |        
(6 rows)

relationshipdb=# SELECT * FROM customers c RIGHT JOIN orders o ON c.cust_id = o.cust_id;
 cust_id | cust_name | ord_id |  ord_date  | price  | cust_id 
---------+-----------+--------+------------+--------+---------
       1 | Raju      |      1 | 2024-01-01 | 250.00 |       1
       1 | Raju      |      2 | 2024-01-15 | 300.00 |       1
       2 | Sham      |      3 | 2024-02-01 | 150.00 |       2
       3 | Paul      |      4 | 2024-03-01 | 450.00 |       3
       2 | Sham      |      5 | 2024-04-04 | 550.00 |       2
(5 rows)

relationshipdb=# 



```