
# 🔢 RELATIONAL (COMPARISON) OPERATORS

These are used to **compare two values** (usually in the `WHERE` clause) and return a boolean result (`TRUE` or `FALSE`).

---

### ✅ 1. `=` (Equal To)

**Checks if two values are equal.**

```sql
SELECT * FROM employees WHERE dept = 'IT';
```

📌 **Explanation**: Returns all rows where the department is exactly `'IT'`.

---

### ✅ 2. `!=` or `<>` (Not Equal To)

**Checks if two values are NOT equal.**

```sql
SELECT * FROM employees WHERE dept != 'HR';
```

**OR**

```sql
SELECT * FROM employees WHERE dept <> 'HR';
```

📌 **Explanation**: Returns all employees **except** those in the HR department.

---

### ✅ 3. `>` (Greater Than)

**Checks if the left value is greater than the right one.**

```sql
SELECT * FROM employees WHERE salary > 50000;
```

📌 **Explanation**: Lists all employees earning **more than ₹50,000**.

---

### ✅ 4. `<` (Less Than)

**Checks if the left value is less than the right one.**

```sql
SELECT * FROM employees WHERE salary < 40000;
```

📌 **Explanation**: Shows all employees with salary **less than ₹40,000**.

---

### ✅ 5. `>=` (Greater Than or Equal To)

```sql
SELECT * FROM employees WHERE salary >= 60000;
```

📌 **Explanation**: Includes employees with salary **equal to or greater than** ₹60,000.

---

### ✅ 6. `<=` (Less Than or Equal To)

```sql
SELECT * FROM employees WHERE salary <= 45000;
```

📌 **Explanation**: Lists employees earning **₹45,000 or less**.

---

### ✅ 7. `BETWEEN ... AND ...`

**Checks if a value lies within a given range (inclusive).**

```sql
SELECT * FROM employees WHERE salary BETWEEN 40000 AND 55000;
```

📌 **Explanation**: Returns employees whose salaries are **between ₹40,000 and ₹55,000 (inclusive)**.

---

### ✅ 8. `IN (...)`

**Checks if a value is present in a list.**

```sql
SELECT * FROM employees WHERE dept IN ('HR', 'IT');
```

📌 **Explanation**: Shows employees who belong to **either HR or IT**.

---

### ✅ 9. `NOT IN (...)`

**Returns rows where the value is not in the list.**

```sql
SELECT * FROM employees WHERE emp_id NOT IN (1, 3, 5);
```

📌 **Explanation**: Returns all employees **except** those with ID 1, 3, or 5.

---

### ✅ 10. `IS NULL`

**Used to find rows where a column has no value.**

```sql
SELECT * FROM employees WHERE email IS NULL;
```

📌 **Explanation**: Lists employees who **do not have an email**.

---

### ✅ 11. `IS NOT NULL`

**Finds rows where a column has a value.**

```sql
SELECT * FROM employees WHERE email IS NOT NULL;
```

📌 **Explanation**: Lists employees **who have an email**.

---

# ⚙️ LOGICAL OPERATORS

These are used to **combine multiple conditions** in a `WHERE` clause.

---

### ✅ 1. `AND`

**All conditions must be TRUE.**

```sql
SELECT * FROM employees
WHERE dept = 'IT' AND salary > 50000;
```

📌 **Explanation**: Returns employees who are in IT **and** earn **more than ₹50,000**.

---

### ✅ 2. `OR`

**At least one condition must be TRUE.**

```sql
SELECT * FROM employees
WHERE dept = 'Marketing' OR salary > 60000;
```

📌 **Explanation**: Returns employees who are in **Marketing** OR have a **salary above ₹60,000**.

---

### ✅ 3. `NOT`

**Negates the condition.**

```sql
SELECT * FROM employees
WHERE NOT dept = 'Finance';
```

📌 **Explanation**: Returns employees who are **not in Finance**.

---

### ✅ 4. Combining Logical Operators

You can use parentheses to control the logic:

```sql
SELECT * FROM employees
WHERE (dept = 'HR' OR dept = 'IT') AND salary >= 50000;
```

📌 **Explanation**:
Returns employees who are **in HR or IT** **and** earn **₹50,000 or more**.

---

### ✅ 5. Precedence Order

| Operator | Precedence |
| -------- | ---------- |
| `NOT`    | Highest    |
| `AND`    | Middle     |
| `OR`     | Lowest     |

Use **parentheses** to ensure clarity in complex queries.

---

## 🧪 Sample Combined Query

```sql
SELECT fname, salary, dept FROM employees
WHERE salary BETWEEN 45000 AND 60000
  AND dept IN ('IT', 'HR')
  AND fname LIKE 'A%';
```

📌 **Explanation**:

* Salary between ₹45,000 and ₹60,000
* In either IT or HR department
* First name starts with **'A'**

---

