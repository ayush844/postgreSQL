

# 🔗 Relationships in PostgreSQL (and RDBMS) – Explained

---

## 🔍 What is a Relationship?

A **relationship** in a database defines how **two or more tables are logically connected** to one another using **keys** — usually `primary keys` and `foreign keys`.

---

## 🎯 Why Relationships Matter?

* They avoid **data duplication** (normalization)
* Enforce **referential integrity**
* Allow **complex queries** across tables (via `JOIN`)

---

## 🧱 Types of Relationships in PostgreSQL

| Relationship Type   | Description                                                           | Example                   |
| ------------------- | --------------------------------------------------------------------- | ------------------------- |
| 1️⃣ One-to-One      | One row in Table A ↔️ One row in Table B                              | Each user has one profile |
| 1️⃣➡️♾️ One-to-Many | One row in Table A ↔️ Many rows in Table B                            | One customer, many orders |
| ♾️↔️♾️ Many-to-Many | Many rows in Table A ↔️ Many rows in Table B (via **junction table**) | Students and courses      |

---

## 🔹 1. One-to-One Relationship

### Example: `Users` and `UserProfiles`

```sql
CREATE TABLE users (
  user_id SERIAL PRIMARY KEY,
  name VARCHAR(50)
);

CREATE TABLE user_profiles (
  profile_id SERIAL PRIMARY KEY,
  user_id INT UNIQUE REFERENCES users(user_id),
  bio TEXT
);
```

✔️ `UNIQUE` on `user_id` ensures only one profile per user.

---

## 🔹 2. One-to-Many Relationship (most common)

### Example: `Customers` and `Orders`

```sql
CREATE TABLE customers (
  customer_id SERIAL PRIMARY KEY,
  name VARCHAR(100)
);

CREATE TABLE orders (
  order_id SERIAL PRIMARY KEY,
  customer_id INT REFERENCES customers(customer_id),
  order_total NUMERIC
);
```

✔️ One customer can have **many orders**, but each order belongs to **only one customer**.

---

## 🔹 3. Many-to-Many Relationship

You create a **junction table** to model this.

### Example: `Students` and `Courses`

```sql
CREATE TABLE students (
  student_id SERIAL PRIMARY KEY,
  name VARCHAR(100)
);

CREATE TABLE courses (
  course_id SERIAL PRIMARY KEY,
  title VARCHAR(100)
);

CREATE TABLE student_courses (
  student_id INT REFERENCES students(student_id),
  course_id INT REFERENCES courses(course_id),
  PRIMARY KEY (student_id, course_id)
);
```

✔️ A student can enroll in multiple courses, and a course can have multiple students.

---

## 🔐 How to Enforce Relationships?

### Using `FOREIGN KEY` Constraint

```sql
FOREIGN KEY (column_name) REFERENCES parent_table(primary_key)
```

🧱 This ensures that:

* Child rows cannot refer to **non-existent** parent rows
* Parent rows cannot be deleted if there are dependent child rows (unless using `ON DELETE`)

---

## 🔄 `ON DELETE` and `ON UPDATE` Options

| Option     | Effect                                                     |
| ---------- | ---------------------------------------------------------- |
| `CASCADE`  | Automatically delete/update child rows                     |
| `SET NULL` | Set child foreign key to NULL if parent is deleted/updated |
| `RESTRICT` | Prevent deletion if child rows exist (default behavior)    |

---

## 📌 Summary of Relationships

| Type         | Foreign Key Placement    | Example Tables           |
| ------------ | ------------------------ | ------------------------ |
| One-to-One   | Either side + `UNIQUE`   | `users`, `user_profiles` |
| One-to-Many  | In the "many" side table | `customers`, `orders`    |
| Many-to-Many | Separate junction table  | `students`, `courses`    |

---

## 🔍 View Relationships in `psql`

```bash
\d tablename
```

It will show foreign keys and constraints.

---

