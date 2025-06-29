
---

## ✅ 1. **List all databases**

### 📌 Option 1: Using psql meta-command

```sql
\l
```

* Shows a list of **all databases** in the PostgreSQL cluster.
* Includes columns like `Name`, `Owner`, `Encoding`, `Collate`, etc.
* Can also be written as:

```sql
\list
```

### 📌 Option 2: Using SQL query

```sql
SELECT datname FROM pg_database;
```

* Only lists the **names** of the databases.
* You can add filters like:

```sql
SELECT datname FROM pg_database WHERE datistemplate = false;
```

* This skips the default `template0` and `template1` databases.

---

## ✅ 2. **Create a new database**

### SQL Command:

```sql
CREATE DATABASE mynewdb;
```

* Replace `mynewdb` with your desired name.
* You can also specify:

```sql
CREATE DATABASE mynewdb
WITH OWNER = postgres
ENCODING = 'UTF8'
LC_COLLATE = 'en_US.UTF-8'
LC_CTYPE = 'en_US.UTF-8'
TEMPLATE = template0;
```

---

## ✅ 3. **Change (connect to) a database**

### Meta-command in `psql`:

```sql
\c mynewdb
```

or

```sql
\connect mynewdb
```

* Switches your current session to `mynewdb`.
* If you want to connect as a different user:

```sql
\c mynewdb username
```

---

## ✅ 4. **Drop a database**

### ⚠️ Important: You **cannot be connected** to the database you are trying to drop.

### SQL Command:

```sql
DROP DATABASE mynewdb;
```

If you are using **pgAdmin** or your terminal is currently connected to the database you want to drop, you’ll get:

```
ERROR:  cannot drop the currently open database
```

### ✅ Solution:

1. First **disconnect** or switch to another database (e.g., `postgres`):

   ```sql
   \c postgres
   ```
2. Then run:

   ```sql
   DROP DATABASE mynewdb;
   ```

In **pgAdmin**:

* Right-click on the database → "Disconnect" → Then right-click again → "Delete/Drop".

---

### 🧠 Summary Table

| Task            | Command                                    | Notes                               |
| --------------- | ------------------------------------------ | ----------------------------------- |
| List databases  | `\l` or `SELECT datname FROM pg_database;` | `\l` gives more details             |
| Create database | `CREATE DATABASE mynewdb;`                 | Use `WITH` for options              |
| Change database | `\c mynewdb`                               | Also `\connect`                     |
| Drop database   | `DROP DATABASE mynewdb;`                   | Not allowed if connected to that DB |

---
---
---


# CRUD operations



## 🔧 What is CRUD?

CRUD stands for the four basic operations we perform on database records:

| Letter | Operation | SQL Keyword | Purpose              |
| ------ | --------- | ----------- | -------------------- |
| **C**  | Create    | `INSERT`    | Add new data         |
| **R**  | Read      | `SELECT`    | Fetch existing data  |
| **U**  | Update    | `UPDATE`    | Modify existing data |
| **D**  | Delete    | `DELETE`    | Remove data          |

---

## ✅ Step-by-Step CRUD with Your PostgreSQL Commands

### 🔁 Before CRUD: Start & Connect to PostgreSQL

```bash
sudo systemctl start postgresql     # Starts the PostgreSQL server
sudo systemctl status postgresql    # Checks server status
sudo -iu postgres                   # Switch to PostgreSQL default user
psql                                # Enters PostgreSQL interactive shell
```

---

## ✅ CREATE – Creating Database, Table, and Data

### 🟡 Create a Database:

```sql
CREATE DATABASE mynewdb;
CREATE DATABASE persondb;
```

> ✅ This creates a new database, a container for your tables.

### 🟡 Create a Table:

```sql
CREATE TABLE person(id INT, name VARCHAR(100), city VARCHAR(100));
```

> ✅ You created a table `person` with 3 columns.

### 🟡 Insert Data (Insert Records):

```sql
INSERT INTO person(id, name, city) VALUES(1, 'aayesha', 'delhi');
INSERT INTO person VALUES(2, 'monika');
INSERT INTO person VALUES(3, 'ayushi', 'delhi'), (4, 'anam', 'hyderabad'), (5, 'nidhi', 'punjab');
```

> ✅ These statements add data to your table.

---

## ✅ READ – Retrieving Data

```sql
SELECT * FROM person;
SELECT id, name FROM person;
SELECT * FROM person WHERE id = 1;
```

> ✅ `SELECT` is used to read/fetch data. You used:

* `*` to fetch all columns.
* `WHERE` to filter data.
* Specific columns like `id, name`.

---

## ✅ UPDATE – Modify Existing Data

### ❌ Incorrect (you used double quotes `"..."`, which PostgreSQL interprets as **column names**):

```sql
UPDATE person SET name = "aayesha sharma" WHERE id = 1;
```

### ✅ Correct (single quotes for string literals):

```sql
UPDATE person SET name = 'aayesha sharma' WHERE id = 1;
```

> ✅ This changes the name where `id = 1`.

---

## ✅ DELETE – Remove Records

```sql
DELETE FROM person WHERE id = 3;
```

> ✅ This deletes the row where `id = 3`.

---

## 🚫 DROP – Deleting Entire Tables or Databases (Destructive)

### Drop a database:

```sql
DROP DATABASE test;
```

> ❌ Got an error because another session was connected.

### Force disconnect & drop:

```sql
SELECT pg_terminate_backend(pg_stat_activity.pid)
FROM pg_stat_activity
WHERE datname = 'test' AND pid <> pg_backend_pid();

DROP DATABASE test;
```

> ✅ This is how you safely drop a database in use.

---

## 🧠 Bonus Tips You Encountered:

### ✅ Check list of databases:

```sql
\l
SELECT datname FROM pg_database;
```

### ✅ Switch databases:

```sql
\c mynewdb
\c persondb
```

### ✅ Check structure of table:

```sql
\d person
```

---

## 📘 Summary: CRUD Mapping with Your Commands

| Operation    | Command You Used                 | Description               |
| ------------ | -------------------------------- | ------------------------- |
| Create DB    | `CREATE DATABASE mynewdb;`       | Creates a new database    |
| Create Table | `CREATE TABLE person(...)`       | Creates a new table in DB |
| Insert       | `INSERT INTO person VALUES(...)` | Adds new records          |
| Read         | `SELECT * FROM person;`          | Reads records             |
| Update       | `UPDATE person SET ...`          | Modifies data             |
| Delete       | `DELETE FROM person WHERE ...`   | Deletes specific records  |
| Drop DB      | `DROP DATABASE test;`            | Destroys entire DB        |

---
