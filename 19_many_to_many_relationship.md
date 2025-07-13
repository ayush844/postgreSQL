

## 🔗 What is a Many-to-Many Relationship?

A **many-to-many relationship** occurs when:

> One record in **Table A** can be related to **multiple** records in **Table B**,
> **and** one record in **Table B** can be related to **multiple** records in **Table A**.

🚫 You **cannot** implement this directly using just foreign keys in two tables —
✅ Instead, you need a **junction table (association table)** to connect them.

---

## 🧠 Real-World Example: Students & Courses

* A **student** can enroll in multiple **courses**.
* A **course** can have multiple **students**.

---

## 🧱 Step-by-Step Implementation in PostgreSQL

### 🔸 1. `students` Table

```sql
CREATE TABLE students (
  student_id SERIAL PRIMARY KEY,
  name VARCHAR(100)
);
```

---

### 🔸 2. `courses` Table

```sql
CREATE TABLE courses (
  course_id SERIAL PRIMARY KEY,
  course_name VARCHAR(100)
);
```

---

### 🔸 3. Junction Table: `student_courses`

This table connects students and courses using **two foreign keys**.

```sql
CREATE TABLE student_courses (
  student_id INT REFERENCES students(student_id),
  course_id INT REFERENCES courses(course_id),
  PRIMARY KEY (student_id, course_id)  -- Composite PK
);
```

📌 This structure:

* Allows one student to register for many courses
* Allows one course to have many students
* Prevents duplicate pairs (via composite PK)

---

## ✅ Inserting Sample Data

```sql
-- Students
INSERT INTO students (name) VALUES ('Ayush'), ('Neha'), ('Arjun');

-- Courses
INSERT INTO courses (course_name) VALUES ('Math'), ('English'), ('Science');

-- Enrollments
INSERT INTO student_courses (student_id, course_id) VALUES
(1, 1),  -- Ayush → Math
(1, 2),  -- Ayush → English
(2, 1),  -- Neha → Math
(3, 3);  -- Arjun → Science
```

---

## 🔍 Query: List all students and their enrolled courses

```sql
SELECT s.name AS student, c.course_name
FROM student_courses sc
JOIN students s ON sc.student_id = s.student_id
JOIN courses c ON sc.course_id = c.course_id;
```

### ✅ Output

| student | course\_name |
| ------- | ------------ |
| Ayush   | Math         |
| Ayush   | English      |
| Neha    | Math         |
| Arjun   | Science      |

---

## 📝 Summary

| Table             | Purpose                                   |
| ----------------- | ----------------------------------------- |
| `students`        | Holds student info                        |
| `courses`         | Holds course info                         |
| `student_courses` | Links students and courses (many-to-many) |

---

## 💡 Notes

* The **junction table** (`student_courses`) is the key to implementing many-to-many relationships.
* Composite primary keys (`student_id, course_id`) ensure uniqueness.
* You can also add extra fields like `enrollment_date`, `grade`, etc., in the junction table if needed.

---
