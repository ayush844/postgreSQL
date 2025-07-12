
---

# ✅ PostgreSQL Practice Q\&A (String, Filtering, Ordering)

---

### 🔹 Q1: How to get the `emp_id`, full name, and department of an employee in a single string (separated by `:`) for a specific employee?

```sql
SELECT CONCAT_WS(':', emp_id, CONCAT_WS(' ', fname, lname), dept)
FROM employees
WHERE emp_id = 1;
```

**🟢 Output:** `1:Raj Sharma:IT`

---

### 🔹 Q2: How to also include the salary in the above concatenated string?

```sql
SELECT CONCAT_WS(':', emp_id, CONCAT_WS(' ', fname, lname), dept, salary)
FROM employees
WHERE emp_id = 1;
```

**🟢 Output:** `1:Raj Sharma:IT:50000.00`

---

### 🔹 Q3: How to convert department to uppercase in the output string?

```sql
SELECT CONCAT_WS(':', emp_id, CONCAT_WS(' ', fname, lname), UPPER(dept))
FROM employees
WHERE emp_id = 4;
```

**🟢 Output:** `4:Suman Patel:FINANCE`

---

### 🔹 Q4: How to create a custom ID by combining the first letter of the department and emp\_id?

```sql
SELECT CONCAT(LEFT(dept, 1), emp_id), fname 
FROM employees;
```

**🟢 Output Example:** `I1 | Raj`, `H2 | Priya`, `F4 | Suman`, etc.

---

### 🔹 Q5: How to find all **distinct departments**?

```sql
SELECT DISTINCT(dept) 
FROM employees;
```

**🟢 Output:** IT, HR, Finance, Marketing

---

### 🔹 Q6: How to sort all employees by **salary in descending order**?

```sql
SELECT * 
FROM employees 
ORDER BY salary DESC;
```

---

### 🔹 Q7: How to fetch only the **first 3 employees** from the table?

```sql
SELECT * 
FROM employees 
LIMIT 3;
```

---

### 🔹 Q8: How to find employees whose first name **starts with 'A'**?

```sql
SELECT * 
FROM employees 
WHERE fname LIKE 'A%';
```

**🟢 Output:** Arjun, Amit, Anjali

---

### 🔹 Q9: How to get employees where the last name is **exactly 4 characters long**?

```sql
SELECT * 
FROM employees 
WHERE LENGTH(lname) = 4;
```

**🟢 Output:** Vijay Nair

---

### 🔹 Q10: How to count the total number of employees?

```sql
SELECT COUNT(*) FROM employees;
```

**🟢 Output:** `10`

---

### 🔹 Q11: How to count the number of employees in each department?

```sql
SELECT dept, COUNT(*) 
FROM employees 
GROUP BY dept;
```

**🟢 Output:**

| dept      | count |
| --------- | ----- |
| IT        | 4     |
| HR        | 2     |
| Finance   | 2     |
| Marketing | 2     |

---

### 🔹 Q12: How to get the minimum salary in each department?

```sql
SELECT dept, MIN(salary) 
FROM employees 
GROUP BY dept;
```

---

### 🔹 Q13: How to get the minimum salary among all employees?

```sql
SELECT MIN(salary) 
FROM employees;
```

**🟢 Output:** `45000.00`

---

### 🔹 Q14: How to get the maximum salary among all employees?

```sql
SELECT MAX(salary) 
FROM employees;
```

**🟢 Output:** `61000.00`

---

### 🔹 Q15: How to get the total (sum) of salaries per department?

```sql
SELECT dept, SUM(salary) 
FROM employees 
GROUP BY dept;
```

---

### 🔹 Q16: How to get the average salary per department?

```sql
SELECT dept, AVG(salary) 
FROM employees 
GROUP BY dept;
```

📝 You may see long decimals — PostgreSQL returns full precision unless rounded.

To round the result:

```sql
SELECT dept, ROUND(AVG(salary), 2) AS avg_salary 
FROM employees 
GROUP BY dept;
```

---
