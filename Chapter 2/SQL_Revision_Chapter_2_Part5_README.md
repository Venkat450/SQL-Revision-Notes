
# 📘 SQL Revision Notes - Chapter 2 (Part 5): Aggregate Functions

## 📌 What You Will Learn

- Counting, Summing, Averaging, Min and Max in SQL
- CONCAT function for combining strings
- Filtering with WHERE vs HAVING in calculations

---

## 📦 Sample Table

| id | name | age | height |
|----|------|-----|--------|
| 1  | John | 40  | 150    |
| 2  | May  | 30  | 140    |
| 3  | Tim  | 25  | 180    |
| 4  | Jay  | 40  | 160    |

---

## 🔢 COUNT()

```sql
SELECT COUNT(*) AS user_count 
FROM `new_schema`.`users` 
WHERE id > 1;
```

✅ Counts records where `id > 1`

---

## ➕ SUM()

```sql
SELECT SUM(`age`) AS sum_of_user_ages 
FROM `new_schema`.`users`;
```

✅ Adds up all age values

---

## 📊 AVG()

```sql
SELECT AVG(`height`) AS avg_user_height 
FROM `new_schema`.`users`;
```

✅ Calculates average height

---

## 📉 MIN() and 📈 MAX()

```sql
SELECT MIN(`height`) AS user_min 
FROM `new_schema`.`users`;

SELECT MAX(`height`) AS user_max 
FROM `new_schema`.`users`;
```

✅ Finds smallest and largest heights

---

## 🔗 CONCAT()

```sql
SELECT CONCAT(`id`, '-', `name`) AS identification, `age` 
FROM `new_schema`.`users`;
```

✅ Joins `id` and `name` → creates "identification"

---

## ❗ WHERE vs HAVING

### Cannot filter computed columns using WHERE directly

```sql
-- ❌ This will NOT work
SELECT CONCAT(`id`, '-', `name`) AS identification, `age`
FROM `new_schema`.`users`
WHERE identification LIKE '%J%';
```

✅ Solution → Use Subquery or CTE

#### Subquery

```sql
SELECT * 
FROM (
  SELECT CONCAT(`id`, '-', `name`) AS identification, `age` 
  FROM `new_schema`.`users`
) AS subquery
WHERE identification LIKE '%J%';
```

#### CTE (Common Table Expression)

```sql
WITH CTE_Users AS (
  SELECT CONCAT(`id`, '-', `name`) AS identification, `age`
  FROM `new_schema`.`users`
)
SELECT * 
FROM CTE_Users
WHERE identification LIKE '%J%';
```

✅ `HAVING` should be used only with aggregates or grouped results.

---

## ✅ Summary

| Function | SQL Keyword | Usage |
|----------|-------------|-------|
| Count Rows | COUNT() | How many |
| Sum Values | SUM() | Total |
| Average | AVG() | Mean |
| Minimum | MIN() | Smallest |
| Maximum | MAX() | Largest |
| Concatenate Strings | CONCAT() | Combine values |

---

## 📎 Best Practices

- Use `WHERE` for filtering raw data → faster and indexed
- Use `HAVING` for filtering aggregate results or computed columns
- Avoid using `HAVING` without need → performance impact
- Use Subquery or CTE for calculated fields + filtering

---
