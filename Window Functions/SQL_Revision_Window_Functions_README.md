
# 📘 SQL Revision Notes - Window Functions

## 📌 What You Will Learn

- What are SQL Window Functions
- Why GROUP BY fails in certain scenarios
- Detailed explanation of all major window functions
- Practical examples and use cases

---

## 🚪 What is a Window Function?

A **Window Function** performs a calculation **across a set of table rows that are somehow related to the current row**, but unlike GROUP BY, it does **not collapse rows into groups**.

✅ Keeps all rows  
✅ Adds a new column with computed result (like running total, rank, etc.)

---

## ❌ Why GROUP BY Fails

GROUP BY **aggregates rows into a single row per group**, which:

- ❌ loses row-level detail
- ❌ cannot show original and aggregated values together

Example:

```sql
SELECT department, COUNT(*) 
FROM employees 
GROUP BY department;
```

➡ This gives count per department  
❌ But you cannot also show employee name in same row — because it groups.

---

## ✅ Window Function Syntax

```sql
<window_function>(<expression>) OVER (
    PARTITION BY <column>
    ORDER BY <column>
)
```

- `PARTITION BY` → Like GROUP BY, splits data into groups
- `ORDER BY` → Defines the order of rows in each partition

---

## 🧮 Types of Window Functions

### 1. ROW_NUMBER()

Assigns a unique row number within partition.

```sql
SELECT name, department,
       ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS dept_rank
FROM employees;
```

✅ Gives each employee a rank **within department**  
Useful for: Top-N in each category

---

### 2. RANK() and DENSE_RANK()

Assigns rank based on value ordering.

```sql
SELECT name, salary,
       RANK() OVER (ORDER BY salary DESC) AS salary_rank
FROM employees;
```

- `RANK()` → Skips numbers on ties (e.g., 1, 1, 3)
- `DENSE_RANK()` → Does not skip (e.g., 1, 1, 2)

---

### 3. NTILE(n)

Divides the result into `n` buckets (quartiles, percentiles etc.)

```sql
SELECT name, salary,
       NTILE(4) OVER (ORDER BY salary DESC) AS quartile
FROM employees;
```

---

### 4. LAG() and LEAD()

Access previous or next row **without JOIN**.

```sql
SELECT name, salary,
       LAG(salary) OVER (ORDER BY id) AS prev_salary,
       LEAD(salary) OVER (ORDER BY id) AS next_salary
FROM employees;
```

✅ Useful in time-series, tracking value changes

---

### 5. SUM(), AVG(), COUNT() as Window Functions

Aggregate but without collapsing rows

```sql
SELECT name, department, salary,
       SUM(salary) OVER (PARTITION BY department) AS dept_total,
       AVG(salary) OVER (PARTITION BY department) AS dept_avg
FROM employees;
```

✅ Shows row and group aggregate **together**

---

### 6. FIRST_VALUE() and LAST_VALUE()

Get first/last value in a window partition

```sql
SELECT name, salary,
       FIRST_VALUE(salary) OVER (PARTITION BY department ORDER BY salary) AS min_dept_salary,
       LAST_VALUE(salary) OVER (PARTITION BY department ORDER BY salary) AS max_dept_salary
FROM employees;
```

---

## 🧠 When to Use Window Functions

| Use Case | Use Window Function? |
|----------|----------------------|
| Need to keep row-level detail | ✅ YES |
| Need running totals / ranking / diff between rows | ✅ YES |
| Only need group total, not detail | ❌ Use GROUP BY |
| Need to show row AND aggregate in same row | ✅ YES |

---

## ✅ Summary

| Function | Use Case |
|----------|----------|
| ROW_NUMBER() | Unique row index within group |
| RANK(), DENSE_RANK() | Ranking with/without gaps |
| NTILE(n) | Bucket division |
| LAG(), LEAD() | Compare current with prev/next |
| SUM(), AVG() OVER | Running or partition-wise aggregates |
| FIRST_VALUE(), LAST_VALUE() | Fetch boundary values |

---

## 📎 Best Practices

- Always use ORDER BY inside OVER() for ranking functions
- Use PARTITION BY for per-group calculations
- Use WHERE for raw row filtering, HAVING for group filters

---
