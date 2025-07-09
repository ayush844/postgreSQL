PostgreSQL has powerful **string functions** that are extremely useful in manipulating and formatting text data.

---

# 📘 PostgreSQL String Functions (Detailed)

---

## 🔹 1. `CONCAT()`

**Combines two or more strings.**

### ✅ Syntax:

```sql
CONCAT(string1, string2, ..., stringN)
```

### 🧪 Example:

```sql
SELECT CONCAT('Ayush', ' ', 'Sharma') AS full_name;
-- Output: Ayush Sharma
```

### 📌 Note:

* Automatically converts non-string types (like numbers) into text.
* If any argument is `NULL`, it is **treated as an empty string** (not NULL).

---

## 🔹 2. `CONCAT_WS()` (With Separator)

**Concatenates strings using a separator.**

### ✅ Syntax:

```sql
CONCAT_WS(separator, string1, string2, ..., stringN)
```

### 🧪 Example:

```sql
SELECT CONCAT_WS('-', '2025', '06', '29') AS formatted_date;
-- Output: 2025-06-29
```

### 📌 WS → "With Separator"

---

## 🔹 3. `SUBSTR()` or `SUBSTRING()`

**Extracts a part of a string from a given position.**

### ✅ Syntax:

```sql
SUBSTR(string, start_position, length)
```

### 🧪 Example:

```sql
SELECT SUBSTR('PostgreSQL', 1, 4) AS result;
-- Output: Post
```

📌 Indexing starts at `1` (not 0).

---

## 🔹 4. `LEFT()` and `RIGHT()`

**Returns characters from the left or right side of a string.**

### ✅ Syntax:

```sql
LEFT(string, n)
RIGHT(string, n)
```

### 🧪 Example:

```sql
SELECT LEFT('Database', 4);   -- Output: Data
SELECT RIGHT('Database', 4);  -- Output: base
```

---

## 🔹 5. `LENGTH()`

**Returns the number of characters in the string.**

### ✅ Syntax:

```sql
LENGTH(string)
```

### 🧪 Example:

```sql
SELECT LENGTH('Ayush');   -- Output: 5
```

---

## 🔹 6. `UPPER()` and `LOWER()`

**Converts a string to uppercase or lowercase.**

### 🧪 Example:

```sql
SELECT UPPER('hello world');  -- Output: HELLO WORLD
SELECT LOWER('HELLO');        -- Output: hello
```

---

## 🔹 7. `TRIM()`, `LTRIM()`, `RTRIM()`

**Removes spaces or specific characters from the string.**

### ✅ Syntax:

```sql
TRIM([LEADING | TRAILING | BOTH] 'char' FROM string)
LTRIM(string, 'char')
RTRIM(string, 'char')
```

### 🧪 Example:

```sql
SELECT TRIM('  Ayush  ');       -- Output: 'Ayush'
SELECT LTRIM('---Hello', '-');  -- Output: 'Hello'
SELECT RTRIM('World***', '*');  -- Output: 'World'
```

---

## 🔹 8. `REPLACE()`

**Replaces all occurrences of a substring with another.**

### ✅ Syntax:

```sql
REPLACE(string, search, replace)
```

### 🧪 Example:

```sql
SELECT REPLACE('I love Java', 'Java', 'PostgreSQL');
-- Output: I love PostgreSQL
```

---

## 🔹 9. `POSITION()`

**Finds the position of a substring in a string.**

### ✅ Syntax:

```sql
POSITION(substring IN string)
```

### 🧪 Example:

```sql
SELECT POSITION('@' IN 'ayush@example.com');  -- Output: 6
```

---

## 🔹 10. `STRING_AGG()`

**Concatenates values from multiple rows into a single string, with a separator.**

### ✅ Syntax:

```sql
STRING_AGG(expression, separator)
```

### 🧪 Example:

Assume this table:

| name  |
| ----- |
| Raj   |
| Priya |
| Arjun |

```sql
SELECT STRING_AGG(name, ', ') FROM employees;
-- Output: Raj, Priya, Arjun
```

### 📌 Use case:

Perfect for showing tags, names, categories, etc. in one row.

---

# ✅ Summary Table

| Function       | Description                      | Example Output        |
| -------------- | -------------------------------- | --------------------- |
| `CONCAT()`     | Join strings                     | `'Ayush Sharma'`      |
| `CONCAT_WS()`  | Join with separator              | `'2025-06-29'`        |
| `SUBSTR()`     | Slice part of string             | `'Post'`              |
| `LEFT()`       | Left `n` chars                   | `'Data'`              |
| `RIGHT()`      | Right `n` chars                  | `'base'`              |
| `LENGTH()`     | Length of string                 | `5`                   |
| `UPPER()`      | All caps                         | `'HELLO'`             |
| `LOWER()`      | All lowercase                    | `'hello'`             |
| `TRIM()`       | Remove leading/trailing spaces   | `'Ayush'`             |
| `REPLACE()`    | Replace substring                | `'I love PostgreSQL'` |
| `POSITION()`   | Substring index                  | `6`                   |
| `STRING_AGG()` | Join multiple rows into a string | `'Raj, Priya, Arjun'` |

---

