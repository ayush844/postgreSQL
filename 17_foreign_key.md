
# 🔑 FOREIGN KEY in PostgreSQL 

## 🔍 What is a Foreign Key?

A **foreign key** is a field (or combination of fields) in one table that **refers to the primary key in another table**.
It is used to **link two tables together** and maintain **referential integrity** — i.e., ensure valid, consistent relationships between rows.

---

## 📘 Key Concepts

| Term           | Meaning                                                              |
| -------------- | -------------------------------------------------------------------- |
| Primary Table  | The table that holds the original (referenced) data.                 |
| Foreign Table  | The table that contains the foreign key (referencing another table). |
| Referenced Key | The column in the primary table (usually a `PRIMARY KEY`).           |
| Foreign Key    | The column in the foreign table that points to the referenced key.   |

---

## 🧱 Syntax

```sql
FOREIGN KEY (column_name) REFERENCES referenced_table (referenced_column)
```

---

## 🧪 Example: `departments` and `employees`

### Step 1: Create Parent Table (`departments`)

```sql
CREATE TABLE departments (
  dept_id SERIAL PRIMARY KEY,
  dept_name VARCHAR(100) UNIQUE NOT NULL
);
```

---

### Step 2: Create Child Table (`employees`)

```sql
CREATE TABLE employees (
  emp_id SERIAL PRIMARY KEY,
  name VARCHAR(100),
  dept_id INT,
  FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
);
```

✅ Now, `dept_id` in `employees` must match a `dept_id` in `departments`, or be NULL (if not `NOT NULL` constrained).

---

## 🔐 Referential Integrity

Foreign keys **enforce rules** that keep your data clean:

* You **cannot insert** an employee with `dept_id = 99` if no department with `dept_id = 99` exists.
* You **cannot delete** a department that has employees (unless specified otherwise).

---

## 🔁 ON DELETE / ON UPDATE Actions

You can control how PostgreSQL reacts when data is deleted or updated in the **parent table**:

| Action        | Description                                                      |
| ------------- | ---------------------------------------------------------------- |
| `CASCADE`     | Delete/update the child rows automatically                       |
| `SET NULL`    | Set the foreign key in child table to `NULL`                     |
| `SET DEFAULT` | Set it to a default value (only if a default is defined)         |
| `RESTRICT`    | Block the operation if it would leave orphans (default behavior) |
| `NO ACTION`   | Like RESTRICT, but checks are deferred (rarely used)             |

### Example:

```sql
FOREIGN KEY (dept_id) 
REFERENCES departments(dept_id) 
ON DELETE SET NULL 
ON UPDATE CASCADE
```

---

## 📍 Adding Foreign Key After Table Creation

```sql
ALTER TABLE employees
ADD CONSTRAINT fk_dept
FOREIGN KEY (dept_id)
REFERENCES departments(dept_id);
```

To drop it:

```sql
ALTER TABLE employees
DROP CONSTRAINT fk_dept;
```

You can see the constraint name via:

```bash
\d employees
```

---

## 🔥 Real-Life Example

Let’s say:

* Table: `customers (customer_id)`
* Table: `orders (order_id, customer_id)`

Foreign key ensures:

* You can’t insert an `order` for a non-existent `customer`
* You can’t delete a `customer` who has `orders` unless you use `ON DELETE CASCADE` or similar

---

## ✅ Summary Table

| Operation           | Description                                                  |
| ------------------- | ------------------------------------------------------------ |
| Define FK           | `FOREIGN KEY (col) REFERENCES other_table(pk)`               |
| Enforce integrity   | Ensures references point to valid rows                       |
| Prevent bad deletes | Prevent deleting referenced data (or handle it with CASCADE) |
| Can be NULL         | Foreign keys are allowed to be `NULL` unless `NOT NULL`      |

---

## 💬 Example Query

```sql
-- Show all employees with department name
SELECT e.emp_id, e.name, d.dept_name
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id;
```

---


```sql

postgres=# CREATE DATABASE RelationshipDB;
CREATE DATABASE
postgres=# \c relationshipdb 
psql (17.5 (Ubuntu 17.5-1.pgdg22.04+1), server 16.9 (Ubuntu 16.9-1.pgdg22.04+1))
You are now connected to database "relationshipdb" as user "postgres".
relationshipdb=# CREATE TABLE customers (  

          cust_id SERIAL PRIMARY KEY, 

          cust_name VARCHAR(100) NOT NULL 

);
CREATE TABLE
relationshipdb=# CREATE TABLE orders ( 

            ord_id SERIAL PRIMARY KEY, 

            ord_date DATE NOT NULL, 

            price NUMERIC NOT NULL,

            cust_id INTEGER NOT NULL, 

            FOREIGN KEY (cust_id) REFERENCES 

            customers (cust_id) 

);
CREATE TABLE
relationshipdb=# \d
                  List of relations
 Schema |         Name          |   Type   |  Owner   
--------+-----------------------+----------+----------
 public | customers             | table    | postgres
 public | customers_cust_id_seq | sequence | postgres
 public | orders                | table    | postgres
 public | orders_ord_id_seq     | sequence | postgres
(4 rows)

relationshipdb=# \dt
           List of relations
 Schema |   Name    | Type  |  Owner   
--------+-----------+-------+----------
 public | customers | table | postgres
 public | orders    | table | postgres
(2 rows)

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

relationshipdb=# 



```