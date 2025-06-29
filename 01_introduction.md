***postgres notes:***    
```
https://www.canva.com/design/DAGGQI3jsQo/aZYZy0_PSEFsRfFUMNZx2g/edit
```

# ✅ **What is a Database?**

A **database** is a structured collection of data that is stored and accessed electronically.
It allows data to be organized, stored, retrieved, and managed efficiently.

🔸 **Example**: A student management system may store data like student names, marks, attendance, etc., in a database.

---

## ✅ **Difference Between Database and DBMS**

| Feature              | Database                                  | DBMS (Database Management System)                          |
| -------------------- | ----------------------------------------- | ---------------------------------------------------------- |
| **Definition**       | A collection of related data              | A software to create, manage, and interact with a database |
| **Functionality**    | Just holds data                           | Provides tools to add, update, delete, and retrieve data   |
| **Example**          | An Excel file with student data           | MySQL, Oracle DB, PostgreSQL, etc.                         |
| **User Interaction** | Cannot interact directly without software | Allows user interaction through queries (e.g., SQL)        |

---

## ✅ **What is RDBMS?**

**RDBMS** stands for **Relational Database Management System**.

It is a type of DBMS based on the **relational model** — where **data is stored in tables (rows and columns)** and **relations** between data are maintained using **keys** (primary key, foreign key).

🔑 **Features of RDBMS**:

* Tables with rows & columns
* Data Integrity
* SQL-based operations
* Relationships between tables

🔸 **Examples of RDBMS**:

* **MySQL**
* **PostgreSQL**
* **Oracle DB**
* **Microsoft SQL Server**
* **SQLite**

---

## ✅ **SQL vs PostgreSQL**

| Feature                 | SQL (Structured Query Language)                   | PostgreSQL                                                   |
| ----------------------- | ------------------------------------------------- | ------------------------------------------------------------ |
| **Type**                | Query Language                                    | Relational Database Management System (RDBMS)                |
| **Function**            | Used to write queries like SELECT, INSERT, UPDATE | A software that uses SQL to manage databases                 |
| **Purpose**             | Syntax/rules to interact with databases           | A system to store, retrieve, and manage data using SQL       |
| **Example Usage**       | `SELECT * FROM users;`                            | Can execute the above SQL in the PostgreSQL environment      |
| **Other DBs using SQL** | MySQL, SQL Server, Oracle DB, SQLite              | PostgreSQL is just one of them                               |
| **Advanced Features**   | Not applicable (it's just a language)             | Supports custom data types, JSON, indexing, full-text search |

👉 **In short**:

* **SQL** is a **language**.
* **PostgreSQL** is an **RDBMS software** that uses SQL.

---
---
---

# **Database vs Schema vs Table**
---

### ✅ **1. What is a Database?**

A **database** is like a **container** or **library** that stores and organizes related data for a particular purpose or application.

* Think of it as the **entire project folder**.
* It can contain multiple schemas and tables.

📦 **Example**:
Imagine a college database called `CollegeDB` — it stores all the data about students, courses, teachers, and departments.

```sql
-- Creating a database in SQL
CREATE DATABASE CollegeDB;
```

---

### ✅ **2. What is a Schema?**

A **schema** is like a **sub-folder** inside a database.
It groups logically related tables, views, functions, etc., within the same namespace.

* It helps in **organizing** objects and controlling **permissions**.
* One database can have multiple schemas.

📁 **Example**:

In `CollegeDB`, you might have these schemas:

* `admissions` → tables like `applicants`, `admission_tests`
* `academics` → tables like `students`, `courses`, `results`
* `staff` → tables like `teachers`, `departments`

```sql
-- Creating a schema
CREATE SCHEMA admissions;
```

---

### ✅ **3. What is a Table?**

A **table** is a structured format inside a schema to store actual data in **rows and columns**.

* Each **column** has a data type (e.g., name = VARCHAR, age = INT).
* Each **row** is a record.

📄 **Example**:

Inside the `academics` schema, a `students` table might look like:

| student\_id | name        | age | course\_id |
| ----------- | ----------- | --- | ---------- |
| 1           | Rahul Gupta | 20  | CS101      |
| 2           | Sneha Jain  | 22  | CS102      |

```sql
-- Creating a table inside a schema
CREATE TABLE academics.students (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    age INT,
    course_id VARCHAR(10)
);
```

---

### 🧠 **Visual Analogy**

```
DATABASE (CollegeDB)
│
├── SCHEMA: admissions
│   ├── TABLE: applicants
│   └── TABLE: admission_tests
│
├── SCHEMA: academics
│   ├── TABLE: students
│   └── TABLE: courses
│
└── SCHEMA: staff
    ├── TABLE: teachers
    └── TABLE: departments
```

---

### 🔑 Key Differences

| Feature  | Database                | Schema                     | Table                   |
| -------- | ----------------------- | -------------------------- | ----------------------- |
| Role     | Stores schemas and data | Organizes DB objects       | Stores actual data      |
| Scope    | Highest level container | Logical division inside DB | Defined inside a schema |
| Contains | Schemas, tables, views  | Tables, views, functions   | Columns & rows          |
| Example  | `CollegeDB`             | `academics`, `staff`       | `students`, `courses`   |

---

---
---

---

## 🚀 Starting PostgreSQL on Terminal

### 🔧 Step 1: Start PostgreSQL service

```bash
sudo systemctl start postgresql
```

You can check its status with:

```bash
sudo systemctl status postgresql
```

**Expected Output:**

```
● postgresql.service - PostgreSQL RDBMS
     Loaded: loaded (/lib/systemd/system/postgresql.service; enabled; vendor preset: enabled)
     Active: active (exited) since Sat 2025-06-28 10:49:37 IST; 11h ago
   Main PID: 1661 (code=exited, status=0/SUCCESS)
        CPU: 960us
```

---

### 🛑 Step 2: Accessing `psql` as current user

```bash
psql
```

**Error you may see:**

```
psql: error: connection to server on socket "/var/run/postgresql/.s.PGSQL.5432" failed: 
FATAL:  role "ayush" does not exist
```

This means the current Linux user (`ayush`) does not have a PostgreSQL role.

---

### 👤 Step 3: Switch to the `postgres` system user

```bash
sudo -iu postgres
```

Now run:

```bash
psql
```

You’ll enter the `psql` prompt:

```
psql (17.5 (Ubuntu 17.5...), server 16.9 ...)
Type "help" for help.
```

---

### 📂 Step 4: List existing databases

```sql
\l
```

**Output:**

```
List of databases
 Name           |  Owner   | Encoding | ...
----------------+----------+----------+-----
 TodoApplicationDatabase | postgres | UTF8     | ...
 my_pgdb                 | postgres | UTF8     | ...
 postgres                | postgres | UTF8     | ...
 template0               | postgres | UTF8     | ...
 template1               | postgres | UTF8     | ...
```

---

### 🔐 Step 5: Set password for `postgres` user

```sql
\password postgres
```

---

### ❌ Mistake: Trying to run SQL command from shell

```bash
CREATE DATABASE TEST;
```

**Output:**

```
CREATE: command not found
```

> ⚠️ This is wrong because `CREATE DATABASE` is an SQL command, not a shell command.

---

### ✅ Correct: Create database from inside `psql`

```bash
psql
```

Then run:

```sql
CREATE DATABASE TEST;
```

**Output:**

```
CREATE DATABASE
```

---

### 📋 Verify database was created

```sql
\l
```

**New Output includes:**

```
test | postgres | UTF8 | libc | ...
```

---

### 🚪 Exit from `psql` and `postgres` user

```sql
\q
```

```bash
exit
```

---
