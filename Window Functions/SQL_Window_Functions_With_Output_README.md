
# 📘 SQL Window Functions - Real World Examples with Output

This guide shows when and how to use SQL Window Functions with real-world use cases, sample data, queries, and expected output.

---

## 📄 Sample Table: Employees

| id | name  | department | salary |
|----|-------|------------|--------|
| 1  | Alice | HR         | 5000   |
| 2  | Bob   | HR         | 6000   |
| 3  | Carol | HR         | 6000   |
| 4  | Dave  | IT         | 7000   |
| 5  | Eve   | IT         | 7500   |
| 6  | Frank | IT         | 7200   |
| 7  | Grace | Finance    | 8000   |

---

## 1️⃣ ROW_NUMBER()

**Use case:** Get each employee's rank within their department by salary.

```sql
SELECT name, department, salary,
       ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS dept_rank
FROM employees;
```

**Expected Output:**

| name  | department | salary | dept_rank |
|-------|------------|--------|-----------|
| Bob   | HR         | 6000   | 1         |
| Carol | HR         | 6000   | 2         |
| Alice | HR         | 5000   | 3         |
| Eve   | IT         | 7500   | 1         |
| Frank | IT         | 7200   | 2         |
| Dave  | IT         | 7000   | 3         |
| Grace | Finance    | 8000   | 1         |

---

## 2️⃣ RANK()

**Use case:** Rank employees by salary with gaps in rank for duplicates.

```sql
SELECT name, department, salary,
       RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS salary_rank
FROM employees;
```

**Expected Output:**

| name  | department | salary | salary_rank |
|-------|------------|--------|-------------|
| Bob   | HR         | 6000   | 1           |
| Carol | HR         | 6000   | 1           |
| Alice | HR         | 5000   | 3           |
| Eve   | IT         | 7500   | 1           |
| Frank | IT         | 7200   | 2           |
| Dave  | IT         | 7000   | 3           |
| Grace | Finance    | 8000   | 1           |

---

## 3️⃣ DENSE_RANK()

**Use case:** Same as RANK, but with no gaps between rank numbers.

```sql
SELECT name, department, salary,
       DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dense_rank
FROM employees;
```

**Expected Output:**

| name  | department | salary | dense_rank |
|-------|------------|--------|------------|
| Bob   | HR         | 6000   | 1          |
| Carol | HR         | 6000   | 1          |
| Alice | HR         | 5000   | 2          |
| Eve   | IT         | 7500   | 1          |
| Frank | IT         | 7200   | 2          |
| Dave  | IT         | 7000   | 3          |
| Grace | Finance    | 8000   | 1          |

---

## 4️⃣ NTILE(2)

**Use case:** Divide employees into 2 salary bands.

```sql
SELECT name, salary,
       NTILE(2) OVER (ORDER BY salary DESC) AS salary_band
FROM employees;
```

**Expected Output:**

| name  | salary | salary_band |
|-------|--------|-------------|
| Grace | 8000   | 1           |
| Eve   | 7500   | 1           |
| Frank | 7200   | 1           |
| Dave  | 7000   | 2           |
| Bob   | 6000   | 2           |
| Carol | 6000   | 2           |
| Alice | 5000   | 2           |

---

## 5️⃣ LAG()

**Use case:** Show each employee’s salary compared to the previous employee.

```sql
SELECT name, salary,
       LAG(salary) OVER (ORDER BY id) AS prev_salary
FROM employees;
```

**Expected Output:**

| name  | salary | prev_salary |
|-------|--------|-------------|
| Alice | 5000   | NULL        |
| Bob   | 6000   | 5000        |
| Carol | 6000   | 6000        |
| Dave  | 7000   | 6000        |
| Eve   | 7500   | 7000        |
| Frank | 7200   | 7500        |
| Grace | 8000   | 7200        |

---

## 6️⃣ LEAD()

**Use case:** Compare current employee's salary with the next employee's.

```sql
SELECT name, salary,
       LEAD(salary) OVER (ORDER BY id) AS next_salary
FROM employees;
```

**Expected Output:**

| name  | salary | next_salary |
|-------|--------|-------------|
| Alice | 5000   | 6000        |
| Bob   | 6000   | 6000        |
| Carol | 6000   | 7000        |
| Dave  | 7000   | 7500        |
| Eve   | 7500   | 7200        |
| Frank | 7200   | 8000        |
| Grace | 8000   | NULL        |

---

## 7️⃣ SUM() OVER

**Use case:** Show department total salary for each employee.

```sql
SELECT name, department, salary,
       SUM(salary) OVER (PARTITION BY department) AS dept_total
FROM employees;
```

**Expected Output:**

| name  | department | salary | dept_total |
|-------|------------|--------|------------|
| Alice | HR         | 5000   | 17000      |
| Bob   | HR         | 6000   | 17000      |
| Carol | HR         | 6000   | 17000      |
| Dave  | IT         | 7000   | 21700      |
| Eve   | IT         | 7500   | 21700      |
| Frank | IT         | 7200   | 21700      |
| Grace | Finance    | 8000   | 8000       |

---

## 8️⃣ FIRST_VALUE() and LAST_VALUE()

**Use case:** Show highest and lowest salary in department.

```sql
SELECT name, department, salary,
       FIRST_VALUE(salary) OVER (PARTITION BY department ORDER BY salary DESC) AS highest,
       LAST_VALUE(salary) OVER (
         PARTITION BY department 
         ORDER BY salary DESC 
         ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
       ) AS lowest
FROM employees;
```

**Expected Output:**

| name  | department | salary | highest | lowest |
|-------|------------|--------|---------|--------|
| Alice | HR         | 5000   | 6000    | 5000   |
| Bob   | HR         | 6000   | 6000    | 5000   |
| Carol | HR         | 6000   | 6000    | 5000   |
| Dave  | IT         | 7000   | 7500    | 7000   |
| Eve   | IT         | 7500   | 7500    | 7000   |
| Frank | IT         | 7200   | 7500    | 7000   |
| Grace | Finance    | 8000   | 8000    | 8000   |

---

